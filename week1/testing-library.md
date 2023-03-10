# ๐ 4. Testing Library

{% hint style="info" %}
[Jest ๊ณต์๋ฌธ์](https://jestjs.io/)

Jest๋ ๊ฑฐ์ ๋ชจ๋  ๊ฒ์ ๊ฐ์ถ ํ์คํ ๋๊ตฌ์ด๋ค. Mocha์ Chai์ฒ๋ผ RSpec์ describe-it์ ์ง์ํ๊ณ , expect๋ก ๋จ์ธ(assertion)ํ  ์ ์๋ค. Mocking๋ ๋ค์ํ ๋ ๋ฒจ์์ ์ฝ๊ฒ ์ฌ์ฉํ  ์ ์๋ค.

{% endhint %}

## ๐ย 4-1. Testing ์ฐธ๊ณ ๋ฌธ์

- [BETTER SPECCS](https://www.betterspecs.org/) : Rspec ๋ฒ ์คํธ ํ๋ํฐ์ค ๋ชจ์. ๊ทธ๋๋ก๋ ์ธ ์ ์์ง๋ง ์ฐธ๊ณ ํด๋ณด์.
- [Ginkgo](https://www.youtube.com/watch?v=gfTsSBRvdqI) - Go ์ธ์ด ๊ฐ๋ฐ์๋ฅผ ์ํ BDD ํ์คํ ํ๋ ์์ํฌ : Go ์ธ์ด ์ฌ๋ก
- [JUnit5๋ก ๊ณ์ธต ๊ตฌ์กฐ์ ํ์คํธ ์ฝ๋ ์์ฑํ๊ธฐ](https://johngrib.github.io/wiki/junit5-nested/) : Java ์ธ์ด ์ฌ๋ก
- [Letโs RSpec](https://github.com/ahastudio/til/blob/main/ruby/20161206-rspec-let.md) : Jest๋ RSpec์ let ๊ฐ์ ๊ฑธ ์ง์ํ์ง ์๊ธฐ ๋๋ฌธ์, ํต์ฌ ์์ด๋์ด๋ฅผ ๊ฐ์ ธ์์ ์ ๋นํ ์์ค์์ ์ ์จ์ผํจ.

## ๐ย 4-2. ํ์คํธ ์ฝ๋ ์์ฑ

```tsx
// ๊ธฐ๋ณธ์ ์ธ ํ์คํธ ์ฝ๋
test('add', () => {
	expect(add(1, 2)).toBe(3);
});
```

```tsx
// BDD ์คํ์ผ ํ์คํธ ์ฝ๋
// ํํ๋ ฅ์ด ์ข์์ง๊ณ , ์ข ๋ ๊ณ ๋ฏผํ  ๊ธฐํ๋ฅผ ์ ๊ณตํ๋ค.
describe('add', () => {
	it('returns sum of two numbers', () => {
		expect(add(1, 2)).toBe(3);
	});

	it('returns numbers', () => {
		expect(typeof add(1, 2)).toBe('number');
	});
});
```

```tsx
// context๋ฅผ ํตํ BDD(๊ธฐ๋ณธ์ผ๋ก context๋ ์ ๊ณต์ํจ)
const context = describe;

describe('add ํจ์๋', () => {
	context('๋ ๊ฐ์ ์์๊ฐ ์ฃผ์ด์ก์ ๋', () => {
		it('ํญ์ ๋ ๊ฐ์ ์ซ์๋ณด๋ค ํฐ ๊ฐ์ ๋๋ ค์ค๋ค.', () => {
			expect(add(1, 2)).toBeGreaterThan(1);
			expect(add(1, 2)).toBeGreaterThan(2);
		});
	})

	context('ํ๋์ ์์์ ์์๊ฐ ์ฃผ์ด์ง๋ฉด', () => {
		it('ํญ์ ํ๋์ ์์๋ณด๋ค ์์ ๊ฐ์ ๋๋ ค์ค๋ค.', () => {
			expect(add(1, -2)).toBeLessThan(1);
		});
	})
});
```

## ๐ย 4-3. React Testing Library

- [React Testing Library ๊ณต์๋ฌธ์](https://testing-library.com/docs/react-testing-library/intro/)
- [jest-dom](https://testing-library.com/docs/ecosystem-jest-dom/)
- UI ํ์คํธ์ ํนํ๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ก, ๊ฑฐ์ E2E Test์ฒ๋ผ ์ธ ์ ์๋ค.
- `F/E ํ์คํธ = only React ์ปดํฌ๋ํธ ํ์คํธ` ๊ฐ ๋๋ ์ํฉ์ ์ต๋ํ ํผํ๋๊ฒ ์ข์. ๋ณธ์ง์ ์ง์คํ์ง ๋ชปํ๊ณ  ๋๋ฌด ๋ง์ ํ์คํธ ์ฝ๋๋ฅผ ์์ฑํ  ์ํ์ด ์์. ์ ์ง๋ณด์๋ฅผ ๋๊ธฐ ์ํด ํ์คํธ ์ฝ๋๋ฅผ ์์ฑํ๋ ๊ฒ์ธ๋ฐ, ํ์คํธ ์ฝ๋๋ฅผ ์ ๋ชป ์์ฑํ๋ฉด ์คํ๋ ค ์ ์ง๋ณด์๋ฅผ ์ ํดํ  ์ ์๋ค.
- ์ฐธ๊ณ  ์์
    - [ํ๋ก ํธ์๋(Front-end)๋ ํ์คํธํด์ผ ํ๋์?](https://www.youtube.com/watch?v=-kUmsKRmOnA)
    - [Mocking ๋๋ฌธ์ ํ์คํธ ์ฝ๋๋ฅผ ์์ฑํ๊ธฐ ์ด๋ ต๋์](https://www.youtube.com/watch?v=RoQtNLl-Wko)

```tsx
test('Greeting', () => {
	render(<Greeting name="world"/>);
	
	// get์ ๋ชป์ฐพ์ผ๋ฉด ์๋ฌ๊ฐ ๋ฐ์!
	screen.getByText('Hello, world!');
	
	screen.getByText(/Hello/);
	
	// query๋ ์์ผ๋ฉด ์๋ฌ๊ฐ ๋ฐ์ํ์ง ์๊ณ  ์๋๊ฑธ๋ก ๋์ด
	expect(screen.queryByText(/Hi/)).not.toBeInTheDocument();
});
```