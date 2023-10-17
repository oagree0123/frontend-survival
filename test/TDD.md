# TDD

## TDD Cycle

1. 테스트 작성 (Red)
   - 실패하는 테스트 코드를 작성
   - 인터페이스와 스펙에 집중
   - 먼저 개발하려는 기능 또는 모듈에 대한 테스트 케이스 작성
   - 아직 개발되지 않은 코드에 대한 기대 동작을 정의
2. 코드 작성 (Green)
   - 재빨리 테스트를 통과
   - 올바른 방법이 아니어도 괜찮음
   - 목표는 테스트를 통과하는 코드를 작성하여 실패하는 테스트를 성공하도록 만드는 것
3. 리팩터링 (Refactor)
   - 리팩터링을 통해 코드를 올바르게 만듬
   - 테스트 케이스를 통과한 코드를 개선하고, 중복 코드를 제거하거나 코드의 가독성을 높이기 위해 리팩터링을 수행
   - TDD에서 가장 중요한 부분

> 작은 단계를 찾고, 코드에서 피드백을 얻는 게 (어렵고) 중요하다. 2번이 어렵다면 1번으로 돌아가서 더 작고 쉬운 문제를 정의하고, 3번을 위해 의도를 드러내고 중복을 찾아 제거하는 연습을 해야 한다. 이 둘이 익숙하지 않으면 TDD를 하는 게 거의 불가능하고, 사실 이 둘이 어려우면 일반적인 개발 또는 클린 코드를 작성하는 것 또한 매우 힘들다.

## JEST

### jest 사용하기

```javascript
test('add', () => {
  expect(add(1, 2)).toBe(3);
});
```

#### BDD (Behavior-Driven Development) 스타일

- BDD 테스트 케이스는 "Given, When, Then" 구조
- 행위나 동작을 중심으로 개발하는 접근 방식

```javascript
const context = describe;

describe('add', () => {
  context('with no arguments', () => {
    it('returns zero', () => {
      expect(add()).toBe(0);
    })
  });

  context('with only one arguments', () => {
    it('returns the same numbers', () => {
      expect(add(2)).toBe(2);
    })
  });

  context('with two arguments', () => {
    it('returns sum of two numbers', () => {
      expect(add(1, 2)).toBe(3);
    })
  });

  context('with three arguments', () => {
    it('returns sum of three numbers', () => {
      expect(add(1, 2, 3)).toBe(6);
    })
  });
});
```

```javascript
function add(...numbers: number[]): number {


  if(numbers.length === 0) {
    return 0;
  }

  // refactor
  return numbers.reduce((acc, number) => acc + number);

  // return add(...numbers.slice(0, numbers.length - 1))
  //   + numbers[numbers.length - 1];

  // if(numbers.length === 1) {
  //   return numbers[0];
  // }
  // if(numbers.length === 2) {
  //   return numbers[0] + numbers[1];
  // } ...
}
```

```javascript
// jest.config.js

const context = describe;

describe('add', () => {
  context('with no argument', () => {
    it('returns zero', () => {
      expect(add()).toBe(0);
    });
  });

  context('with only one number', () => {
    it('returns the same number', () => {
      expect(add(1)).toBe(1);
    });
  });

  context('with two numbers', () => {
    it('returns sum of two numbers', () => {
      expect(add(1, 2)).toBe(3);
    });
  });

  context('with three numbers', () => {
    it('returns sum of three numbers', () => {
      expect(add(1, 2, 3)).toBe(6);
    });
  });
});
```

## Describe-Context-It 패턴

- 테스트 케이스를 명확하고 읽기 쉽게 구성하는 데 도움을 주는 구조적 패턴

1. Describe
   - 일반적으로 특정 기능, 모듈, 또는 컴포넌트에 대한 테스트를 그룹화할 때 사용
   - 수행할 테스트 케이스에 대한 전반적인 설명을 포함
2. Context (내부의 Describe)
   - Context 블록은 Describe 내부에 추가로 그룹화를 할 때 사용
   - 특정컨텍스트를 정의하고 테스트 케이스를 해당 컨텍스트 내에서 실행
   - 여러 상황에서 동일한 행위를 테스트하려는 경우 유용
3. It
   - 개별 테스트 케이스를 작성하는 데 사용
   - 특정 동작, 예상 결과를 설명하며 해당 동작이 기대대로 수행되는지 확인
   - 테스트 케이스의 이름과 테스트 코드가 포함됩니다.

## 단위 테스트 (Unit Test)

- 가장 작은 "단위"를 테스트하는 것을 의미
- 함수, 메서드, 클래스, 모듈과 같이 개별적으로 테스트할 수 있는 부분을 가리킴
- 단위 테스트의 목적은 코드가 정확하게 동작하는지 확인하고, 코드 변경 시 이전에 동작하던 것이 여전히 동작하도록 보장하는 것
