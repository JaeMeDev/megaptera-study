# ๐ 5. usehooks-ts

{% hint style="info" %}
[๊ณต์๋ฌธ์](https://usehooks-ts.com/)๋ฅผ ์ฐธ๊ณ ํ์.
{% endhint %}

## ๐ย 5-1. usehooks-ts ์ค์น

- ๋ชจ๋  Hook์ ๋ํ ์ฝ๋๊ฐ ์น ์ฌ์ดํธ์ ์ง์  ๋ธ์ถ๋๋ค.

```bash
npm i usehooks-ts
```

## ๐ย 5-2. ์ฌ๋ฌ๊ฐ์ง hook๋ค

### โย 5-2-1. useBoolean

- ์ฐธ/๊ฑฐ์ง์ ๋ค๋ฃฐ ๋ toggle ๊ฐ์ด ์๋๊ฐ ๋ชํํ ํจ์๋ฅผ ์ฐ๋ ๊ฒ์ด ์ข๋ค.

```tsx
function TimerContorl() {
	const { value: playing, toggle } = useBoolean(); 
	
	const handleClick = () => {
		toggle();
	}
	// ...
}
```

### โย 5-2-2. useEffectOnce

- ์์กด์ฑ ๋ฐฐ์ด์ ๋น ๋ฐฐ์ด๋ก ๋ฃ์ด์ ํ ๋ฒ๋ง ์คํํ๋ Effect๋ฅผ ์ก์์ค ๋๊ฐ ๋ง์๋ฐ, ์ด๊ฑธ ์ฌ์ฉํ๋ฉด ๋ ๋ชํํ ๋๋ฌ๋๋ค.

```tsx
function useFetchProducts() {
	const [products, setProducts] = useState<Product[]>([]);
	
	useEffectOnce(() => {
		const fetchProducts = async () => {
			const url = 'http://localhost:3000/products';
			const response = await fetch(url);
			const data = await response.json();
			setProducts(data.products);
		};

		fetchProducts();
	});

	return products;
}
```

### โย 5-2-2. useFetch

- ์ ๋ง ๊ฐ๋จํ ์ฌ์ฉํ  ๋ ์ข๋ค.
- ๋ช ๊ฐ์ง ๊ธฐ๋ฅ์ด ๋ ์๋ useFetch ๋ผ์ด๋ธ๋ฌ๋ฆฌ๊ฐ ๋ฐ๋ก ์๋ค.([use-http](https://use-http.com/))
- ์กฐ๊ธ ๋ ๋ณต์กํด๋ ๊ด์ฐฎ๋ค๋ฉด, ์บ์ ์ด์๋ฅผ ๊ณ ๋ คํ ์ข์ ๋์์ด ์๋ค.
- [SWR](https://swr.vercel.app/ko), [TanStack Query](https://tanstack.com/query/latest)

```tsx
function useFetchProducts() {
	const url = 'http://localhost:3000/products';
	const { data } useFetch(url);
	
	if(!data){
		return [];
	}

	return data.products;
}
```

### โย 5-2-3. useInterval

- React์์ setInterval ๋ฑ์ ์ธ ๋๋ ์ฃผ์ํด์ผ ํ  ๋ถ๋ถ์ด ์์ด์ Custom Hook์ ๋ง๋ค์ด์ ํด๊ฒฐํด์ผ ํ๋ค.
- [useEffect ๊ด์ ](https://overreacted.io/ko/a-complete-guide-to-useeffect/)
- [React์์์ ํ์ด๋จธ ์์](https://www.youtube.com/watch?v=2tUdyY5uBSw)
- [Ref ํ์ฉ](https://overreacted.io/making-setinterval-declarative-with-react-hooks/)

### โย 5-2-4. useEventListener

- ๋ชจ๋  ์ข๋ฅ์ ์ด๋ฒคํธ๋ฅผ ํ์ธํ  ์ ์์. ํนํ dispatch Event๋ก ์ ๋ฌ๋๋ ์ปค์คํ ์ด๋ฒคํธ์ ๋ฐ์ํ๊ธฐ ์ข๋ค.(์ถ์ฒ)

### โย 5-2-5. useLocalStorage

- localStorage์ JSON์ผ๋ก ๊ฐ์ฒด ์์ํ.
- ์ด๋ฒคํธ๋ฅผ ํตํด(dispatchEvent + useEventListener) ๋ค๋ฅธ ์ปดํฌ๋ํธ์ ๋๊ธฐํํ๋ ๊ฒ ๋งค์ฐ ์ค์ํ ํน์ง.

### โย 5-2-6. useDarkMode

- ๋คํฌ๋ชจ๋๋ฅผ ์ฌ์ฉํ  ๋ ์ฌ์ฉํ๋ค.