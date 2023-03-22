# 🌈 6. Global Style & Theme

## 🚘 6-1. Reset CSS

- CSS 초기화 라이브러리
- [Reset CSS](https://meyerweb.com/eric/tools/css/reset/)
- [styled-reset](https://github.com/zacanger/styled-reset)
- 패키지 설치

```bash
npm i styled-reset
```

- App 컴포넌트에서 사용. (시작점)

```tsx
import { Reset } from 'styled-reset';

export default function App() {
	return (
		<>
			<Reset />
			<Greeting />
		</>
	);
}
```

## 🚘 6-2. Global Style

- 참고 문서
    - [createGlobalStyle](https://styled-components.com/docs/api#createglobalstyle)
    - [대체 CSS box model](https://developer.mozilla.org/ko/docs/Learn/CSS/Building_blocks/The_box_model#대체_css_box_model)
    - [box-sizing](https://developer.mozilla.org/ko/docs/Web/CSS/box-sizing)
    - [The 62.5% Font Size Trick](https://www.aleksandrhovhannisyan.com/blog/62-5-percent-font-size-trick/)
    - [keep-all-villain](https://twitter.com/keepallvillain)
    - [word-break](https://developer.mozilla.org/ko/docs/Web/CSS/word-break)
- Global Style은 말 그대로 전역 스타일을 지정할 수 있다.

```tsx
import { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
	html {
		box-sizing: border-box;
	}
	
	*,
	*::before,
	*::after {
		box-sizing: inherit;
	}
	
	html {
		font-size: 62.5%;
	}
	
	body {
		font-size: 1.6rem;
	}
	
	:lang(ko) {
		h1, h2, h3 {
			word-break: keep-all;
		}
	}
`;

export default GlobalStyle;
```

- 마찬가지로 시작점임 App에 적용해준다.

```tsx
import { Reset } from 'styled-reset';

import GlobalStyle from './styles/GlobalStyle';

export default function App() {
	return (
		<>
			<Reset />
			<GlobalStyle />
			<Greeting />
		</>
	);
}
```

## 🚘 6-3. Theme

- [Theming](https://styled-components.com/docs/advanced#theming)
- [Create a declarations file](https://styled-components.com/docs/api#create-a-declarations-file)
- 디자인 시스템의 근간을 마련하는데 활용된다. 잘 정의하면 다크 모드 등에 대응하기 쉬워짐. 눈에 보이는 단편적인 정보를 넘어서, “의미”에 집중할 수 있게 된다.
- Theme을 먼저 정의해보자.

```tsx
const defaultTheme = {
	colors: {
		background: '#FFF',
		text: '#000',
		primary: '#F00',
		secondary: '#00F',
	},
};

export default defaultTheme;
```

- 마찬가지로 App 컴포넌트에서 사용하자.

```tsx
import { ThemeProvider } from 'styled-components';

import { Reset } from 'styled-reset';

import defaultTheme from './styles/defaultTheme';

import GlobalStyle from './styles/GlobalStyle';

export default function App() {
	return (
		<ThemeProvider theme={defaultTheme}>
			<Reset />
			<GlobalStyle />
			<Greeting />
		</ThemeProvider>
	);
}
```

- `props.theme` 를 쓸 수 있다.

```tsx
import { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
	body {
		background: ${(props) => props.theme.colors.background};
		color: ${(props) => props.theme.colors.text};
	}
	
	a {
		color: ${(props) => props.theme.colors.text};
	}
	
	button,
	input,
	select,
	textarea {
		background: ${(props) => props.theme.colors.background};
		color: ${(props) => props.theme.colors.text};
	}
`;

export default GlobalStyle;
```

- 타입 문제를 해결하기 위해 `styled.d.ts` 를 만들어주자.

```tsx
import 'styled-components';

declare module 'styled-components' {
	export interface DefaultTheme extends Theme {
		colors: {
			background: string;
			text: string;
			primary: string;
			secondary: string;
		}
	}
}
```

- 타입을 정의하고 defaultTheme를 맞추는 건 불편하니 반대로 defaultTheme에서 타입을 추출하자.

```tsx
import defaultTheme from './defaultTheme';

type Theme = typeof defaultTheme;

export default Theme;
```

- 타입 파일을 변경하자.

```tsx
import 'styled-components';

import Theme from './Theme';

declare module 'styled-components' {
	export interface DefaultTheme extends Theme {}
}
```

- 다를 theme을 추가할 때 Theme타입을 사용한다. (예시: 다크모드)

```tsx
import Theme from './Theme';

const darkTheme: Theme = {
	colors: {
		background: '#000',
		text: '#FFF',
		primary: '#F00',
		secondary: '#00F',
	},
};

export default darkTheme;
```

- 의미를 명확하게 드러냈다면, 다크모드를 지원하는 것도 쉬워지게 된다.

```tsx
import { useDarkMode } from 'usehooks-ts';

import { ThemeProvider } from 'styled-components';

import { Reset } from 'styled-reset';

import defaultTheme from './styles/defaultTheme';
import darkTheme from './styles/darkTheme';

import GlobalStyle from './styles/GlobalStyle';

import Greeting from './components/Greeting';
import Button from './components/Button';

export default function App() {
const { isDarkMode, toggle } = useDarkMode();

const theme = isDarkMode ? darkTheme : defaultTheme;

return (
	<ThemeProvider theme={theme}>
		<Reset />
		<GlobalStyle />
		<Greeting />
		<Button onClick={toggle}>
			Dark Theme Toggle
		</Button>
	</ThemeProvider>
	);
}
```

- Jest 테스트 쪽에서 `window.matchMedia` 라는 문제가 발생한다. ([Mocking methods which are not implemented in JSDOM](https://jestjs.io/docs/manual-mocks#mocking-methods-which-are-not-implemented-in-jsdom))
- `src/setupTests.ts` 파일에 공식문서에 나온 코드를 넣으면 해결된다.

```tsx
Object.defineProperty(window, 'matchMedia', {
	writable: true,
	value: jest.fn().mockImplementation((query) => ({
		matches: false,
		media: query,
		onchange: null,
		addListener: jest.fn(), // deprecated
		removeListener: jest.fn(), // deprecated
		addEventListener: jest.fn(),
		removeEventListener: jest.fn(),
		dispatchEvent: jest.fn(),
	})),
});
```

- 참고
    - [https://code.visualstudio.com/api/references/theme-color](https://code.visualstudio.com/api/references/theme-color)
    - [https://getbootstrap.com/docs/5.3/customize/color/](https://getbootstrap.com/docs/5.3/customize/color/)