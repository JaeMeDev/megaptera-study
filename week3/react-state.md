# ๐ 2. React State

{% hint style="info" %}
[Thinking in React](https://beta.reactjs.org/learn/thinking-in-react)

- Step3 : ์์ง๋ง ์ต์ํ์ ๊ฒ UI ์ํ๋ฅผ ์ฐพ์๋ผ
- Step4 : ๊ฑ๋ค์ด ์ด๋์ ์๊ฒ ํ ๊ฑด์ง
- Step5 : ๋ฐ์ดํฐ ํ๋ก์ฐ์ ์ธ๋ฒ์คํ ๊ฒ์ ๋ฃ๋๋ค.

{% endhint %}

## ๐ย 2-1. React์ State

- React์ state๋ ๋ณ๊ฒฝ์ ๋ค๋ฃจ๊ธฐ ์ํ ์์. Re-rendering์ด๋ ์ฃผ์ ์์ ๋ค๋ค์ง๋ค.
- ๊ฑฐ์น ๊ฒ ์ด์ผํ๊ธฐํ๋ฉด, ์ด๋ค ์ปดํฌ๋ํธ์ state๊ฐ ๋ฐ๋๋ฉด ํด๋น ์ปดํฌ๋ํธ์ ํ์ ์ปดํฌ๋ํธ๋ฅผ ๋ค์ ๋ ๋๋งํ๊ฒ ๋๋ค.
- ์๋ฌด๋ ๊ฒ๋ ๋ง ๋ง๋ค์ด๋ ๋์ง๋ง, ์ผ๊ด์ฑ๊ณผ ํจ์จ์ ์ํด DRY์์น์ ๋ฐ๋ฅด๋ SSOT๋ฅผ ๋ง๋ ๋ค.
- React State์ ์กฐ๊ฑด
    - ๋ณ๊ฒฝ๋ผ์ผ ํจ. ๋ณ๊ฒฝ๋์ง ์๋ ๊ฑด state๋ก ๋ค๋ฃฐ ๊ฐ์น๊ฐ ์๋ค.
    - ๋ถ๋ชจ ์ปดํฌ๋ํธ๊ฐ props๋ฅผ ํตํด ์ ๋ฌํ๋ค๋ฉด state๊ฐ ์๋.
    - ๋ค๋ฅธ state๋ props๋ฅผ ์ด์ฉํด ๊ณ์ฐ ๊ฐ๋ฅํ๋ค๋ฉด state๊ฐ ์๋.
- ๋ค๋ฃจ๋ ์ํ๊ฐ ๋๋ฌด ๋ง์ผ๋ฉด ๋ณต์กํ๋ค. TypeScript๋ฅผ ์ ์ฐ๋ฉด ์ง์  ๊ด๋ฆฌํ๋ ์ํ์ ์๋ฅผ ์ค์ฌ์ค ์ ์๋ค.
- ์ํ๋ ๋๊ฐ ๊ด๋ฆฌํด์ผํ๋ฉฐ ๋ ์ ํํ ๋๊ฐ ์์ ํด์ผ ํ ๊น? (React๋ง ์ด๋ค๋ฉด)ํด๋น ์ํ์ ์์กด์ ์ธ ์ปดํฌ๋ํธ๋ฅผ ๋ชจ๋ ํฌํจํ๋ ์ํ๋ฅผ ์์ ํด์ผ ํจ.
- ์ฐธ๊ณ ๋ฌธ์
    - [DRY(Donโt Repeat Yourself)](https://ko.wikipedia.org/wiki/%EC%A4%91%EB%B3%B5%EB%B0%B0%EC%A0%9C)
    - [SSOT(Single Source of Truth)](https://ko.wikipedia.org/wiki/%EB%8B%A8%EC%9D%BC_%EC%A7%84%EC%8B%A4_%EA%B3%B5%EA%B8%89%EC%9B%90)
    - [Lifting State Up](https://ko.reactjs.org/docs/lifting-state-up.html)
    - [Sharing State Between Components](https://beta.reactjs.org/learn/sharing-state-between-components)

## ๐ย 2-2. Inverse Data Flow

- ํ์ ์ปดํฌ๋ํธ์ props๋ก ํจ์๋ฅผ ์ ๋ฌ. ํํ ์ฝ๋ฐฑ ํจ์๋ผ ๋ถ๋ฅธ๋ค.
- TypeScript๋ ํจ์๊ฐ ์ผ๊ธ ๊ฐ์ฒด๋ค. ์ฆ ์ด๋ค ํจ์๋ฅผ ๋ค๋ฅธ ํจ์์ ์ธ์๋ก ๋๊ฒจ์ฃผ๊ฑฐ๋, ์ด๋ค ํจ์๋ฅผ ๋ฆฌํด๊ฐ์ผ๋ก ์ฌ์ฉํ  ์ ์๋ค. ์ต๋ช ํจ์, ํด๋ก์  ๋ฑ๊ณผ ํจ๊ป ์ฌ์ฉํ๋ฉด ์๋์ง๊ฐ ํฌ๋ค.
- [์ผ๊ธ ํจ์](https://developer.mozilla.org/ko/docs/Glossary/First-class_Function)

## ๐ย 2-3. ์์ดํ ๋ชฉ๋ก์์ key๊ฐ value์ธ ๊ฒ๋ง ์ ํํ๊ธฐ.

```tsx
function select<ItemType, ValueType>(
	items: ItemType[],
	key: keyof ItemType,
	value: ValueType
) {
	return items.filter((item) => item[key] === value);
}
```

## ๐ย 2-4. ์ฝ๋๋ก ๊ตฌํํด๋ณด๊ธฐ

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