# 🌈 4. styled-components

{% hint style="info" %}
💡 styled-components에 대해 알아보자.

- [styled-components](https://styled-components.com/)
- [Babel Plugin](https://styled-components.com/docs/tooling#babel-plugin)
- [@swc/plugin-styled-components](https://github.com/swc-project/plugins/tree/main/packages/styled-components)
- [vscode-styled-components extension](https://marketplace.visualstudio.com/items?itemName=styled-components.vscode-styled-components)

{% endhint %}

## 🚘 4-1. styled-components란?

- 스타일이 적용된 컴포넌트를 쉽게 만들 수 있는 도구.
- 반드시 VS Code Extension을 설치해서 쓰자. 도구에서 제대로 지원 안 하면 사람이 쓰기 힘들다.
- Babel Plugin을 SWC에서 쓸 수 있도록 포팅한 것도 함께 설치하자(SSR 지원 등을 위한 공식 권장 사항)

```bash
npm i styled-components
npm i -D @types/styled-components @swc/plugin-styled-components
```

- `.swcrc` 파일을 작성하자.

```tsx
{
	"jsc": {
		"experimental": {
			"plugins": [
				[
					"@swc/plugin-styled-components",
					{
						"displayName": true,
						"ssr": true
					}
				]
			]
		}
	}
}
```

- 간단한 Styled Component 예제

```tsx
import styled from 'styled-components';

const Paragraph = styled.p`
	color: #00F;
`;

export default function Greeting() {
	return (
		<Paragraph>
			Hello, world!
		</Paragraph>
	);
}
```

- Styled Component에 추가로 스타일을 입히는 것도 가능하다.

```tsx
import styled from 'styled-components';

const Paragraph = styled.p`
	color: #00F;
`;

const BigParagraph = styled(Paragraph)`
	font-size: 5rem;
`;

export default function Greeting() {
	return (
		<BigParagraph>
			Hello, world!
		</BigParagraph>
	);
}
```

- 기존 컴포넌트에 스타일을 입히는 것도 가능하다. 단 기존 컴포넌트가 Class를 잡아야 한다는 점에 주의하자.

```tsx
import styled from 'styled-components';

function HelloWorld({ className }: React.HTMLAttributes<HTMLElement>) {
	return (
		<p className={className}>
			Hello, world!
		</p>
	);
}

const Greeting = styled(HelloWorld)`
	color: #00F;
`;

export default Greeting;
```