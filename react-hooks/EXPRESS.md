# Express

- Node.js를 위한 빠르고 개방적인 간결한 웹 프레임워크

## npm 패키지 세팅하기

- [Express 설치하기](https://expressjs.com/ko/starter/installing.html)
- [ts-node](https://github.com/TypeStrong/ts-node)

npm 시작하기

```bash
npm init -y
```

Typescript

```bash
npm i -D typescript
npx tsc --init
```

ts-node

```bash
npm i -D ts-node
```

ESLint

```bash
npm i -D eslint
npx eslint --init
```

Express

```bash
npm i express
npm i -D @types/express
```

## Express 실행하기

### express 기본 코드

```typescript
import express from 'express';

const port = 3000;

const app = express();

// http://localhost:3000
app.get('/', (req, res) => {
  res.send('Hello, world!');
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

### ts-node로 실생하는 방법

```bash
npx ts-node app.ts
```

```json
"scripts": {
  "start": "ts-node app.ts",
  "lint": "eslint --fix ."
},
```

### 코드를 수정할 때 마다 서버를 재실행하는 문제 해결

- [nodemon](https://github.com/remy/nodemon) 설치

```bash
npm i -D nodemon
npx nodemon app.ts
```

```json
"scripts": {
  "start": "nodemon app.ts",
  "lint": "eslint --fix ."
},
```

## REST API

- 웹 기반 애플리케이션에서 자원(데이터)을 다루기 위한 아키텍처 스타일 또는 웹 서비스 디자인 패턴

### REST API 특징

#### 1. 자원 (Resource)

- REST API는 자원을 나타내는 데이터를 다루는데 사용
- 자원은 고유한 식별자(URI)를 가지며, 예를 들어 웹 사이트의 사용자, 제품, 주문과 같은 데이터를 자원으로 볼 수 있음
  - 이렇게 안 하고: /write-post
  - 이렇게 하자: /posts → 뭔가를 한다 (CRUD)

#### 2. HTTP 메소드

- REST API는 HTTP 메소드(GET, POST, PUT, DELETE)를 사용하여 자원을 다룸
- CRUD에 대해 HTTP Method를 대입, Read는 Collection(복수), Item(단수)로 나뉨
- 기본 리소스 URL: /products
  - Read (Collection) → GET /products ⇒ 상품 목록 확인
  - Read (Item) → GET /products/{id} ⇒ 특정 상품 정보 확인
  - Create (Collection Pattern 활용) → POST /products ⇒ 상품 추가 (JSON 정보 함께 전달)
  - Update (Item) → PUT 또는 PATCH /products/{id} ⇒ 특정 상품 정보 변경 (JSON 정보 함께 전달)
  - Delete (Item) → DELETE /products/{id} ⇒ 특정 상품 삭제

#### 3. 상태 없음 (Stateless)

- REST API는 요청 간에 상태를 유지하지 않으며, 각 요청은 모든 필요한 정보를 포함해야함

#### Thinking in React 예제

```typescript
app.get('/products', (req, res) => {
  const products = [
    {
      category: 'Fruits', price: '$1', stocked: true, name: 'Apple',
    },
    {
      category: 'Fruits', price: '$1', stocked: true, name: 'Dragonfruit',
    },
    {
      category: 'Fruits', price: '$2', stocked: false, name: 'Passionfruit',
    },
    {
      category: 'Vegetables', price: '$2', stocked: true, name: 'Spinach',
    },
    {
      category: 'Vegetables', price: '$4', stocked: false, name: 'Pumpkin',
    },
    {
      category: 'Vegetables', price: '$1', stocked: true, name: 'Peas',
    },
  ];

  res.send({ products });
});
```
