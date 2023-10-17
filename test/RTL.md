# React Testing Library

- React 컴포넌트를 사용자 입장에 가깝게 테스트할 수 있는 도구
- 애플리케이션의 동작이 예상대로 작동하는지 확인할 수 있음
- DOM 요소를 실제로 클릭하거나 입력할 수 있는 방법을 제공

## RTL 사용하기

```jsx
import { fireEvent, render, screen } from "@testing-library/react"
import TextField from "./TextField"

test('TextField', () => {
  // given
  const label = 'Name';
  const text = 'Tester';

  let called = false;
  const setText = () => {
    called = true;
  };

  // when
  render((
    <TextField 
      label={label}
      placeholder="Input your name"
      text={text}
      setText={setText}
    />
  ));

  // then
  screen.getByLabelText(label);
  screen.getByPlaceholderText(/name/);
  screen.getByDisplayValue(text);

  // -----

  // when
  fireEvent.change(screen.getByLabelText(label), {
    target: { value: 'New Name '},
  });

  // then
  expect(called).toBeTruthy();
})
```

### BDD 스타일

```jsx
const context = describe;

describe('TextField', () => {
  // given
  const label = 'Name';
  const text = 'Tester';

  const setText = jest.fn();

  beforeEach(() => {
    //setText.mockClear();
    jest.clearAllMocks();
  })

  function renderTextField() {
    render((
      <TextField
        label={label}
        placeholder="Input your name"
        text={text}
        setText={setText}
      />
    ));
  }

  function inputText(value: string) {
    fireEvent.change(screen.getByLabelText(label), {
      target: { value },
    });
  }

  it('renders elements', () => {
    // when
    renderTextField();

    // then
    screen.getByLabelText(label);
    screen.getByPlaceholderText(/name/);
    screen.getByDisplayValue(text);
  });

  context('when user types name', () => {
    it('calls "setText" handler', () => {
      // given
      renderTextField();

      // when
      inputText('New Name');

      // then
      expect(setText).toBeCalledWith('New Name');
    });
  });
})
```

### Given-When-Then

- 테스트 케이스를 이해하고 문서화하기 쉽게 만들어줌
- 테스트의 전체 흐름을 명확하게 정의할 수 있음

1. Given (주어진 상황)
   - 테스트 시작 전의 초기 상황 또는 사전 조건을 설명
   - 테스트 시작 전에 필요한 상황, 데이터, 설정 등을 설정하거나 제공
2. When (만약에)
   - 테스트를 실행하려는 동작 또는 이벤트를 설명
   - 특정 작업 또는 이벤트가 발생했을 때 무엇이 예상되는지를 정의
   - "Given" 상황이 설정되었을 때 "When" 섹션에서 어떤 동작을 수행하는지 설명
3. Then (그러면)
   - 테스트가 성공적으로 완료되었을 때 나타나야 하는 예상 결과를 설명
   - "When"에서 정의한 동작에 대한 결과를 "Then" 예상된 결과를 반환하는지 확인

## Mocking

- 외부 의존성이나 함수 호출을 흉내내거나 가짜 구현을 사용하여 특정 동작을 시뮬레이션하는 프로세스

### 함수 Mocking

- 함수 호출을 가짜 함수로 대체하는 것을 의미
- jest.fn()을 사용하여 함수를 mocking하고 호출 및 반환 값을 조작할 수 있음

### 모듈 Mocking

- 외부 라이브러리 또는 모듈의 동작을 흉내내거나 조작하는 것을 의미
- Jest와 RTL의 모듈 mocking을 사용하면 외부 모듈을 가짜 모듈로 대체하여 컴포넌트 내에서 해당 모듈을 사용하는 동작을 테스트할 수 있음

```jsx
jest.mock('./hooks/useFetchProducts', () => () => [
  {
    category: 'Fruits', price: '$1', stocked: true, name: 'Apple'
  }
]);

test('App', () => {
  render(<App />);

  screen.getByText('Apple');
})
```

## Test Fixture

- 테스트 케이스가 실행될 때 필요한 초기 상태를 설정하고, 환경을 준비하는 데 사용되는 데이터나 객체의 집합
- 다양한 상황에서 동일한 테스트를 실행할 수 있도록 돕는 중요한 구성 요소
- 테스트 픽스처는 테스트 데이터를 제공하고, 예상 결과를 가진 데이터를 설정

```jsx
// fixtures/index.ts
import products from "./products";

export default {
  products
}
```

```jsx
// fixtures/products.ts
const products = [
  {
    category: 'Fruits', price: '$1', stocked: true, name: 'Apple'
  }
];

export default products;
```

```jsx
// src/hooks/__mocks__/useFetchProducts.ts

import fixtures from '../../../fixtures';

const useFetchProducts = jest.fn(() => fixtures.products);

export default useFetchProducts;
```

```jsx
// src/App.test.ts

// jest.mock('./hooks/useFetchProducts', () => () => fixtures.products);
jest.mock('./hooks/useFetchProducts');

test('App', () => {
  render(<App />);

  screen.getByText('Apple');

  expect(useFetchProducts).toBeCalled();
})
```
