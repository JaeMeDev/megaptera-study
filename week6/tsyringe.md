# ๐ 2. TSyringe

{% hint style="info" %}
๐ก Microsoft์์ ๊ฐ๋ฐํ ๊ฒ.

์ฐธ๊ณ ๋ฌธ์
- [TSyringe](https://github.com/microsoft/tsyringe)
- [reflect-metadata](https://github.com/rbuckton/reflect-metadata)
- [The problem with passing props](https://beta.reactjs.org/learn/passing-data-deeply-with-context#the-problem-with-passing-props)

{% endhint %}

## ๐ย 2-1. TSyringe์ ๋ํด์

- TypeScript์ฉ DI ๋๊ตฌ๋ก External Store๋ฅผ ๊ด๋ฆฌํ๋๋ฐ ํ์ฉํ  ์ ์๋ค. React ์ปดํฌ๋ํธ ์์ฅ์์๋ โ์ ์ญโ์ฒ๋ผ ์ฌ๊ฒจ์ง๋ค. โProps Drillingโ ๋ฌธ์ ๋ฅผ ์ฐ์ํ๊ฒ ํด๊ฒฐํ  ์ ์๋ ๋ฐฉ๋ฒ ์ค ํ๋์ด๋ค.(React๋ก ํ์ ํ๋ค๋ฉด Context๋ ์ธ ์ ์๋ค.)

## ๐ย 2-2. Tsyringe ์ฌ์ฉ

- ์์กด์ฑ ์ค์น

```bash
npm i tsyringe reflect-metadata
```

- `src/main.tsx` ํ์ผ๊ณผ `src/setupTests.ts` ํ์ผ์์ reflect-metadata ์ํฌํธํ๋ค.(ํ๋ก๊ทธ๋จ์ ์์์ )

```tsx
import 'reflect-metadata'
```

- ์ฑ๊ธํค์ผ๋ก ๊ด๋ฆฌํ  CounterStore ํด๋์ค๋ฅผ ์ค๋นํ๋ค.

```tsx
import { singleton } from 'tsyringe';

type Listener = () => void;

@singleton()
class CounterStore {
	count = 0;

	listeners = new Set<Listener>();

	publish() {
		this.forceUpdates.forEach((listener) => {
			listener();
		});
	}

	addListener(listener: () => void) {
		this.listeners.add(listener);
	}

	removeListener(listener: () => void) {
		this.listeners.delete(listener);
	}
}
```

- ์ฑ๊ธํค CounterStore ๊ฐ์ฒด๋ฅผ ์ฌ์ฉํ๋ค.

```tsx
import { container } from 'tsyringe';

const counterStore = container.resolve(CounterStore);
```

- ํ์คํธ์์ TSyringe์์ ๊ด๋ฆฌํ๋ ๊ฐ์ฒด๋ฅผ ์ด๊ธฐํํ  ์ ์๋ค.

```tsx
container.clearInstances();
```

- ์์ ํ์คํธ

```tsx

const context = describe;

describe('App', () => {
	beforeEach(() => {
		container.clearInstances();
	});
	context('when press increase button once', () => {
			test('counter' ,() => {
			render(<App/>);

			fireEvent.click(screen.getByTeaxt('Increase');
	
			const el = screen.getAllByText('Count: 1');

			expect(el).toHaveLength(2);
		})
	});
	context('when press increase button once', () => {
		test('counter' ,() => {
			render(<App/>);
	
			fireEvent.click(screen.getByTeaxt('Increase');
			fireEvent.click(screen.getByTeaxt('Increase');		
	
			const el = screen.getAllByText('Count: 2');
	
			expect(el).toHaveLength(2);
		});
	});
})
```

## ๐ย 2-3. ์ํ ๋ณ๊ฒฝ ์๋ฆผ

- Store๋ ์ด๋ค ์์ผ๋ก๋  action์ ์ฒ๋ฆฌํ๊ณ , ์ํ๊ฐ ๋ฐ๋๋ฉด ์ฐ๊ฒฐ๋ ์ปดํฌ๋ํธ๋ฅผ forceUpdateํ๋ค.
- ์ปดํฌ๋ํธ๋ ํด๋น Store์์ ์ํ๋ฅผ ์ป์ด์ UI๋ฅผ ์๋ฐ์ดํธํ๊ฒ ๋๋๋ฐ, ์ ์ธํ UI๊ฐ ์ผ๋ง๋ ํธํ์ง ์ ์คํ ๋๋ ์ ์๋ค.