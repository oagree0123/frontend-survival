# TypeScript

## REPL

> REPL은 "Read-Eval-Print Loop"의 약자로, 사용자가 입력한 명령어나 코드를 읽어들이고(eval), 그 결과를 출력(print)한 후 다시 사용자의 입력을 받는(loop) 대화형 프로그래밍 환경을 말합니다.

```sh
npx ts-node
```

## 타입 지정

```typescript
let name: string;
name = '홍길동'; 
name = 20; // error

// 객체 타입
let human: {
  name: string;
  age: number;
}

hunman = { name: '홍길동', age: 20 };
hunman = { name: '홍길동', age: 20, hp: 100 }; // error : 타입 지정 이외 키 값 불가능
```

### 타입 지정하는 다양한 방법

```typescript
// type
type Human = {
  name: string;
  age: number;
};

let boy: Human;
boy = { name: '홍길동', age: 20 };

// interface
interface Person {
  name: string;
  age: number;
};

let girl: Human;
girl = { name: '하니', age: 20 };

// function
// function func(매개변수: 매개변수 타입): 반환 타입 { return; }
function add(x: number, y: number): number {
  return x + y;
}

add(1, 2)
add('Hello', 'World') // error TS1002: Unterminated string literal.

function sub(x: number, y: number): string {
  return x - y;
} // error TS2322: Type 'number' is not assignable to type 'string'.

// 정해진 값으로 지정
let category: 'food';

// 배열 | Tuple
let numbers: number[];
let anything: any[];
let pair: [string, number]; pair = ['hp', 100];
```

#### 타입 추론

직접 타입을 정해주지 않아도 자동으로 타입을 추론해서 지정해주는 것

```typescript
let name = '홍길동'
typeof(name) // string
```

#### Union Type

유니온 타입은 OR연산자와 같이 A 또는 B 라는 의미를 가진다.

```typescript
type numOrStr = number | string;
let num: numOrString = 3;
let str: numOrString = 'hi';

type bool = true | false;

type Category = 'food' | 'toy' | 'bag';
function fetchProducts({ category }: { category: Category }) {
  console.log(`Fetch ${category}`);
}
```

>타입스크립트의 장점으로는 타입을 지정함에 따라 자동 완성이나 타입에 따른 메소드를 자동으로 보여주는 것이 있다.

#### Optional Parameter

파라미터 중에서 필요하지 않은 것을 지정하는 방법

```typescript
function greeting(name?: string): string {
  return `Hello, ${name || 'world'}`;
}

// default parameter : 기본값 설정
function greeting(name: string = 'world'): string {
  return `Hello, ${name}`;
}

// 객체를 파라미터로 받을 때
type Human = {
  name: string;
  age?: number;
};

// age 유무에 따른 return
function greeting({ name, age }: Human): string {
  return age ? `${name} (${age})` : name;
}
```

#### 확장하기

```typescript
type Human = {
  name: string;
  age: number;
};

type Creature = {
  hp: number;
  mp: number;
};

type Person = Human & Creature;

let person: Person;
person = { name: '홍길동', age: 13, hp: 256, mp: 16 };
```

### 타입 별칭과 인터페이스 차이점

interface가 가지는 대부분의 기능은 type에서도 사용할 수 있습니다.
가장 핵심적인 차이는 type은 새 프로퍼티를 추가하도록 개방될 수 없는 반면,
인터페이스 경우 항상 확장될 수 있습니다.

#### 확장하기

```typescript
// interface
interface Animal {
  name: string
}

interface Bear extends Animal {
  honey: boolean
}

const bear = getBear()
bear.name
bear.honey

//type
type Animal = {
  name: string
}

type Bear = Animal & {
  honey: Boolean
}

const bear = getBear();
bear.name;
bear.honey;
```

#### 새 필드 추가하기

```typescript
// interface
interface Window {
  title: string
}

interface Window {
  ts: TypeScriptAPI
}

const src = 'const a = "Hello World"';
window.ts.transpileModule(src, {});

// type
type Window = {
  title: string
}

type Window = {
  ts: TypeScriptAPI
}
// Error: Duplicate identifier
```

### Generic 타입

```typescript
function identity(arg: any): any {
  return arg;
}

// 제네릭 타입
function identity<Type>(arg: Type): Type {
  console.log(typeof(arg));
  return arg;
}
identify('hello')
// string
// 'hello'
```
