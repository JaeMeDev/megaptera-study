# 🌈 2. React Testing Library

{% hint style="info" %}
💡 React Testing Library는 React 컴포넌트를 사용자 입장에 가깝게 테스트할 수 있는 도구이다.

- [React Testing Library](https://github.com/testing-library/react-testing-library)
- [jest-dom](https://github.com/testing-library/jest-dom)

{% endhint %}

## 🚘 2-1. React Testing Library에 대해서

- 거의 E2E Test의 가까운 느낌을 준다.
- 테스트 코드, 즉 컴포넌트를 사용하는 코드를 작성하면서 해당 컴포넌트의 인터페이스를 점검할 수 있다. 기존에 `label` 이 빠져있고, `text` 같이 범용적인 표현을 사용하지 않은 문제가 있었다. 개발하면서 발견할 수 있지만 테스트를 작성했거나 빠르게 테스트코드를 작성했다면 작성하기 전 또는 바로 직후에 문제를 발견해서 수정할 수 있었을 것이다.
- 시간이 지나면 해당 코드에 대한 지식이 감소하고, 자신감 또한 감소하기 때문에 건드리기 힘든 코드가 되기 십상이니 주의하자.
- 테스트 코드를 작성해보자.

```tsx
import {fireEvent, render, screen} from '@testing-library/react';

import TextField from './TextField';

test('TextField', () => {
	// Given
	const label = 'Name';
	const text = 'Tester';

	const setText = jest.fn();

	// When
	render(
		(<TextField
			label={label}
			text={text}
			placeholder='Input your name'
			setText={setText}/>),
	);

	// Then
	screen.getByLabelText(label);
	screen.getByDisplayValue(text);
	screen.getByPlaceholderText(/name/);

	// ----

	// when
	fireEvent.change(screen.getByLabelText(label), {
		target: {value: 'New Name'},
	});
	expect(setText).toBeCalledWith('New Name');
});
```

- BDD 스타일로 코드를 바꿔보자.

```tsx
import {fireEvent, render, screen} from '@testing-library/react';

import TextField from './TextField';

const context = describe;

describe('TextField', () => {
	// Given
	const label = 'Name';
	const text = 'Tester';

	const setText = jest.fn();

	// 매번 테스트 때마다 초기화해줌
	// 초기화 하지 않으면 어디서든 call 하면 call 했다고 인식해버림
	beforeEach(() => {
		setText.mockClear();
		// 모든 mock clear
		// jest.clearAllMocks();
	});

	function renderTextField() {
		render(
			(<TextField
				label={label}
				text={text}
				placeholder='Input your name'
				setText={setText}/>),
		);
	}

	it('renders elements', () => {
		// When
		renderTextField();

		// Then
		screen.getByLabelText(label);
		screen.getByDisplayValue(text);
		screen.getByPlaceholderText(/name/);
	});

	context('when user enter name', () => {
		beforeEach(() => {
			// Given
			renderTextField();
		});
		it('calls "setText" handler', () => {
			// When
			fireEvent.change(screen.getByLabelText(label), {
				target: {value: 'New Name'},
			});

			// Then
			expect(setText).toBeCalledWith('New Name');
		});
	});
});
```

- 외부 의존성이 큰 코드 테스트

```tsx
import { render, screen } from '@testing-library/react';

import App from './App';

jest.mock('./hooks/useFetchProducts', () => () => [
	{
		category: 'Fruits', price: '$1', stocked: true, name: 'Apple',
	},
]);

test('App', () => {
	render(<App />);

	screen.getByText('Apple');
});
```

- 일반적으로 백엔드와 소통하는 부분이 차지하는 비중이 큰데, 이 부분을 하나씩 가짜 구현으로 바꾸다 보면 어려울 때가 있다. 이럴 땐 `MSW` 등 다른 대안을 고려해보자.