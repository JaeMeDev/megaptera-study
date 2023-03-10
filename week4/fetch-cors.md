# ๐ 2. Fetch API & CORS

{% hint style="info" %}
์ฐธ๊ณ ๋ฌธ์

- [Fetch API](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API)
- [Fetch ์ฌ์ฉํ๊ธฐ](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API/Using_Fetch)
- [ReadableStream](https://developer.mozilla.org/ko/docs/Web/API/ReadableStream)
- [ํ์คํธ ๋์ฝ๋์ ํ์คํธ ์ธ์ฝ๋](https://ko.javascript.info/text-decoder)

{% endhint %}

## ๐ย 2-1. ๊ธฐ๋ณธ์ ์ธ ์ฌ์ฉ๋ฐฉ๋ฒ

```tsx
fetch('http://localhost:3000/products');
// โ Promise

await fetch('http://localhost:3000/products');
// โ Response

const response = await fetch('http://localhost:3000/products');
// โ response.body๋ ReadableStream

const reader = response.body.getReader();

const chunk = await reader.read();
// โ chunk.value๋ Uint8Array ํ์.
// โ ์๋๋ chunk.done์ด true์ผ ๋๊น์ง ๋ฐ๋ณตํด์ผ ํ๋ค.

const body = new TextDecoder().decode(chunk.value);

const data = JSON.parse(body);
```

- fetch API๋ JSON์ ๊ธฐ๋ณธ ์ง์ํ๋ค.

```tsx
const response = await fetch('http://localhost:3000/products');
const data = await response.json();
```

- ๋ค๋ฅธ HTTP Method ์ฌ์ฉ๋ฒ

```tsx
const response = fetch(url, {
	method: 'POST',
});
```

## ๐ย 2-2. CORS

- ์ฐธ๊ณ  ๋ฌธ์
    - [๋์ผ ์ถ์ฒ ์ ์ฑ](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy)
    - [CORS](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)
- ์น ๋ธ๋ผ์ฐ์ ๊ฐ ๊ฐ์ง๊ณ  ์๋ ๊ธฐ๋ณธ ์ ์ฑ
- ์น ๋ธ๋ผ์ฐ์ ๋ Same Origin Policy์ ๋ฐ๋ผ ์น ํ์ด์ง์ ๋ฆฌ์์ค๋ฅผ ์์ฒญํ ๊ณณ(REST API ์๋ฒ)์ด ์๋ก ๋ค๋ฅธ ์ถ์ฒ(ํฌํธ๊น์ง ํฌํจ)์ผ ๋ ์๋ฒ์์ ์ป์ ๊ฒฐ๊ณผ๋ฅผ ์ฌ์ฉํ  ์ ์๊ฒ ๋ง๋๋ค.
- ์๋ฒ์ ์์ฒญํ๊ณ  ์๋ต์ ๋ฐ์์ค๋ ๊ฒ ๊น์ง๋ ์ด๋ฏธ ์งํ์ด ๋ค ๋ ์ํฉ์ด๋ ์ ์ ์ฃผ์ํด์ผ ํ๋ค.
- REST API ์๋ฒ์์ Headers์ `Access-Control-Allow-Origin` ์์ฑ์ ์ถ๊ฐํ๋ฉด ๋๋ค.
- Express์์  ๊ทธ๋ฅ [CORS ๋ฏธ๋ค์จ์ด](https://expressjs.com/en/resources/middleware/cors.html)๋ฅผ ์ค์นํด์ ์ฌ์ฉํ๋ฉด ๋๋ค.
- cors ํจํค์ง ์ค์น

```bash
npm i cors
npm i -D @types/cors
```

- cors ๋ฏธ๋ค์จ์ด ์ฌ์ฉํ๊ธฐ

```tsx
import express from 'express';
import cors from 'cors';

const app = express();

app.use(cors());
```