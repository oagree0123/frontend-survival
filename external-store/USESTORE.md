# usestore-ts

## Action으로 메서드 처리

```jsx
// src/stores/ObjectStore.ts
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

```jsx
// src/stores/CounterStore.ts
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

```jsx
// src/hooks/useObjectStore.ts
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

```jsx
// src/hooks/useCounterStore.ts
import { container } from 'tsyringe';

import useObjectStore from './useObjectStore';

import CounterStore from '../stores/CounterStore';

export default function useCounterStore() {
  const store = container.resolve(CounterStore);

  return useObjectStore(store);
}
```

## usestore-ts 사용하기

```jsx
// src/stores/CounterStore.ts
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

```jsx
// src/hooks/useCounterStore.ts
import { container } from 'tsyringe';

import { useStore } from 'usestore-ts';

import CounterStore from '../stores/CounterStore';

export default function useCounterStore() {
  const store = container.resolve(CounterStore);
  return useStore(store);
}

// 컴포넌트에서 사용하기
const [, store] = useCountStore();
```

### 비동기 함수에서 사용하기

```jsx
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

## useSyncExternalStore

- React 상태와 외부 상태 또는 외부 데이터 소스 간의 동기화를 단순화하고 관리하기 위해 설계된 도구
- Redux, MobX, GraphQL, 또는 다른 상태 관리 도구와 통합하여 React 컴포넌트에서 상태를 동기화할 때 유용

### useSyncExternalStore 목적

1. 외부 상태와 React 상태 간의 연결
   - 외부 상태 와 React 컴포넌트의 로컬 상태를 연결할 수 있음
2. 외부 상태의 변경 감지
   - useSyncExternalStore를 통해 외부 상태의 변경을 감지
   - 변경에 따라 React 컴포넌트를 업데이트
3. 이벤트 핸들러와 동작 연결
   - 외부 상태의 변경에 대한 이벤트 핸들러를 제공
   - 상태가 변경될 때 특정 동작을 수행하도록 설정할 수 있음

4. 성능 최적화
   - useSyncExternalStore는 최적화를 고려하여 상태 변경을 모니터링
   - 필요한 경우만 React 컴포넌트를 다시 렌더링