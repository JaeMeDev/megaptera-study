# ๐ 4. useRef & Custom Hook

{% hint style="info" %}
์ฐธ๊ณ  ๋ฌธ์

**useRef**
- [beta ๋ฌธ์์ useRef](https://beta.reactjs.org/reference/react/useRef)
- [๊ณต์ ๋ฌธ์์ useRef](https://ko.reactjs.org/docs/hooks-reference.html#useref)

**Custom Hook**
- [Reusing Logic with Custom Hooks](https://beta.reactjs.org/learn/reusing-logic-with-custom-hooks)
- [์์๋ง์ Hook ๋ง๋ค๊ธฐ](https://ko.reactjs.org/docs/hooks-custom.html)

{% endhint %}

## ๐ย 4-1. useRef

- ์ปดํฌ๋ํธ์ ์์ ์ฃผ๊ธฐ ์ ์ฒด์ ๊ฑธ์ณ์ ์ ์ง๋๋ ๊ฐ์ฒด.
- ์ปดํฌ๋ํธ๊ฐ ์์ด์ง ๋๊น์ง ๋์ผํ ๊ฐ์ฒด๊ฐ ์ ์ง๋๋ค.
- ๊ฐ์ฒด ์์ฒด๊ฐ ๊ฐ์ ์๋๊ณ , ๊ฐ์ ์ฐธ์กฐํ๊ธฐ ์ํ ๊ฐ์ฒด์ด๋ค. ์ฆ ์ธ์ ๋ ์ง ๊ฐ์ ๋ณ๊ฒฝํ  ์ ์๋ค.
- ์ํ(state)๊ฐ ๋ณ๊ฒฝ๋๋ฉด ํด๋น ์ปดํฌ๋ํธ์ ํ์ ์ปดํฌ๋ํธ๋ฅผ ๋ค์ ๋ ๋๋งํ์ง๋ง, ๋ ํผ๋ฐ์ค ๊ฐ์ฒด์ ํ์ฌ ๊ฐ(current)์ด ๋ฐ๋๋๋ผ๋ ๋ ๋๋ง์ ์ํฅ์ ์ฃผ์ง ์๋๋ค.
- ์ฃผ์ ์ฉ๋
    - ์ปดํฌ๋ํธ๊ฐ ์ฌ๋ผ์ง ๋๊น์ง ๋์ผํ ๊ฐ์ ์จ์ผ ํ๋ ๊ฒฝ์ฐ. โ input ๋ฑ์ ID ๊ด๋ฆฌ.
    - (ํนํ useEffect ๋ฑ๊ณผ ํจ๊ป ์ฐ๋ฉด์ ๋ง๋๊ฒ ๋๋) ๋น๋๊ธฐ ์ํฉ์์ ํ์ฌ ๊ฐ์ ์ ๋๋ก ์ฐ๊ณ  ์ถ์ ๊ฒฝ์ฐ ์ฌ์ฉํ๋ค.
- Closure โ ๋ณ์๋ฅผ capture, bind๋ฅผ ๊น๋นกํ๋ ๋ฌธ์ ๊ฐ ์ข์ข ์ผ์ด๋๋ค.
- ์ ๋ ์ธ ์ผ ์๋ ์ต์ง๋ก ๊พธ๋ฉฐ๋ธ ์ํฉ. ์ด๋ฐ ๋ฌธ์ ๋ state๊ฐ ๋ณํ  ๋ ref๊ฐ์ ๋ณํ๋๋ก ํ๋ฉด ๋๋ค.

```tsx
useEffect(() => {
	setTimeout(() => {
		// ๊ฐ์ด ๋์ค์ง ์์.
		console.log(filterText);
	}, 5_000);
}, []);
```

## ๐ย 4-2. Custom Hook

- ๋ก์ง์ ์ฌ์ฌ์ฉํ๊ธฐ ์ํ ์ ์ผ ์ฌ์ด ๋ฐฉ๋ฒ์ด๋ค.
- ํ๋ฒํ๊ฒ Extract Function์ ์ํํ๋ฉด ๋๋ค. ์ปดํฌ๋ํธ๊ฐ ๋๋ฌธ์๋ก ์์ํ๋ PascalCase๋ก ์ด๋ฆ์ ๋ถ์ธ๋ค๋ฉด, Hook์ โuseโ๋ก ์์ํ๋ camelCase๋ก ์ด๋ฆ์ ๋ถ์ด๋ฉด ๋๋ค.
- ์ปดํฌ๋ํธ ์ฝ๋๋ ๋ชํํด์ง๊ณ , setProducts๊ฐ ์ค์๋ก ์ฐ์ผ ๋ฌธ์ ๊น์ง ํด์๋๋ค.

```tsx
function useFetchProducts() {
	const [products, setProducts] = useState<Product[]>([]);
	
	useEffect(() => {
		const fetchProducts = async () => {
			const url = 'http://localhost:3000/products';
			const response = await fetch(url);
			const data = await response.json();
			setProducts(data.products);
		};

		fetchProducts();
	}, []);

	return products;
}
```

## ๐ย 4-3. Hook ๊ท์น

- [์ฐธ๊ณ  ๋ฌธ์](https://ko.reactjs.org/docs/hooks-rules.html)
- Hook ํธ์ถ์ ๊ท์น์ด ์์ด์ ๋จ์ํ๊ฒ ์ฐ๋๋ก ๋ธ๋ ฅํด์ผ ํ๋ค.
    - Function Component ๋ฐ๋ก ์์ชฝ(ํจ์์ ์ต์์)์์๋ง ํธ์ถํ๋ค.
    - Function Component ๋๋ Custom Hook์์๋ง ํธ์ถํด์ผ ํ๋ค.
- ์ฒ์์๋ ์ฝ๋ฐฑ ํจ์๋ ์กฐ๊ฑด๋ฌธ ์์์ Hook์ ํธ์ถํ๋ ์ค์๋ฅผ ์ ์ง๋ฅด๊ธฐ ์ฌ์ฐ๋ ์ฃผ์ํด์ผ ํ๋ค.

```tsx
if (playing) {
	const products = useFetchProducts();
	console.log(products);
}
```