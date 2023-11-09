# styled-components

- 스타일이 적용된 컴포넌트를 쉽게 만들 수 있는 도구

## styled-components 사용하기

### 환경 준비

- 반드시 VS Code Extension을 설치
  - vscode-styled-components

```bash
npm i styled-components
npm i -D @types/styled-components @swc/plugin-styled-components
```

```jsx
// .swcrc
{
  "jsc": {
    "experimental": {
      "plugins": [
        [
          "@swc/plugin-styled-components",
          {
            "displayName": true,
            "ssr": true
          }
        ]
      ]
    }
  }
}
```

### 사용하기

```jsx
import styled from 'styled-components';

const Paragraph = styled.p`
  color: #00f;

  strong {
    font-size: 2rem;
  }
`;

const BigParagraph = styled(Paragraph)`
  font-size: 1.5rem;
`;

export default function Greeting() {
  return (
    <BigParagraph>
      Hello, World
      <strong>!</strong>
    </BigParagraph>
  );
}
```

- 기존 컴포넌트에 스타일을 입히는 것도 가능
- 기존 컴포넌트가 Class를 잡아줘야 한다는 점에 주의!

```jsx
function HelloWorld({ className }: React.HTMLAttributes<HTMLElement>) {
  return (
    <BigParagraph
      className={className}
    >
      Hello, World
      <strong>!</strong>
    </BigParagraph>
  );
}

const SmallHelloWorld = styled(HelloWorld)`
  font-size: 0.1rem;
`;

export default function Greeting() {
  return <SmallHelloWorld />;
}
```
