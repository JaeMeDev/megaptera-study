# 🌈 5. props와 attrs

## 🚘 5-1. Props 활용

- [Passed props](https://styled-components.com/docs/basics#passed-props)
- 활성화 여부를 표현하거나, 특정 스타일을 잡아주고 싶을 때 유용함.

```tsx
import { useBoolean } from 'usehooks-ts';

import styled, { css } from 'styled-components';

type ParagraphProps = {
	active?: boolean;
}

const Paragraph = styled.p<ParagraphProps>`
	color: ${(props) => (props.active ? '#F00' : '#888')};
	${(props) => props.active && css`
		font-weight: bold;
	`}
`;

export default function Greeting() {
	const { value: active, toggle } = useBoolean(false);
	
	return (
		<div>
			<Paragraph>
				Inactive
			</Paragraph>
			<Paragraph active>
				Active
			</Paragraph>
			<Paragraph active={active}>
				Hello, world
				{' '}
				<button type="button" onClick={toggle}>
					Toggle
				</button>
			</Paragraph>
		</div>
	);
}
```

## 🚘 5-2. attrs 활용

- [Attaching additional props](https://styled-components.com/docs/basics#attaching-additional-props)
- 기본 속성을 추가할 수 있음. 불필요하게 반복되는 속성을 처리할 때 유용한데, 버튼 등을 만들 때 적극 활용한다.

```tsx
import styled from 'styled-components';

const Button = styled.button.attrs({
	type: 'button',
})`
	border: 1px solid #888;
	background: transparent;
	cursor: pointer;
`;

export default Button;
```