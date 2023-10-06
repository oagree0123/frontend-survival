# React State

## React State 란?

- React의 state는 “변경”을 다루기 위한 요소
- 거칠게 말해, 어떤 컴포넌트의 state가 바뀌면 해당 컴포넌트와 하위 컴포넌트를 다시 렌더링
- 일관성과 효율을 위해 DRY 원칙을 따르는 SSOT를 만듬
  - [DRY (Don’t Repeat Yourself)](https://ko.wikipedia.org/wiki/중복배제)
  - [SSOT (Single Source of Truth)](https://ko.wikipedia.org/wiki/단일_진실_공급원)

### React Re-rendering

1. 초기 렌더링 (Initial Rendering)
2. 상태(State) 변경
   - 사용자 상호 작용 또는 데이터의 변경과 같은 이벤트가 발생하면, 컴포넌트의 상태가 변경
   - 상태가 변경되면, 리액트는 해당 컴포넌트의 render 메서드를 재호출하여 새로운 가상 DOM을 생성
3. Virtual DOM과 비교
   - 이전 가상 DOM과 새로 생성된 가상 DOM 간에 차이를 비교
4. DOM 업데이트
   - 변경된 부분만을 실제 DOM에 반영

### React State의 조건

1. 변경돼어야 함. 변경되지 않는 state는 필요 없음
2. 부모 컴포넌트에서 props로 전달된다면 state가 아님
3. 다른 state나 props를 이용해 계산 가능하다면 state가 아님

### 상태를 누가 관리해야하나?

- 다루는 상태가 너무 많으면 복잡함
- TypeScript를 잘 쓰면 직접 관리하는 상태의 수를 줄여줄 수 있음'

(React만 쓴다면) 해당 상태에 의존적인 컴포넌트를 모두 포함하는 컴포넌트가 상태를 소유해야 함.

- [“Lifting State Up”](https://ko.reactjs.org/docs/lifting-state-up.html)
  - 컴포넌트 아래에서부터 가장 상위의 컴포넌트로 상태를 소유하도록 만들기
- [Sharing State Between Components](https://beta.reactjs.org/learn/sharing-state-between-components)

### InverseData Flow

- 하위 컴포넌트의 props로 함수를 전달
- 콜백 함수라고 부름
- JavaScript는 함수가 일급(first-class) 객체이다.
  - 어떤 함수를 다른 함수에 인자로 넘겨주거나, 어떤 함수를 리턴값으로 사용가능
  - 익명 함수, 클로저 등과 함께 사용하면 시너지가 큼
  - [일급 함수](https://developer.mozilla.org/ko/docs/Glossary/First-class_Function)