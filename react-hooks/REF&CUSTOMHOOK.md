# useRef & Custom Hook

## useRef

- 컴포넌트의 생애주기 전체에 걸쳐서 유지되는 객체
- 컴포넌트가 없어질 때까지 동일한 객체가 유지
- **값을 참조**하기 위한 객체
  - 언제든지 값을 변경할 수 있음

### state vs Ref

- 상태(state)가 변경되면 해당 컴포넌트와 하위 컴포넌트를 다시 렌더링
- 레퍼런스 객체의 현재 값(current)이 바뀌더라도 렌더링에 영향을 주지 않음

### Ref 사용 용도

- 컴포넌트가 사라질 때까지 동일한 값을 써야 하는 경우. ⇒ input 등의 ID 관리
- (특히 useEffect 등과 함께 쓰면서 만나게 되는) 비동기 상황에서 현재 값을 제대로 쓰고 싶은 경우
  - Closure → 변수를 capture, bind를 깜빡하는 문제가 종종 일어남

## Custom Hook

- React에서 재사용 가능한 로직을 구성하고 관리하기 위한 방법
- 컴포넌트 간에 공유되는 상태 관리, 데이터 로딩, 라우팅 및 기타 로직을 추상화하고 중복을 최소화
- Hook은 "use"로 시작하는 camelCase로 만듬

```tsx
function useFetchProducts() {
  const [products, setProducts] = useState<Product[]>([]);

  useEffect(() => {
    const fetchProducts = async () => {
      const url ='http://localhost:3000/products'
      const response = await fetch(url);
      const data = await response.json();
      setProducts(data.products);
    }

    fetchProducts();
  }, []);

  return products;
}
```

### Hook 규칙

- 최상위(at the Top Level)에서만 Hook을 호출
  - 반복문, 조건문 혹은 중첩된 함수 내에서 Hook을 호출하면 안됨
- 오직 React 함수 내에서 Hook을 호출
  - 일반적인 JavaScript 함수에서 호출하면 안됨
