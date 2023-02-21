# 🌈 2. Fetch API & CORS

{% hint style="info" %}
참고문서

- [Fetch API](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API)
- [Fetch 사용하기](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API/Using_Fetch)
- [ReadableStream](https://developer.mozilla.org/ko/docs/Web/API/ReadableStream)
- [텍스트 디코더와 텍스트 인코더](https://ko.javascript.info/text-decoder)

{% endhint %}

## 🚘 2-1. 기본적인 사용방법

```tsx
fetch('http://localhost:3000/products');
// → Promise

await fetch('http://localhost:3000/products');
// → Response

const response = await fetch('http://localhost:3000/products');
// → response.body는 ReadableStream

const reader = response.body.getReader();

const chunk = await reader.read();
// → chunk.value는 Uint8Array 타입.
// → 원래는 chunk.done이 true일 때까지 반복해야 한다.

const body = new TextDecoder().decode(chunk.value);

const data = JSON.parse(body);
```

- fetch API는 JSON을 기본 지원한다.

```tsx
const response = await fetch('http://localhost:3000/products');
const data = await response.json();
```

- 다른 HTTP Method 사용법

```tsx
const response = fetch(url, {
	method: 'POST',
});
```

## 🚘 2-2. CORS

- 참고 문서
    - [동일 출처 정책](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy)
    - [CORS](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)
- 웹 브라우저가 가지고 있는 기본 정책
- 웹 브라우저는 Same Origin Policy에 따라 웹 페이지와 리소스를 요청한 곳(REST API 서버)이 서로 다른 출처(포트까지 포함)일 때 서버에서 얻은 결과를 사용할 수 없게 막는다.
- 서버에 요청하고 응답을 받아오는 것 까지는 이미 진행이 다 된 상황이란 점에 주의해야 한다.
- REST API 서버에서 Headers에 `Access-Control-Allow-Origin` 속성을 추가하면 된다.
- Express에선 그냥 [CORS 미들웨어](https://expressjs.com/en/resources/middleware/cors.html)를 설치해서 사용하면 된다.
- cors 패키지 설치

```bash
npm i cors
npm i -D @types/cors
```

- cors 미들웨어 사용하기

```tsx
import express from 'express';
import cors from 'cors';

const app = express();

app.use(cors());
```