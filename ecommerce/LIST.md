# 목록 보기

## 상품 목록

- 숫자를 읽기 좋게 보여주는 방법

```jsx
export default function numberFormat(value: number) {
  return new Intl.NumberFormat().format(value);
}
```

- store로 상품 목록 관리

```jsx
@singleton()
@Store()
export default class ProductsStore {
  products: ProductSummary[] = [];

  async fetchProducts() {
    this.setProducts([]);

    const { data } = await axios.get(`${apiBaseUrl}/products`);
    const { products } = data;

    this.setProducts(products);
  }

  @Action()
  setProducts(products: ProductSummary[]) {
    this.products = products;
  }
}
```

```jsx
export default function useFetchProducts(): {
  products: ProductSummary[];
} {
  const store = container.resolve(ProductsStore);

  const [{ products }] = useStore(store);

  useEffectOnce(() => {
    store.fetchProducts();
  });

  return { products };
}
```

## 카테고리

- API 호출을 모아주는 ApiService

```jsx
// src/services/apiService.ts
const API_BASE_URL = process.env.API_BASE_URL
                     || 'https://shop-demo-api-01.fly.dev';

export default class ApiService {
  private instance = axios.create({
    baseURL: API_BASE_URL,
  });

  async fetchCategories(): Promise<Category[]> {
    const { data } = await this.instance.get('/categories');
    const { categories } = data;
    return categories;
  }

  async fetchProducts(): Promise<ProductSummary[]> {
    const { data } = await this.instance.get('/products');
    const { products } = data;
    return products;
  }
}

export const apiService = new ApiService();
```

- categoryId를 사용하면 변경될 때 함수 호출 되도록 useEffect

```jsx
export default function useFetchProducts({ categoryId }: {
  categoryId: string;
}): {
  products: ProductSummary[];
} {
  const store = container.resolve(ProductsStore);

  const [{ products }] = useStore(store);

  useEffect(() => {
    store.fetchProducts({ categoryId });
  }, [store, categoryId]);

  return { products };
}
```

- axios get요청의 param 사용법

```jsx
async fetchProducts({ categoryId }: {
  categoryId?: string;
} = {}): Promise<ProductSummary[]> {
  const { data } = await this.instance.get('/products', {
    params: { categoryId },
  });
  const { products } = data;
  return products;
}
```
