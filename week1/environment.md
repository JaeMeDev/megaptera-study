# 🌈 1. 개발 환경

{% hint style="info" %}
프론트엔드에서 돌아가는 React를 하더라도 Node.js를 사용하기 때문에 세팅할 줄 알아야 한다. 이 세팅은 굉장히 까다롭고 어려운 부분이며, `Deno` 를 사용한다면 훨씬 간단하겠지만, 대부분의 업무 환경이 Node.js를 기반으로 하기 때문에 여기서도 Node.js를 사용한 세팅을 진행한다.
{% endhint %}

## 🚘‍ 1-1. Node.js 세팅은 왜 어려울까?

- 계속 바뀐다. (도구들이 계속 새로 나옴)
- 도구들이 새로 나오기 때문에 우리는 **전체적인 흐름을 파악하고 앞으로의 변경에 대응할 수 있는 능력을 키우는 것**이 중요하다. (새로운 것이 나올때 용도와 장단점을 구분하여 대응할 수 있는 능력)

## 🚘 1-2. JavaScript 개발 환경(Node.js) 세팅

### ✅  1-2-1. Node.js 최신버전 확인

- [node.js](https://nodejs.org/en/)
- **LTS** 버전과 **Current** 버전이 있다. Current 버전은 현재 개발하고 있는 최신버전이지만 안정적으로 사용할 수 있는 것은 아니다. LTS 버전은 Long Term Support로 장기적으로 서포트해주기 때문에 안정적이다.
- LTS 버전은 짝수이며, Current 버전은 홀수이다.

### ✅ 1-2-2. fnm (Fast Node Manager) 설치 - MAC

- [fnm](https://github.com/Schniz/fnm)
- `fnm` 은 최신 버전의 관리자이며 `nvm` 보다 훨씬 빠르다.
1. `Homebrew` 를 이용해 `fnm` 을 설치

```bash
brew install fnm
```

2. `fnm` 환경 설정

```bash
eval "$(fnm env --use-on-cd)"
```

3. `fnm` 을 통해 LTS 버전을 설치하고 기본 버전으로 지정

```bash
fnm install --lts
```

4. Node.js 버전 확인

```bash
# 현재 LTS 버전 18.13.0
node --version
```

## 🚘 1-3. TS + React + Jest + ESLint + Parcel 개발 환경 세팅

1. 적절한 작업 폴더를 준비

```bash
mkdir my-app
cd my-app
```

2. 해당 위치에서 `VSCode` 또는 `Webstorm` 을 열기(개인적으로 Webstorm을 사용하기 때문에 Webstorm으로 진행)

```bash
webstorm . 
```

3. npm 패키지를 준비

```bash
npm init -y
```

4. `.gitignore` 파일 작성(최소한 `node_modules` 를 커밋하는 것을 방지하자)
    - [gitignore.io](https://www.toptal.com/developers/gitignore) 에서 쉽게 `.gitignore` 를 만들어 줄 수도 있다.
    - [github gitignore](https://github.com/github/gitignore) 에서도 언어별 `.gitignore` 를 확인할 수 있으며, 이는 새로운 레퍼지토리 생성 시 지정 가능하다.

5. 타입스크립트 설정

```bash
# -D 옵션을 통해 devDependencies(개발환경) 설치
npm i -D typesciprt

# tsc는 타입스크립트 컴파일러
# npx는 /node_modules/.bin 안에 있는 것을 실행하는 것
# npx의 장점은 해당 프로젝트에 설치되지 않은 패키지도 실행 가능하다.
# tsc 초기화
npx tsc --init
```

6. `tsconfig.json` 파일의 jsx 속성을 변경

```json
{
  "jsx": "react-jsx"
}
```

7. ESLint 설정

```bash
npm i -D eslint

npx eslint --init
# ✔ How would you like to use ESLint? · style
# ✔ What type of modules does your project use? · esm
# ✔ Which framework does your project use? · react
# ✔ Does your project use TypeScript? · No / Yes
# ✔ Where does your code run? · browser
# ✔ How would you like to define a style for your project? · guide
# ✔ Which style guide do you want to follow? · xo-typescript
# ✔ What format do you want your config file to be in? · JavaScript
# ✔ Would you like to install them now? · No / Yes
# ✔ Which package manager do you want to use? · npm
```

8. `.eslint.js` 파일을 적절히 수정(아직 Jest를 설치하지 않았지만 미리 `jest:true` 를 잡아주면 좋다.)

```jsx
module.exports = {
   env: {
      browser: true,
      es2021: true,
      // 추가
      jest: true,
   },
   extends: [
      'plugin:react/recommended',
      // 추가
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

9. `.eslintignore` 파일을 작성(eslint 실행할 때 여기에서는 제외 설정)

```
/node_modules/
/dist/
/.parcel-cache/
```

10. React 설치

```bash
npm i react react-dom

npm i -D @types/react @types/react-dom
```

11. 테스팅 도구 설치(jest + swc)

```bash
npm i -D jest @types/jest @swc/core @swc/jest \
    jest-environment-jsdom \
    @testing-library/react @testing-library/jest-dom
```

12. `jest.config.js` 파일을 작성해 테스트에서 SWC를 사용하자.

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

13. Parcel 설치

```bash
npm i -D parcel
```

14. `pacakge.json` 파일의 scripts, source 수정

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

15. 기본 코드 작성
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