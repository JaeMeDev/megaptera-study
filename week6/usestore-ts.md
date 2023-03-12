# 🌈 4. usestore-ts

## 🚘 4-1. Action을 메서드로 처리하기

- `src/stores/ObjectStore.ts`  파일 만들기

```tsx
type Listener = () => void;

export default class ObjectStore {
	private listeners = new Set<Listener>();

	addListener(listener: Listener) {
		this.listeners.add(listener);
	}
	
	removeListener(listener: Listener) {
		this.listeners.delete(listener);
	}
	
	protected publish() {
		this.listeners.forEach((listener) => listener());
	}
}
```

- `src/stores/CounterStore.ts` 파일 만들기

```tsx
import { singleton } from 'tsyringe';

import ObjectStore from './ObjectStore';

@singleton()
export default class CounterStore extends ObjectStore {
	count = 0;
	
	increase(step = 1) {
		this.count += step;
		this.publish();
	}
	
	decrease() {
		this.count -= 1;
		this.publish();
	}
}
```

- `src/hooks/useObjectStore.ts` 파일 만들기

```tsx
import { useEffect } from 'react';

import useForceUpdate from './useForceUpdate';

import ObjectStore from '../stores/ObjectStore';

export default function useObjectStore<T extends ObjectStore>(store: T) {
	const forceUpdate = useForceUpdate();
	
	useEffect(() => {
		store.addListener(forceUpdate);
		return () => store.removeListener(forceUpdate);
	}, [store]);

	return store;
}
```

- `src/hooks/useCounterStore.ts` 파일 만들기

```tsx
import { container } from 'tsyringe';

import useObjectStore from './useObjectStore';

import CounterStore from '../stores/CounterStore';

export default function useCounterStore() {
	const store = container.resolve(CounterStore);

	return useObjectStore(store);
}
```

## 🚘 4-2. usestore-ts 사용

- [usestore-ts](https://usestore-ts.com/)
- Store 작성

```tsx
import { singleton } from 'tsyringe';

import { Store, Action } from 'usestore-ts';

@singleton()
@Store()
export default class CounterStore {
	count = 0;
	
	@Action()
	increase(step = 1) {
		this.count += step;
	}
	
	@Action()
	decrease() {
		this.count -= 1;
	}
}
```

- 커스텀 Hook 작성

```tsx
import { container } from 'tsyringe';

import { useStore } from 'usestore-ts';

import CounterStore from '../stores/CounterStore';

export default function useCounterStore() {
	const store = container.resolve(CounterStore);
	return useStore(store);
}
```

- 비동기 함수는 `@Action` 을 붙이면 다르게 작동할 수 있다는 점에 주의해야 한다. 별도의 액션을 만들면 신경 쓸 부분이 줄어든다.

```tsx
@singleton()
@Store()
class PostStore {
	posts: Post[] = [];
	
	async fetchPosts() {
		this.startLoading();
	
		const posts = await fetchPosts();

		this.completeLoading(posts);
	}
	
	@Action()
	startLoading() {
		this.posts = [];
	}
	
	@Action()
	completeLoading(posts: Post[]) {
		this.posts = posts;
	}
}
```

- 참고 문서
    - [useSyncExternalStore](https://beta.reactjs.org/reference/react/useSyncExternalStore)
    - [FECONF 2022 - 상태관리 이 전쟁을 끝내러 왔다](https://www.youtube.com/watch?v=KEDUqA9JeIo)