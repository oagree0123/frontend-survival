# Style Basic

## Basic Style

### Class

- CSS는 컴포넌트를 전제로 하고 있지 않음
- 공통된 부분이 많을 때 재사용하기 쉬움
  - 따라서, 컴포넌트 중심으로 빠르게 작업하려고 하면 불편할 때가 많음

index.html의 head 태그 안에 작성

```html
<style>
  .greeting {
    color: #00F;
  }
</style>
```

```jsx
export default function Greeting() {
  return (
    <p className="greeting">
      Hello, world!
    </p>
  );
}
```

### Inline Style

- "style" 속성 활용
- 평범한 JavaScript 객체를 활용하므로 변수, 함수 등을 재사용할 수 있음

```jsx
// 객체 활용하기
export default function Greeting() {
  const style = {
    color: '#00F',
  };

  return (
    <p style={style}>
      Hello, world!
    </p>
  );
}

// 바로 사용하기
export default function Greeting() {
  return (
    <p
      style={{
        color: '#00F',
      }}
    >
      Hello, world!
    </p>
  );
}
```

## 의미있는 마크업 (Semantic Markup)

- HTML 요소와 속성을 사용하여 웹 페이지의 구조와 콘텐츠를 명확하게 정의하는 것을 의미
- 웹 접근성, 검색 엔진 최적화(SEO), 코드 유지 보수 등 다양한 이점을 제공

### 사용의 예시

1. 의미있는 태그 사용
   - header 태그, nav 태그, main 태그, article 태그, section 태그, footer 태그 등을 활용하여 웹 페이지의 구조를 정의
2. 의미있는 제목 사용
   - h1 태그, h2 태그, h3 태그 등의 제목 태그를 사용하여 각 섹션과 페이지의 제목을 정의
3. 목록과 표 사용
   - ul, ol, li 태그를 사용하여 목록을 작성
   - table, thead, tbody 등 태그를 사용하여 표를 작성할 때 의미있는 구조 유지
4. 링크와 이미지에 대체 텍스트
   - a 태그, img 태그를 사용할 때 alt 속성 등을 제공하여 사용자에게 콘텐츠 설명

- 의미 있는 마크업을 사용하면 웹 페이지가 더 이해하기 쉽움
- 검색 엔진이 콘텐츠를 더 잘 이해하며, 웹 접근성을 개선
