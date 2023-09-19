# 프로젝트 세팅하기

## Node.js

### Node.js 란?

- Node.js는 Chrome V8 JavaScript 엔진으로 빌드 된 JavaScript 런타임
- 다양한 자바스크립트 애플리케이션을 실행할 수 있으며, 서버를 실행하는 데 제일 많이 사용
- 내장 HTTP 서버 라이브러리를 포함하고 있어 웹 서버에서 아파치 등의 별도 소프트웨어 없이 동작하는 것이 가능

> 런타임은 프로그래밍 언어가 구동되는 환경이다. 즉 어떤 프로그램이 동작할 때, 프로그램이 동작하는 장소

### Node.js의 버전

- LTS (Long-Term Support)
  - 장기 지원 버전
  - 안정 적으로 사용할 수 있음
- 최선 버전
  - 불안정 할 수 있음

### 윈도우에서 fnm 설치하기

> fnm은 Node.js 버전 관리자

#### 1. WSL

  ```sh
  wsl --install

  sudo apt update
  sudo apt upgrade
  ```

#### 2. Node.js와 npm 설치

  ```sh
  curl -fsSL https://fnm.vercel.app/install | bash
  ```

#### 3. fnm 초기화

```sh
echo 'eval "$(fnm env --multi)"' >> ~/.zshrc
```

#### 4. fnm 으로 Node.js 설치

```sh
fnm install {Node.js version}
fnm install --lts
```

## 개발 환경 설정하기

### 1. 시작하기

```sh
npm init
npm init -y
```

#### .gitignore 쉽게 만들기

https://www.toptal.com/developers/gitignore/

### 2. 타입스크립트

타입스크립트는 자바스크립트에 타입을 부여한 언어

```sh
npm i -D typescript
```

#### 실행(컴파일)

```sh
npx tsc --init
```

- tsconfig.json 파일로 설정

### 3.lint

린트는 소스코드를 분석하여 문법적인 오류나 스타일적인 오류, 적절하지 않은 구조 등에 표시를 달아주는 행위
다른 사람과 협업을 할 때 코드의 형식을 맞추는데 도움을 준다.

```sh
npm i -D eslint
```

- .eslintrc.js 파일을 생성 후 설정
- .eslintignore 파일 생성 후 설정

### 4. 리액트 설치

```sh
npm i react react-dom
npm i -D @types/react @types/react-dom
```

### 5. 테스팅 라이브러리

```sh
npm i -D jest @types/jest @swc/core @swc/jest jest-environment-jsdom  @testing-library/react @testing-library/jest-dom
```

- jest.config.js 파일 설정
  - 테스트에서 SWC 사용

```javascript
module.exports = {
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: [
    '@testing-library/jest-dom/extend-expect',
    './jest.setup',
  ],
  transform: {
    '^.+\\.(t|j)sx?$': ['@swc/jest', {
      jsc: {
        parser: {
          syntax: 'typescript',
          jsx: true,
          decorators: true,
        },
        transform: {
          react: {
            runtime: 'automatic',
          },
        },
      },
    }],
  },
  testPathIgnorePatterns: [
    '<rootDir>/node_modules/',
    '<rootDir>/dist/',
  ],
};
```

### 6. parcel

parcel은 모듈 번들러이다.

#### 모듈 번들러(bundler)란? 

> 웹 애플리케이션을 구성하는 자원(HTML, CSS, Javscript, Images 등)을 모두 각각의 모듈로 보고 이를 조합해서 병합된 하나의 결과물을 만드는 도구

```sh
npm i -D parcel
```

### 7. package.json

parcel로 웹 서버를 띄워주기 때문에 source 추가

```json
"source": "index.html", 
"scripts": {
    "start": "parcel --port 8080",
    "build": "parcel build",
    "check": "tsc --noEmit",
    "lint": "eslint --fix --ext .js,.jsx,.ts,.tsx .",
    "test": "jest",
    "coverage": "jest --coverage --coverage-reporters html",
    "watch:test": "jest --watchAll"
  },
```

### 8. 기본 파일 작성

- `index.html`
- `src/main.tsx`
- `src/App.tsx`
- `src/App.test.tsx`
- `src/components/Greeting.test.tsx`
- `src/components/Greeting.tsx`

#### 기본 작성

#### 1. index.html

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>React Demo App</title>
</head>
<body>
  <div id="root"></div>
</body>
</html>
```

#### 2. main.tsx

```typescript
import ReactDOM from 'react-dom/client';

function App() {
  return <p>Hello, World!</p>;
}

const element = document.getElementById('root');

if (element) {
  const root = ReactDOM.createRoot(element);

  root.render(<App />);
}
```
