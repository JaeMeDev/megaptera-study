# 🌈 3. Redux 따라하기

## 🚘 3-1. 따라해보기

- `src/stores/BaseStore.ts` 파일 만들기

```tsx
export type Action<Payload> = {
	type: string;
	payload?: Payload;
}

type Reducer<State, Payload> = (state: State, action: Action<Payload>) => State;

type Reducers<State> = Record<string, Reducer<State, any>>;

type Listener = () => void;

export default class BaseStore<State> {
	state: State;

	reducer: Reducer<State, any>;

	listeners = new Set<Listener>();
	
	constructor(initialState: State, reducers: Reducers<State>) {
		this.state = initialState;

		this.reducer = (state: State, action: Action<any>): State => {
			const reducer = Reflect.get(reducers, action.type);			
			if (!reducer) {
				return state;
			}
			return reducer(state, action);
		};
	}
	
	dispatch<T>(action: Action<T>) {
		this.state = this.reducer(this.state, action);
		this.listeners.forEach((listener) => listener());
	}
	
	addListener(listener: Listener) {
		this.listeners.add(listener);
	}
	
	removeListener(listener: Listener) {
		this.listeners.delete(listener);
	}
}
```

- `src/stores/Store.ts` 파일 만들기

```tsx
import { singleton } from 'tsyringe';

import BaseStore, { Action } from './BaseStore';

const initialState = {
	count: 0,
	name: 'Tester',
};

export type State = typeof initialState;

function increase(state: State, action: Action<number>) {
	return {
		...state,
		count: state.count + (action.payload ?? 1),
	};
}

function decrease(state: State) {
	return {
		...state,
		count: state.count - 1,
	};
}

const reducers = {
	increase,
	decrease,
};

@singleton()
export default class Store extends BaseStore<State> {
	constructor() {
		super(initialState, reducers);
	}
}
```

- `src/hooks/useDispatch.ts` 파일 만들기

```tsx
import { container } from 'tsyringe';

import { Action } from '../stores/BaseStore';

import Store from '../stores/Store';

export default function useDispatch<Payload>() {
	const store = container.resolve(Store);
	return (action: Action<Payload>) => store.dispatch(action);
}
```

- `src/hooks/useSelector.ts` 파일 만들기

```tsx
import { container } from 'tsyringe';

import { useEffect, useState } from 'react';

import Store, { State } from '../stores/Store';

import useForceUpdate from './useForceUpdate';

type Selector<T> = (state: State) => T;

export default function useSelector<T>(selector: Selector<T>): T {
	const store = container.resolve(Store);
	
	const [state, setState] = useState<T>(selector(store.state));
	
	const forceUpdate = useForceUpdate();
	
	useEffect(() => {
		const update = () => {
			const newState = selector(store.state);
			// TODO: T가 object일 때 처리 필요함.
			if (newState !== state) {
				setState(newState);
				forceUpdate();
			}
		};
	
		store.addListener(update);
		return () => store.removeListener(update);
	}, [store, forceUpdate]);

	return state;
}
```

- Dispatch와 Selector 사용하기

```tsx
const dispatch = useDispatch();
const count = useSelector((state) => state.count);

dispatch({ type: 'increase' });
dispatch(increase());

dispatch({ type: 'decrease' });
dispatch(decrease());

dispatch({ type: 'increase', payload: 10 });
```