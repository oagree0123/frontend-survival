# Routing

- 일반적인 웹 사이트는 URL에 따라 다른 웹 페이지를 보여줌
- React에서의 Router
  - 하나의 웹 페이지를 하나의 컴포넌트로 만들고, URL에 따라 적절한 컴포넌트가 보이게 함으로써 구현 가능함

## Routing 간단하게 구현하기

```jsx
function App() {
  const { pathname } = window.location;

  return (
    <div>
      <Header />
      <main>
        {pathname === '/' && <HomePage />}
        {pathname === '/about' && <AboutPage />}
      </main>
      <Footer />
    </div>
  );
}
```

## HTML DOM API

- Document Object Model Application Programming Interface
- HTML 문서를 조작하고 제어하는 데 사용되는 프로그래밍 인터페이스
- API를 통해 웹 페이지의 HTML 요소, 속성, 스타일, 이벤트 등을 동적으로 조작할 수 있음

### Location

- 현재 웹 페이지의 URL 정보를 포함하는 JavaScript 객체
- 객체를 사용하여 웹 페이지의 URL을 조작하거나 URL의 다양한 부분을 확인할 수 있음

#### location 속성

- location.href 
  - 현재 페이지의 전체 URL
  - https://www.example.com/page.html 같은 형식입니다.
- location.protocol
  - URL의 프로토콜 부분
  - 일반적으로 https: 또는 https:
- location.host
  - URL의 호스트(도메인) 부분
  - www.example.com
- location.pathname
  - URL 경로
  - /page.html

### pathname

- location 객체의 속성 중 하나로, 현재 웹 페이지의 URL 경로
- https://www.example.com/products/electronics/laptop URL의 경우
  - pathname은 "/products/electronics/laptop
