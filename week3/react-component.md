# ๐ 1. React Component

{% hint style="info" %}
[Thinking in React](https://beta.reactjs.org/learn/thinking-in-react)

- Step1 : UI๋ฅผ ์ปดํฌ๋ํธ ๊ณ์ธต๊ตฌ์กฐ๋ก ์ชผ๊ฐ ๋ค.
- Step2 : ์ ์ ์ธ ๋ฒ์ ์ผ๋ก ๋ง๋ค์ด๋ผ.(React๋ก)

{% endhint %}

## ๐ย 1-1. ๋ฐ์ดํฐ

- B/E์์ JSON ํํ์ ๋ฐ์ดํฐ๋ฅผ ๋๋ ค์ฃผ๋ API๋ฅผ ์ ๊ณตํ๋ค๊ณ  ๊ฐ์ ํ๋ค.(REST API or GraphQL)
- ํ๋ก ํธ์๋๋ ๋ฐ์ดํฐ๋ฅผ ์ฌ์ฉ์๊ฐ ๋ณผ ์ ์๋๋ก UI๋ฅผ ๊ตฌ์ฑํ๋ค. React๋ ์ ์ธํ(HTML๊ณผ ์ ์ฌํ ๋ชจ์์ DSL์ ์ฌ์ฉ)์ผ๋ก UI๋ฅผ ๊ตฌ์ฑํ  ์ ์๋ค.

### โย 1-1-1. REST API

- GET, POST, PUT/PATCH, DELETE(CRUD)
- Resource ์ค์ฌ

### โย 1-1-2. GraphQL

- Graph ์๋ฃ๊ตฌ์กฐ
- Query์์ ์ป๊ณ ์ ํ๋ ๊ฑธ ์ง์ 
- Query(Read), Mutation(Command: Create, Update, Delete), Subscription(Event)

### โย 1-1-3. ์ฐธ๊ณ ์๋ฃ

- [JSON](https://ko.wikipedia.org/wiki/JSON)
- [JSON ๊ฐ์](https://www.json.org/json-ko.html)
- [JSON์ผ๋ก ์์ํ๊ธฐ](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/JSON)
- [๋ช๋ นํ ํ๋ก๊ทธ๋๋ฐ](https://ko.wikipedia.org/wiki/%EB%AA%85%EB%A0%B9%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)
- [์ ์ธํ ํ๋ก๊ทธ๋๋ฐ](https://ko.wikipedia.org/wiki/%EC%84%A0%EC%96%B8%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)

## ๐ย 1-2. ์ปดํฌ๋ํธ ๊ณ์ธต ๊ตฌ์กฐ

- ์์ ์ปดํฌ๋ํธ = ๋ถํ
- ์์ ์ปดํฌ๋ํธ๋ฅผ ๋ง๋ค์ด ์กฐ๋ฆฝํ๋ค.
- ์กฐํฉ์ ๊ฐ์ง์๋ฅผ ํญ๋ฐ์ ์ผ๋ก ๋๋ฆด ์ ์๋ ๊ฐ์ฅ ์ ํ์ ์ธ ๋ฐฉ๋ฒ์ด๋ค.
- [Atomic Design](https://bradfrost.com/blog/post/atomic-web-design/)์ ์ฐ๋ฆฌ๊ฐ ์ ์๊ณ  ์๋ ๊ณ์ธตํ ๊ตฌ์กฐ๋ฅผ ๋ช ๊ฐ์ง ์นดํ๊ณ ๋ฆฌ๋ก ๋ฌถ์ ๋ฐฉ๋ฒ์ด๋ค.

### โย 1-2-1. React์ ๊ฐ๋ ฅํ ํน์ง ๋ ์ค ํ๋

- ์ปดํฌ๋ํธ๋ฅผ ๊ธฐ๋ฐ์ผ๋ก ํ๋ค.
- ์บก์ํ๋ ์ปดํฌ๋ํธ๊ฐ ์กฐํฉ๋์ด ๋ณต์กํ UI๋ฅผ ๋ง๋ค์ด๋ธ๋ค. (๊ฐ๋จํ ์ปดํฌ๋ํธ๋ฅผ ๋ชจ์ ๋ณต์กํ UI๋ฅผ ๋ง๋ ๋ค.)

### โย 1-2-2. ์ปดํฌ๋ํธ๋ฅผ ๋๋๋ ๊ธฐ์ค

- [SRP(Single Responsibility Principle)](https://ko.wikipedia.org/wiki/%EB%8B%A8%EC%9D%BC_%EC%B1%85%EC%9E%84_%EC%9B%90%EC%B9%99)
    - ๋จ์ผ ์ฑ์ ์์น
    - ์ปดํฌ๋ํธ๊ฐ ๋๋ฌด ์ปค์ง๊ณ  ์๋ค๋ฉด ์ปดํฌ๋ํธ๋ฅผ ์ชผ๊ฐ์ฃผ์.
- CSS โ ์ด๋ฏธ ์๊ณ  ์๋ ๊ธฐ์ค์ ์ฌํ์ฉํ๋ค.
    - css class
- Designโs Layer
- Information Architecture (JSON Schema์ ์ํฅ)
    - ์ค์ ๋ก ์์ฒญ ๋ง์ด ์ฐ๊ฒ ๋จ.
    - ์์ฐ์ค๋ฌ์ด SRP๋ฅผ ์ํด ์ฌ์ค์ ๊ฐ์ ๋จ

## ๐ย 1-3. Extract Function

- ์ฐธ๊ณ ๋ฌธ์
    - [Extract Function](https://refactoring.com/catalog/extractFunction.html)
    - [Inline Function](https://refactoring.com/catalog/inlineFunction.html)
- ์์ฃผ ํํ ์ฐ์ด๋ SRP๋ฅผ ์ํ ์๋จ์ผ๋ก ๋ณํ์ ํฌ๊ธฐ๋ฅผ ์ ์ฝํ๋ค.
- ์ผ๋จ ๊ธธ๊ฒ ์ฝ๋๋ฅผ ์์ฑํ๊ณ , ์ ์ ํ ์๋ฅผ ์ ์๋ ๋ถ๋ถ์ด ๋ณด์ผ ๋ ํจ์๋ก ์ถ์ถํ๋ค. ๋๋ ์ฝ๋๋ฅผ ์์ฑํ๊ธฐ ์ด๋ ค์ด ์ํฉ์ ์ง๋ฉดํ์ ๋ ํจ์๋ก ์ถ์ถํ๋ค.(๋ฐ๋ก ๋ค๋ฅธ ํ์ผ์ ๋ง๋ค์ด์ผ ํ๋ค๊ณ  ์๊ฐํ์ง ์์๋ ๋๋ค.)
- ์ปดํฌ๋ํธ ๋๋๋ ๊ธฐ์ค์ด ์ ๋งคํ๋ฉด ๋ค์ ํ๋์ ์ปดํฌ๋ํธ๋ก ํฉ์ณค๋ค๊ฐ ๋ค์ ๋๋ ์ค๋ ๋๋ค.

## ๐ย 1-4. Props

- ์ฐธ๊ณ ๋ฌธ์
    - [Passing Props to a Component](https://beta.reactjs.org/learn/passing-props-to-a-component)
    - [Components์ Props](https://ko.reactjs.org/docs/components-and-props.html)
- ๋๋ ์ง ์ปดํฌ๋ํธ๋ฅผ ์๋ก ์ฐ๊ฒฐํ๋ ๋ฐฉ๋ฒ
- Typescript๋ฅผ ์ ์ฐ๊ฑฐ๋ ์๋ชป ์ฐ๊ฒ ๋๋ ํฌ์ธํธ ์ค ํ๋์ด๋ค. ์ ์ ํ ๊ท ํ์ ์ ์ก๋ ๊ฒ ์ค์ํ๋ค.
- ํ์คํธ ์ฝ๋๋ฅผ ์์ฑํ๋ฉด ์ฌ์ฌ์ฉ์ฑ์ ํ๊ฐํ๊ธฐ ์ฌ์์ง๋ค.

## ๐ย 1-5. ์ฝ๋๋ก ๊ตฌํํด๋ณด๊ธฐ

1. UI ๊ทธ๋ฆฌ๊ธฐ(๋ฐ์ดํฐ ์์ด)

```tsx
export default function App() {
	return (
		<div className='filterable-product-table'>
			<div className='search-bar'>
				<div>
					<input type='text' placeholder='Search...' />
				</div>
				<div>
					<input type='checkbox' id='only-stock' />
					<label htmlFor='only-stock'>
                        Only show products in stock
					</label>
				</div>
			</div>
			<table className='product-table'>
				<thead>
					<th>Name</th>
					<th>Price</th>
				</thead>
				<tbody>
					<tr>
						<th colSpan={2}>์นดํ๊ณ ๋ฆฌ</th>
					</tr>
					<tr>
						<td>์ด๋ฆ</td>
						<td>$1</td>
					</tr>
					<tr>
						<td>๋๋ฒ์งธ</td>
						<td>$2</td>
					</tr>
					<tr>
						<td>์ธ๋ฒ์งธ</td>
						<td>$3</td>
					</tr>
				</tbody>
			</table>
		</div>
	);
}
```

1. ๋ฐ์ดํฐ๋ฅผ ์ฌ์ฉํด UI๊ทธ๋ฆฌ๊ธฐ

```tsx
type Product = {
	category: string; price: string; stocked: boolean; name: string;
};

const products: Product[] = [
	{category: 'Fruits', price: '$1', stocked: true, name: 'Apple'},
	{category: 'Fruits', price: '$1', stocked: true, name: 'Dragonfruit'},
	{category: 'Fruits', price: '$2', stocked: false, name: 'Passionfruit'},
	{category: 'Vegetables', price: '$2', stocked: true, name: 'Spinach'},
	{category: 'Vegetables', price: '$4', stocked: false, name: 'Pumpkin'},
	{category: 'Vegetables', price: '$1', stocked: true, name: 'Peas'},
];

export default function App() {
	const categories = products.reduce((acc: string[], product: Product) => (
		acc.includes(product.category) ? acc : [...acc, product.category]
	), []);
	return (
		<div className='filterable-product-table'>
			<div className='search-bar'>
				<div>
					<input type='text' placeholder='Search...' />
				</div>
				<div>
					<input type='checkbox' id='only-stock' />
					<label htmlFor='only-stock'>
                        Only show products in stock
					</label>
				</div>
			</div>
			<table className='product-table'>
				<thead>
					<th>Name</th>
					<th>Price</th>
				</thead>
				<tbody>
					<tr>
						<th colSpan={2}>{categories[0]}</th>
					</tr>
					{products.filter(product => product.category === categories[0]).map(product => (
						<tr key={product.name}>
							<td>{product.name}</td>
							<td>{product.price}</td>
						</tr>
					))}
				</tbody>
			</table>
		</div>
	);
}
```

1. ์ฌ์ฌ์ฉ ๊ฐ๋ฅํ๊ฒ ๋ถํดํ๊ธฐ
- App.tsx

```tsx
import FilterableProductTable from './components/FilterableProductTable';

import type Product from './types/Product';

const products: Product[] = [
	{category: 'Fruits', price: '$1', stocked: true, name: 'Apple'},
	{category: 'Fruits', price: '$1', stocked: true, name: 'Dragonfruit'},
	{category: 'Fruits', price: '$2', stocked: false, name: 'Passionfruit'},
	{category: 'Vegetables', price: '$2', stocked: true, name: 'Spinach'},
	{category: 'Vegetables', price: '$4', stocked: false, name: 'Pumpkin'},
	{category: 'Vegetables', price: '$1', stocked: true, name: 'Peas'},
];

export default function App() {
	return (
		<FilterableProductTable products={products}/>
	);
}
```

- types/Product.ts

```tsx
type Product = {
	category: string; price: string; stocked: boolean; name: string;
};

export default Product;
```

- components/FilterableProductTable.tsx

```tsx
import SearchBar from './SearchBar';
import ProductTable from './ProductTable';

import type Product from '../types/Product';

type FilterableProductTableProps = {
	products: Product[];
};
export default function FilterableProductTable({products}: FilterableProductTableProps) {
	return (
		<div>
			<SearchBar/>
			<ProductTable products={products}/>
		</div>
	);
}
```

- components/SearchBar.tsx

```tsx
import CheckBoxField from './CheckBoxField';

export default function SearchBar() {
	return <div className='search-bar'>
		<div>
			<input type='text' placeholder='Search...' />
		</div>
		<CheckBoxField label='only-stock'/>
	</div>;
}
```

- components/CheckBoxField

```tsx
type CheckBoxFieldProps = {
	label: string;
};

export default function CheckBoxField({label}: CheckBoxFieldProps) {
	const id = `checkbox-${label}`.replace(/ /g, '-').toLowerCase();
	return (
		<div>
			<input type='checkbox' id={id} />
			<label htmlFor={id}>
                Only show products in stock
			</label>
		</div>
	);
}
```

- components/ProductTable.tsx

```tsx
import ProductsInCategory from './ProductsInCategory';

import selectCategories from '../utils/selectCategories';

import type Product from '../types/Product';

type ProductTableProps = {
	products: Product[];
};
export default function ProductTable({products}: ProductTableProps) {
	const categories = selectCategories(products);

	return (
		<table className='product-table'>
			<thead>
				<th>Name</th>
				<th>Price</th>
			</thead>
			<tbody>
				{categories.map(category => <ProductsInCategory key={category} category={category} products={products}/>)}
			</tbody>
		</table>
	);
}
```

- utils/selectCategories.ts

```tsx
import type Product from '../types/Product';

export default function selectCategories(products: Product[]): string[] {
	return products.reduce((acc: string[], product: Product) => {
		const {category} = product;
		return acc.includes(category) ? acc : [...acc, category];
	}, []);
}
```

- components/ProductsInCategory

```tsx
import ProductCategoryRow from './ProductCategoryRow';
import ProductRow from './ProductRow';

import type Product from '../types/Product';

import selectProducts from '../utils/selectProducts';

type ProductsInCategoryProps = {
	category: string;
	products: Product[];
};

export default function ProductsInCategory({category, products}: ProductsInCategoryProps) {
	const productsInCategory = selectProducts(products, category);
	return (
		<>
			<ProductCategoryRow category={category}/>
			{productsInCategory.map(product => (
				<ProductRow key={product.name} product={product}/>
			))}
		</>
	);
}
```

- utils/selectProducts.ts

```tsx
import type Product from '../types/Product';

export default function selectProducts(items: Product[], category: string): Product[] {
	return items.filter(item => item.category === category);
}
```

- components/ProductCategoryRow.tsx

```tsx
type ProductCategoryRowProps = {
	category: string;
};

export default function ProductCategoryRow({category}: ProductCategoryRowProps) {
	return (
		<tr>
			<th colSpan={2}>{category}</th>
		</tr>
	);
}
```

- components/ProductRow.tsx

```tsx
import type Product from '../types/Product';

type ProductRowProps = {
	product: Product;
};

export default function ProductRow({product}: ProductRowProps) {
	return (
		<tr>
			<td>
				<span style={product.stocked ? {} : {color: 'red'}}>
					{product.name}
				</span>
			</td>
			<td>{product.price}</td>
		</tr>
	);
}
```