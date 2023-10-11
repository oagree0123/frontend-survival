# usehooks-ts

```bash
npm i usehooks-ts
```

## usehooks 소개

### [useBoolean](https://usehooks-ts.com/react-hook/use-boolean)

- 참/거짓을 다룰 땐 toggle 같이 의도가 명확한 함수를 사용

### [useEffectOnce](https://usehooks-ts.com/react-hook/use-effect-once)

의존성 배열을 빈 배열로 넣어서 한 번만 실행하는 Effect를 잡아줄 때가 많은데, 이걸 쓰면 더 명확히 드러난다.

→ 위에서 만든 useFetchProducts에 써보자.

### [useFetch](https://usehooks-ts.com/react-hook/use-fetch)

정말 간단히 쓸 때 좋음.

몇 가지 기능이 살짝 더 있는 useFetch 라이브러리가 따로 있다.

- [use-http](https://use-http.com/)

조금 더 복잡해도 괜찮다면, 캐시 이슈를 고려한 좋은 대안이 있다.

- [SWR](https://swr.vercel.app/ko)
- [TanStack Query](https://tanstack.com/query)

### [useInterval](https://usehooks-ts.com/react-hook/use-interval)

- React에서 setInterval 등을 쓸 때는 주의해야 할 부분이 있어서 Custom Hook을 만들어서 해결해야 함

- [useEffect 관점](https://overreacted.io/ko/a-complete-guide-to-useeffect/)
  - [React에서의 타이머 part 1 : setInterval 말고 이것! - 코드종님 영상](https://youtu.be/2tUdyY5uBSw)
- [Ref 활용](https://overreacted.io/making-setinterval-declarative-with-react-hooks/)

### [useEventListener](https://usehooks-ts.com/react-hook/use-event-listener)

- 모든 종류의 이벤트를 확인할 수 있음
  - 특히 dispatchEvent로 전달되는 커스텀 이벤트에 반응하기 좋음

### [useLocalStorage](https://usehooks-ts.com/react-hook/use-local-storage)

- localStorage와 JSON으로 객체 영속화.
- 이벤트를 통해(dispatchEvent + useEventListener) 다른 컴포넌트와 동기화하는 게 매우 중요한 특징.

### [useDarkMode](https://usehooks-ts.com/react-hook/use-dark-mode)

### useFetch 대한

#### SWR (Stale-While-Revalidate)

- 데이터를 가져올 때 사용자에게 최신 데이터를 제공하는 방식
- SWR은 데이터를 가져온 후 바로 캐시에 저장하며, 캐시된 데이터를 반환
  - 동시에 백그라운드에서 데이터를 다시 가져와 새로운 데이터로 업데이트

```tsx
import useSWR from 'swr';

function MyComponent() {
  // SWR 훅을 사용하여 데이터 가져오기
  const { data, error } = useSWR('/api/data', async (url) => {
    const response = await fetch(url);
    const data = await response.json();
    return data;
  });

  if (error) return <div>Error: {error.message}</div>;
  if (!data) return <div>Loading...</div>;

  return (
    <div>
      <h1>Data:</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
}

export default MyComponent;
```

#### tanstack-query

- React Query는 데이터 페칭을 관리하기 위한 라이브러리
- 서버 데이터와 로컬 상태를 동기화하는 기능을 제공
- 데이터를 캐싱하고, 캐시된 데이터를 자동으로 업데이트

```tsx
import React from 'react';
import { useQuery } from 'react-query';

async function fetchData() {
  const response = await fetch('/api/data');
  if (!response.ok) {
    throw new Error('Network response was not ok');
  }
  return response.json();
}

function MyComponent() {
  const { data, error, isLoading } = useQuery('data', fetchData);

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <div>
      <h1>Data:</h1>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
}

export default MyComponent;
```
