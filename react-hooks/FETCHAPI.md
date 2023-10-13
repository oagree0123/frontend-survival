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

## Promise

![promise](./promises.png)

- 비동기 작업을 보다 효율적으로 다루기 위한 패턴 중 하나
- Promise는 비동기 작업이 완료되었을 때 또는 실패했을 때 처리할 수 있는 객체
- Promise는 다음 중 하나의 상태를 가짐
  - 대기(pending): 이행하지도, 거부하지도 않은 초기 상태.
  - 이행(fulfilled): 연산이 성공적으로 완료됨.
  - 거부(rejected): 연산이 실패함.

## Readable Stream

- 데이터 소스로부터 데이터를 비동기적으로 읽을 수 있는 Node.js의 개념
- Readable Stream은 주로 파일, 네트워크 연결, 또는 다른 데이터 소스로부터 데이터를 읽는 데 사용

### 예제

```javascript
fetch("https://www.example.org/").then((response) => {
  const reader = response.body.getReader();
  const stream = new ReadableStream({
    start(controller) {
      // 아래 함수는 각 data chunck를 다룬다.
      function push() {
        // "done"은 Boolean 이며 value는 "Uint8Array 이다."
        reader.read().then(({ done, value }) => {
          // 더이상 읽은 데이터가 없는가?
          if (done) {
            // 브라우저에게 데이터 전달이 끝났다고 알린다.
            controller.close();
            return;
          }

          // 데이터를 얻고 컨트롤러를 통하여 그 데이터를 브라우저에 넘긴다.
          controller.enqueue(value);
          push();
        });
      }

      push();
    },
  });

  return new Response(stream, { headers: { "Content-Type": "text/html" } });
});
```

## Unicode

- Unicode(유니코드)는 전 세계의 모든 문자를 나타내기 위한 표준 문자 인코딩 시스템
- 문자와 숫자를 컴퓨터에서 저장하고 표현하기 위한 국제 표준
- 각 문자에 대해 고유한 코드 포인트가 있으며, 이 코드 포인트는 16진수로 표현
  -  Unicode 문자를 바이트로 인코딩하는 여러 가지 방식 중 하나입니다. UTF-8, UTF-16 및 UTF-32와 같은 다양한 인코딩 형식

## CORS (Cross-Origin Resource Sharing)

- 웹 애플리케이션에서 다른 도메인(또는 프로토콜 또는 포트)으로부터 리소스를 요청하는 과정에서 발생하는 보안 정책
- 웹 브라우저는 보안상의 이유로 동일 출처 정책(Same-Origin Policy)을 적용하여 기본적으로 다른 도메인의 리소스 요청을 제한

- CORS를 사용
  - 다른 도메인의 API에서 데이터를 가져올 때 CORS를 사용하여 브라우저가 이를 허용하도록 설정
  - 다른 도메인의 서버로 AJAX 요청을 보낼 때, 브라우저에서 CORS 정책을 준수하도록 서버에서 응답 헤더를 설정

- REST API 서버에서 Headers에 “Access-Control-Allow-Origin” 속성을 추가
- Express에선 그냥 [CORS 미들웨어](https://expressjs.com/en/resources/middleware/cors.html)를 설치해서 사용W