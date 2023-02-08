# 🌈 1. JSX

{% hint style="info" %}
JSX는 XML같은 문법 확장(ECMAScript)이다. JSX는 React를 만들면서 생긴 부산물 같은 것이지만, JSX는 꼭 React에서만 사용가능한 것은 아니다.

- [facebook의 JSX 소개](https://facebook.github.io/jsx/)
- [React 공식문서의 JSX 소개](https://ko.reactjs.org/docs/introducing-jsx.html)
- [Babel, JSX, 그리고 빌드 과정들](https://ko.reactjs.org/docs/faq-build.html)
- [JSX 이해하기](https://ko.reactjs.org/docs/jsx-in-depth.html)

{% endhint %}

## 🚘 1-1. JSX에 대해서

- JSX는 XML처럼 작성된 부분을 `React.createElement` 를 쓰는 JavaScript 코드로 변환한다. 중괄호를 써서 JavaScript 코드를 그대로 쓸 수 있고, 결국은 JavaScript 코드와 1:1로 매칭된다.
- 변환기 중 제일 유명한 [Bebel](https://babeljs.io/repl)로 확인 가능하다.
    - **Presets**에서 **react**를 체크하거나, **Plugin**에서 `@babel/plugin-transform-react-jsx` 를 추가하면 JSX를 실험할 수 있다.
    - JSX 파일에 `/* @jsx 어쩌고 */` 주석을 추가하면 React.createElement 대신 “어쩌고”를 쓰게 된다.
- Example 1

```jsx
// JSX 코드
<p>Hello, world</p>

// 변환된 JS 코드
React.createElement("p", null, "Hello, world");
```

- Example 2(사용할 일은 거의 **없음**)

```jsx
// JSX 코드
/* @jsx jaejun.createElement */
<p>Hello, world</p>

// 변환된 JS 코드
jaejun.createElement("p", null, "Hello, world");
```

- Example 3

```jsx
// JSX 코드
function Greeting({ name }){
	return (
		<p>Hello, {name}!</p>
	);
}

<Greeting name="world" age={13} />

// 변환된 JS 코드
function Greeting({ name }){
	return React.createElement("p", null, "Hello, ", name, "!");
}

React.createElement(Greeting, { name: "world", age: 13 });
```

- Example 4

```jsx
// JSX 코드
<Button type="submit">Send</Button>

// 변환된 JS 코드
React.createElement(Button, { type: "submit" }, "Send");
```

- Example 5

```jsx
// JSX 코드
<>
	<p>Hello, world!</p>
	<Button type="submit">Send</Button>
</>

// 변환된 JS 코드
React.createElement(
	React.Fragment,
	null,
	React.createElement("p", null, "Hello, world!"),
	React.createElement(Button, { type: "submit" }, "Send")
);
```

- Example 6

```jsx
// JSX 코드
<div>
	<p>Count: {count}!</p>
	<button type="button" onClick={() => setCount(count + 1)}>Increase</button>
</div>

// 변환된 JS 코드
React.createElement(
	"div",
	null,
	React.createElement("p", null, "Count: ", count, "!"),
	React.createElement("button", { type: "button", onClick: () => setCount(count + 1) }, "Increase")
);
```

## 🚘 1-2. React Element

- 참고문서
    - [JSX 없이 사용하는 React](https://ko.reactjs.org/docs/react-without-jsx.html)
    - [createElement](https://beta.reactjs.org/reference/react/createElement)
- JSX대신 그냥 `React.createElement` 를 써서 React Element 트리를 갱신하는데 쓸 수 있다.
- JSX로 할 수 있는 모든 것은 순수 JavaScript로도 할 수 있다.
- JSX Runtime은 `_jsx` 란 함수를, Preact는 `h` 란 함수를 직접 지원함.
    - [_jsx](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html)
    - [h()](https://preactjs.com/guide/v10/api-reference/#h--createelement)

## 🚘 1-3. VDOM(Virtual DOM)

- 참고문서
    - [VDOM (Virtual DOM)](https://ko.reactjs.org/docs/faq-internals.html)
    - [재조정 (Reconciliation)](https://ko.reactjs.org/docs/reconciliation.html)
- 트리는 프랙탈과 같다. 트리의 구성요소는 트리이다. 우리는 매번 작은 React Element 트리, VDOM 트리를 만든다. **VDOM은 실제 DOM과 비교를 통해 변경사항을 적용한다.**

## 🚘 1-4. React Developer Tools

- [문서](https://github.com/facebook/react/tree/main/packages/react-devtools-extensions)
- [Strict Mode](https://ko.reactjs.org/docs/strict-mode.html)를 쓰지 않으면 경고함.

```jsx
root.render((
	<React.StrictMode>
		<App/>
	</React.StrictMode>
));
```

- devtools Components를 사용하면 훨씬 더 간편하게 React Element들의 내용을 확인할 수 있다.

## 🚘 1-5. VDOM을 쓰는 이유?

- 미신 : VDOM을 쓰는 건 빠르기 때문이다. (현실은 빠르기 때문에 사용하는 것이 아니다.)
    - [현실](https://twitter.com/dan_abramov/status/842329893044146176)
    - fast enough(충분히 빠르다.)
    - maintainable(유지보수가 가능한 애플리케이션을 만드는 것을 도와준다. → 핵심)
- 위 현실을 쓴 사람은 **Dan Abramov**로 [Redux](https://redux.js.org/)의 창시자이면서 [React Core 개발자](https://beta.reactjs.org/community/team)이다.
- [VDOM 문서](https://ko.reactjs.org/docs/faq-internals.html)
    - 이 접근방식이 React의 **선언적 API**를 가능하게 한다.
- VDOM이 무엇이고, 왜 쓰는지 안다면 활용할 수 있는 [최적화 기법](https://ko.reactjs.org/docs/optimizing-performance.html)이 존재한다.

## 🚘 1-6. 재미로 만들어보는 React

- [전체코드](https://github.com/JaeMeDev/tiny-react)
- 아직 `Diff 알고리즘` 적용 전이라 vdom의 성능상 이점을 가져오지 못하고 있다. 추후 `Diff 알고리즘` 을 적용해보자.

```jsx
/* @jsx createElement */
function renderElement(node) {
  if (typeof node === "string") {
    return document.createTextNode(node);
  }

  if (node === undefined) return;

  const $el = document.createElement(node.type);

  node.children.map(renderElement).forEach((element) => {
    $el.appendChild(element);
  });

  return $el;
}

function render(vdom, container) {
  container.appendChild(renderElement(vdom));
}

function createElement(type, props, ...children) {
  if (typeof type === "function") {
    return type.apply(null, [props, ...children]);
  }
  return { type, props, children };
}

function App() {
  return (
    <div>
      <h1>메가테라 프론트엔드</h1>
      <h2>안녕하세요</h2>
    </div>
  );
}

render(<App />, document.getElementById("root"));
```