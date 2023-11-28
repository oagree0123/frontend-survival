# 주문 결제하기

## 포트원

> [포트원](https://portone.io/korea/ko)
>

> [결제 연동하기](https://portone.gitbook.io/docs/console/guide/connect)
>

> [API Keys](https://portone.gitbook.io/docs/console/guide/api-keys)
>

> [인증결제 연동하기](https://portone.gitbook.io/docs/auth/guide)
>

> [JavaScript SDK](https://portone.gitbook.io/docs/sdk/javascript-sdk)
>

- 포트원은 여러 PG사를 하나의 API로 사용할 수 있게 하는 통합 결제 솔루션
- 인증 결제 프로세스
  1. 결제 요청 (F/E에서 요청) → 우리가 자주 보는 결제창이 뜬다
  2. 결제 결과 확인 (F/E에서 결과 받음)
  3. 결제 금액 위변조 검증 → B/E로 결제 결과를 전송하면 B/E에서 처리
  4. 결제 완료 → B/E에서 성공하면 F/E는 주문 완료 페이지로 이동

- 결제 연동 > 결제대행사 설정 및 추가
  1. 실 연동, 테스트 중 선택 → 우리는 "테스트"를 선택.
  2. PG사 선택 → 원하는 것 아무거나.
  3. "아임포트 결제모듈"을 누르면 우리가 선택한 PG사에 따라 적절한 선택지가 나옴.
  4. "+ 추가" 버튼을 누르면 상세 정보를 입력할 수 있는데, 우리는 테스트라서 그냥 "저장" 버튼을 누르면 됨.
  5. "PG상점아이디" 항목을 클릭하면 해당 정보가 복사된다. ⇒ 챙겨두자

PG사를 "카카오페이"로 선택하면 부담 없이 테스트할 수 있기 때문에 이쪽을 강력 추천함.

- 상점 ・ 계정 관리 > 내 식별코드 ・ API Keys
  - "가맹점 식별코드" 부분을 클릭하면 해당 코드가 클립보드로 복사

## 결제 요청

```jsx
// index.html
<script src="https://cdn.iamport.kr/v1/iamport.js"></script>

// main.tsx
Reflect.get(window, 'IMP').init('가맹점_식별코드');
```

```bash
API_BASE_URL=https://shop-demo-api-03.fly.dev
PORTONE_IMP=<가맹점 식별코드>
PORTONE_PG_CODE=<PG사 코드>.<PG상점아이디>
```

```jsx
Reflect.get(window, 'IMP').init(process.env.PORTONE_IMP);
```

### Payment

- [결제요청 파라미터](https://portone.gitbook.io/docs/sdk/javascript-sdk/payrq)

```jsx
// services/PaymentService.ts

const PG_CODE = process.env.PORTONE_PG_CODE || '';

type Product = {
  name: string;
  price: number;
};

type Buyer = {
  name: string;
  email: string;
  phoneNumber: string;
  address: string;
  postalCode: string;
};

type PaymentResponse = {
  success: boolean;
  error_code: string;
  error_msg: string;
  imp_uid: string | null;
  merchant_uid: string;
}

export default class PaymentService {
  instance = Reflect.get(window, 'IMP');

  async requestPayment({ merchantId, product, buyer }: {
    merchantId: string;
    product: Product;
    buyer?: Buyer;
  }): Promise<{
    merchantId: string;
    transactionId: string;
  }> {
    return new Promise((resolve, reject) => {
      this.instance.request_pay({
        pg: PG_CODE,
        pay_method: 'card',
        merchant_uid: merchantId,
        name: product.name,
        amount: product.price,
        buyer_email: buyer?.email,
        buyer_name: buyer?.name,
        buyer_tel: buyer?.phoneNumber,
        buyer_addr: buyer?.address,
        buyer_postcode: buyer?.postalCode,
      }, (response: PaymentResponse) => {
        if (response.success) {
          resolve({
            merchantId: response.merchant_uid,
            transactionId: response.imp_uid ?? '',
          });
        } else {
          reject(Error(response.error_msg));
        }
      });
    });
  }
}

export const paymentService = new PaymentService();
```

```jsx
// hooks/usePayment.ts
export default function usePayment(cart: Cart) {
  return {
    async requestPayment() {
      const now = new Date();
      const date = now.toISOString().slice(0, 10).replace(/-/g, '');
      const time = now.getTime();
      const nonce = Math.random().toString().slice(-5);
      const merchantId = `ORDER-${date}-${time}${nonce}`;

      const result = await paymentService.requestPayment({
        merchantId,
        product: {
          name: cart.lineItems[0].product.name,
          price: cart.totalPrice,
        },
      });

      return result;
    },
  };
}
```

```jsx
// components/PaymentButton.tsx

const Container = styled.div`
  p {
    margin-block: 2rem;
    color: ${(props) => props.theme.colors.primary};
    font-size: 2rem;
    font-weight: bold;
  }
`;

type PaymentButtonProps = {
  cart: Cart;
};

export default function PaymentButton({ cart }: PaymentButtonProps) {
  const navigate = useNavigate();

  const [{ valid }] = useOrderFormStore();

  const { requestPayment } = usePayment(cart);

  const [error, setError] = useState('');

  const handleClick = async () => {
    setError('');

    try {
      const { merchantId, transactionId } = await requestPayment();

      // TODO: B/E로 주문 및 결제 정보 전달.

      navigate('/order/complete');
    } catch (e) {
      if (e instanceof Error) {
        setError(e.message);
      }
    }
  };

  return (
    <Container>
      <Button onClick={handleClick} disabled={!valid}>
        결제
      </Button>
      {error && (
        <p>{error}</p>
      )}
    </Container>
  );
}
```
