# 🌈 2. React State

{% hint style="info" %}
[Thinking in React](https://beta.reactjs.org/learn/thinking-in-react)

- Step3 : 작지만 최소한의 것 UI 상태를 찾아라
- Step4 : 걔들이 어디에 있게 할건지
- Step5 : 데이터 플로우의 인버스한 것을 넣는다.

{% endhint %}

## 🚘 2-1. React의 State

- React의 state는 변경을 다루기 위한 요소. Re-rendering이란 주제에서 다뤄진다.
- 거칠게 이야하기하면, 어떤 컴포넌트의 state가 바뀌면 해당 컴포넌트와 하위 컴포넌트를 다시 렌더링하게 된다.
- 아무렇게나 막 만들어도 되지만, 일관성과 효율을 위해 DRY원칙을 따르는 SSOT를 만든다.
- React State의 조건
    - 변경돼야 함. 변경되지 않는 건 state로 다룰 가치가 없다.
    - 부모 컴포넌트가 props를 통해 전달한다면 state가 아님.
    - 다른 state나 props를 이용해 계산 가능하다면 state가 아님.
- 다루는 상태가 너무 많으면 복잡하다. TypeScript를 잘 쓰면 직접 관리하는 상태의 수를 줄여줄 수 있다.
- 상태는 누가 관리해야하며 더 정확히 누가 소유해야 할까? (React만 쓴다면)해당 상태에 의존적인 컴포넌트를 모두 포함하는 상태를 소유해야 함.
- 참고문서
    - [DRY(Don’t Repeat Yourself)](https://ko.wikipedia.org/wiki/%EC%A4%91%EB%B3%B5%EB%B0%B0%EC%A0%9C)
    - [SSOT(Single Source of Truth)](https://ko.wikipedia.org/wiki/%EB%8B%A8%EC%9D%BC_%EC%A7%84%EC%8B%A4_%EA%B3%B5%EA%B8%89%EC%9B%90)
    - [Lifting State Up](https://ko.reactjs.org/docs/lifting-state-up.html)
    - [Sharing State Between Components](https://beta.reactjs.org/learn/sharing-state-between-components)

## 🚘 2-2. Inverse Data Flow

- 하위 컴포넌트의 props로 함수를 전달. 흔히 콜백 함수라 부른다.
- TypeScript는 함수가 일급 객체다. 즉 어떤 함수를 다른 함수에 인자로 넘겨주거나, 어떤 함수를 리턴값으로 사용할 수 있다. 익명 함수, 클로저 등과 함께 사용하면 시너지가 크다.
- [일급 함수](https://developer.mozilla.org/ko/docs/Glossary/First-class_Function)

## 🚘 2-3. 아이템 목록에서 key가 value인 것만 선택하기.

```tsx
function select<ItemType, ValueType>(
	items: ItemType[],
	key: keyof ItemType,
	value: ValueType
) {
	return items.filter((item) => item[key] === value);
}
```

## 🚘 2-4. 코드로 구현해보기

- components/CheckBoxField.tsx

```tsx
type CheckBoxFieldProps = {
	inStockOnly: boolean;
	setInStockOnly: (value: boolean) => void;
	label: string;
};

export default function CheckBoxField({label, inStockOnly, setInStockOnly}: CheckBoxFieldProps) {
	const id = `checkbox-${label}`.replace(/ /g, '-').toLowerCase();

	const handleChange = () => {
		setInStockOnly(!inStockOnly);
	};

	return (
		<div>
			<input type='checkbox' id={id} checked={inStockOnly} onChange={handleChange}/>
			<label htmlFor={id}>
                Only show products in stock
			</label>
		</div>
	);
}
```

- components/TextField.tsx

```tsx
import {type ChangeEvent} from 'react';

type TextFieldProps = {
	filterText: string;
	placeholder: string;
	setFilterText: (value: string) => void;
};

export default function TextField({filterText, placeholder, setFilterText}: TextFieldProps) {
	const onChange = (e: ChangeEvent<HTMLInputElement>) => {
		const {value} = e.target;
		setFilterText(value);
	};

	return (
		<div>
			<input type='text' placeholder={placeholder} value={filterText} onChange={onChange}/>
		</div>
	);
}
```

- components/SearchBar.tsx

```tsx
import CheckBoxField from './CheckBoxField';
import TextField from './TextField';
import {useState} from 'react';

type SearchBarProps = {
	filterText: string;
	setFilterText: (value: string) => void;
	inStockOnly: boolean;
	setInStockOnly: (value: boolean) => void;
};
export default function SearchBar({filterText, setFilterText, inStockOnly, setInStockOnly}: SearchBarProps) {
	return (
		<div className='search-bar'>
			<TextField placeholder='Search...' filterText={filterText} setFilterText={setFilterText} />
			<CheckBoxField label='only-stock' inStockOnly={inStockOnly} setInStockOnly={setInStockOnly}/>
		</div>
	);
}
```

- components/FilterableProductTable.tsx

```tsx
import {useState} from 'react';

import SearchBar from './SearchBar';
import ProductTable from './ProductTable';

import type Product from '../types/Product';

import filterProducts from '../utils/filterProducts';

type FilterableProductTableProps = {
	products: Product[];
};
export default function FilterableProductTable({products}: FilterableProductTableProps) {
	const [filterText, setFilterText] = useState<string>('');
	const [inStockOnly, setInStockOnly] = useState<boolean>(false);

	const filteredProducts = filterProducts(products, {filterText, inStockOnly});

	return (
		<div>
			<SearchBar filterText={filterText} setFilterText={setFilterText} inStockOnly={inStockOnly} setInStockOnly={setInStockOnly}/>
			<ProductTable products={filteredProducts}/>
		</div>
	);
}
```

- utils/filterProducts.ts

```tsx
import type Product from '../types/Product';

function normalize(text: string) {
	return text.trim().toLocaleLowerCase();
}

type FilterConditions = {
	filterText: string;
	inStockOnly: boolean;
};

export default function filterProducts(
	products: Product[],
	{filterText, inStockOnly}: FilterConditions,
): Product[] {
	const filteredProducts = products.filter(product => !inStockOnly || product.stocked);

	const query = normalize(filterText);

	if (!query) {
		return filteredProducts;
	}

	const contains = (product: Product) => normalize(product.name).includes((query));

	return filteredProducts.filter(contains);
}
```