# ๐ 3. React์ Hook

{% hint style="info" %}
์ฐธ๊ณ  ๋ฌธ์

- [Hook์ ๊ฐ์](https://ko.reactjs.org/docs/hooks-intro.html)
- [Hook ๊ฐ์](https://ko.reactjs.org/docs/hooks-overview.html)
- [Hooks API Reference](https://ko.reactjs.org/docs/hooks-reference.html)

{% endhint %}

## ๐ย 3-1. React Hook ๊ฐ์

- React 16.8์์ Hooks๊ฐ ๋์๋์๋ค. ๊ธฐ์กด ๋ฐฉ์์ ์๋ ๋ช ๊ฐ์ง ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ์๋ค.([์์](https://www.youtube.com/watch?v=dpw9EHDh2bM))
- ๊ธฐ์กด ๋ฐฉ์์ ๋ฌธ์ ์ 
    - Wrapper Hell(HoC)
    - Huge Components
    - Confusing Classes
- [HoC(Higher-Order Components)](https://ko.reactjs.org/docs/higher-order-components.html)
- React๋ฅผ ์ฐ๋ ๋ฐฉ์์ ์์ ํ ๋ฐ๊พผ ์ปค๋ค๋ ๋ณํ์ด๋ค. โ ์์ ์ผ๋ก ๋์๊ฐ๋ ๊ฒ ๋ถ๊ฐ๋ฅํ๋ค.
- ๊ธฐ์กด์๋?
    - ์ํ๋ฅผ ๊ฐ์ง ์ปดํฌ๋ํธ๋ Class Component๋ฅผ ๋ง๋ค๊ณ , props๋ง ์ฌ์ฉํ๋ ์ฌ์ฌ์ฉ์ด ์ฉ์ดํ ์์ ์ปดํฌ๋ํธ๋ Function Component๋ก ์์ฑ.
    - Redux์์๋ ๋น์ทํ ๊ตฌ๋ถ์ด ์กด์ฌํ๋ค.([๋ฌธ์](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0))
- ํ์ฌ๋ ์ด๋จ๊น?
    - ๊ทธ๋ฅ Function Component๋ง ์ฌ์ฉํ๋ค.
    - ์ํ ๊ด๋ฆฌ ์ ๋ฌด๋ฅผ ๋ฐ๋ก ์๊ธฐ ์ด๋ ต๋ค. โ ์ ๊ฒฝ์ฐ์ง ์์๋ ๋๋ค.
    - ๋ณต์กํ ์์๋ ์ ๋ถ Hook์ผ๋ก ๊ฒฉ๋ฆฌ ๋ฐ ์ฌ์ฌ์ฉ์ด ๊ฐ๋ฅํ๋ค.
- ๋ํ์ ์ธ Hooks
    - useState โ State Hook โ React์ State
    - useEffect โ Side-effect
    - useContext
    - useRef
    - useLayoutEffect โ useEffect์ ์กฐ๊ธ ๋ค๋ฅด๋ค.

## ๐ย 3-2. useEffect

- ์ฐธ๊ณ ๋ฌธ์
    - Synchronizing with Effects
    - You Might Not Need an Effect
    - Using the Effect Hook
    - useEffect
    - useEffect ์๋ฒฝ ๊ฐ์ด๋
- ๋ ๋๋ง ์ดํ ํด์ผ ํ  ์ผ, ์ฆ React์ ์ธ๋ถ์ ๊ด๋ จ๋ ์ผ์ ์ ํด์ค ์ ์๋ค.
- ๊ธฐ๋ณธ์ ์ผ๋ก ๋ ๋๋ง ๋๋ง๋ค ์คํ๋๋ฏ๋ก, ์์กด์ฑ ๋ฐฐ์ด์ ํตํด ์ธ์  ์ดํํธ๋ฅผ ์คํํ ์ง ์ง์ ํ  ์ ์๋ค.(๋ถํ์ํ ๊ฒฝ์ฐ์ ๊ฑด๋๋ธ ์ ์๋ค.)
- ํจ์๋ฅผ ๋ฆฌํดํจ์ผ๋ก์จ ์ข๋ฃ ์ฒ๋ฆฌ๋ฅผ ํ  ์ ์๋ค.

### โย 3-2-1. ํ์ด๋จธ ์์  ๋ง๋ค์ด๋ณด๊ธฐ

- React์ ์ธ๋ถ์ ์ฐ์ํ๊ฒ ์ ๊ทผ. ์ด์ ๋๋ useEffect๋ฅผ ์ฌ์ฉํ์ง ์์๋ ๋์ง๋ง, ์ด๋ ๊ฒ ์ฐ๋ ์ต๊ด์ ๋ค์ด์.

```tsx
useEffect(() => {
	document.title = `Now: ${new Date().getTime()}`;
});
```

- ํ์ด๋จธ๋ฅผ on/offํ๋ ๊ธฐ๋ฅ์ ๊ทธ๋ฅ ๋ง๋ค๋ฉด ๋ฌธ์ ๊ฐ ์๊ธด๋ค.

```tsx
function Timer() {
	// setIntervalํ๋ ๊ฒ์ด ์ข๋ฃ๋๋ ๋ถ๋ถ์ด ์์.
	// ์ปดํฌ๋ํธ๋ฅผ ์์ด์ ๋ ํ์ด๋จธ๋ ์์ ์ผ ํ๋ค.
	useEffect(() => {
		setInterval(() => {
			document.title = `Now: ${new Date().getTime()}`;
		}, 100);
	});

	return (
		<p>Playing</p>
	);
}

export default function TimerControl() {
	const [playing, setPlaying] = useState(false);
	
	const handleClick = () => {
		setPlaying(!playing);
	};

	return (
		<div>
			{playing ? (
				<Timer />
			) : (
				<p>Stop</p>
			)}
			<button type="button" onClick={handleClick}>
				Toggle
			</button>
		</div>
	);
}
```

- ํจ์๋ฅผ ๋ฆฌํดํจ์ผ๋ก์จ ์ข๋ฃ๋ฅผ ์ฒ๋ฆฌํ  ์ ์๋ค.

```tsx
useEffect(() => {
	const savedTitle = document.title;

	const id = setInterval(() => {
		document.title = `Now: ${new Date().getTime()}`;
	}, 100);

	return () => {
		document.title = savedTitle;
		clearInterval(id);
	};
});
```

### โย 3-2-2. ์ฒ์์ ํ๋ฒ๋ง ์คํํ๊ธฐ

- ์์กด์ฑ ๋ฐฐ์ด์์ ์๋ฌด ๊ฒ๋ ์ง์ ํ์ง ์์ผ๋ฉด ๋งจ ์ฒ์์ ๋ฑ ํ๋ฒ๋ง ์คํํ๊ฒ ํ  ์ ์๋ค. ์ฃผ๋ก API๋ฅผ ํธ์ถํด์ ๋ฐ์ดํฐ๋ฅผ ์ป์ ๋ ์ฌ์ฉํ๋ค.

```tsx
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
```

- Fetch ํจ์ ์์น๊ฐ ๊ณ ๋ฏผ๋๋ฉด [๋ฌธ์](https://overreacted.io/ko/a-complete-guide-to-useeffect/#%ED%95%A8%EC%88%98%EB%A5%BC-%EC%9D%B4%ED%8E%99%ED%8A%B8-%EC%95%88%EC%9C%BC%EB%A1%9C-%EC%98%AE%EA%B8%B0%EA%B8%B0)๋ฅผ ์ฐธ๊ณ ํ์.

### โย 3-2-3. Effect๊ฐ ๋ ๋ฒ ์คํ๋๋ ๋ฌธ์ 

- `React.StrictMode` ๋ก ์ปดํฌ๋ํธ ์ ์ฒด๋ฅผ ๊ฐ์ ๊ฒฝ์ฐ, ์์์น ๋ชปํ Side Effect๋ฅผ ์ฐพ์ผ๋ ค๊ณ  Effect ๋ฑ์ ๋ ๋ฒ์ฉ ์คํํ๋ค. ํ์์ ํฐ ๋ฌธ์ ๊ฐ ์์ง๋ง API ๋ฑ์ ์ฌ์ฉํ๋ฉด ๋๋ ์ ์์ผ๋ ์ฐธ๊ณ ํ์.
- Production ๋น๋๋์ ์๊ด์๋ค.
- [๋ฌธ์](https://reactjs.org/docs/strict-mode.html#detecting-unexpected-side-effects)

### โย 3-2-4. ์์กด์ฑ ๋ฐฐ์ด์ ์ด์ฉํด Fetchํ  ๋ ์ฃผ์์ฌํญ

- ignore๊ฐ์ ๊ฒ(flag)๋ฅผ ๋ฃ์ด์ ์ฒ๋ฆฌํ๋ค.
- [๋ฌธ์](https://beta.reactjs.org/learn/synchronizing-with-effects#fetching-data)