# Testing Library

## Jest

- 테스팅 도구
- Mocah와 Chai 처럼 RSpec의 describe-it 의 형태를 지원
- expect로 단언(assertion)할 수 있음
- Mocking도 다양한 레벨에서 쉽게 사용

> Mock 이란 "모의, 가짜의" 라는 의미로 테스트 시 실제 객체를 제대로 구현하기 어려울 경우 가짜 객체를 만들어서 테스트의 효용성을 높이는 기술이다.

## TDD

- TDD란 Test Driven Development의 약자로 테스트 주도 개발
- 테스트 코드를 작성한 뒤에 실제 코드를 작성하는 방법

### TDD 개발주기
- Red 단계에서는 실패하는 테스트 코드를 먼저 작성한다.
- Green 단계에서는 테스트 코드를 성공시키기 위한 실제 코드를 작성한다.
- Blue 단계에서는 중복 코드 제거, 일반화 등의 리팩토링을 수행한다.

### TDD 장점
1. 코드 작성 전 셀계에 대해서 구체적인 작성 가능
2. 오류에 대한 파악이 빨라 디버깅 시간 단축
3. 문서의 대체 가능

### TDD 단점
1. 단기적으로 시간 소모가 큼
2. 100%의 안정성을 보장하는 것은 아님

## BDD

- Behavior Driven Development 약자로 행동 주도 개발
- Given - When -Then 세가지로 테스트를 진행

### Given-When-Then
Given - 시나리오 상에서 주어진 환경을 정의합니다.
When - 사용자가 어떤 행위를 하는 것을 정의합니다.
Then - 그에 따른 어떠한 결과를 정의합니다.

#### 예제

700W 전자렌지를 준비해서 3분만 돌리면 완성!

Given - 700W 전자렌지와 3분의 시간 준비
When - 주어진 시간동안 전자렌지를 돌림
Then - 요리가 완성

```Ruby
# Given
power = 700.watts
time = 3.minutes
stove = Stove.new(power)

# When
food = stove.cook(time)

# Then
assert food.complete?`
```

## jest 작성하기

```typescript
// 기본 형태 테스트
test('add', () => {
  expect(add(1, 2)).toBe(3);
});

// BDD 스타일의 테스트
describe('add 함수', () => {
	it('returns sum of two numbers', () => {
		expect(add(1, 2)).toBe(3);
	});

	it('returns numbers', () => {
		expect(typeof add(1, 2)).toBe('number');
	});
});
```

## React Testing Library(RTL)

- UI 테스트에 특화된 라이브러리
- React 컴포넌트 테스트만 되는 것은 피하는 것이 좋다
  - 너무 많은 테스트를 작성할 수 있음
  - 유지보수를 하는데 어려움을 줄 수 있음

```typescript
// Greeting 컴포넌트의 텍스트 유무 테스트
test('Greeting', () => {
	render(<Greeting name="world" />);

	screen.getByText('Hello, world!');
	screen.getByText(/Hello/);

	expect(screen.queryByText(/Hi/)).not.toBeInTheDocument();
});
```