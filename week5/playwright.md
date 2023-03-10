# ๐ 4. Playwright

{% hint style="info" %}
๐ก Playwright๋ ์น ๋ธ๋ผ์ฐ์  ๊ธฐ๋ฐ E2E ํ์คํธ ์๋ํ ๋๊ตฌ์ด๋ค. Headless Chrome์ ๊ธฐ๋ฐ์ผ๋ก ํ Puppeteer๋ฅผ ๊ณ์นํ๋ฉด์, ๋ ๋ง์ ์น ๋ธ๋ผ์ฐ์ ๋ฅผ ์ง์ํ๋ค.

- [Playwright](https://playwright.dev/)
- [Playwright Configuration](https://playwright.dev/docs/test-configuration)
- [Ashal์ Playwright](https://github.com/ahastudio/til/blob/main/test/playwright.md)

{% endhint %}

## ๐ย 4-1. Playwright Setting

- Playwright ํจํค์ง๋ฅผ ์ค์นํ์.

```bash
npm i -D @playwright/test eslint-plugin-playwright
```

- `playwright.config.ts` ํ์ผ ์์ฑ

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

- `tests/.eslintrc.js` ํ์ผ ์์ฑ

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

## ๐ย 4-2. E2E ์งํํด๋ณด๊ธฐ

- `test/home.spec.ts` ํ์ผ ์์ฑ

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

test('Click the โIncreaseโ button', async ({ page }) => {
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

- ํ์คํธ ์คํ

```bash
npx playwright test
```

```bash
CI=true npx playwright test
```

- `.gitignore` ํ์ผ์ ์๋ฌ ์ํฉ์ ์คํฌ๋ฆฐ์ท ๋ฑ์ด ๋ด๊ธฐ๋ `test-result` ๋๋ ํฐ๋ฆฌ๋ฅผ ์ถ๊ฐํ์.

```
/test-results/
```

- ๋ค๋ฅธ ๋๊ตฌ๋ก ์ธ๊ฐ ์นํ์ ์ธ E2E ํ์คํ ๋๊ตฌ CodeceptJS๊ฐ ์๋ค.
    - [CodeceptJS](https://codecept.io/)
    - [CodeceptJS 3 ์์ํ๊ธฐ](https://github.com/ahastudio/til/blob/main/test/20201207-codeceptjs.md)
    - [CodeceptJS ์ฌ์ฉํ๊ธฐ](https://github.com/ahastudio/CodingLife/tree/main/20211012/react#codeceptjs-%EC%82%AC%EC%9A%A9)