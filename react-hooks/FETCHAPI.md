# Fetch API

- 웹 브라우저와 웹 서버 사이에서 데이터를 주고받는 데 사용되는 JavaScript의 표준 API
- Fetch API를 사용하면 웹 애플리케이션에서 데이터를 가져오거나 보낼 수 있음
- XMLHttpRequest와 비교하여 더 간단하고 유연한 방법을 제공
- Promise 기반
  - Fetch API는 Promise를 기반으로 동작
  - 비동기 작업을 처리하기 위해 콜백 함수 대신 Promise를 사용하여 더 간결하고 효율적인 코드를 작성

## Fetch API 사용하기

```typescript
fetch('http://localhost:3000/products');
// Promise {<pending>}

await fetch('http://localhost:3000/products');
// Response

const response = await fetch('http://localhost:3000/products');
// response.body는 ReadableStream

const reader = response.body.getReader();

const chunk = await reader.read();
// chunk.value는 Uint8Array 타입.
// 원래는 chunk.done이 true일 때까지 반복해야 한다.
// chunk는 데이터 조각으로 대용량의 데이터를 모두 받기 위해서는
// chunk.done이 true될 때까지 반복

const body = new TextDecoder().decode(chunk.value);

const data = JSON.parse(body);

// json 기본 지원
const response = await fetch('http://localhost:3000/products');
const data = await response.json();

// HTTP Method 사용 방법
const response = fetch(url, {
  method: 'POST',
});
```

## CORS (Cross-Origin Resource Sharing)

- 웹 애플리케이션에서 다른 도메인(또는 프로토콜 또는 포트)으로부터 리소스를 요청하는 과정에서 발생하는 보안 정책
- 웹 브라우저는 보안상의 이유로 동일 출처 정책(Same-Origin Policy)을 적용하여 기본적으로 다른 도메인의 리소스 요청을 제한

- CORS를 사용
  - 다른 도메인의 API에서 데이터를 가져올 때 CORS를 사용하여 브라우저가 이를 허용하도록 설정
  - 다른 도메인의 서버로 AJAX 요청을 보낼 때, 브라우저에서 CORS 정책을 준수하도록 서버에서 응답 헤더를 설정

- REST API 서버에서 Headers에 “Access-Control-Allow-Origin” 속성을 추가
- Express에선 그냥 [CORS 미들웨어](https://expressjs.com/en/resources/middleware/cors.html)를 설치해서 사용W