# 프로젝트 세팅하기
## Node.js 버전
- LTS (Long-Term Support)
  - 장기 지원 버전
  - 안정 적으로 사용할 수 있음   
- 최선 버전
  - 불안정 할 수 있음
<br>

### 윈도우에서 fnm 설치하기

> fnm은 Node.js 버전 관리자

#### 1. WSL
  ```sh
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

.gitignore
https://www.toptal.com/developers/gitignore/

### 2. 타입스크립트

```sh
npm i -D typescript
```
#### 실행(컴파일)
```sh
npx tsc --init
```

### 3.lint
```sh
npm i -D eslint
```

> .eslintignore

### 4. 리액트 설치
```sh
npm i react react-dom
npm i -D @types/react @types/react-dom
```

### 5. 테스팅 라이브러리
```sh
npm i -D jest @types/jest @swc/core @swc/jest jest-environment-jsdom  @testing-library/react @testing-library/jest-dom
```

### 6. percel
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