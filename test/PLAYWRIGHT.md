# Playwright

- 웹 브라우저 기반 E2E 테스트 자동화 도구
- Headless Chrome을 기반으로 한 Puppeteer를 계승하면서, 더 많은 웹 브라우저를 지원

### Headless Chrome

- Google Chrome 브라우저의 무화면(Headless) 모드를 가리키는 용어
- GUI 없이 명령줄에서 실행되며 웹 페이지를 렌더링하고 조작할 수 있음
- 웹 테스트 자동화, 웹 스크레이핑, 웹 페이지 캡처 등 같은 작업에 적합

### Puppeteer

- Node.js 기반의 오픈 소스 라이브러리로
- Headless Chrome 또는 Chromium 브라우저를 사용하여 웹 페이지를 스크레이핑하거나 자동화 테스트를 수행하는 데 사용되는 강력한 도구

## E2E (End to End) Test

- e2e

### Playwright 사용하기

```bash
npm i -D @playwright/test eslint-plugin-playwright
```

```typescript
// playwright.config.js
import { PlaywrightTestConfig } from '@playwright/test';

const config: PlaywrightTestConfig = {
  testDir: './tests',
  retries: 0,
  use: {
    baseURL: 'http://localhost:8080',
    headless: !!process.env.CI,
    screenshot: 'only-on-failure',
  },
};

export default config;
```

```jsx
// tests/.eslintrc.js
import { PlaywrightTestConfig } from '@playwright/test';

const config: PlaywrightTestConfig = {
  testDir: './tests',
  retries: 0,
  use: {
    baseURL: 'http://localhost:8080',
    headless: !!process.env.CI,
    screenshot: 'only-on-failure',
  },
};

export default config;
```

```tsx
// tests/home.spec.ts
import { test, expect } from '@playwright/test';

test('Filter products', async ({ page }) => {
  await page.goto('/');

  await expect(page.getByText('Apple')).toBeVisible();

  const searchInput = page.getByLabel('Search');

  await searchInput.fill('a');

  await expect(page.getByText('Apple')).toBeVisible();

  await searchInput.fill('aa');

  await expect(page.getByText('Apple')).toBeHidden();
});

test('Click the “Increase” button', async ({ page }) => {
  await page.goto('/');

  const count = 13;

  await Promise.all((
    [...Array(count)].map(async () => {
      await page.getByText('Increase').click();
    })
  ));

  await expect(page.getByText(`${count}`)).toBeVisible();
});
```

테스트 실행

```bash
npx playwright test
```

.gitignroe

```bash
/test-results/
```

### CodeceptJS

- 자동화 테스트를 쉽게할 수 있도록 도와주는 자동화 테스트 프레임워크
- 인간 친화적인 E2E 테스팅 도구

- [CodeceptJS](https://codecept.io/)
- [CodeceptJS 3 시작하기](https://github.com/ahastudio/til/blob/main/test/20201207-codeceptjs.md)
- [CodeceptJS 사용](https://github.com/ahastudio/CodingLife/tree/main/20211012/react#codeceptjs-사용)

```bash
npx create-codeceptjs .
npx codeceptjs init
```

```jsx
// google_test.ts
Feature('Google');

Scenario('Search “CodeceptJS”', ({ I }) => {
  I.amOnPage('https://www.google.com/ncr');
  I.fillField('[name="q"]', 'CodeceptJS');
  I.click('Google Search');
  I.see('codecept.io');
});
```

```bash
npm run codeceptjs
```
