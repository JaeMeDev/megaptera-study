# 🌈 4. Navigation

{% hint style="info" %}
💡 Navigation에 대해 알아보자.

{% endhint %}

## 🚘 4-1. History.pushState

- [문서](https://developer.mozilla.org/ko/docs/Web/API/History/pushState)

```tsx
const state = {};
const title = '';
const url = '/about';

history.pushState(state, title, url);
```

## 🚘 4-2. Link

- [문서](https://reactrouter.com/en/main/components/link)
- 위에서 `History.pushState` 로 구현했던 부분을 쉽게 구현할 수 있도록 해줌.
- 가장 큰 특징은 미리 컴포넌트를 불러오고 새로 로딩을 하지 않는다.

```tsx
function Header() {

return (
	<header>
		<nav>
			<ul>
				<li><Link to="/">Home</Link></li>
				<li><Link to="/about">About</Link></li>
			</ul>
		</nav>
	</header>
	);
}
```

## 🚘 4-3. NavLink

- [문서](https://reactrouter.com/en/main/components/nav-link)
- 특정 페이지로 이동하면 현재 active(활성화)된 링크를 확인할 수 있음.

```tsx
function Header() {
	return (
		<header>
			<nav>
				<ul>
					<li><NavLink to="/">Home</NavLink></li>
					<li><NavLink to="/about">About</NavLink></li>
				</ul>
			</nav>
		</header>
	);
}
```

## 🚘 4-4. Navigate

- [문서](https://reactrouter.com/en/main/components/navigate)
- 테스트에서 “ReferenceError: Request is not defined” 에러가 나면 “whatwg-fetch”를 임포트해서 해결할 수 있다.
- 렌더링 될 때 현재 위치를 변경함. redirect 시킴.

```tsx
import { Navigate } from 'react-router-dom';

export default function LoginPage() {
	return (
		<Navigate to="/" />
	);
}
```

```tsx
describe('routes', () => {	
	function renderRouter(path: string) {
		const router = createMemoryRouter(routes, { initialEntries: [path] });
		render(<RouterProvider router={router} />);
	}
	
	context('when the current path is “/”', () => {
		it('renders the home page', () => {
			renderRouter('/');
	
			screen.getByText(/Welcome/);
		});
	});
	
	context('when the current path is “/about”', () => {
		it('renders the about page', () => {
			renderRouter('/about');
	
			screen.getByText(/This is test/);
		});
	});

		context('when the current path is “/logout”', () => {
		it('redirect to the home page', () => {
			renderRouter('/logout');
	
			screen.getByText(/Welcome/);
		});
	});
});
```

## 🚘 4-5. useNavigate

- [문서](https://reactrouter.com/en/main/hooks/use-navigate)

```tsx
import { useNavigate } from 'react-router-dom';

export default function Footer() {
	const navigate = useNavigate();
	
	const handleClick = () => {
		navigate('/about');
	};
	
	return (
		<footer>
			<button type="button" onClick={handleClick}>
				Press
			</button>
		</footer>
	);
}
```