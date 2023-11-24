# 로그인

## API 호출시 Access Token

```jsx
private accessToken = '';

setAccessToken(accessToken: string) {
  if (accessToken === this.accessToken) {
    return;
  }

  const authorization = accessToken ? `Bearer ${accessToken}` : undefined;

  this.instance = axios.create({
    baseURL: API_BASE_URL,
    headers: { Authorization: authorization },
  });

  this.accessToken = accessToken;
}
```
