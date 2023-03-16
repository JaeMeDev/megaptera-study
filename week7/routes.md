# 🌈 2. Routes

{% hint style="info" %}
💡 React Router의 Routes에 대해 알아보자.

- [React Router](https://reactrouter.com/en/main)
- [Routes](https://reactrouter.com/en/main/components/routes)
- [Route](https://reactrouter.com/en/main/route/route)

{% endhint %}

## 🚘 2-1. Routes에 대해 알아보자.

- 먼저 `react-router-dom` 을 설치하자.

```bash
npm i react-router-dom
```

- 코드를 작성해보자.

```tsx
import { Routes, Route } from 'react-router-dom';

function App() {
	return (
		<div>
			<Header />
			<main>
				<Routes>
					<Route path="/" element={<HomePage />} />
					<Route path="/about" element={<AboutPage />} />
				</Routes>
			</main>
			<Footer />
		</div>
	);
}
```

- 브라우저 라우터를 쓴다고 main에서 처리해보자. (**Context**)
- **[BrowserRouter](https://reactrouter.com/en/main/router-components/browser-router)**

```tsx
import { BrowserRouter } from 'react-router-dom';

root.render((
	<BrowserRouter>
		<App />
	</BrowserRouter>
));
```

- 테스트 환경에서는 어떻게 할까? `MemoryRouter` 를 사용한다.
- **[MemoryRouter](https://reactrouter.com/en/main/router-components/memory-router)**

```tsx
describe('App', () => {
	function renderApp(path: string) {
		render((
			<MemoryRouter initialEntries={[path]}>
				<App />
			</MemoryRouter>
		));
	}
	
	context('when the current path is “/”', () => {
		it('renders the home page', () => {
			renderApp('/');

			screen.getByText(/Welcome/);
		});
	});
	
	context('when the current path is “/about”', () => {
		it('renders the about page', () => {
			renderApp('/about');

			screen.getByText(/This is test/);
		});
	});
});
```