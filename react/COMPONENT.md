# React로 사고하기

## Thinking in React

[Thinking in React](https://beta.reactjs.org/learn/thinking-in-react)

- "Step 1: Break the UI into a component hierarchy"
- "Step 2: Build a static version in React"

## 데이터

Back End에서 JSON 형태의 데이터를 주는 API를 제공한다고 가정

### Rest API

- GET, POST, PUT/PATCH, DELETE (CRUD)
- Resource 중심

#### Rest API란?

- 모든 자원은 고유한 URI를 가지며, 각 자원은 유일하게 식별
- HTTP 메서드(GET, POST, PUT, DELETE 등)를 사용하여 자원에 대한 작업을 수행
- 요청은 서버에 대한 이전 요청과 상관없이 독립적으로 처리

### GreaphQL

- Graph 자료 구조
- Query에서 얻고자 하는 것을 지정
- Query(Read), Mutation(Command: Create, Update, Delete), Subscription(Event)

Front End는 받은 데이터를 사용자가 볼 수 있도록 UI구성
React는 선언형(HTML과 유사항 모양의 DSL)으로 UI를 구성

>DSL이란?  
Domain-Specific Languages의 약자로 도메인 특화 언어라는 의미를 가집니다.
DSL은 관련 특정 분야에 최적화된 프로그래밍 언어입니다.

### JSON

### 명령형 vs 선언형 프로그래밍

- [JSON](https://ko.wikipedia.org/wiki/JSON)
- [JSON 개요](https://www.json.org/json-ko.html)
- [JSON으로 작업하기](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/JSON)
- [명령형 프로그래밍](https://ko.wikipedia.org/wiki/명령형_프로그래밍)
- [선언형 프로그래밍](https://ko.wikipedia.org/wiki/선언형_프로그래밍)

## 컴포넌트 계층 구조

### React 특징

- 컴포넌트를 기반으로 만들어짐
- 스스로 상태를 관리하는 캡슐화된 컴포넌트들이 하나하나 모여 복잡한 UI를 만듬
  - 반대로 말하면 컴포넌트는 복잡해서는 안됨

### 컴포넌트를 만드는 기준

- 단일 책임 원칙 [SRP(Single Responsibility Principle)](https://ko.wikipedia.org/wiki/%EB%8B%A8%EC%9D%BC_%EC%B1%85%EC%9E%84_%EC%9B%90%EC%B9%99)
- CSS에서 사용하는 class나 id를 기준
- Design's Layer
- Information Architecture (JSON Schema의 영향)
  - 실제로 엄청 많이 쓰게 됨
  - 자연스러운 SRP를 위해서 사실상 강제됨
- 작은 컴포넌트
  - 레고의 조립과 같이, 부품들을 만들어서 조립
- Atomic Design

#### Atomic Design

[Atomic Design](https://bradfrost.com/blog/post/atomic-web-design/)

## Extract Function

[Extract Function](https://refactoring.com/catalog/extractFunction.html)
[Inline Function](https://refactoring.com/catalog/inlineFunction.html)

- 코드를 전체 작성 후 분리할 수 있는 부분이 보이면 "함수로 추출"
- 코드를 작성하기 어려운 상황에 직면할 경우
  - 다른 파일로 만들어야 한닥 생각하지 않아도 가능

## Props

- 나눠진 컴포넌트를 서로 연결하는 방법
- 테스트 코드를 작성하면 재사용성을 평가하기 쉬움
