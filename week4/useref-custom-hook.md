# 🌈 4. useRef & Custom Hook

{% hint style="info" %}
참고 문서

**useRef**
- [beta 문서의 useRef](https://beta.reactjs.org/reference/react/useRef)
- [공식 문서의 useRef](https://ko.reactjs.org/docs/hooks-reference.html#useref)

**Custom Hook**
- [Reusing Logic with Custom Hooks](https://beta.reactjs.org/learn/reusing-logic-with-custom-hooks)
- [자시만의 Hook 만들기](https://ko.reactjs.org/docs/hooks-custom.html)

{% endhint %}

## 🚘 4-1. useRef

- 컴포넌트의 생애주기 전체에 걸쳐서 유지되는 객체.
- 컴포넌트가 없어질 때까지 동일한 객체가 유지된다.
- 객체 자체가 값은 아니고, 값을 참조하기 위한 객체이다. 즉 언제든지 값을 변경할 수 있다.
- 상태(state)가 변경되면 해당 컴포넌트와 하위 컴포넌트를 다시 렌더링하지만, 레퍼런스 객체의 현재 값(current)이 바뀌더라도 렌더링에 영향을 주지 않는다.
- 주요 용도
    - 컴포넌트가 사라질 때까지 동일한 값을 써야 하는 경우. ⇒ input 등의 ID 관리.
    - (특히 useEffect 등과 함께 쓰면서 만나게 되는) 비동기 상황에서 현재 값을 제대로 쓰고 싶은 경우 사용한다.
- Closure → 변수를 capture, bind를 깜빡하는 문제가 종종 일어난다.
- 절대 쓸 일 없는 억지로 꾸며낸 상황. 이런 문제는 state가 변할 때 ref값을 변하도록 하면 된다.

```tsx
useEffect(() => {
	setTimeout(() => {
		// 값이 나오지 않음.
		console.log(filterText);
	}, 5_000);
}, []);
```

## 🚘 4-2. Custom Hook

- 로직을 재사용하기 위한 제일 쉬운 방법이다.
- 평범하게 Extract Function을 수행하면 된다. 컴포넌트가 대문자로 시작하는 PascalCase로 이름을 붙인다면, Hook은 “use”로 시작하는 camelCase로 이름을 붙이면 된다.
- 컴포넌트 코드도 명확해지고, setProducts가 실수로 쓰일 문제까지 해소된다.

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

## 🚘 4-3. Hook 규칙

- [참고 문서](https://ko.reactjs.org/docs/hooks-rules.html)
- Hook 호출은 규칙이 있어서 단순하게 쓰도록 노력해야 한다.
    - Function Component 바로 안쪽(함수의 최상위)에서만 호출한다.
    - Function Component 또는 Custom Hook에서만 호출해야 한다.
- 처음에는 콜백 함수나 조건문 안에서 Hook을 호출하는 실수를 저지르기 쉬우니 주의해야 한다.

```tsx
if (playing) {
	const products = useFetchProducts();
	console.log(products);
}
```