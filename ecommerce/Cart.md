# 장바구니

## 장바구니 추가하기

### getter 로 금액 계산

```tsx
export default function Price() {
  const [{ product }] = useProductDetailStore();
  const [{ price }, productFormStore] = useProductFormStore();

  useEffect(() => {
    productFormStore.setProduct(product);
  }, [productFormStore, product]);

  return (
    <Container>
      {numberFormat(price)}
      원
    </Container>
  );
}
```

```jsx
@singleton()
@Store()
export default class ProductFormStore {
  product: ProductDetail = nullProductDetail;

  quantity = 1;

  @Action()
  setProduct(product: ProductDetail) {
    this.product = product;
  }
...
```

### Immutable Array

#### Push

```jsx
const a = [1, 2, 3];
const b = [...a, 4];

function push(xs, value) {
  return [...xs, value];
}
```

#### Shift

```jsx
const a = [1, 2, 3];
const [b, ...c] = a;

function shift(xs) {
  const [head, ...tail] = xs;
  return [head, tail];
}
```

#### Pop

```jsx
const a = [1, 2, 3];
const b = a[a.length - 1];
const c = a.slice(0, a.length - 1);

function pop(xs) {
  return [
    xs[xs.length - 1],
    xs[slice(0, xs.length - 1)],
  ];
}
```

#### Remove (by value)

```jsx
const a = [1, 2, 3];
const b = a.filter(i => i !== 2);

function remove(xs, value) {
  return xs.filter(x => x !== value);
}
```

#### Remove (by index)

```jsx
const a = [1, 2, 3];
const b = a.filter((_, i) => i !== 1);

function removeAt(xs, index) {
  return xs.filter((_, i) => i !== index);
}

//

const a = [1, 2, 3];
const b = [...a.slice(0, 1), ...a.slice(1 + 1)];

function removeAt(xs, index) {
  return [...a.slice(0, index), ...a.slice(index + 1)];
}
```

- 죽어도 splice

```jsx
const a = [1, 2, 3];
const b = [...a];
b.splice(1, 1);

function removeAt(xs, index) {
  const clone = [...xs];
  clone.splice(index, 1);
  return clone;
}
```
