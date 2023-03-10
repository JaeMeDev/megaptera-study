# π 2. React Testing Library

{% hint style="info" %}
π‘ React Testing Libraryλ React μ»΄ν¬λνΈλ₯Ό μ¬μ©μ μμ₯μ κ°κΉκ² νμ€νΈν  μ μλ λκ΅¬μ΄λ€.

- [React Testing Library](https://github.com/testing-library/react-testing-library)
- [jest-dom](https://github.com/testing-library/jest-dom)

{% endhint %}

## πΒ 2-1. React Testing Libraryμ λν΄μ

- κ±°μ E2E Testμ κ°κΉμ΄ λλμ μ€λ€.
- νμ€νΈ μ½λ, μ¦ μ»΄ν¬λνΈλ₯Ό μ¬μ©νλ μ½λλ₯Ό μμ±νλ©΄μ ν΄λΉ μ»΄ν¬λνΈμ μΈν°νμ΄μ€λ₯Ό μ κ²ν  μ μλ€. κΈ°μ‘΄μ `label` μ΄ λΉ μ Έμκ³ , `text` κ°μ΄ λ²μ©μ μΈ ννμ μ¬μ©νμ§ μμ λ¬Έμ κ° μμλ€. κ°λ°νλ©΄μ λ°κ²¬ν  μ μμ§λ§ νμ€νΈλ₯Ό μμ±νκ±°λ λΉ λ₯΄κ² νμ€νΈμ½λλ₯Ό μμ±νλ€λ©΄ μμ±νκΈ° μ  λλ λ°λ‘ μ§νμ λ¬Έμ λ₯Ό λ°κ²¬ν΄μ μμ ν  μ μμμ κ²μ΄λ€.
- μκ°μ΄ μ§λλ©΄ ν΄λΉ μ½λμ λν μ§μμ΄ κ°μνκ³ , μμ κ° λν κ°μνκΈ° λλ¬Έμ κ±΄λλ¦¬κΈ° νλ  μ½λκ° λκΈ° μ­μμ΄λ μ£Όμνμ.
- νμ€νΈ μ½λλ₯Ό μμ±ν΄λ³΄μ.

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

- BDD μ€νμΌλ‘ μ½λλ₯Ό λ°κΏλ³΄μ.

```tsx
import {fireEvent, render, screen} from '@testing-library/react';

import TextField from './TextField';

const context = describe;

describe('TextField', () => {
	// Given
	const label = 'Name';
	const text = 'Tester';

	const setText = jest.fn();

	// λ§€λ² νμ€νΈ λλ§λ€ μ΄κΈ°νν΄μ€
	// μ΄κΈ°ν νμ§ μμΌλ©΄ μ΄λμλ  call νλ©΄ call νλ€κ³  μΈμν΄λ²λ¦Ό
	beforeEach(() => {
		setText.mockClear();
		// λͺ¨λ  mock clear
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

- μΈλΆ μμ‘΄μ±μ΄ ν° μ½λ νμ€νΈ

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

- μΌλ°μ μΌλ‘ λ°±μλμ μν΅νλ λΆλΆμ΄ μ°¨μ§νλ λΉμ€μ΄ ν°λ°, μ΄ λΆλΆμ νλμ© κ°μ§ κ΅¬νμΌλ‘ λ°κΎΈλ€ λ³΄λ©΄ μ΄λ €μΈ λκ° μλ€. μ΄λ΄ λ `MSW` λ± λ€λ₯Έ λμμ κ³ λ €ν΄λ³΄μ.