# 🌈 5. Parcel & ESLint

{% hint style="info" %}

- [Parcel 공식문서](https://parceljs.org/)
- [ESLint 공식문서](https://eslint.org/)

{% endhint %}

## 🚘 5-1. Parcel

- **Zero Configuration**
- 특별한 설정 없이 바로 사용 가능한 빌드 툴. 내부적으로 `SWC` 를 사용해 기존 도구보다 빠름(ES Module을 적극 활용하는 Vite도 엄청나게 빠름)
- 번들링(여러개의 자바스크립트의 파일을 한개의 파일로), 최신버전의 자바스크립트의 기능을 옛날 버전의 기능으로 변환해주는 기능(bebel 같은)의 역할을 모두 하는 빌드도구.
- [vite](https://github.com/ahastudio/til/tree/main/vite)
- 설정이 필요 없다고 했지만, 두 가지 작업은 하는 게 좋다.
1. `pacakge.json` 파일에 source 속성 추가

```json
"source" : "./index.html"
```

1. [parcel-reporter-static-files-copy](https://github.com/elwin013/parcel-reporter-static-files-copy) 패키지 설치 후 `.parcelrc` 파일 작성.(이렇게 하면 static 폴더의 파일을 정적 파일로 Serving할 수 있다. → 이미지 등 Assets)

```json
{
	"extends": ["@parcel/config-default"],
	"reporters": ["...", "parcel-reporter-static-files-copy"]
}
```

```tsx
const TestComponent = () => {
	// static 폴더에 /image/test.jpg 파일을 가져온다
	return <img src="/image/test.jpg"/>
}
```

- 빌드 + 정적 서버 실행

```bash
npx parcel build
npx servor ./dist
```

- [servor](https://github.com/lukejacksonn/servor) : zero configuration server

## 🚘 5-2. ESLint

- 참고문서(용어에 대해서)
    - [린트](https://ko.wikipedia.org/wiki/%EB%A6%B0%ED%8A%B8_(%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4))
    - [정적 프로그램 분석](https://ko.wikipedia.org/wiki/%EC%A0%95%EC%A0%81_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%A8_%EB%B6%84%EC%84%9D)
    - [Coding_conventions](https://en.wikipedia.org/wiki/Coding_conventions)
- 무엇을 위해 사용할까?
    - 스타일 통일
    - 잠재적 문제 발견
    - 베스트 프랙티스 추천 → 최신 트렌드를 학습하는데 활용할 수 있다.
- [VSCode ESLint extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
    - `.vscode/setting.json` 파일을 추가해 JS, TS파일 저장 시 ESLint를 실행하고 문제점을 고치게 설정할 수 있다.

```json
{
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    }
}
```

- 내가 사용하는 Webstorm에서는?
    - Webstorm에서 ESLint 설정에 들어가면 onSave 옵션이 있다. 활성화하면 됨.
- 아샬님이 쓰시는 VS Code 기본 세팅(참고)
    - [VS Code 기본 세팅](https://github.com/ahastudio/CodingLife/blob/main/20211008/react/.vscode/settings.json)
    - [Trailing Spaces](https://marketplace.visualstudio.com/items?itemName=shardulm94.trailing-spaces)