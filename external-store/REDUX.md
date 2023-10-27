# Redux

- 태를 하나의 중앙 저장소(Store)에 저장
- 저장소를 통해 상태를 관리, 업데이트, 및 제어할 수 있도록 도와줌

1. 액션 (Actions)
   - 액션은 상태 변경을 나타내는 객체
   - ex. "사용자가 로그인했다" 또는 "새로운 데이터를 불러와서 저장했다"
   - 타입(type)과 함께 데이터(payload)를 포함
2. 리듀서 (Reducers)
   - 리듀서는 액션을 받아 이전 상태를 새로운 상태로 변환하는 함수
   - Redux의 핵심 아이디어 중 하나는 상태 변경 로직을 순수 함수로 작성
   - 함수가 예측 가능하고 불변적인 상태 변경을 보장
3. 스토어 (Store)
   - 스토어는 어플리케이션의 상태를 보유하고 관리하는 곳
   - 액션을 받아 리듀서를 호출하여 상태를 업데이트
   - 업데이트된 상태를 뷰(React 컴포넌트)에게 제공

4. 디스패치 (Dispatch)
   - 디스패치는 액션을 스토어에 보내는 메서드
   - 액션을 디스패치하면 스토어는 해당 액션을 리듀서에게 전달하고 상태를 업데이트
5. 구독 (Subscription)
   - Redux는 스토어의 상태가 변경될 때 뷰를 업데이트할 수 있도록 구독 메커니즘을 제공
   - 컴포넌트는 스토어를 구독하고 상태가 변경될 때 알림을 받아 업데이트

## Reflect

- 메타프로그래밍과 객체 리플렉션(Reflection)을 지원하기 위해 사용

### Reflect 목적

1. 객체 리플렉션 (Object Reflection)
   - 객체의 속성을 가져오거나 설정하고, 객체의 프로토타입 체인을 조작하는 등의 작업이 가능
2. 프록시 (Proxy) 지원
   - Reflect 메서드는 프록시 객체와 함께 사용될 때 프록시 동작을 정의하거나 조작하는 데 사용
   - 프록시는 객체에 대한 행위를 가로채거나 수정하는 기능을 제공하며, Reflect 메서드는 프록시에서 주로 활용
3. 예외 처리 (Exception Handling)
   - Reflect 메서드는 객체와 관련된 작업에서 예외를 발생시키는 대신 true 또는 false와 같은 부울 값을 반환하는 경우가 많음

## Redux 따라하기

```jsx
// src/stores/BaseStore.ts
export type Action<Payload> = {
  type: string;
  payload?: Payload;
};

type Reducer<State, Payload> = (state: State, action: Action<Payload>) => State;

type Reducers<State> = Record<string, Reducer<State, any>>;

type Listener = () => void;

export default class BaseStore<State> {
  state: State;

  reducer: Reducer<State, any>;

  listeners = new Set<Listener>();

  constructor(initialState: State, reducers: Reducers<State>) {
    this.state = initialState;

    this.reducer = (state: State, action: Action<any>) => {
      const reducer = Reflect.get(reducers, action.type);
      if (!reducer) {
        return state;
      }

      return reducer(state, action);
    };
  }

  dispatch(action: Action<any>) {
    this.state = this.reducer(this.state, action);
    this.publish();
  }

  publish() {
    this.listeners.forEach(forceUpdate => {
      forceUpdate();
    });
  }

  addListener(listener: Listener) {
    this.listeners.add(listener);
  }

  removeListener(listener: Listener) {
    this.listeners.delete(listener);
  }
}
```

```jsx
// src/stores/Store.ts
import {singleton} from 'tsyringe';
import BaseStore, {type Action} from './BaseStore';

// Type State = {
//  count: number;
// }

const initialState = {
  count: 0,
  name: 'Tester',
};

export type State = typeof initialState;

const reducers = {
  increase(state: State, action: Action<number>) {
    return {
      ...state,
      count: state.count + (action.payload ?? 1),
    };
  },
  decrease(state: State, action: Action<number>) {
    return {
      ...state,
      count: state.count - (action.payload ?? 1),
    };
  },
};

export function increase(step?: number) {
  return {type: 'increase', payload: step};
}

export function decrease(step?: number) {
  return {type: 'decrease', payload: step};
}

@singleton()
export default class Store extends BaseStore<State> {
  constructor() {
    super(initialState, reducers);
  }
}
```

```jsx
// src/hooks/useDispatch.ts
import {type Action} from '../stores/BaseStore';
import {container} from 'tsyringe';
import Store from '../stores/Store';

export default function useDispatch<Payload>() {
  const store = container.resolve(Store);

  return (action: Action<Payload>) => {
    store.dispatch(action);
  };
}
```

```jsx
// src/hooks/useSelector.ts
import {useEffect, useRef, useState} from 'react';
import {container} from 'tsyringe';
import Store, {type State} from '../stores/Store';
import useForceUpdate from './useForceUpdate';

type Selector<T> = (state: State) => T;

export default function useSelector<T>(selector: Selector<T>) {
  const store = container.resolve(Store);

  const forceUpdate = useForceUpdate();

  const state = useRef(selector(store.state));

  useEffect(() => {
    const update = () => {
      // Selector의 결과가 객체인 경우 처리 필요함.
      const newState = selector(store.state);
      if (newState !== state.current) {
        forceUpdate();
        state.current = newState;
      }
    };

    store.addListener(update);

    return () => {
      store.removeListener(forceUpdate);
    };
  }, [store, forceUpdate]);

  return selector(store.state);
}
```

Dispatch / Selector 사용하기

```jsx
const dispatch = useDispatch();
const count = useSelector((state) => state.count);

dispatch({ type: 'increase' });
dispatch({ type: 'decrease' });

dispatch({ type: 'increase', payload: 10 });
```

```jsx
import useDispatch from '../hooks/useDispatch';
import useSelector from '../hooks/useSelector';
import {decrease, increase} from '../stores/Store';

export default function CountControl() {
  const dispatch = useDispatch();

  const count = useSelector(state => state.count);

  return (
    <div>
      <p>{count}</p>
      <button
        type='button'
        onClick={() => {
          dispatch(increase());
        }}>
        Increase
      </button>
      <button
        type='button'
        onClick={() => {
          dispatch(increase(10));
        }}>
        Increase 10
      </button>
      <button
        type='button'
        onClick={() => {
          dispatch(decrease());
        }}>
        Decrease
      </button>
      <button
        type='button'
        onClick={() => {
          dispatch(decrease(10));
        }}>
        Decrease 10
      </button>
    </div>
  );
}
```
