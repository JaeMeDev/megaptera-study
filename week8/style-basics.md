# 🌈 2. Style Basics

## 🚘 2-1. Basic: Class

- [스타일링과 CSS](https://ko.reactjs.org/docs/faq-styling.html)
- id를 안쓰는 이유는 중복된 id를 사용해도 문제가 없고. 만약 찾으면 가장 첫번째 element만 반환하기 때문에 안쓴다.
- `index.html` 파일에 간단히 CSS를 추가한다.

```html
<style>
	.greeting {
		color: #00F;
	}
</style>
```

- “clssName”을 지정한다.

```tsx
export default function Greeting() {
	return (
		<p className="greeting">
			Hello, world!
		</p>
	);
}
```

- CSS는 컴포넌트를 전제로 하고 있지 않다. 공통된 부분이 많을 때 재사용하기 쉽다.
- 따라서, 컴포넌트 중심으로 빠르게 작업하려고 하면 불편할 때가 많다. 재사용은 그냥 컴포넌트를 사용하면 된다.
- 절충안으로 아주 작은 스타일 그룹을 클래스로 지정하는 방법도 있긴 하다.(Bootstrap, Tailwind CSS 등의 접근법).

## 🚘 2-2. Basic: Inline Style

- “style” 속성을 활용한다. 평범한 JS 객체를 활용하므로 변수, 함수 등을 재사용하기 쉽다.
- 단점으로는 텍스트가 아니라서 실수하거나 불편할 때가 있다. TS의 힘으로 자동완성, 타입 검사 등의 도움을 받을 수 있다.

```tsx
export default function Greeting() {
	const style = {
		color: '#00F',
	};
	
	return (
		<p style={style}>
			Hello, world!
		</p>
	);
}
```

- 태그 안에 넣어서 바로 사용할 수 있다.

```tsx
export default function Greeting() {
	return (
		<p
			style={{
				color: '#00F',
			}}
		>
			Hello, world!
		</p>
	);
}
```