# 장바구니

## 장바구니 추가하기

### getter 로 금액 계산

```tsx
export default function Price() {
  const [{ product }] = useProductDetailStore();
  const [{ price }, productFormStore] = useProductFormStore();

  // TODO: product 변경에 따른 setProduct 호출은 여기가 아니라 page 등에서 처리할 것!
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
