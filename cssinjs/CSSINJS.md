# CSS in JS

- CSS in JS는 단어 그대로 자바스크립트 코드에서 CSS를 작성하는 방식

## CSS in JS 장단점

### 장점

- CSS-in-JS는 CSS 모델을 문서 레벨이 아니라 컴포넌트 레벨로 추상화
- 컴포넌트 파일을 하나로 유지하면서 기능과 스타일을 같이 관리할 수 있음
- JavaScript 환경을 최대한 활용하여 CSS를 향상

### 단점

- CSS-in-JS는 런타임 오버헤드
  - 컴포넌트가 렌더링 될 때 CSS-in-JS 라이브러리는 document에 삽입할 수 있는 일반 CSS로 스타일을 “직렬화”
- CSS-in-JS는 번들 크기를 늘림

### 성능에서의 문제

- JS의 크기를 줄이는 것이 성능에 유리하고 결과적으로 CSS는 별도로 분리하는 것이 더 좋음
  - 스타일드 컴포넌트(Styled Components)를 르내리아(Linaria)로 교체하고 빌드할 때 CSS를 뽑아낼 수 있음
  - vanilla-extract나 페이스북 스타일링 라이브러리

## CSS

### 선택자

```jsx
// 1. 전체 선택자: 모든 요소 선택
* {
  color: red;
}

// 2. 유형 선택자: element 이름을 선택
input {
  font-size: 16px;
}

// 3. 클래스 선택자: class 특성을 가진 모든 요소 선택
.className {
  font-size: 16px;
}

// 4. id 선택자: id 특성에 따라 요소를 선택
#id {
  font-size: 16px;
}
```

### Box model

- 기본적으로 모든 HTML 요소를 둘러싸는 Box
- content, padding, border, margin

1. 내용(content): 텍스트나 이미지가 들어있는 박스의 실질적인 내용 부분
2. 패딩(padding): 내용과 테두리 사이의 간격
3. 테두리(border): 내용와 패딩 주변을 감싸는 테두리
4. 마진(margin): 테두리와 이웃하는 요소 사이의 간격

>height와 width 속성  
>height와 width 속성을 설정할 때 그 크기가 가르키는 부분은 내용(content) 부분만을 대상

### media query

- 화면 해상도나 브라우저 뷰포트 너비와 같은 기타 특성에 따라 CSS 스타일을 적용

```jsx
@media (조건) {
  스타일
}

@media (max-width: 800px) {
  .box {
    background-color: tomato;
  }
}
```

### Flex Box

- 인터페이스 내의 아이템 간 공간 배분과 강력한 정렬 기능을 제공

```jsx
.flex-container {
  display: flex;

  // 주축은 flex-direction 속성을 사용하여 지정하며 교차축은 이에 수직인 축
  flex-direction: row;
  /* flex-direction: column; */
  /* flex-direction: row-reverse; */
  /* flex-direction: column-reverse; */

  // 주축을 따라 flex 항목 행을 정렬하는 방식을 지정
  justify-content: flex-start;
  /* justify-content: flex-end; */
  /* justify-content: center; */
  /* justify-content: space-between; */
  /* justify-content: space-around; */
  /* justify-content: space-evenly; */

  //flex 컨테이너에 지정하는 속성이며, 교차축을 따라 flex 항목 열을 정렬하는 방식
  align-items: stretch;
  /* align-items: flex-start; */
  /* align-items: flex-end; */
  /* align-items: center; */
  /* align-items: baseline; */
}
```

### Grid

- 웹페이지를 위한 이차원 레이아웃 시스템

```jsx
.container {
  display: grid;
  // 컨테이너에 Grid 트랙의 크기들을 지정해주는 속성 (rows로도 가능)
  grid-template-columns: 200px 200px 500px;
  /* grid-template-columns: 1fr 1fr 1fr */
  /* grid-template-columns: repeat(3, 1fr) */
  /* grid-template-columns: 200px 1fr */
  /* grid-template-columns: 100px 200px auto */

  // 그리드 셀 사이의 간격을 설정
  row-gap: 10px;
  column-gap: 20px;
  gap: 10px 20px;
  grid-gap: 20px;
}
```