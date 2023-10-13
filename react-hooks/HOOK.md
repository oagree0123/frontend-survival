# React Hook

- React 16.8 버전에서 도입된 기능
- 함수형 컴포넌트에서 상태(state)와 생명주기(lifecycle) 기능을 사용할 수 있게 해주는 API
- 상태 관리와 생명주기 관리를 위해 클래스 컴포넌트를 사용해야 했지만, Hooks를 사용하면 함수형 컴포넌트에서도 이러한 기능을 사용할 수 있음

## React 생명주기

- 최신 버전의 리액트에서는 클래스 컴포넌트의 생명주기 메서드를 활용하는 대신 함수 컴포넌트와 훅(Hook)을 사용하는 추세가 있으므로 이를 참고하여 개발

![Life Cycle](./lifecycle.png)

### Mount

- 컴포넌트가 생성되고 DOM에 삽입될 때 발생
- 주로 초기화 작업을 수행하거나 외부 데이터를 가져오는 등의 작업을 수행

### Update

업데이트(Update)는 4가지 상황에 의해 발생합니다.

- props가 변경될 때 (New props)
- state가 변경될 때 (setState())
- 부모 컴포넌트가 리렌더링 될 때
- forceUpdate()로 강제렌더링 할 때

### Unmount

-언마운트(Unmount)는 마운트와 반대로 DOM에서 제거되는 것을 말합니다.

## 기존 방식의 문제

- Wrapper Hell (HoC)
- Huge Components
- Confusing Classes

### 고차 컴포넌트 (HoC)

[HoC (Higher-Order Components)](https://ko.reactjs.org/docs/higher-order-components.html)

- 컴포넌트 로직을 재사용하고 컴포넌트 간의 코드를 추상화하기 위한 방법
- 고차 컴포넌트는 함수로 구현되며, 컴포넌트를 입력으로 받아서 변형된 컴포넌트를 반환

#### 고차 컴포넌트 특성

1. 컴포넌트 간의 로직 재사용 
   - 고차 컴포넌트를 사용하면 컴포넌트 간의 공통 로직을 한 번만 정의하고 여러 컴포넌트에서 재사용할 수 있음

2. Props 및 상태의 조작
   - 고차 컴포넌트를 사용하여 컴포넌트의 props나 상태(state)를 조작하거나 가공할 수 있음
   - 이를 통해 컴포넌트의 동작을 변경하거나 보강할 수 있음

3. 렌더링의 조절
   - 고차 컴포넌트를 사용하여 조건에 따라 컴포넌트를 렌더링하거나 렌더링을 제어할 수 있음

4. 컴포넌트 컴포지션
   - 여러 고차 컴포넌트를 조합하여 복잡한 로직을 가진 컴포넌트를 만들 수 있음

### 기존의 방식

- 상태를 가진 컴포넌트는 Class Component로 만듬
- props만 사용하는 재사용이 용이한 작은 컴포넌트는 Function Component로 작성
- Redux에서도 비슷한 구분이 존재
  - [Presentational and Container Components - Dan Abramov](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)

### 현재의 방식

- Function Component만 사용.
- 상태 관리 유무 신경쓰지 않아도 됨
  - 상태 관리 유무 바로 알기 워려움
- 복잡한 요소는 전부 Hook으로 격리 및 재사용 가능

#### 대표적인 Hooks

- useState → State Hook ⇒ React의 State
- useEffect ⇒ Side-effect
- useContext
- useRef
- useLayoutEffect → useEffect와 조금 다름

## useEffect

[useEffect 완벽 가이드](https://overreacted.io/ko/a-complete-guide-to-useeffect/)

- React 생명 주기에서 렌더링 이후 해야 할 일을 정해준다.
  - React의 외부와 관련된 일을 정해줄 수 있음
- 렌더링 때마다 실행됨
  - 의존성 배열을 통해 언제 effect를 실행할지 정할 수 있음
- 함수를 리턴함으로 종료를 처리할 수 있음
  - ex. setInterval - clearInterval
  - ex. window.addEventListener - window.removeEventListener

### 타이머 예제

```jsx
function Timer() {
  useEffect(() => {
    const savedTitle = document.title;

    const id = setInterval(() => {
        document.title = `Now: ${new Date().getTime()}`;
    }, 100);

    return () => {
        document.title = savedTitle;
        clearInterval(id);
    };
});

  return (
    <p>Playing</p>
  );
}

export default function TimerControl() {
  const [playing, setPlaying] = useState(false);
    
  const handleClick = () => {
    setPlaying(!playing);
  };

  return (
    <div>
      {playing ? (
        <Timer />
      ) : (
        <p>Stop</p>
      )}
      <button type="button" onClick={handleClick}>
        Toggle
      </button>
    </div>
  );
}
```

### 처음 한 번만 실행하기

```jsx
useEffect(() => {
  const fetchProducts = async () => {
    const url = 'http://localhost:3000/products';
    const response = await fetch(url);
    const data = await response.json();
    setProducts(data.products);
  };

  fetchProducts();
}, []);
```

### Effect가 두 번 실행되는 문제

- <React.StrictMode>로 컴포넌트 전체를 감쌀 경우, 예상치 못한 Side Effect를 찾으려고 Effect 등을 두 번씩 실행
  - 개발할 때 생기는 문제
- API 등을 사용하면 이상하다고 느낄 수 있으니 참고할 것.
  - [예상치 못한 부작용 검사](https://ko.reactjs.org/docs/strict-mode.html#detecting-unexpected-side-effects)

#### React StrictMode

- React 애플리케이션에서 개발자에게 개선 사항 및 잠재적인 문제를 식별하고 경고해주는 도구
- Strict Mode는 개발 환경에서만 활성화되며, 프로덕션 환경에서는 동작하지 않음

### 의존성 배열을 이용해 Fetch할 때 주의사항

- fetch를 하던 중 데이터를 받아오는 것을 중단해야하는 경우 결과를 무시해야함
- [Fetching data](https://beta.reactjs.org/learn/synchronizing-with-effects#fetching-data)