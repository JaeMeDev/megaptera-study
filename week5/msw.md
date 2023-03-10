# π 3. MSW

{% hint style="info" %}
π‘ MSW(Mock Service Worker)

- [MSW](https://mswjs.io/)
- [Service Worker API](https://developer.mozilla.org/ko/docs/Web/API/Service_Worker_API)
- [μμ¬μ Mock Service Worker(MSW)](https://github.com/ahastudio/til/blob/main/mock-api/msw.md)
- [Mocking REST API](https://mswjs.io/docs/getting-started/mocks/rest-api)
- [integrate mocking into Node](https://mswjs.io/docs/getting-started/integrate/node)

{% endhint %}

## πΒ 3-1. MSWλ?

- μ½λ λ λ²¨μ΄ μλλΌ λ€νΈμν¬ λ λ²¨μμ κ°μ§ κ΅¬ννλ€.
- μ€νλΌμΈ μμ λ±μ μ§μνκΈ° μν μλΉμ€ μμ»€μ κΈ°λ₯μ μ μ©ν νμ©ν κ²μ΄λ€.
- MSW ν¨ν€μ§ μ€μΉ

```bash
npm i -D msw
```

- `jest.config.js` νμΌμ setupFilesAfterEnv μμ±μ `setupTests.ts` νμΌ μΆκ°

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

// jest.mock λΆνμ.

test('App', async () => {
	render(<App />);
	
	await waitFor(() => {
		screen.getByText('Apple');
	});
});
```

- λλ¬΄ λ³Έκ²©μ μΌλ‘ μ½λ©νλ©΄ μ¬μ€μ λ°±μλ κ°λ°μ΄λ μ΄ λΆλΆμ μ£Όμνμ.
- νμ€νΈ νκ²½μΈμ λΈλΌμ°μ λ μ§μνκΈ° λλ¬Έμ, API μ€νμ λμμ§λ§ μμ§ κ΅¬νλμ§ μμ κ²½μ° μμλ‘ μ¬μ©ν  μ μλ€.
- λ¨μν μμ μλ²λ₯Ό λ§λ€ κ±°λΌλ©΄ Expressλ₯Ό μ°λκ² λ λ«μ§λ§, νμ€νΈ μ½λλ μ§μνλ©΄μ κ²Έμ¬κ²Έμ¬ μΉ λΈλΌμ°μ λ₯Ό μ§μνλ μ©λλ‘λ λμμ§ μμ μ νμ΄λ€.
- https://github.com/github/fetch