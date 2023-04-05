# 🌈 2. 목록보기

## 🚘 2-1. 상품 목록

- 상품 목록을 얻어서 표시하는 화면을 만들어보자. 다음과 같이 두가지로 나누어 작업할 수 있다.
    - 상품 목록 얻기(useFetchProducts)
    - 상품 목록 보여주기(Products 컴포넌트)
- `ProductListPage.tsx`

```tsx
import Products from '../components/product-list/Products';

import useFetchProducts from '../hooks/useFetchProducts';

export default function ProductListPage() {
  const { products } = useFetchProducts();

  return (
    <div>
      <h2>Products</h2>
      <Products products={products} />
    </div>
  );
}
```

- `useFetchProducts.ts`

```tsx

const apiBaseUrl = 'https://shop-demo-api-01.fly.dev';

export default function useFetchProducts() {
  type Data = {
    products: ProductSummary[];
  };

  const { data } = useFetch<Data>(`${apiBaseUrl}/products`);

  return {
    products: data?.products ?? [],
  };
}
```

- `Products.tsx`

```tsx
const Container = styled.div`
  ul {
    display: flex;
    flex-wrap: wrap;
  }

  li {
    width: 20%;
    padding: 1rem;
  }

  a {
    display: block;
    text-decoration: none;
  }
`;

type ProductsProps = {
  products: ProductSummary[];
}

export default function Products({ products }: ProductsProps) {
  if (!products.length) {
    return null;
  }

  return (
    <Container>
      <ul>
        {products.map((product) => (
          <li key={product.id}>
            <Link to={`/products/${product.id}`}>
              <Product product={product} />
            </Link>
          </li>
        ))}
      </ul>
    </Container>
  );
}
```

- 숫자 읽기 좋게 보여주도록 numberFormat 유틸리티 함수 준비

```tsx
export default function numberFormat(value: number) {
  return new Intl.NumberFormat().format(value);
}
```

- Store 관리를 해보자.(캐시를 사용하려면 SWR, React Query를 활용하자)

```tsx
@singleton()
@Store()
export default class ProductsStore {
  products: ProductSummary[] = [];

  async fetchProducts() {
    this.setProducts([]);

    const { data } = await axios.get(`${apiBaseUrl}/products`);
    const { products } = data;

    this.setProducts(products);
  }

  @Action()
  setProducts(products: ProductSummary[]) {
    this.products = products;
  }
}
```

```tsx
export default function useFetchProducts(): {
  products: ProductSummary[];
} {
  const store = container.resolve(ProductsStore);

  const [{ products }] = useStore(store);

  useEffectOnce(() => {
    store.fetchProducts();
  });	

  return { products };
}
```

## 🚘 2-2. 카테고리 목록

- 카테고리 목록을 Header에 보여주자.

```tsx
export default function Header() {
  const { categories } = useFetchCategories();

  return (
    <Container>
      <h1>Shop</h1>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/products">Products</Link>
            {!!categories.length && (
              <ul>
                {categories.map((category) => (
                  <li key={category.id}>
                    <Link to={`/products?categoryId=${category.id}`}>
                      {category.name}
                    </Link>
                  </li>
                ))}
              </ul>
            )}
          </li>
          <li>
            <Link to="/cart">Cart</Link>
          </li>
        </ul>
      </nav>
    </Container>
  );
}
```

- CategoryStore를 준비한다.

```tsx
@singleton()
@Store()
export default class CategoriesStore {
  categories: Category[] = [];

  async fetchCategories() {
    this.setCategories([]);

    const categories = await apiService.fetchCategories();

    this.setCategories(categories);
  }

  @Action()
  setCategories(categories: Category[]) {
    this.categories = categories;
  }
}
```

- ApiService를 만든다. API의 base URL을 지정하기 위해 환경변수를 활용한다.

```tsx
const API_BASE_URL = process.env.API_BASE_URL
                     || 'https://shop-demo-api-01.fly.dev';

export default class ApiService {
  private instance = axios.create({
    baseURL: API_BASE_URL,
  });

  async fetchCategories(): Promise<Category[]> {
    const { data } = await this.instance.get('/categories');
    const { categories } = data;
    return categories;
  }

  async fetchProducts(): Promise<ProductSummary[]> {
    const { data } = await this.instance.get('/products');
    const { products } = data;
    return products;
  }
}

export const apiService = new ApiService();
```

- `useFetchCategories` 훅을 만들어준다.

```tsx
export default function useFetchCategories() {
  const store = container.resolve(CategoriesStore);

  const [{ categories }] = useStore(store);

  useEffectOnce(() => {
    store.fetchCategories();
  });

  return { categories };
}
```

## 🚘 2-3. 카테고리별 상품 목록

- ProdcutListPage 컴포넌트에서 categoryId를 얻는다.

```tsx
export default function ProductListPage() {
  const [params] = useSearchParams();

  const categoryId = params.get('categoryId') ?? undefined;

  const { products } = useFetchProducts({ categoryId });

  return (
    <div>
      <h2>Products</h2>
      <Products products={products} />
    </div>
  );
}
```

- 카테고리 ID를 쓰도록 훅을 변경한다.

```tsx
export default function useFetchProducts({ categoryId }: {
  categoryId: string;
}): {
  products: ProductSummary[];
} {
  const store = container.resolve(ProductsStore);

  const [{ products }] = useStore(store);

  useEffect(() => {
    store.fetchProducts({ categoryId });
  }, [store, categoryId]);

  return { products };
}
```

- Store도 변경한다.

```tsx
async fetchProducts({ categoryId }: {
  categoryId?: string;
}) {
  this.setProducts([]);

  const products = await apiService.fetchProducts({ categoryId });

  this.setProducts(products);
}
```

- API Service도 변경한다.

```tsx
async fetchProducts({ categoryId }: {
  categoryId?: string;
} = {}): Promise<ProductSummary[]> {
  const { data } = await this.instance.get('/products', {
    params: { categoryId },
  });
  const { products } = data;
  return products;
}
```
