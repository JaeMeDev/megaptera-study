# ๐ 5. Parcel & ESLint

{% hint style="info" %}

- [Parcel ๊ณต์๋ฌธ์](https://parceljs.org/)
- [ESLint ๊ณต์๋ฌธ์](https://eslint.org/)

{% endhint %}

## ๐ย 5-1. Parcel

- **Zero Configuration**
- ํน๋ณํ ์ค์  ์์ด ๋ฐ๋ก ์ฌ์ฉ ๊ฐ๋ฅํ ๋น๋ ํด. ๋ด๋ถ์ ์ผ๋ก `SWC` ๋ฅผ ์ฌ์ฉํด ๊ธฐ์กด ๋๊ตฌ๋ณด๋ค ๋น ๋ฆ(ES Module์ ์ ๊ทน ํ์ฉํ๋ Vite๋ ์์ฒญ๋๊ฒ ๋น ๋ฆ)
- ๋ฒ๋ค๋ง(์ฌ๋ฌ๊ฐ์ ์๋ฐ์คํฌ๋ฆฝํธ์ ํ์ผ์ ํ๊ฐ์ ํ์ผ๋ก), ์ต์ ๋ฒ์ ์ ์๋ฐ์คํฌ๋ฆฝํธ์ ๊ธฐ๋ฅ์ ์๋  ๋ฒ์ ์ ๊ธฐ๋ฅ์ผ๋ก ๋ณํํด์ฃผ๋ ๊ธฐ๋ฅ(bebel ๊ฐ์)์ ์ญํ ์ ๋ชจ๋ ํ๋ ๋น๋๋๊ตฌ.
- [vite](https://github.com/ahastudio/til/tree/main/vite)
- ์ค์ ์ด ํ์ ์๋ค๊ณ  ํ์ง๋ง, ๋ ๊ฐ์ง ์์์ ํ๋ ๊ฒ ์ข๋ค.
1. `pacakge.json` ํ์ผ์ source ์์ฑ ์ถ๊ฐ

```json
"source" : "./index.html"
```

2. [parcel-reporter-static-files-copy](https://github.com/elwin013/parcel-reporter-static-files-copy) ํจํค์ง ์ค์น ํ `.parcelrc` ํ์ผ ์์ฑ.(์ด๋ ๊ฒ ํ๋ฉด static ํด๋์ ํ์ผ์ ์ ์  ํ์ผ๋ก Servingํ  ์ ์๋ค. โ ์ด๋ฏธ์ง ๋ฑ Assets)

```json
{
	"extends": ["@parcel/config-default"],
	"reporters": ["...", "parcel-reporter-static-files-copy"]
}
```

```tsx
const TestComponent = () => {
	// static ํด๋์ /image/test.jpg ํ์ผ์ ๊ฐ์ ธ์จ๋ค
	return <img src="/image/test.jpg"/>
}
```

- ๋น๋ + ์ ์  ์๋ฒ ์คํ

```bash
npx parcel build
npx servor ./dist
```

- [servor](https://github.com/lukejacksonn/servor) : zero configuration server

## ๐ย 5-2. ESLint

- ์ฐธ๊ณ ๋ฌธ์(์ฉ์ด์ ๋ํด์)
    - [๋ฆฐํธ](https://ko.wikipedia.org/wiki/%EB%A6%B0%ED%8A%B8_(%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4))
    - [์ ์  ํ๋ก๊ทธ๋จ ๋ถ์](https://ko.wikipedia.org/wiki/%EC%A0%95%EC%A0%81_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8_%EB%B6%84%EC%84%9D)
    - [Coding_conventions](https://en.wikipedia.org/wiki/Coding_conventions)
- ๋ฌด์์ ์ํด ์ฌ์ฉํ ๊น?
    - ์คํ์ผ ํต์ผ
    - ์ ์ฌ์  ๋ฌธ์  ๋ฐ๊ฒฌ
    - ๋ฒ ์คํธ ํ๋ํฐ์ค ์ถ์ฒ โ ์ต์  ํธ๋ ๋๋ฅผ ํ์ตํ๋๋ฐ ํ์ฉํ  ์ ์๋ค.
- [VSCode ESLint extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
    - `.vscode/setting.json` ํ์ผ์ ์ถ๊ฐํด JS, TSํ์ผ ์ ์ฅ ์ ESLint๋ฅผ ์คํํ๊ณ  ๋ฌธ์ ์ ์ ๊ณ ์น๊ฒ ์ค์ ํ  ์ ์๋ค.

```json
{
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    }
}
```

- ๋ด๊ฐ ์ฌ์ฉํ๋ Webstorm์์๋?
    - Webstorm์์ ESLint ์ค์ ์ ๋ค์ด๊ฐ๋ฉด onSave ์ต์์ด ์๋ค. ํ์ฑํํ๋ฉด ๋จ.
- ์์ฌ๋์ด ์ฐ์๋ VS Code ๊ธฐ๋ณธ ์ธํ(์ฐธ๊ณ )
    - [VS Code ๊ธฐ๋ณธ ์ธํ](https://github.com/ahastudio/CodingLife/blob/main/20211008/react/.vscode/settings.json)
    - [Trailing Spaces](https://marketplace.visualstudio.com/items?itemName=shardulm94.trailing-spaces)