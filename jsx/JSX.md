# JSX

## JSX 란?

- JavaScript를 확장한 문법
- HTML과 유사한 마크업을 작성할 수 있게 해주는 JavaScript용 구문 확장
- XML 처럼 작성된 부분을 React.createElement를 쓰는 JavaScript 코드로 변환
- 중괄호를 사용해 JavaScript 문법을 그대로 사용 가능

### React에서 설명하는 JSX

- React에서는 본질적으로 렌더링 로직이 UI 로직(이벤트가 처리되는 방식, 시간에 따라 state가 변하는 방식, 화면에 표시하기 위해 데이터가 준비되는 방식 등)과 연결
- React는 별도의 파일에 마크업과 로직을 넣어 기술을 인위적으로 분리하는 대신, 둘 다 포함하는 “컴포넌트”라고 부르는 느슨하게 연결된 유닛으로 관심사를 분리

## JSX 문법

### 1. Bable로 확인하기

[Babel](https://babeljs.io/repl#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=false&corejs=3.21&spec=false&loose=false&code_lz=Q&debug=false&forceAllTransforms=false&modules=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=env%2Creact%2Cstage-2&prettier=false&targets=&version=7.22.20&externalPlugins=&assumptions=%7B%7D)

- "Presets"에서 "react"를 체크, "Plugins"에서 "@babel/plugin-transform-react-jsx"를 추가하면 JSX를 실험 가능
- JSX 파일에 @jsx something 주석을 추가하면 React.createElement 대신 something을 사용

### 2. Example

```jsx
<MyButton color="blue" shadowSize={2}>
  Click Me
</MyButton>

// 컴파일
React.createElement(
  MyButton,
  {color: 'blue', shadowSize: 2},
  'Click Me'
)
```

```jsx
<div>
  <p>Count: {count}!</p>
  <button type="button" onClick={() => setCount(count + 1)}>Increase</button>
</div>

// 컴파일
React.createElement(
  "div",
  null,
  React.createElement("p", null, "Count: ", count, "!"),
  React.createElement("button", { type: "button", onClick: () => setCount(count + 1) }, "Increase")
);
```

## React Element

- 사용자 인터페이스의 한 부분, React의 가장 작은 단위
  - 브라우저 DOM Element와 달리 일반 객체이며 쉽게 생성
- JSX 대신 React.createElement를 사용해 React Element 트리 갱신 가능
- React Element는 나중에 React가 컴포넌트를 렌더링하도록 지시하는 것
  - React Element는 브라우저 화면에 실제로 어떤 DOM 노드를 생성할지 알려줌

### Javascript createElement

```javascript
const element = document.createElement('div');
element.classList.add('test')
```

### React createElement

```javascript
import { createElement } from 'react';

function Greeting({ name }) {
  return createElement(
    'h1',
    { className: 'greeting' },
    'Hello ',
    createElement('i', null, name),
    '. Welcome!'
  );
}

export default function App() {
  return createElement(
    Greeting,
    { name: 'Taylor' }
  );
}

```

## VDOM (Virtual DOM)

- React의 상태 변경이 VDOM에 먼저 적용됨
- UI의 변경이 필요한 경우 ReactDOM 라이브러리는 업데이트 해야 할 DOM만 업데이트
- VDOM이 업데이트되면, React는 이전의 VDOM 스냅샷과 비교한 뒤 실제 DOM에서 변경된 내용만 업데이트
  - 이전 VDOM과 새 VDOM을 비교하는 이 프로세스를 diffing

>재조정(diffing) 이란?  
“가상”적인 표현을 메모리에 저장하고 ReactDOM과 같은 라이브러리에 의해 “실제” DOM과 동기화

### VDOM 사용하는 이유

- 속도는 충분히 빠르다
- 유지보수하는데 도움
- 선언적 API를 가능하게 함

#### 선언적 API

- 어떤 작업을 수행하는 대신 어떤 결과를 얻을 것인지 설명하는 방식으로 동작하는 프로그래밍 인터페이스

> React는 선언적 API를 사용하여 UI를 정의하고, Virtual DOM을 사용하여 선언적인 방식으로 UI 업데이트를 처리합니다. 이러한 조합은 React 애플리케이션의 개발과 유지보수를 단순화하고 성능을 최적화하는 데 도움