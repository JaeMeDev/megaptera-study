# 🌈 3. MSW

{% hint style="info" %}
💡 MSW(Mock Service Worker)

- [MSW](https://mswjs.io/)
- [Service Worker API](https://developer.mozilla.org/ko/docs/Web/API/Service_Worker_API)
- [아샬의 Mock Service Worker(MSW)](https://github.com/ahastudio/til/blob/main/mock-api/msw.md)
- [Mocking REST API](https://mswjs.io/docs/getting-started/mocks/rest-api)
- [integrate mocking into Node](https://mswjs.io/docs/getting-started/integrate/node)

{% endhint %}

## 🚘 3-1. MSW란?

- 코드 레벨이 아니라 네트워크 레벨에서 가짜 구현한다.
- 오프라인 작업 등을 지원하기 위한 서비스 워커의 기능을 유용히 활용한 것이다.
- MSW 패키지 설치

```bash
npm i -D msw
```

- `jest.config.js` 파일의 setupFilesAfterEnv 속성에 `setupTests.ts` 파일 추가

```jsx
module.exports = {
	testEnvironment: 'jsdom',
	setupFilesAfterEnv: [
		'@testing-library/jest-dom/extend-expect',
		'<rootDir>/src/setupTests.ts',
	],
	transform: {
		'^.+\\.(t|j)sx?$': ['@swc/jest', {
			jsc: {
				parser: {
					syntax: 'typescript',
					jsx: true,
					decorators: true,
				},
				transform: {
					react: {
						runtime: 'automatic',
					},
				},
			},
		}],
	},
};
```

- `src/setupTest.js`

```tsx
import 'whatwg-fetch';
import server from './mocks/server';

beforeAll(() => server.listen({ onUnhandledRequest: 'error' }));

afterAll(() => server.close());

afterEach(() => server.resetHandlers());
```

- `src/mocks/server.ts`

```tsx
import { setupServer } from 'msw/node';

import handlers from './handlers';

const server = setupServer(...handlers);

export default server;
```

- `src/mocks/handler.ts`

```tsx
import { rest } from 'msw';

const BASE_URL = 'http://localhost:3000';

const handlers = [
	rest.get(`${BASE_URL}/products`, (req, res, ctx) => {
		const products = [
			{
				category: 'Fruits', price: '$1', stocked: true, name: 'Apple',
			},
		];

		return res(
			ctx.status(200),
			ctx.json({ products }),
			);
		}),
	];

export default handlers;
```

- `App.test.ts`

```tsx
import { render, screen, waitFor } from '@testing-library/react';

import App from './App';

// jest.mock 불필요.

test('App', async () => {
	render(<App />);
	
	await waitFor(() => {
		screen.getByText('Apple');
	});
});
```

- 너무 본격적으로 코딩하면 사실상 백엔드 개발이니 이 부분에 주의하자.
- 테스트 환경외에 브라우저도 지원하기 때문에, API 스펙은 나왔지만 아직 구현되지 않은 경우 임시로 사용할 수 있다.
- 단순히 임시 서버를 만들 거라면 Express를 쓰는게 더 낫지만, 테스트 코드도 지원하면서 겸사겸사 웹 브라우저를 지원하는 용도로는 나쁘지 않은 선택이다.
- https://github.com/github/fetch