# Design System

- 프로젝트에서 일관된 디자인과 사용자 경험을 구축하고 유지하기 위한 표준화된 가이드라인, 구성 요소, 리소스, 및 디자인 패턴의 모음을 나타냄
- 재사용 가능한 구성 요소와 패턴을 사용하여 대규모로 디자인을 관리하기 위한 완전한 표준의 집합

## 참고 사이트

### 사례

- [Atlassian Design System](https://atlassian.design/)
- [Material Design (Google)](https://material.io/)
- [Base Web (Uber)](https://baseweb.design/)
- [Polaris (Shopify)](https://polaris.shopify.com/)
- [Lightning Design System (Salesforce)](https://www.lightningdesignsystem.com/)
- [Mailchimp Pattern Library](https://ux.mailchimp.com/patterns)
- [Ant Design](https://ant.design/)

### Gallery

- [Design Systems Gallery](https://designsystemsrepo.com/design-systems/)
- [Design Systems](https://www.designsystems.com/open-design-systems/)

## 반응형 웹 디자인

- 여러 디바이스와 화면 크기에 맞게 조정되도록 디자인하는 웹 디자인 접근 방식
- 다양한 디바이스에서 일관된 레이아웃 및 내용을 제공하는 데 중점둬 사용자 경험을 향상

### 특징

1. 유연한 그리드 시스템
   - 화면 크기에 따라 다르게 배치되는 요소의 배율을 조정할 수 있음

2. 미디어 쿼리(Media Queries)
   - CSS 미디어 쿼리를 사용하여 디바이스의 화면 크기 및 해상도에 따라 스타일 및 레이아웃을 동적으로 조절
   - 이를 통해 특정 화면 크기 범위에서 다른 스타일 및 레이아웃이 적용

3. 이미지 및 미디어 조정
   - 이미지와 미디어는 화면 크기에 맞게 크기를 조절
   - 다른 해상도의 이미지를 제공함으로써 사용자의 대역폭을 절약하고 빠른 로딩 속도를 제공

## Atomic Design

- 디자인 시스템을 만들기 위해 사용되는 디자인 패턴
- 웹 디자인 및 개발에서 사용되는 디자인 패턴 및 컴포넌트 구축 방법론

### Atomic Design 계층

1. Atoms (원자)
   - 원자는 디자인의 가장 기본적인 요소
   - 단순한 UI 요소가 포함되며, 예를 들어 버튼, 텍스트 필드, 아이콘, 레이블 등이 있음
   - 개별적으로 사용 가능하며, 다른 계층에서 조합하여 더 복잡한 구성 요소를 만들 수 있음

2. Molecules (분자)
   - 분자는 원자의 조합으로, 좀 더 복잡한 UI 구성 요소
   - 예를 들어, 검색 바, 로그인 폼, 카드 등

3. Organisms (조직):
   - 조직은 더 큰 UI 컴포넌트나 모듈
   - 예를 들어 네비게이션 메뉴, 헤더, 사이드바, 푸터 등이 조직에 해당합니다.

4. Templates (템플릿)
   - 템플릿은 페이지의 레이아웃을 정의하는데 사용되는 구성 요소
   - 여러 조직을 조합하여 페이지의 구조를 생성하며, 페이지에서 사용될 특정 레이아웃 및 구성을 정의

5. Pages (페이지)
   - 페이지 계층은 실제 웹 페이지
   - 템플릿과 컨텐츠를 결합하여 사용자에게 표시되는 구체적인 웹 페이지를 형성
