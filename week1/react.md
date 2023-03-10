# ๐ 3. React

{% hint style="info" %}
[React ๊ณต์๋ฌธ์](https://ko.reactjs.org/)์ [React Beta ๋ฌธ์](https://beta.reactjs.org/)๋ฅผ ๊ผญ ์ฝ์ด๋ณด๊ณ  ํ์ตํด๋ณด์. (React Beta ๋ฌธ์๋ ์ต๊ทผ๋ฒ์ ์ด์ง๋ง ํ๊ตญ์ด ๋ฒ์ ์ด ์๊ณ , ์์ฑ๋๊ฐ ๋ฎ์ง๋ง ์์ฆ React ์ฌ์ฉ๋ฒ์ ๋ค๋ฃธ)
{% endhint %}

## ๐ย 3-1. React ์ฐธ๊ณ ๋ฌธ์

- React๋ก ์์ํ๋ ํ๋ก์ธ์ค๋ [Thinking in React](https://beta.reactjs.org/learn/thinking-in-react)๋ฅผ ์ฐธ๊ณ ํ์. **์ํ**๋ฅผ ๊ณจ๋ผ๋ด๋ ๊ฒ์ด ํต์ฌ.
- ํ๊ตญ์ด๋ก ์ฝ๊ณ  ์ถ์ผ๋ฉด [์์  ๋ฌธ์์ ์ค๋ช](https://ko.reactjs.org/docs/thinking-in-react.html)๋ง ์ด์ง ์ฐธ๊ณ ํ์.(ํด๋์คํ ์ปดํฌ๋ํธ๊ฐ ์๊ธฐ ๋๋ฌธ์ ์ฝ๋๋ ์ฐธ๊ณ ํ์ง ๋ง์)
- [React ์ฝ์ด ๊ฐ๋ฐ์๊ฐ ์ด React์ ๋ํ ์ดํด๋ฅผ ๋๋ ๊ธ](https://overreacted.io/ko/react-as-a-ui-runtime/)(๊ผญ ์ฝ์ด๋ณด์)

## ๐ย 3-2. ๋ ๋๋ง

- [์ฐธ๊ณ ๋ฌธ์(createRoot)](https://beta.reactjs.org/reference/react-dom/client/createRoot)
- [ํ๊ตญ์ด ์ฐธ๊ณ ๋ฌธ์](https://ko.reactjs.org/docs/react-dom-client.html#createroot)

```tsx
function Greeting() {
	return (
		<p>Hello, world!</p>
	);
}

function main() {
	const element = document.getElementById('root');
	
	if(!element){
            return;
	}

	const root = ReactDOM.createRoot(element);
	root.render(<Greething/>);
}

main();
```

- Root๋ฅผ ์ฌ๋ฌ ๊ฐ ๋ง๋ค๊ฑฐ๋, ์ฌ๋ฌ ๋ฒ renderํด๋ ๋๋ค.

```tsx
function Greeting() {
	return (
		<p>Hello, world!</p>
	);
}

function Demo({ count }: { count: number })) {
	return (
		<p>DEMO: {count}</p>
	);
}

function main() {
	const rootElement = document.getElementById('root');
	const demoElement = document.getElementById('demo');

	if(!element || !demoElement){
		return;
	}

	const root = ReactDOM.createRoot(rootElement);
	const demo = ReactDOM.createRoot(demo);

	root.render(<Greething/>);

	let count = 0;
	demo.render(<Demo count={count} />);

	setInterval(()=>{
		count += 1;
		demo.render(<Demo count={count} />);
	}, 1_000);
}

main();
```

- ์ค์ํ ๊ฑด ๋ ๋๋งํ  ๋ ๋ณ๊ฒฝ๋ ๋ถ๋ถ(ํ์ํ ๋ถ๋ถ)๋ง ์๋ฐ์ดํธํ๊ธฐ ๋๋ฌธ์, ๊ธฐ์กด์ ๋ค๋ฅธ ๊ฐ๋ค์ ๋ ๋ผ๊ฐ์ง ์๊ณ  ์ ์ง๋จ.

## ๐ย 3-3. ๋ฆฌ๋ ๋๋ง

- [(์ฐธ๊ณ ๋ฌธ์)React๋ ์ปดํฌ๋ํธ๋ฅผ ์ธ์  ๋ค์ ๋ฆฌ๋ ๋๋ง ํ ๊น?](https://velog.io/@surim014/react-rerender)
- [(์ฐธ๊ณ ๋ฌธ์)์ ๋ฆฌ์กํธ์์ ๋ฆฌ๋ ๋๋ง์ด ๋ฐ์ํ๋๊ฐ.](https://medium.com/@yujso66/%EB%B2%88%EC%97%AD-%EC%99%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8%EC%97%90%EC%84%9C-%EB%A6%AC%EB%A0%8C%EB%8D%94%EB%A7%81%EC%9D%B4-%EB%B0%9C%EC%83%9D%ED%95%98%EB%8A%94%EA%B0%80-74dd239b0063)
- [(์ฐธ๊ณ ๋ฌธ์)React ๋ ๋๋ง ๋์์ ๋ํ (๊ฑฐ์) ์๋ฒฝํ ๊ฐ์ด๋](https://velog.io/@superlipbalm/blogged-answers-a-mostly-complete-guide-to-react-rendering-behavior)
- React๋ State๊ฐ ๋ณ๊ฒฝ๋  ๋ ๋ฆฌ๋ ๋๋ง ๋๋ฉฐ, ๋ถ๋ชจ ์ปดํฌ๋ํธ์ ์ํ์ ๋ฐ๋ผ ์๋ ์ปดํฌ๋ํธ๋ ๋ฆฌ๋ ๋๋ง ๋  ์ ์๋ค.

```tsx
import { useState } from 'react';

export default function App() {
	const [count, setCount] = useState<number>(0);	
	
	const handleClick = () => setCount(count +1);

	return (
		<div>
			<p>Hello, world!</p>
			<p>Count: {count}</p>
			<button type="button" onClick={handleClick}>
				ํด๋ฆญ!
			</button>
		</div>
	);
}
```

## ๐ย 3-4. React๋ ํ๋ ์์ํฌ์ธ๊ฐ, ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ธ๊ฐ

- [๋ฌธ์](https://twitter.com/trueadm/status/1194567962784653312)
- [์ ์ด์ ์ญ์ ](https://martinfowler.com/bliki/InversionOfControl.html)(Ioc: Inversion of Control)์ด ํ๋ ์์ํฌ์ ์ฃผ์ํ ํน์ง์ด๊ณ , React๋ IoC๋ฅผ ํตํด ์ํ์ ์๋ฐ์ดํธ๊ฐ ์ฝํ ๋ณต์กํ ์ํฉ์ ๊ฐ๋จํ ์ ์ธํ UI๋ก ๊ตฌ์ฑํ๋ ํํ์ ๋๋ฆฐ๋ค.(React์ ์ฒซ ๋ฒ์งธ ํน์ง)๊ทธ ๋๊ตฌ๋ ๋งค๋ฒ root๋ฅผ renderํ๋ ๋ฐฉ์์ผ๋ก ์ฐ๋ฉด์ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ผ๋ฉฐ ๊ฐํํ์ง ์๋๋ค.
- ํ์ง๋ง ์ผ๋ฐ์ ์ผ๋ก๋ IoC๋ IoC์ปจํ์ด๋์ ์ฎ์ด์ DI์ ๋์์ด์ฒ๋ผ ์ฐ์ด๊ณ , `Next.js` , `Remix` ๊ฐ์ ๊ฑธ ํ๋ ์์ํฌ๋ผ๊ณ  ๋ถ๋ฅด๋ ๋ฉด์  ๋ ๊ดํ ์ด๋ฐ ๊ฑธ๋ก ์ธ์ฐ์ง๋ง์. ์๋ก ๊ฐ์ ๋ง ์ํ  ๋ฟ์ด๋ค.