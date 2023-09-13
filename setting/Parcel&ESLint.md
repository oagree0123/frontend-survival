# Parcel & ESLint

## Parcel

- 모듈 번들러
- 특별한 설정없이 사용 가능한 빌드 툴
- swc 사용 함으로 속도가 빠름

> 빌드는 소프트웨어 개발에서 프로그램 소스 코드를 실행 가능한 형태로 변환하는 프로세스

#### 모듈 번들러(bundler)란? 
> 웹 애플리케이션을 구성하는 자원(HTML, CSS, Javscript, Images 등)을 모두 각각의 모듈로 보고 이를 조합해서 병합된 하나의 결과물을 만드는 도구

```sh
// 기본 실행 명령어
npx parcel index.html --port 8080
```
#### package.json 활용

```json
"source": "index.html",
  "scripts": {
    "start": "parcel --port 8080",
    ...
  }
```

### Static 파일 처리

#### 1. 패키지 설치
```sh
npm install -D parcel-reporter-static-files-copy
```
#### 2. .parcelrc 파일 설정
```json
{
  "extends": ["@parcel/config-default"],
  "reporters":  ["...", "parcel-reporter-static-files-copy"]
}
```

#### 3. 폴더 생성

- parcel의 기본 경로 static 폴더 생성
- static 폴더 내부에 이미지 파일

### 빌드 + 정적 서버 실행

```sh
npx parcel build
npx servor ./dist
```

## ESLint

>린트는 소스코드를 분석하여 문법적인 오류나 스타일적인 오류, 적절하지 않은 구조 등에 표시를 달아주는 행위

- 스타일 통일
  - 다른 사람과 협업에 코드의 형식을 맞춰줌
- 베스트 프랙티스 추천
  - 최신 트렌드를 학습하는데 활용할 수 있다.

#### 코드 변경 저장 시 자동 ESLint 실행

1. VS Code Extension 설치 : ESLint
2. .vscode > setting.json 파일 추가
```json
{
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    }
}
```

아샬님의 설정
- Trailing Spaces Extension 설치
```json
{
    "editor.rulers": [
        80
    ],
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    },
    "trailing-spaces.trimOnSave": true
}
```