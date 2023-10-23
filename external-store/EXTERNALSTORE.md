# External Store

## 관심사의 분리 (Separation of Concerns. SoC)

### Layered Architecture

- 사용자에게 가까운 것과 사용자에게서 먼 것으로 구분
- 가장 가까운 건 UI를 다루는 부분
- 그 다음엔 Business Logic을 다루는 부분
- 그 너머에는 데이터에 접근하고 저장하는 부분
- 각 부분은 하나의 역할, 관심사로 분리됨으로 복잡도를 낮춤

### Input - Process - Output

- 단계로 코드를 적절히 구분만 해도 코드를 이해하고 유지보수하는데 크게 도움
- 하나의 Output은 사용자에게 다시 Input을 요청

#### 순환구조

1. Input: 프로그램 시작
2. Process: 프로그램 초기화
3. Output: 사용자에게 초기 UI 보여주기
4. Input: 사용자의 입력
5. Process: 사용자의 입력에 따라 처리
6. Output: 처리 결과 보여주기
7. Input: 사용자의 또 다른 입력
8. …반복…

MVC로 거칠게 매핑

- Model → Process
- View → Output
- Controller → Input

## Flux Architecture

[Flux 번역 글](https://haruair.github.io/flux/docs/overview.html)

![Flux 흐름](./flux-archi.png);

- MVC의 대안으로 내세운 아키텍처
- Model-View의 복잡한 관계를 겨냥해 "unidirectional data flow(단방향)"를 강조

1. Action → 이벤트/메시지 같은 객체
2. Dispatcher → (여러 개) Store로 Action을 전달. 메시지 브로커와 유사
3. Store (여러 개) → 받은 Action에 따라 상태를 변경
4. View → Store의 상태를 반영

### Redux 의 형태

- Redux는 단일 Store를 사용함으로써 좀 더 단순한 그림을 제안한다.

1. Action
2. Store → dispatch를 통해 Action을 받고, 사용자가 정의한 reducer를 통해 State를 변경
3. View → State를 반영

거칠게 매핑하기

- Input → Action + dispatch
- Process → reducer
- Output → View(React)

## External Store

- 데이터를 영구적으로 저장하고 관리하는 데 사용되는 외부 저장 장치 또는 시스템
- React 애플리케이션에서 상태를 관리하고, 컴포넌트 간의 데이터 공유와 효율적인 업데이트를 지원하는 중요한 구성 요소

### useReducer

- React에서 상태 관리를 위한 훅 중 하나
- 상태 논리를 관리하고 컴포넌트의 상태를 업데이트하는 데 사용
- useReducer는 주로 useState로 처리하기 어려운 복잡한 상태 논리를 다룰 때 유용

```jsx
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

1. reducer
   - 현재 상태와 액션을 받아 새로운 상태를 반환
2. initialArg
   - 초기 상태 값을 나타내는 인수
   - 첫 번째 렌더링 시 reducer 함수의 첫 번째 매개변수로 전달됩니다.
3. init (선택적)
   - 초기 상태를 생성하는 함수로
   - initialArg가 제공되지 않았을 때 호출됩니다.

### useCallback

- 함수를 기억하고 불필요한 렌더링을 방지하기 위해 사용
- 주로 이벤트 핸들러 함수나 다른 함수형 프로퍼티를 컴포넌트의 의존성 배열(dependency array)에 포함시킬 때 유용

```jsx
const memoizedCallback = useCallback(callback, dependencies);
```

1. callback
   - 기억하고자 하는 함수입니다.

2. dependencies
   - 함수가 의존하는 상태나 프로퍼티의 배열
   - 이 배열에 나열된 값 중 하나라도 변경될 때에만 함수가 새로 생성

```jsx
const [, setState] = useState({});
const forceUpdate = () => setState({});

// -------

function useForceUpdate() {
  const [, setState] = useState({});
  return useCallback(() => setState({}), []);
}
```

> 이런 접근을 잘 하면, React가 UI를 담당하고, 순수한 TypeScript(또는 JavaScript)가 비즈니스 로직을 담당하는, 관심사의 분리(Separation of Concerns)를 명확히 할 수 있다. 자주 바뀌는 UI 요소에 대한 테스트 대신, 오래 유지되는(바뀌면 치명적인) 비즈니스 로직에 대한 테스트 코드를 작성해 유지보수에 도움이 되는 테스트 코드를 치밀하게 작성할 수 있다.
