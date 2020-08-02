---
title: React Typescript Cheatsheet
tags:
  - react
  - typescript
emoji: 📔
link: https://github.com/typescript-cheatsheets/react-typescript-cheatsheet
---

## React

### Functional Components

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

### Styled Components

```jsx
import styled from 'styled-components'

interface ComponentProps {}

export const Component = styled.div<ComponentProps>`
```

## React Native

### Passing Through Styles

```jsx
import React from "react";
import { StyleSheet, View, ViewStyle } from "react-native";

type Props = {
	style: ViewStyle;
};

const Card: React.FC<Props> = ({ children, style }) => {
	return <View style={{ ...styles.card, ...style }}>{children}</View>;
};

export default Card;
```

### Extending Props
```jsx
import React from "react";
import { TextInput, StyleSheet, TextStyle, TextInputProps } from "react-native";

interface Props extends TextInputProps {
	style?: TextStyle;
}

const Input: React.FC<Props> = (props) => {
	return (
		<TextInput
			{...props}
			style={{ ...styles.input, ...props.style }}
		></TextInput>
	);
};
```

### Handling Text Input
```jsx
import {
	NativeSyntheticEvent,
	TextInputTextInputEventData,
} from "react-native";

function numberInputHandler(
		e: NativeSyntheticEvent<TextInputTextInputEventData>
	) {
		setEnteredValue(e.nativeEvent.text.replace(/[^0-9]/g, ""));
	}
```