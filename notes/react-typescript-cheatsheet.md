---
title: React Typescript Cheatsheet
tags:
  - react
  - typescript
emoji: 📔
link: https://github.com/typescript-cheatsheets/react-typescript-cheatsheet
---

## Functional Components

```tsx
import React from "react";

export type Question = {
	incorrect_answers: string[];
};

export type QuestionState = Question & { answers: string[] };

type Props = {
	question: string;
	answer: string[];
	callback: (e: React.MouseEvent<HTMLButtonElement>) => void;
};

const SomeComponent: React.FC<Props> = ({
	question,
	answer,
	callback
}) => {
	const [questions, setQuestions] = useState<QuestionState[]>([]);
	return <div></div>;
};

export default SomeComponent;
```

## Styled Components

```jsx
import styled from 'styled-components'

interface ComponentProps {}

export const Component = styled.div<ComponentProps>`
```

