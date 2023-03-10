# ๐ 1. ๊ฐ๋ฐ ํ๊ฒฝ

{% hint style="info" %}
ํ๋ก ํธ์๋์์ ๋์๊ฐ๋ React๋ฅผ ํ๋๋ผ๋ Node.js๋ฅผ ์ฌ์ฉํ๊ธฐ ๋๋ฌธ์ ์ธํํ  ์ค ์์์ผ ํ๋ค. ์ด ์ธํ์ ๊ต์ฅํ ๊น๋ค๋กญ๊ณ  ์ด๋ ค์ด ๋ถ๋ถ์ด๋ฉฐ, `Deno` ๋ฅผ ์ฌ์ฉํ๋ค๋ฉด ํจ์ฌ ๊ฐ๋จํ๊ฒ ์ง๋ง, ๋๋ถ๋ถ์ ์๋ฌด ํ๊ฒฝ์ด Node.js๋ฅผ ๊ธฐ๋ฐ์ผ๋ก ํ๊ธฐ ๋๋ฌธ์ ์ฌ๊ธฐ์๋ Node.js๋ฅผ ์ฌ์ฉํ ์ธํ์ ์งํํ๋ค.
{% endhint %}

## ๐โย 1-1. Node.js ์ธํ์ ์ ์ด๋ ค์ธ๊น?

- ๊ณ์ ๋ฐ๋๋ค. (๋๊ตฌ๋ค์ด ๊ณ์ ์๋ก ๋์ด)
- ๋๊ตฌ๋ค์ด ์๋ก ๋์ค๊ธฐ ๋๋ฌธ์ ์ฐ๋ฆฌ๋ **์ ์ฒด์ ์ธ ํ๋ฆ์ ํ์ํ๊ณ  ์์ผ๋ก์ ๋ณ๊ฒฝ์ ๋์ํ  ์ ์๋ ๋ฅ๋ ฅ์ ํค์ฐ๋ ๊ฒ**์ด ์ค์ํ๋ค. (์๋ก์ด ๊ฒ์ด ๋์ฌ๋ ์ฉ๋์ ์ฅ๋จ์ ์ ๊ตฌ๋ถํ์ฌ ๋์ํ  ์ ์๋ ๋ฅ๋ ฅ)

## ๐ย 1-2. JavaScript ๊ฐ๋ฐ ํ๊ฒฝ(Node.js) ์ธํ

### โย  1-2-1. Node.js ์ต์ ๋ฒ์  ํ์ธ

- [node.js](https://nodejs.org/en/)
- **LTS** ๋ฒ์ ๊ณผ **Current** ๋ฒ์ ์ด ์๋ค. Current ๋ฒ์ ์ ํ์ฌ ๊ฐ๋ฐํ๊ณ  ์๋ ์ต์ ๋ฒ์ ์ด์ง๋ง ์์ ์ ์ผ๋ก ์ฌ์ฉํ  ์ ์๋ ๊ฒ์ ์๋๋ค. LTS ๋ฒ์ ์ Long Term Support๋ก ์ฅ๊ธฐ์ ์ผ๋ก ์ํฌํธํด์ฃผ๊ธฐ ๋๋ฌธ์ ์์ ์ ์ด๋ค.
- LTS ๋ฒ์ ์ ์ง์์ด๋ฉฐ, Current ๋ฒ์ ์ ํ์์ด๋ค.

### โย 1-2-2. fnm (Fast Node Manager) ์ค์น - MAC

- [fnm](https://github.com/Schniz/fnm)
- `fnm` ์ ์ต์  ๋ฒ์ ์ ๊ด๋ฆฌ์์ด๋ฉฐ `nvm` ๋ณด๋ค ํจ์ฌ ๋น ๋ฅด๋ค.
1. `Homebrew` ๋ฅผ ์ด์ฉํด `fnm` ์ ์ค์น

```bash
brew install fnm
```

2. `fnm` ํ๊ฒฝ ์ค์ 

```bash
eval "$(fnm env --use-on-cd)"
```

3. `fnm` ์ ํตํด LTS ๋ฒ์ ์ ์ค์นํ๊ณ  ๊ธฐ๋ณธ ๋ฒ์ ์ผ๋ก ์ง์ 

```bash
fnm install --lts
```

4. Node.js ๋ฒ์  ํ์ธ

```bash
# ํ์ฌ LTS ๋ฒ์  18.13.0
node --version
```

## ๐ย 1-3. TS + React + Jest + ESLint + Parcel ๊ฐ๋ฐ ํ๊ฒฝ ์ธํ

1. ์ ์ ํ ์์ ํด๋๋ฅผ ์ค๋น

```bash
mkdir my-app
cd my-app
```

2. ํด๋น ์์น์์ `VSCode` ๋๋ `Webstorm` ์ ์ด๊ธฐ(๊ฐ์ธ์ ์ผ๋ก Webstorm์ ์ฌ์ฉํ๊ธฐ ๋๋ฌธ์ Webstorm์ผ๋ก ์งํ)

```bash
webstorm . 
```

3. npm ํจํค์ง๋ฅผ ์ค๋น

```bash
npm init -y
```

4. `.gitignore` ํ์ผ ์์ฑ(์ต์ํ `node_modules` ๋ฅผ ์ปค๋ฐํ๋ ๊ฒ์ ๋ฐฉ์งํ์)
    - [gitignore.io](https://www.toptal.com/developers/gitignore) ์์ ์ฝ๊ฒ `.gitignore` ๋ฅผ ๋ง๋ค์ด ์ค ์๋ ์๋ค.
    - [github gitignore](https://github.com/github/gitignore) ์์๋ ์ธ์ด๋ณ `.gitignore` ๋ฅผ ํ์ธํ  ์ ์์ผ๋ฉฐ, ์ด๋ ์๋ก์ด ๋ ํผ์งํ ๋ฆฌ ์์ฑ ์ ์ง์  ๊ฐ๋ฅํ๋ค.

5. ํ์์คํฌ๋ฆฝํธ ์ค์ 

```bash
# -D ์ต์์ ํตํด devDependencies(๊ฐ๋ฐํ๊ฒฝ) ์ค์น
npm i -D typesciprt

# tsc๋ ํ์์คํฌ๋ฆฝํธ ์ปดํ์ผ๋ฌ
# npx๋ /node_modules/.bin ์์ ์๋ ๊ฒ์ ์คํํ๋ ๊ฒ
# npx์ ์ฅ์ ์ ํด๋น ํ๋ก์ ํธ์ ์ค์น๋์ง ์์ ํจํค์ง๋ ์คํ ๊ฐ๋ฅํ๋ค.
# tsc ์ด๊ธฐํ
npx tsc --init
```

6. `tsconfig.json` ํ์ผ์ jsx ์์ฑ์ ๋ณ๊ฒฝ

```json
{
  "jsx": "react-jsx"
}
```

7. ESLint ์ค์ 

```bash
npm i -D eslint

npx eslint --init
# โ How would you like to use ESLint? ยท style
# โ What type of modules does your project use? ยท esm
# โ Which framework does your project use? ยท react
# โ Does your project use TypeScript? ยท No / Yes
# โ Where does your code run? ยท browser
# โ How would you like to define a style for your project? ยท guide
# โ Which style guide do you want to follow? ยท xo-typescript
# โ What format do you want your config file to be in? ยท JavaScript
# โ Would you like to install them now? ยท No / Yes
# โ Which package manager do you want to use? ยท npm
```

8. `.eslint.js` ํ์ผ์ ์ ์ ํ ์์ (์์ง Jest๋ฅผ ์ค์นํ์ง ์์์ง๋ง ๋ฏธ๋ฆฌ `jest:true` ๋ฅผ ์ก์์ฃผ๋ฉด ์ข๋ค.)

```jsx
module.exports = {
   env: {
      browser: true,
      es2021: true,
      // ์ถ๊ฐ
      jest: true,
   },
   extends: [
      'plugin:react/recommended',
      // ์ถ๊ฐ
      'plugin:react/jsx-runtime',
      'xo',
   ],
   settings: {
      react: {
         version: "detect"
      }
   },
}
```

9. `.eslintignore` ํ์ผ์ ์์ฑ(eslint ์คํํ  ๋ ์ฌ๊ธฐ์์๋ ์ ์ธ ์ค์ )

```
/node_modules/
/dist/
/.parcel-cache/
```

10. React ์ค์น

```bash
npm i react react-dom

npm i -D @types/react @types/react-dom
```

11. ํ์คํ ๋๊ตฌ ์ค์น(jest + swc)

```bash
npm i -D jest @types/jest @swc/core @swc/jest \
    jest-environment-jsdom \
    @testing-library/react @testing-library/jest-dom
```

12. `jest.config.js` ํ์ผ์ ์์ฑํด ํ์คํธ์์ SWC๋ฅผ ์ฌ์ฉํ์.

```jsx
module.exports = {
    testEnvironment: 'jsdom',
    setupFilesAfterEnv: [
        '@testing-library/jest-dom/extend-expect',
    ],
    transform: {
        '^.+\\.(t|j)sx?$': ['@swc/jest', {
            jsc: {
                parser: {
                    syntax: 'typescript',
                    jsx: true,
                    decorators: true,
                },
                transform: {
                    react: {
                        runtime: 'automatic',
                    },
                },
            },
        }],
    },
    testPathIgnorePatterns: [
        '<rootDir>/node_modules/',
        '<rootDir>/dist/',
    ],
};
```

13. Parcel ์ค์น

```bash
npm i -D parcel
```

14. `pacakge.json` ํ์ผ์ scripts, source ์์ 

```json
{
  "source" : "./index.html",
  "scripts": {
    "start": "parcel --port 8080",
    "build": "parcel build",
    "check": "tsc --noEmit",
    "lint": "eslint --fix --ext .js,.jsx,.ts,.tsx .",
    "test": "jest",
    "coverage": "jest --coverage --coverage-reporters html",
    "watch:test": "jest --watchAll"
  }
}
```

15. ๊ธฐ๋ณธ ์ฝ๋ ์์ฑ
- `index.html`

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>React Demo App</title>
</head>
<body>
<div id="root"></div>
<p>Hello. world!</p>
<script type="module" src="./src/main.tsx"></script>
</body>
</html>
```

- `src/main.tsx`

```jsx
import ReactDOM from 'react-dom/client';

function App() {
    return (
        <p>Hello, world!</p>
    );
}

const element = document.getElementById('root');

if (element) {
    const root = ReactDOM.createRoot(element);

    root.render(<App/>);
}
```

- `src/App.tsx`
- `src/App.test.tsx`
- `src/App.test.tsx`
- `src/components/Greeting.test.tsx`
- `src/components/Greeting.tsx`