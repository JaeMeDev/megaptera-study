# 🌈 4. Playwright

{% hint style="info" %}
💡 Playwright는 웹 브라우저 기반 E2E 테스트 자동화 도구이다. Headless Chrome을 기반으로 한 Puppeteer를 계승하면서, 더 많은 웹 브라우저를 지원한다.

- [Playwright](https://playwright.dev/)
- [Playwright Configuration](https://playwright.dev/docs/test-configuration)
- [Ashal의 Playwright](https://github.com/ahastudio/til/blob/main/test/playwright.md)

{% endhint %}

## 🚘 4-1. Playwright Setting

- Playwright 패키지를 설치하자.

```bash
npm i -D @playwright/test eslint-plugin-playwright
```

- `playwright.config.ts` 파일 작성

```tsx
import { PlaywrightTestConfig } from '@playwright/test';

const config: PlaywrightTestConfig = {
	testDir: './tests',
	retries: 0,
	use: {
		baseURL: 'http://localhost:8080',
		headless: !!process.env.CI,
		screenshot: 'only-on-failure',
		// channel: 'chrome'
	},
};

export default config;
```

- `tests/.eslintrc.js` 파일 작성

```tsx
module.exports = {
	env: {
		jest: false,
	},
	extends: ['plugin:playwright/playwright-test'],
	rules: {
		'import/no-extraneous-dependencies': 'off',
	},
};
```

## 🚘 4-2. E2E 진행해보기

- `test/home.spec.ts` 파일 작성

```tsx
import { test, expect } from '@playwright/test';

test('Show all products', async ({ page }) => {
	await page.goto('/');

	await expect(page.getByText('Apple')).toBeVisible();
	await expect(page.getByText('Grape')).toBeHidden();
});

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

- 테스트 실행

```bash
npx playwright test
```

```bash
CI=true npx playwright test
```

- `.gitignore` 파일에 에러 상황의 스크린샷 등이 담기는 `test-result` 디렉터리를 추가하자.

```
/test-results/
```

- 다른 도구로 인간 친화적인 E2E 테스팅 도구 CodeceptJS가 있다.
    - [CodeceptJS](https://codecept.io/)
    - [CodeceptJS 3 시작하기](https://github.com/ahastudio/til/blob/main/test/20201207-codeceptjs.md)
    - [CodeceptJS 사용하기](https://github.com/ahastudio/CodingLife/tree/main/20211012/react#codeceptjs-%EC%82%AC%EC%9A%A9)