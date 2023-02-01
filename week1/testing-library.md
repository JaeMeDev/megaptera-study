# 🌈 4. Testing Library

{% hint style="info" %}
[Jest 공식문서](https://jestjs.io/)

Jest는 거의 모든 것을 갖춘 테스팅 도구이다. Mocha와 Chai처럼 RSpec의 describe-it을 지원하고, expect로 단언(assertion)할 수 있다. Mocking도 다양한 레벨에서 쉽게 사용할 수 있다.

{% endhint %}

## 🚘 4-1. Testing 참고문서

- [BETTER SPECCS](https://www.betterspecs.org/) : Rspec 베스트 프랙티스 모음. 그대로는 쓸 수 없지만 참고해보자.
- [Ginkgo](https://www.youtube.com/watch?v=gfTsSBRvdqI) - Go 언어 개발자를 위한 BDD 테스팅 프레임워크 : Go 언어 사례
- [JUnit5로 계층 구조의 테스트 코드 작성하기](https://johngrib.github.io/wiki/junit5-nested/) : Java 언어 사례
- [Let’s RSpec](https://github.com/ahastudio/til/blob/main/ruby/20161206-rspec-let.md) : Jest는 RSpec의 let 같은 걸 지원하지 않기 때문에, 핵심 아이디어를 가져와서 적당한 수준에서 잘 써야함.

## 🚘 4-2. 테스트 코드 작성

```tsx
// 기본적인 테스트 코드
test('add', () => {
	expect(add(1, 2)).toBe(3);
});
```

```tsx
// BDD 스타일 테스트 코드
// 표현력이 좋아지고, 좀 더 고민할 기회를 제공한다.
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
// context를 통한 BDD(기본으로 context는 제공안함)
const context = describe;

describe('add 함수는', () => {
	context('두 개의 양수가 주어졌을 때', () => {
		it('항상 두 개의 숫자보다 큰 값을 돌려준다.', () => {
			expect(add(1, 2)).toBeGreaterThan(1);
			expect(add(1, 2)).toBeGreaterThan(2);
		});
	})

	context('하나의 양수와 음수가 주어지면', () => {
		it('항상 하나의 양수보다 작은 값을 돌려준다.', () => {
			expect(add(1, -2)).toBeLessThan(1);
		});
	})
});
```

## 🚘 4-3. React Testing Library

- [React Testing Library 공식문서](https://testing-library.com/docs/react-testing-library/intro/)
- [jest-dom](https://testing-library.com/docs/ecosystem-jest-dom/)
- UI 테스트에 특화된 라이브러리로, 거의 E2E Test처럼 쓸 수 있다.
- `F/E 테스트 = only React 컴포넌트 테스트` 가 되는 상황은 최대한 피하는게 좋음. 본질에 집중하지 못하고 너무 많은 테스트 코드를 작성할 위험이 있음. 유지보수를 돕기 위해 테스트 코드를 작성하는 것인데, 테스트 코드를 잘 못 작성하면 오히려 유지보수를 저해할 수 있다.
- 참고 영상
    - [프론트엔드(Front-end)도 테스트해야 하나요?](https://www.youtube.com/watch?v=-kUmsKRmOnA)
    - [Mocking 때문에 테스트 코드를 작성하기 어렵나요](https://www.youtube.com/watch?v=RoQtNLl-Wko)

```tsx
test('Greeting', () => {
	render(<Greeting name="world"/>);
	
	// get은 못찾으면 에러가 발생!
	screen.getByText('Hello, world!');
	
	screen.getByText(/Hello/);
	
	// query는 없으면 에러가 발생하지 않고 없는걸로 나옴
	expect(screen.queryByText(/Hi/)).not.toBeInTheDocument();
});
```