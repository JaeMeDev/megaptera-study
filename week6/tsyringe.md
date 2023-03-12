# 🌈 2. TSyringe

{% hint style="info" %}
💡 Microsoft에서 개발한 것.

참고문서
- [TSyringe](https://github.com/microsoft/tsyringe)
- [reflect-metadata](https://github.com/rbuckton/reflect-metadata)
- [The problem with passing props](https://beta.reactjs.org/learn/passing-data-deeply-with-context#the-problem-with-passing-props)

{% endhint %}

## 🚘 2-1. TSyringe에 대해서

- TypeScript용 DI 도구로 External Store를 관리하는데 활용할 수 있다. React 컴포넌트 입장에서는 “전역”처럼 여겨진다. “Props Drilling” 문제를 우아하게 해결할 수 있는 방법 중 하나이다.(React로 한정한다면 Context도 쓸 수 있다.)

## 🚘 2-2. Tsyringe 사용

- 의존성 설치

```bash
npm i tsyringe reflect-metadata
```

- `src/main.tsx` 파일과 `src/setupTests.ts` 파일에서 reflect-metadata 임포트한다.(프로그램의 시작점)

```tsx
import 'reflect-metadata'
```

- 싱글톤으로 관리할 CounterStore 클래스를 준비한다.

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

- 싱글톤 CounterStore 객체를 사용한다.

```tsx
import { container } from 'tsyringe';

const counterStore = container.resolve(CounterStore);
```

- 테스트에서 TSyringe에서 관리하는 객체를 초기화할 수 있다.

```tsx
container.clearInstances();
```

- 예시 테스트

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

## 🚘 2-3. 상태 변경 알림

- Store는 어떤 식으로든 action을 처리하고, 상태가 바뀌면 연결된 컴포넌트를 forceUpdate한다.
- 컴포넌트는 해당 Store에서 상태를 얻어서 UI를 업데이트하게 되는데, 선언형 UI가 얼마나 편한지 절실히 느낄 수 있다.