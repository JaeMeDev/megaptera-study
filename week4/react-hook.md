# 🌈 3. React의 Hook

{% hint style="info" %}
참고 문서

- [Hook의 개요](https://ko.reactjs.org/docs/hooks-intro.html)
- [Hook 개요](https://ko.reactjs.org/docs/hooks-overview.html)
- [Hooks API Reference](https://ko.reactjs.org/docs/hooks-reference.html)

{% endhint %}

## 🚘 3-1. React Hook 개요

- React 16.8에서 Hooks가 도입되었다. 기존 방식에 있던 몇 가지 문제를 해결하였다.([영상](https://www.youtube.com/watch?v=dpw9EHDh2bM))
- 기존 방식의 문제점
    - Wrapper Hell(HoC)
    - Huge Components
    - Confusing Classes
- [HoC(Higher-Order Components)](https://ko.reactjs.org/docs/higher-order-components.html)
- React를 쓰는 방식을 완전히 바꾼 커다란 변화이다. → 예전으로 돌아가는 게 불가능하다.
- 기존에는?
    - 상태를 가진 컴포넌트는 Class Component를 만들고, props만 사용하는 재사용이 용이한 작은 컴포넌트는 Function Component로 작성.
    - Redux에서도 비슷한 구분이 존재했다.([문서](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0))
- 현재는 어떨까?
    - 그냥 Function Component만 사용한다.
    - 상태 관리 유무를 바로 알기 어렵다. → 신경쓰지 않아도 된다.
    - 복잡한 요소는 전부 Hook으로 격리 및 재사용이 가능하다.
- 대표적인 Hooks
    - useState → State Hook → React의 State
    - useEffect → Side-effect
    - useContext
    - useRef
    - useLayoutEffect → useEffect와 조금 다르다.

## 🚘 3-2. useEffect

- 참고문서
    - Synchronizing with Effects
    - You Might Not Need an Effect
    - Using the Effect Hook
    - useEffect
    - useEffect 완벽 가이드
- 렌더링 이후 해야 할 일, 즉 React의 외부와 관련된 일을 정해줄 수 있다.
- 기본적으로 렌더링 때마다 실행되므로, 의존성 배열을 통해 언제 이펙트를 실행할지 지정할 수 있다.(불필요한 경우에 건너뛸 수 있다.)
- 함수를 리턴함으로써 종료 처리를 할 수 있다.

### ✅ 3-2-1. 타이머 예제 만들어보기

- React의 외부에 우아하게 접근. 이정도는 useEffect를 사용하지 않아도 되지만, 이렇게 쓰는 습관을 들이자.

```tsx
useEffect(() => {
	document.title = `Now: ${new Date().getTime()}`;
});
```

- 타이머를 on/off하는 기능을 그냥 만들면 문제가 생긴다.

```tsx
function Timer() {
	// setInterval하는 것이 종료되는 부분이 없음.
	// 컴포넌트를 없앴을 때 타이머도 없애야 한다.
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

- 함수를 리턴함으로써 종료를 처리할 수 있다.

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

### ✅ 3-2-2. 처음에 한번만 실행하기

- 의존성 배열에서 아무 것도 지정하지 않으면 맨 처음에 딱 한번만 실행하게 할 수 있다. 주로 API를 호출해서 데이터를 얻을 때 사용한다.

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

- Fetch 함수 위치가 고민되면 [문서](https://overreacted.io/ko/a-complete-guide-to-useeffect/#%ED%95%A8%EC%88%98%EB%A5%BC-%EC%9D%B4%ED%8E%99%ED%8A%B8-%EC%95%88%EC%9C%BC%EB%A1%9C-%EC%98%AE%EA%B8%B0%EA%B8%B0)를 참고하자.

### ✅ 3-2-3. Effect가 두 번 실행되는 문제

- `React.StrictMode` 로 컴포넌트 전체를 감쌀 경우, 예상치 못한 Side Effect를 찾으려고 Effect 등을 두 번씩 실행한다. 평소에 큰 문제가 없지만 API 등을 사용하면 느낄 수 있으니 참고하자.
- Production 빌드랑은 상관없다.
- [문서](https://reactjs.org/docs/strict-mode.html#detecting-unexpected-side-effects)

### ✅ 3-2-4. 의존성 배열을 이용해 Fetch할 때 주의사항

- ignore같은 것(flag)를 넣어서 처리한다.
- [문서](https://beta.reactjs.org/learn/synchronizing-with-effects#fetching-data)