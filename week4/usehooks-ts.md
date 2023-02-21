# 🌈 5. usehooks-ts

{% hint style="info" %}
[공식문서](https://usehooks-ts.com/)를 참고하자.
{% endhint %}

## 🚘 5-1. usehooks-ts 설치

- 모든 Hook에 대한 코드가 웹 사이트에 직접 노출된다.

```bash
npm i usehooks-ts
```

## 🚘 5-2. 여러가지 hook들

### ✅ 5-2-1. useBoolean

- 참/거짓을 다룰 땐 toggle 같이 의도가 명확한 함수를 쓰는 것이 좋다.

```tsx
function TimerContorl() {
	const { value: playing, toggle } = useBoolean(); 
	
	const handleClick = () => {
		toggle();
	}
	// ...
}
```

### ✅ 5-2-2. useEffectOnce

- 의존성 배열을 빈 배열로 넣어서 한 번만 실행하는 Effect를 잡아줄 때가 많은데, 이걸 사용하면 더 명확히 드러난다.

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

### ✅ 5-2-2. useFetch

- 정말 간단히 사용할 때 좋다.
- 몇 가지 기능이 더 있는 useFetch 라이브러리가 따로 있다.([use-http](https://use-http.com/))
- 조금 더 복잡해도 괜찮다면, 캐시 이슈를 고려한 좋은 대안이 있다.
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

### ✅ 5-2-3. useInterval

- React에서 setInterval 등을 쓸 때는 주의해야 할 부분이 있어서 Custom Hook을 만들어서 해결해야 한다.
- [useEffect 관점](https://overreacted.io/ko/a-complete-guide-to-useeffect/)
- [React에서의 타이머 영상](https://www.youtube.com/watch?v=2tUdyY5uBSw)
- [Ref 활용](https://overreacted.io/making-setinterval-declarative-with-react-hooks/)

### ✅ 5-2-4. useEventListener

- 모든 종류의 이벤트를 확인할 수 있음. 특히 dispatch Event로 전달되는 커스텀 이벤트에 반응하기 좋다.(추천)

### ✅ 5-2-5. useLocalStorage

- localStorage와 JSON으로 개체 영속화.
- 이벤트를 통해(dispatchEvent + useEventListener) 다른 컴포넌트와 동기화하는 게 매우 중요한 특징.

### ✅ 5-2-6. useDarkMode

- 다크모드를 사용할 때 사용한다.