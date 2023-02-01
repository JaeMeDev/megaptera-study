# 🌈 3. React

{% hint style="info" %}
[React 공식문서](https://ko.reactjs.org/)와 [React Beta 문서](https://beta.reactjs.org/)를 꼭 읽어보고 학습해보자. (React Beta 문서는 최근버전이지만 한국어 버전이 없고, 완성도가 낮지만 요즘 React 사용법을 다룸)
{% endhint %}

## 🚘 3-1. React 참고문서

- React로 작업하는 프로세스는 [Thinking in React](https://beta.reactjs.org/learn/thinking-in-react)를 참고하자. **상태**를 골라내는 것이 핵심.
- 한국어로 읽고 싶으면 [예전 문서의 설명](https://ko.reactjs.org/docs/thinking-in-react.html)만 살짝 참고하자.(클래스형 컴포넌트가 있기 때문에 코드는 참고하지 말자)
- [React 코어 개발자가 쓴 React에 대한 이해를 돕는 글](https://overreacted.io/ko/react-as-a-ui-runtime/)(꼭 읽어보자)

## 🚘 3-2. 렌더링

- [참고문서(createRoot)](https://beta.reactjs.org/reference/react-dom/client/createRoot)
- [한국어 참고문서](https://ko.reactjs.org/docs/react-dom-client.html#createroot)

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

- Root를 여러 개 만들거나, 여러 번 render해도 된다.

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

- 중요한 건 렌더링할 때 변경된 부분(필요한 부분)만 업데이트하기 때문에, 기존의 다른 값들은 날라가지 않고 유지됨.

## 🚘 3-3. 리렌더링

- [(참고문서)React는 컴포넌트를 언제 다시 리렌더링 할까?](https://velog.io/@surim014/react-rerender)
- [(참고문서)왜 리액트에서 리렌더링이 발생하는가.](https://medium.com/@yujso66/%EB%B2%88%EC%97%AD-%EC%99%9C-%EB%A6%AC%EC%95%A1%ED%8A%B8%EC%97%90%EC%84%9C-%EB%A6%AC%EB%A0%8C%EB%8D%94%EB%A7%81%EC%9D%B4-%EB%B0%9C%EC%83%9D%ED%95%98%EB%8A%94%EA%B0%80-74dd239b0063)
- [(참고문서)React 렌더링 동작에 대한 (거의) 완벽한 가이드](https://velog.io/@superlipbalm/blogged-answers-a-mostly-complete-guide-to-react-rendering-behavior)
- React는 State가 변경될 때 리렌더링 되며, 부모 컴포넌트의 상태에 따라 자녀 컴포넌트도 리렌더링 될 수 있다.

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
				클릭!
			</button>
		</div>
	);
}
```

## 🚘 3-4. React는 프레임워크인가, 라이브러리인가

- [문서](https://twitter.com/trueadm/status/1194567962784653312)
- [제어의 역전](https://martinfowler.com/bliki/InversionOfControl.html)(Ioc: Inversion of Control)이 프레임워크의 주요한 특징이고, React는 IoC를 통해 상태와 업데이트가 얽힌 복잡한 상황을 간단히 선언형 UI로 구성하는 혜택을 누린다.(React의 첫 번째 특징)그 누구도 매번 root를 render하는 방식으로 쓰면서 라이브러리라며 감탄하지 않는다.
- 하지만 일반적으로는 IoC는 IoC컨테이너와 엮어서 DI와 동의어처럼 쓰이고, `Next.js` , `Remix` 같은 걸 프레임워크라고 부르니 면접 때 괜히 이런 걸로 싸우지말자. 서로 감정만 상할 뿐이다.