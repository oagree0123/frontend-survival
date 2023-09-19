# React

- 사용자 인터페이스를 만들기 위한 JavaScript 라이브러리

## 리액트 공식문서

- [React 공식문서](https://ko.legacy.reactjs.org/)
  - 옛날 문서이지만, 조금씩 업데이트
- [React Beta 문서](https://react.dev/)
  - 요즘 React 문서, 베타 버전이라 완성도 낮음

## 렌더링

### 웹에서 렌더링이란?

- 웹 개발에서의 렌더링은 웹 페이지가 브라우저에서 어떻게 표시되는지 나타내는 것

- 웹 페이지를 브라우저에 표시하기 위해서는 HTML, CSS 및 JavaScript와 같은 웹 기술을 사용하여 컨텐츠와 디자인을 결합하고, 브라우저에서 이를 해석하여 화면에 보여주어야 합니다. 이 과정은 브라우저가 웹 페이지를 "렌더링"하는 것입니다.

```tsx
function Demo({ count }: { count: number }) {
  return <p>Demo: {count}</p>;
}

function main() {
  const element = document.getElementById('root');
  const element2 = document.getElementById('demo');

  if (!element || !element2) {
  return;
  }

  const root = ReactDOM.createRoot(element);
  const root2 = ReactDOM.createRoot(element2);

  root.render(<App />);

  // count 값이 바뀌는 부분만 업데이트
  let count = 0;
  setInterval(() => {
  count += 1;
  root2.render(<Demo count={count} />);
  }, 1000);
}
```

## React 렌더링

### 렌더링이란

- 렌더링이란 현재 props 및 상태를 기반으로 리액트가 컴포넌트에게 UI 영역이 어떻게 보이길 원하는지 설명을 요청하는 프로세스입니다.

#### DOM (Documnet Object Model)

>"W3C Document Object Model(DOM)은 프로그램 및 스크립트가 동적으로 접근하여 문서의 스타일과 구조 및 콘텐츠를 업데이트할 수 있는 언어 중립적 인터페이스입니다."

- DOM은 웹 사이트를 열 때, 화면에 표시되는 내용을 HTML을 통해 표현하는 것
- 브라우저에서 Javascript를 통해 DOM을 수정할 수 있음

```javascript
function changeText() {
  // DOM을 사용하여 h1 요소에 접근하고 텍스트 변경
  let heading = document.getElementById("myHeading");
  heading.innerHTML = "반가워요!";
}
```

#### VDOM (Virtual DOM)

- React의 상태 변경이 VDOM에 먼저 적용됨
- UI의 변경이 필요한 경우 ReactDOM 라이브러리는 업데이트 해야 할 DOM만 업데이트
- VDOM이 업데이트되면, React는 이전의 VDOM 스냅샷과 비교한 뒤 실제 DOM에서 변경된 내용만 업데이트
  - 이전 VDOM과 새 VDOM을 비교하는 이 프로세스를 diffing

#### 성능에 미치는 영향

- 렌더링이 항상 UI 업데이트를 의미하지는 않음
- 함수형 컴포넌트에서 함수 전체를 실행하는 것은 클래스형 컴포넌트의 render 함수와 동일

아래 예시 보기

```jsx
const App = () => {
  const [message, setMessage] = React.useState('');
  return (
    <>
      <Tile message={message} />
      <Tile />
    </>
  );
};
```

1. App 컴포넌트의 상태가 변경될 경우, props를 받지 않아도 Title 컴포넌트 모두 리렌더링
2. render 함수가 3번 호출되는 것처럼 해석
3. 실제 DOM에서 수정은 message를 표시한는 Title에서만 발생

위 방식의 단점

- React는 UI를 업데이트해야 하는지 여부를 확인하기 위해 각 컴포넌트에서 diffing 알고리즘을 실행
- render 함수 또는 함수형 컴포넌트의 모든 코드가 재실행

### 리렌더링

- React에서 리렌더링은 컴포넌트가 다시 화면에 그려지는 과정
- 컴포넌트의 상태(state) 또는 속성(props)이 변경될 때 발생하며, 변경된 데이터에 따라 UI가 업데이트

#### React 리렌더링 조건

1. 컴포넌트 상태(state) 변경: 컴포넌트의 setState() 메서드를 사용하여 상태를 변경하면 해당 컴포넌트가 다시 렌더링됩니다.

2. 컴포넌트 속성(props) 변경: 부모 컴포넌트가 자식 컴포넌트에게 새로운 속성을 전달할 때, 자식 컴포넌트가 새로운 속성으로 다시 렌더링됩니다.

3. 컴포넌트의 생명주기 메서드 호출: 컴포넌트의 생명주기 메서드 중 하나가 호출될 때(예: componentDidMount, componentDidUpdate), 해당 컴포넌트가 다시 렌더링됩니다.

    3.1 클래스 컴포넌트에서 사용되는 메서드

4. 컨텍스트(Context) 변경: React의 컨텍스트를 사용하여 데이터를 공유하는 경우, 컨텍스트 값이 변경될 때 관련된 컴포넌트가 다시 렌더링됩니다

### 렌더링 성능 개선

#### React.memo

- 특정 리렌더링 요청을 무시
- 메모이제이션, props가 하나도 변경되지 않은 경우 리액트는 오래된 스냅샷을 재사용하여 완전히 새로운 스냅샷을 생성하는 문제를 피할 수 있음

아래 예시의 경우 props를 전달 받는 BigCountNumber 컴포넌트만 리렌더링

```jsx
function Decoration() {
  return <div className="decoration">⛵️</div>;
}
export default React.memo(Decoration);
```

```jsx
import React from "react";

function BigCountNumber({ count }) {
  console.info("BigCountNumber render");

  return (
    <p>
      <span className="prefix">Count:</span>
      {count}
    </p>
  );
}

export default React.memo(BigCountNumber);
```

```jsx
function Counter() {
  const [count, setCount] = React.useState(0);

  return (
    <main>
      <BigCountNumber count={count} />
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <Decoration />
    </main>
  );
}

export default Counter;
```

## 라이브러리 vs 프레임 워크

- 라이브러리와 프레임워크의 차이는 제어 흐름에 대한 주도성이 누구에게/어디에 있는가에 있음

### 라이브러리

- 라이브러리를 사용하면 개발자가 코드의 흐름과 제어를 가짐
- 라이브러리는 보통 특정 작업을 수행하기 위해 개발자가 코드를 호출
- 라이브러리는 주로 특정 작업을 수행하기 위한 도구로 사용됩니다. 예를 들어, 데이터베이스 연결, HTTP 요청

### 프레임워크

- 프레임워크는 일반적으로 제어 흐름을 자체적으로 관리하며, 개발자는 프레임워크에서 제공하는 규칙과 지침을 따라야 함
- 프레임워크는 애플리케이션의 구조와 흐름을 정의하고, 개발자가 프레임워크에 따라 코드를 작성하도록 유도
- 프레임워크는 주로 애플리케이션의 구조와 아키텍처를 제공하며, 개발자가 프로젝트를 시작하는 데 필요한 기본 구성 요소를 정의
