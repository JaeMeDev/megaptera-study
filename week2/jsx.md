# ๐ 1. JSX

{% hint style="info" %}
JSX๋ XML๊ฐ์ ๋ฌธ๋ฒ ํ์ฅ(ECMAScript)์ด๋ค. JSX๋ React๋ฅผ ๋ง๋ค๋ฉด์ ์๊ธด ๋ถ์ฐ๋ฌผ ๊ฐ์ ๊ฒ์ด์ง๋ง, JSX๋ ๊ผญ React์์๋ง ์ฌ์ฉ๊ฐ๋ฅํ ๊ฒ์ ์๋๋ค.

- [facebook์ JSX ์๊ฐ](https://facebook.github.io/jsx/)
- [React ๊ณต์๋ฌธ์์ JSX ์๊ฐ](https://ko.reactjs.org/docs/introducing-jsx.html)
- [Babel, JSX, ๊ทธ๋ฆฌ๊ณ  ๋น๋ ๊ณผ์ ๋ค](https://ko.reactjs.org/docs/faq-build.html)
- [JSX ์ดํดํ๊ธฐ](https://ko.reactjs.org/docs/jsx-in-depth.html)

{% endhint %}

## ๐ย 1-1. JSX์ ๋ํด์

- JSX๋ XML์ฒ๋ผ ์์ฑ๋ ๋ถ๋ถ์ `React.createElement` ๋ฅผ ์ฐ๋ JavaScript ์ฝ๋๋ก ๋ณํํ๋ค. ์ค๊ดํธ๋ฅผ ์จ์ JavaScript ์ฝ๋๋ฅผ ๊ทธ๋๋ก ์ธ ์ ์๊ณ , ๊ฒฐ๊ตญ์ JavaScript ์ฝ๋์ 1:1๋ก ๋งค์นญ๋๋ค.
- ๋ณํ๊ธฐ ์ค ์ ์ผ ์ ๋ชํ [Bebel](https://babeljs.io/repl)๋ก ํ์ธ ๊ฐ๋ฅํ๋ค.
    - **Presets**์์ **react**๋ฅผ ์ฒดํฌํ๊ฑฐ๋, **Plugin**์์ `@babel/plugin-transform-react-jsx` ๋ฅผ ์ถ๊ฐํ๋ฉด JSX๋ฅผ ์คํํ  ์ ์๋ค.
    - JSX ํ์ผ์ `/* @jsx ์ด์ฉ๊ณ  */` ์ฃผ์์ ์ถ๊ฐํ๋ฉด React.createElement ๋์  โ์ด์ฉ๊ณ โ๋ฅผ ์ฐ๊ฒ ๋๋ค.
- Example 1

```jsx
// JSX ์ฝ๋
<p>Hello, world</p>

// ๋ณํ๋ JS ์ฝ๋
React.createElement("p", null, "Hello, world");
```

- Example 2(์ฌ์ฉํ  ์ผ์ ๊ฑฐ์ **์์**)

```jsx
// JSX ์ฝ๋
/* @jsx jaejun.createElement */
<p>Hello, world</p>

// ๋ณํ๋ JS ์ฝ๋
jaejun.createElement("p", null, "Hello, world");
```

- Example 3

```jsx
// JSX ์ฝ๋
function Greeting({ name }){
	return (
		<p>Hello, {name}!</p>
	);
}

<Greeting name="world" age={13} />

// ๋ณํ๋ JS ์ฝ๋
function Greeting({ name }){
	return React.createElement("p", null, "Hello, ", name, "!");
}

React.createElement(Greeting, { name: "world", age: 13 });
```

- Example 4

```jsx
// JSX ์ฝ๋
<Button type="submit">Send</Button>

// ๋ณํ๋ JS ์ฝ๋
React.createElement(Button, { type: "submit" }, "Send");
```

- Example 5

```jsx
// JSX ์ฝ๋
<>
	<p>Hello, world!</p>
	<Button type="submit">Send</Button>
</>

// ๋ณํ๋ JS ์ฝ๋
React.createElement(
	React.Fragment,
	null,
	React.createElement("p", null, "Hello, world!"),
	React.createElement(Button, { type: "submit" }, "Send")
);
```

- Example 6

```jsx
// JSX ์ฝ๋
<div>
	<p>Count: {count}!</p>
	<button type="button" onClick={() => setCount(count + 1)}>Increase</button>
</div>

// ๋ณํ๋ JS ์ฝ๋
React.createElement(
	"div",
	null,
	React.createElement("p", null, "Count: ", count, "!"),
	React.createElement("button", { type: "button", onClick: () => setCount(count + 1) }, "Increase")
);
```

## ๐ย 1-2. React Element

- ์ฐธ๊ณ ๋ฌธ์
    - [JSX ์์ด ์ฌ์ฉํ๋ React](https://ko.reactjs.org/docs/react-without-jsx.html)
    - [createElement](https://beta.reactjs.org/reference/react/createElement)
- JSX๋์  ๊ทธ๋ฅ `React.createElement` ๋ฅผ ์จ์ React Element ํธ๋ฆฌ๋ฅผ ๊ฐฑ์ ํ๋๋ฐ ์ธ ์ ์๋ค.
- JSX๋ก ํ  ์ ์๋ ๋ชจ๋  ๊ฒ์ ์์ JavaScript๋ก๋ ํ  ์ ์๋ค.
- JSX Runtime์ `_jsx` ๋ ํจ์๋ฅผ, Preact๋ `h` ๋ ํจ์๋ฅผ ์ง์  ์ง์ํจ.
    - [_jsx](https://reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html)
    - [h()](https://preactjs.com/guide/v10/api-reference/#h--createelement)

## ๐ย 1-3. VDOM(Virtual DOM)

- ์ฐธ๊ณ ๋ฌธ์
    - [VDOM (Virtual DOM)](https://ko.reactjs.org/docs/faq-internals.html)
    - [์ฌ์กฐ์  (Reconciliation)](https://ko.reactjs.org/docs/reconciliation.html)
- ํธ๋ฆฌ๋ ํ๋ํ๊ณผ ๊ฐ๋ค. ํธ๋ฆฌ์ ๊ตฌ์ฑ์์๋ ํธ๋ฆฌ์ด๋ค. ์ฐ๋ฆฌ๋ ๋งค๋ฒ ์์ React Element ํธ๋ฆฌ, VDOM ํธ๋ฆฌ๋ฅผ ๋ง๋ ๋ค. **VDOM์ ์ค์  DOM๊ณผ ๋น๊ต๋ฅผ ํตํด ๋ณ๊ฒฝ์ฌํญ์ ์ ์ฉํ๋ค.**

## ๐ย 1-4. React Developer Tools

- [๋ฌธ์](https://github.com/facebook/react/tree/main/packages/react-devtools-extensions)
- [Strict Mode](https://ko.reactjs.org/docs/strict-mode.html)๋ฅผ ์ฐ์ง ์์ผ๋ฉด ๊ฒฝ๊ณ ํจ.

```jsx
root.render((
	<React.StrictMode>
		<App/>
	</React.StrictMode>
));
```

- devtools Components๋ฅผ ์ฌ์ฉํ๋ฉด ํจ์ฌ ๋ ๊ฐํธํ๊ฒ React Element๋ค์ ๋ด์ฉ์ ํ์ธํ  ์ ์๋ค.

## ๐ย 1-5. VDOM์ ์ฐ๋ ์ด์ ?

- ๋ฏธ์  : VDOM์ ์ฐ๋ ๊ฑด ๋น ๋ฅด๊ธฐ ๋๋ฌธ์ด๋ค. (ํ์ค์ ๋น ๋ฅด๊ธฐ ๋๋ฌธ์ ์ฌ์ฉํ๋ ๊ฒ์ด ์๋๋ค.)
    - [ํ์ค](https://twitter.com/dan_abramov/status/842329893044146176)
    - fast enough(์ถฉ๋ถํ ๋น ๋ฅด๋ค.)
    - maintainable(์ ์ง๋ณด์๊ฐ ๊ฐ๋ฅํ ์ ํ๋ฆฌ์ผ์ด์์ ๋ง๋๋ ๊ฒ์ ๋์์ค๋ค. โ ํต์ฌ)
- ์ ํ์ค์ ์ด ์ฌ๋์ **Dan Abramov**๋ก [Redux](https://redux.js.org/)์ ์ฐฝ์์์ด๋ฉด์ [React Core ๊ฐ๋ฐ์](https://beta.reactjs.org/community/team)์ด๋ค.
- [VDOM ๋ฌธ์](https://ko.reactjs.org/docs/faq-internals.html)
    - ์ด ์ ๊ทผ๋ฐฉ์์ด React์ **์ ์ธ์  API**๋ฅผ ๊ฐ๋ฅํ๊ฒ ํ๋ค.
- VDOM์ด ๋ฌด์์ด๊ณ , ์ ์ฐ๋์ง ์๋ค๋ฉด ํ์ฉํ  ์ ์๋ [์ต์ ํ ๊ธฐ๋ฒ](https://ko.reactjs.org/docs/optimizing-performance.html)์ด ์กด์ฌํ๋ค.

## ๐ย 1-6. ์ฌ๋ฏธ๋ก ๋ง๋ค์ด๋ณด๋ React

- [์ ์ฒด์ฝ๋](https://github.com/JaeMeDev/tiny-react)
- ์์ง `Diff ์๊ณ ๋ฆฌ์ฆ` ์ ์ฉ ์ ์ด๋ผ vdom์ ์ฑ๋ฅ์ ์ด์ ์ ๊ฐ์ ธ์ค์ง ๋ชปํ๊ณ  ์๋ค. ์ถํ `Diff ์๊ณ ๋ฆฌ์ฆ` ์ ์ ์ฉํด๋ณด์.

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
      <h1>๋ฉ๊ฐํ๋ผ ํ๋ก ํธ์๋</h1>
      <h2>์๋ํ์ธ์</h2>
    </div>
  );
}

render(<App />, document.getElementById("root"));
```