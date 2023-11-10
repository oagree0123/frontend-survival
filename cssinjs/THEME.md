# Global Style & Theme

## Reset CSS

- 웹 페이지를 일관된 기본 스타일 상태로 초기화
- 모든 브라우저에서 동일한 기본 스타일을 갖도록 웹 페이지의 초기 스타일을 정규화

```bash
npm i styled-reset
```

```jsx
export default function App() {
  return (
    <>
      <Reset />
      <Greeting />
    </>
  );
}
```

## Global Style

- 전역 스타일을 정의할 때 사용

```jsx
import {createGlobalStyle} from 'styled-components';

const GlobalStyle = createGlobalStyle`
  html {
    box-sizing: border-box;
  }

  *,
  *::before,
  *::after {
    box-sizing: inherit;
  }

  html {
    font-size: 62.5%; // 1rem = 10px
  }

  body {
    font-size: 1.6rem; // 1.6px
  }

  :lang(ko) {
    h1, h2, h3 {
      word-break: keep-all;
    }
  }
`;

export default GlobalStyle;
```

### box-sizing

- 박스의 크기 계산 방법을 지정
- 요소의 크기는 기본적으로 해당 요소의 콘텐츠 크기와 여백(margin), 테두리(border), 패딩(padding) 등을 모두 포함하여 계산
  - 기본적으로 브라우저는 content-box 값

1. content-box

- width: 200px;를 지정했을 때, 실제로 표시되는 요소의 전체 크기는 콘텐츠, 패딩, 테두리, 여백을 모두 포함

```jsx
div {
  box-sizing: content-box;
  width: 200px;
  padding: 20px;
  border: 5px solid #000;
  margin: 10px;
}
```

2. border-box

- 요소의 크기는 콘텐츠, 패딩, 테두리를 포함한 값으로 계산
- width: 200px;를 지정했을 때
  - 요소의 크기는 콘텐츠, 패딩, 테두리를 포함한 값
  - 실제로 표시되는 크기는 width 값에 테두리와 패딩을 추가한 크기

```jsx
div {
  box-sizing: border-box;
  width: 200px;
  padding: 20px;
  border: 5px solid #000;
  margin: 10px;
}
```

### word-break

- 텍스트가 자신의 콘텐츠 박스 밖으로 오버플로 할 때 줄을 바꿀 지 지정

```jsx
word-break: normal;
word-break: break-all;
word-break: keep-all;
```

1. normal
   - 본값으로, 단어가 길어서 줄 바꿈이 필요한 경우에만 줄 바꿈이 수행

2. break-all
   - 단어가 컨테이너의 크기를 벗어나도록 허용
   - 긴 단어를 여러 줄로 나눌 수 있음

3. keep-all
   - CJK(중국어, 일본어, 한국어) 텍스트에서는 단어를 나누지 않고 줄 바꿈
   - 영어와 같은 언어에서는 normal과 동일하게 작동

## Theme

- 디자인 시스템의 근간을 마련하는데 활용
- 잘 정의하면 다크 모드 등에 대응하기 쉬움
- 눈에 보이는 단편적인 정보를 넘어서, "의미"에 집중

```jsx
// src/styles/defaultTheme.ts
// 의미에 집중해서 이름 짓기
const defaultTheme = {
  colors: {
    background: '#FFF',
    text: '#000',
    primary: '#F00',
    secondary: '#00F',
  },
};

export default defaultTheme;

// src/styles/Theme.ts
import type defaultTheme from './defaultTheme';

type Theme = typeof defaultTheme;

export default Theme;

// src/styles/darkTheme.ts
import type Theme from './Theme';

const darkTheme: Theme = {
  colors: {
    background: '#FFF',
    text: '#000',
    primary: '#F00',
    secondary: '#00F',
  },
};

export default darkTheme;

// src/App.tsx
export default function App() {
  const theme = defaultTheme;

  return (
    <ThemeProvider theme={theme}>
      <Reset />
      <GlobalStyle />
      <Greeting />
      <Switch />
    </ThemeProvider>
  );
}

// src/styles/GlobalSytle.ts
const GlobalStyle = createGlobalStyle`
  body {
    background: ${(props) => props.theme.colors.background};
    color: ${(props) => props.theme.colors.text};
  }
  ...
```

### 타입 문제 해결하기

```jsx
// src/styles/styled.d.ts

import 'styled-components';

declare module 'styled-components' {
  export interface DefaultTheme extends Theme {
    colors: {
      background: string;
      text: string;
      primary: string;
      secondary: string;
    }
  }
}

// 상속하는 방법
import 'styled-components';

import Theme from './Theme';

declare module 'styled-components' {
  export interface DefaultTheme extends Theme {}
}
```

### 테스트에서 문제 해결하기

- Jest 테스트 쪽에서 “window.matchMedia” 문제 발생.

>[Mocking methods which are not implemented in JSDOM](https://jestjs.io/docs/manual-mocks#mocking-methods-which-are-not-implemented-in-jsdom)

```jsx
// src/setupTests.ts

Object.defineProperty(window, 'matchMedia', {
  writable: true,
  value: jest.fn().mockImplementation((query) => ({
    matches: false,
    media: query,
    onchange: null,
    addListener: jest.fn(), // deprecated
    removeListener: jest.fn(), // deprecated
    addEventListener: jest.fn(),
    removeEventListener: jest.fn(),
    dispatchEvent: jest.fn(),
  })),
});
```
