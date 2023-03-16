# 🌈 1. Routing

{% hint style="info" %}
💡 Routing에 대해 알아보자.

- [Window.location](https://developer.mozilla.org/ko/docs/Web/API/Window/location)
- [Location](https://developer.mozilla.org/ko/docs/Web/API/Location)

{% endhint %}

## 🚘 1-1. Routing이란?

- 일반적인 웹 사이트는 URL에 따라 다른 웹 페이지를 보여준다.
- 하나의 웹 페이지를 하나의 컴포넌트로 만들고, URL에 따라 적절한 컴포넌트가 보이게 함으로써 구현 가능하다.
- pathname에 따라 다양한 처리가 가능하다.

```tsx
function App() {
	const { pathname } = window.location;

	return (
		<div>
			<Header />
			<main>
				{pathname === '/' && <HomePage />}
				{pathname === '/about' && <AboutPage />}
			</main>
			<Footer />
		</div>
	)
}
```

- pages를 모아주고 처리도 가능하다.

```tsx
function App() {
	const { pathname } = window.location;

	const pages: Record = {
		'/' : HomePage,
		'/about' : AboutPage
	};

	const Page = Reflect.get(pages, path) || HomePage

	return (
		<div>
			<Header />
			<main>
				<Page />
			</main>
			<Footer />
		</div>
	)
}
```