# MSW

- 코드 레벨이 아니라 네트워크 레벨에서 가짜 구현. 오프라인 작업 등을 지원하기 위한 서비스 워커의 기능을 활용한 것
- API 스펙은 나왔지만 아직 구현되지 않은 경우 임시로 사용할 수도 있음
- 단순히 임시 서버를 만들 거라면 Express를 쓰는 게 더 낫지만, 테스트 코드도 지원하면서 겸사겸사 웹 브라우저를 지원하는 용도로는 나쁘지 않은 선택

## Service Worker

- 웹 응용 프로그램, 브라우저, 그리고 (사용 가능한 경우) 네트워크 사이의 프록시 서버 역할
- 효과적인 오프라인 경험을 생성하고, 네트워크 요청을 가로채서 네트워크 사용 가능 여부에 따라 적절한 행동을 취하고, 서버의 자산을 업데이트할 수 있음

> 프록시 서버(Proxy Server)는 인터넷 트래픽을 중계하고 필터링하는 중간 서버입니다.

## MSW 사용하기

```bash
npm i -D msw
```

```javascript
// jest.config.js
setupFilesAfterEnv: [
  '@testing-library/jest-dom/extend-expect',
  '<rootDir>/src/setupTests.ts',
],
```

```javascript
// src/setupTests.js
import server from './mocks/server';

beforeAll(() => server.listen({ onUnhandledRequest: 'error' }));

afterAll(() => server.close());

afterEach(() => server.resetHandlers());
```

```javascript
// src/mocks/server.ts
import { setupServer } from 'msw/node';

import handlers from './handlers';

const server = setupServer(...handlers);

export default server;
```

```javascript
// src/mocks/handlers.ts
import { rest } from 'msw';

const BASE_URL = 'http://localhost:3000';

const handlers = [
  rest.get(`${BASE_URL}/products`, (req, res, ctx) => {
    const products = [
      {
        category: 'Fruits', price: '$1', stocked: true, name: 'Apple',
      },
    ];

    return res(
      ctx.status(200),
      ctx.json({ products }),
      );
    }),
  ];

export default handlers;
```

```javascript
// App.test.ts
import { render, screen, waitFor } from '@testing-library/react';
import App from './App';
import useFetchProducts from './hooks/useFetchProducts';

// jest.mock('./hooks/useFetchProducts', () => () => fixtures.products);
// jest.mock('./hooks/useFetchProducts');

test('App', async () => {
  render(<App />);

  await waitFor(() => {
    screen.getByText('Apple');

    expect(useFetchProducts).toBeCalled();
  })
});
```

=> 최신의 Node.js 경우 fetch가 있지만, 버전에 따라 없는 경우가 있음

## fetch polyfill

[GitHub에서 만든 fetch polyfill](https://github.com/github/fetch)

```bash
npm i -D whatwg-fetch
```

```js
import 'whatwg-fetch';
```

### polyfill 

- 이전 버전의 웹 브라우저에서 지원하지 않는 새로운 기능이나 API를 구현하고, 이전 버전의 브라우저에서도 동작하도록 하는 코드 조각
- 웹 개발자가 브라우저 호환성을 유지하면서 최신 웹 기술을 활용할 수 있도록 돕는 역할
