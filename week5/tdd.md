# 🌈 1. TDD

{% hint style="info" %}
💡 TDD란 **Test Driven Development**의 약자이다.(테스트 주도 개발)

- [테스트 주도 개발](https://github.com/ahastudio/til/blob/main/agile/test-driven-development.md)
- [TDD FAQ](https://github.com/ahastudio/til/blob/main/blog/2016/12-03-tdd-faq.md)
- [Jest를 이용한 간단한 TDD 예제](https://github.com/ahastudio/til/blob/main/jest/20201204-simple-tdd-example.md)

{% endhint %}

## 🚘 1-1. TDD란?

- TDD는 테스트 코드를 먼저 작성하는, 즉 구현보다 **인터페이스와 스펙**을 먼저 정의함으로써 개발을 진행하는 방식이다.
- 여기서 인터페이스란 예) add(x, y) ⇒ z 이런 것들의 집합 체
- TDD를 진행할 때는 Cycle를 반복 진행해야 한다.
    1. **RED** : 실패하는 테스트 코드를 작성. 인터페이스와 스펙에 집중한다.
    2. **GREEN** : 재빨리 테스트를 통과시킨다. 올바른 방법이 아니어도 괜찮다.
    3. **Refactor** : 리팩터링을 통해 코드를 올바르게 만든다. TDD에서 가장 중요한 부분이지만, 간과될 때가 많다.
- TDD의 목표는 제대로 작동하는 클린코드를 만드는 것이다.
- 작은 단계를 찾고, 코드에서 피드백을 얻는게 어렵지만 중요하다. 2번이 어렵다면 1번으로 돌아가서 더 작고 쉬운 문제를 정의하고, 3번을 위해 의도를 드러내고 중복을 찾아 제거하는 연습을 해야 한다. 이 둘이 익숙하지 않으면 TDD를 하는 게 거의 불가능하고, 사실 이 둘이 어려우면 일반적인 개발 또는 클린코드를 작성하는 것 또한 매우 힘들다.
- 매우 많은 **연습**이 필요하다.

## 🚘 1-2. Jest

- 참고 문서
    - [Jest](https://jestjs.io/)
    - [Given-When-Then](https://github.com/ahastudio/til/blob/main/blog/2018/12-08-given-when-then.md)
- 테스트 케이스를 정의할 때 크게 두 가지 방법을 사용한다.
- Jest에서 TypeScript를 사용할 때 꼭 `jest.config.js` 를 작성하자.

```jsx
module.exports = {
	testEnvironment: 'jsdom',
	setupFilesAfterEnv: [
		'@testing-library/jest-dom/extend-expect',
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

1. test 함수로 개별 테스트를 나열하는 방식.

```tsx
function add(x: number, y: number): number {
	return x + y;
}

test('add', () => {
	expect(add(1, 2)).toBe(3);
});
```

2. BDD 스타일로 주체-행위 중심으로 테스트를 조직화하는 방식.(대상과 행위를 명확히 드러내자)

```tsx
function add(x: number, y: number): number {
	return x + y;
}

describe('add', () => {
	it('두 숫자의 합을 리턴한다.', () => {
		expect(add(1, 2)).toBe(3);
	});
});
```

다양한 경우를 고려할 때는 `context`를 사용한다.

```tsx
function add(...numbers: number[]): number {
	if (numbers.length === 0) {
		return 0;
	}

	return numbers.reduce((acc, currentValue) => acc + currentValue);
}

const context = describe;

describe('add', () => {
	context('인자가 없다면', () => {
		it('0을 리턴한다.', () => {
			expect(add()).toBe(0);
		});
	});
	context('인자가 하나만 주어지면', () => {
		it('같은 값을 리턴한다.', () => {
			expect(add(2)).toBe(2);
		});
	});
	context('인자가 두개 주어지면', () => {
		it('두 숫자의 합을 리턴한다.', () => {
			expect(add(1, 2)).toBe(3);
		});
	});
	context('인자가 세개 주어지면', () => {
		it('세 숫자의 합을 리턴한다.', () => {
			expect(add(1, 2, 3)).toBe(6);
		});
	});
});
```

- 중요한 것은 테스트를 통과하는 것보다 테스트를 믿고 리팩터링 해서 더 좋은 클린코드를 만드는 것이 중요하다.
- 코딩테스트 문제로 연습해보면 실력이 향상될 것이다.