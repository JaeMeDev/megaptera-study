# ๐ 4. usestore-ts

## ๐ย 4-1. Action์ ๋ฉ์๋๋ก ์ฒ๋ฆฌํ๊ธฐ

- `src/stores/ObjectStore.ts`  ํ์ผ ๋ง๋ค๊ธฐ

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

- `src/stores/CounterStore.ts` ํ์ผ ๋ง๋ค๊ธฐ

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

- `src/hooks/useObjectStore.ts` ํ์ผ ๋ง๋ค๊ธฐ

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

- `src/hooks/useCounterStore.ts` ํ์ผ ๋ง๋ค๊ธฐ

```tsx
import { container } from 'tsyringe';

import useObjectStore from './useObjectStore';

import CounterStore from '../stores/CounterStore';

export default function useCounterStore() {
	const store = container.resolve(CounterStore);

	return useObjectStore(store);
}
```

## ๐ย 4-2. usestore-ts ์ฌ์ฉ

- [usestore-ts](https://usestore-ts.com/)
- Store ์์ฑ

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

- ์ปค์คํ Hook ์์ฑ

```tsx
import { container } from 'tsyringe';

import { useStore } from 'usestore-ts';

import CounterStore from '../stores/CounterStore';

export default function useCounterStore() {
	const store = container.resolve(CounterStore);
	return useStore(store);
}
```

- ๋น๋๊ธฐ ํจ์๋ `@Action` ์ ๋ถ์ด๋ฉด ๋ค๋ฅด๊ฒ ์๋ํ  ์ ์๋ค๋ ์ ์ ์ฃผ์ํด์ผ ํ๋ค. ๋ณ๋์ ์ก์์ ๋ง๋ค๋ฉด ์ ๊ฒฝ ์ธ ๋ถ๋ถ์ด ์ค์ด๋ ๋ค.

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

- ์ฐธ๊ณ  ๋ฌธ์
    - [useSyncExternalStore](https://beta.reactjs.org/reference/react/useSyncExternalStore)
    - [FECONF 2022 - ์ํ๊ด๋ฆฌ ์ด ์ ์์ ๋๋ด๋ฌ ์๋ค](https://www.youtube.com/watch?v=KEDUqA9JeIo)