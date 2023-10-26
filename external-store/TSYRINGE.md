# TSyringe

- TypeScript용 DI(Dependency Injection) 도구(IoC Container)
- External Store를 관리하는데 활용할 수 있음
- React 컴포넌트 입장에서는 “전역”처럼 여겨짐
  - Prop Drilling 문제 해결

## 사용하기

- 의존성 설치
  - scr/main.tsx src/setupTests.ts

```bash
import 'reflect-metadata';
```

- 싱글톤으로 관리할 CounterStore 클래스

```jsx
// stores/CounterStore.ts
import {singleton} from 'tsyringe';

type Listener = () => void;

@singleton()
export default class CounterStore {
  count = 0;

  listeners = new Set<Listener>();

  publish() {
    this.listeners.forEach(forceUpdate => {
      forceUpdate();
    });
  }

  increase() {
    this.count += 1;
    this.publish();
  }

  decrease() {
    this.count -= 1;
    this.publish();
  }

  addListener(listener: Listener) {
    this.listeners.add(listener);
  }

  removeListener(listener: Listener) {
    this.listeners.delete(listener);
  }
}
```

- 싱글톤 CounterStore 사용

```jsx
// src/hooks/useCountStore
import {container} from 'tsyringe';
import useForceUpdate from './useForceUpdate';
import {useEffect} from 'react';
import CounterStore from '../stores/CounterStore';

export default function useCountStore() {
  const store = container.resolve(CounterStore);

  const forceUpdate = useForceUpdate();

  useEffect(() => {
    store.addListener(forceUpdate);

    return () => {
      store.removeListener(forceUpdate);
    };
  }, [store, forceUpdate]);

  return store;
}
```

```jsx
// src/components/CountControl.tsx
import useCountStore from '../hooks/useCountStore';

export default function CountControl() {
  const store = useCountStore();

  const handleClickIncrease = () => {
    store.increase();
  };

  const handleClickDecrease = () => {
    store.decrease();
  };

  return (
    <div>
      <p>{store.count}</p>
      <button type='button' onClick={handleClickIncrease}>
        Increase
      </button>
      <button type='button' onClick={handleClickDecrease}>
        Decrease
      </button>
    </div>
  );
}

// src/components/Counter.tsx
import useCountStore from '../hooks/useCountStore';

export default function Counter() {
  const store = useCountStore();

  return (
    <div>
      <p>
        Count:
        {' '}
        {store.count}
      </p>
    </div>
  );
}
```

- test에서의 초기화

```jsx
describe('App', () => {
  beforeEach(() => {
    container.clearInstances();
  });

  context('when press increase button once', () => {
    test('counter', () => {
      render(<App />);

      fireEvent.click(screen.getByText('Increase'));

      const elemnets = screen.getAllByText('Count: 1');

      expect(elemnets).toHaveLength(2);
    });
  });

  context('when press increase button twice', () => {
    test('counter', () => {
      render(<App />);

      fireEvent.click(screen.getByText('Increase'));
      fireEvent.click(screen.getByText('Increase'));

      const elemnets = screen.getAllByText('Count: 2');

      expect(elemnets).toHaveLength(2);
    });
  });
});
```

## 의존성 주입

- 의존 관계를 객체 외부에서 관리하도록 하는 설계 패턴

### 의존성 주입의 장점

1. 유연성(Flexibility)
   - 의존성 주입을 사용하면 동작을 구성 및 변경하기가 쉬움
   - 외부에서 제공하므로 다른 부분에 영향을 미치지 않고 의존성을 교체하거나 업데이트할 수 있음
2. 재사용성(Reusability)
   - 독립적으로 테스트 가능한 컴포넌트를 만들 수 있음
3. 테스트 용이성(Testability)
   - 단위 테스트 및 모의 객체(Mock Objects)를 쉽게 만들 수 있음

## reflect-metadata

- 클래스 및 프로퍼티에 메타데이터를 저장하고 검색할 수 있도록 도와주는 라이브러리
- 자바스크립트 런타임에서 TypeScript의 데코레이터(Decorators)와 함께 사용

> 메타데이터란?  
> 데이터에 대한 데이터라고 생각할 수 있으며, 클래스나 프로퍼티와 같은 코드 엔터티에 대한 추가 정보를 포함할 수 있습니다.

## 싱글톤

- 소프트웨어 디자인 패턴 중 하나
- 특정 클래스의 인스턴스가 시스템 전체에서 단 하나만 생성되도록 보장하는 패턴
- 리소스를 공유하거나 특정 객체를 중앙에서 제어하고 싶을 때 사용

### 싱글톤 패턴 특징

1. 단일 인스턴스
   - 클래스의 인스턴스가 단 하나만 생성
   - 전역적으로 접근 가능하며, 모든 코드에서 동일한 인스턴스에 접근
2. 전역적인 접근성
   - 싱글톤 인스턴스는 주로 전역 상태나 리소스 관리, 설정 객체 등 공유 리소스를 관리하는 데 사용

3. 지연 로딩
   - 싱글톤 패턴은 인스턴스가 필요할 때 생성되도록 구현 가능
   - 시작 시 모든 리소스를 로드하지 않고 필요한 순간에 로드할 수 있음