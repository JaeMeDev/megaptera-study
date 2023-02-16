# 🌈 1. React Component

{% hint style="info" %}
[Thinking in React](https://beta.reactjs.org/learn/thinking-in-react)

- Step1 : UI를 컴포넌트 계층구조로 쪼갠다.
- Step2 : 정적인 버전으로 만들어라.(React로)

{% endhint %}

## 🚘 1-1. 데이터

- B/E에서 JSON 형태의 데이터를 돌려주는 API를 제공한다고 가정한다.(REST API or GraphQL)
- 프론트엔드는 데이터를 사용자가 볼 수 있도록 UI를 구성한다. React는 선언형(HTML과 유사한 모양의 DSL을 사용)으로 UI를 구성할 수 있다.

### ✅ 1-1-1. REST API

- GET, POST, PUT/PATCH, DELETE(CRUD)
- Resource 중심

### ✅ 1-1-2. GraphQL

- Graph 자료구조
- Query에서 얻고자 하는 걸 지정
- Query(Read), Mutation(Command: Create, Update, Delete), Subscription(Event)

### ✅ 1-1-3. 참고자료

- [JSON](https://ko.wikipedia.org/wiki/JSON)
- [JSON 개요](https://www.json.org/json-ko.html)
- [JSON으로 작업하기](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/JSON)
- [명령형 프로그래밍](https://ko.wikipedia.org/wiki/%EB%AA%85%EB%A0%B9%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)
- [선언형 프로그래밍](https://ko.wikipedia.org/wiki/%EC%84%A0%EC%96%B8%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)

## 🚘 1-2. 컴포넌트 계층 구조

- 작은 컴포넌트 = 부품
- 작은 컴포넌트를 만들어 조립한다.
- 조합은 가지수를 폭발적으로 늘릴 수 있는 가장 전형적인 방법이다.
- [Atomic Design](https://bradfrost.com/blog/post/atomic-web-design/)은 우리가 잘 알고 있는 계층형 구조를 몇 가지 카테고리로 묶은 방법이다.

### ✅ 1-2-1. React의 강력한 특징 둘 중 하나

- 컴포넌트를 기반으로 한다.
- 캡슐화된 컴포넌트가 조합되어 복잡한 UI를 만들어낸다. (간단한 컴포넌트를 모아 복잡한 UI를 만든다.)

### ✅ 1-2-2. 컴포넌트를 나누는 기준

- [SRP(Single Responsibility Principle)](https://ko.wikipedia.org/wiki/%EB%8B%A8%EC%9D%BC_%EC%B1%85%EC%9E%84_%EC%9B%90%EC%B9%99)
    - 단일 책임 원칙
    - 컴포넌트가 너무 커지고 있다면 컴포넌트를 쪼개주자.
- CSS → 이미 알고 있는 기준을 재활용한다.
    - css class
- Design’s Layer
- Information Architecture (JSON Schema의 영향)
    - 실제로 엄청 많이 쓰게 됨.
    - 자연스러운 SRP를 위해 사실상 강제됨

## 🚘 1-3. Extract Function

- 참고문서
    - [Extract Function](https://refactoring.com/catalog/extractFunction.html)
    - [Inline Function](https://refactoring.com/catalog/inlineFunction.html)
- 아주 흔히 쓰이는 SRP를 위한 수단으로 변화의 크기를 제약한다.
- 일단 길게 코드를 작성하고, 적절히 자를 수 있는 부분이 보일 때 함수로 추출한다. 또는 코드를 작성하기 어려운 상황에 직면했을 때 함수로 추출한다.(바로 다른 파일을 만들어야 한다고 생각하지 않아도 된다.)
- 컴포넌트 나누는 기준이 애매하면 다시 하나의 컴포넌트로 합쳤다가 다시 나눠줘도 된다.

## 🚘 1-4. Props

- 참고문서
    - [Passing Props to a Component](https://beta.reactjs.org/learn/passing-props-to-a-component)
    - [Components와 Props](https://ko.reactjs.org/docs/components-and-props.html)
- 나눠진 컴포넌트를 서로 연결하는 방법
- Typescript를 잘 쓰거나 잘못 쓰게 되는 포인트 중 하나이다. 적절한 균형점을 잡는 게 중요하다.
- 테스트 코드를 작성하면 재사용성을 평가하기 쉬워진다.

## 🚘 1-5. 코드로 구현해보기

1. UI 그리기(데이터 없이)

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
						<th colSpan={2}>카테고리</th>
					</tr>
					<tr>
						<td>이름</td>
						<td>$1</td>
					</tr>
					<tr>
						<td>두번째</td>
						<td>$2</td>
					</tr>
					<tr>
						<td>세번째</td>
						<td>$3</td>
					</tr>
				</tbody>
			</table>
		</div>
	);
}
```

1. 데이터를 사용해 UI그리기

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

1. 재사용 가능하게 분해하기
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