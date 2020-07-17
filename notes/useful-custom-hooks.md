---
title: Useful Custom Hooks
tags:
  - react
emoji: ⚓
link:
---

## Debounce and Throttle Hook

[Div’s Blog](https://divyanshu013.dev/blog/react-debounce-throttle-hooks/)

```jsx
import React, { useState, useCallback } from 'react';
import debounce from 'lodash.debounce';

function useDebounce(callback, delay) {
	// Memoizing the callback because if it's an arrow function
	// it would be different on each render
	const memoizedCallback = useCallback(callback, []);
	const debouncedFn = useRef(debounce(memoizedCallback, delay));

	useEffect(() => {
		debouncedFn.current = debounce(memoizedCallback, delay);
	}, [memoizedCallback, debouncedFn, delay]);

	return debouncedFn.current;
}

// Usage
function App() {
	const [value, setValue] = useState('');
	const [dbValue, saveToDb] = useState(''); // would be an API call normally

	const debouncedSave = useDebounce(nextValue => saveToDb(nextValue), 1000);
	const handleChange = event => {
		const { value: nextValue } = event.target;
		setValue(nextValue);
		debouncedSave(nextValue);
	};

	return <main>{/* Same as before */}</main>;
}
```

## Unmount Animation Hook

[Recruiterra](https://github.com/serCJm/recruiterra/blob/master/client/src/utils/hooks.js)

```jsx
import React, { useState } from 'react';

function animateUnmountReducer(state, action) {
  switch (action.type) {
    case "updateActiveContent":
      return { ...state, activeContent: action.activeContent };
    case "updateNewContent":
      return { ...state, newContent: action.newContent };
    default:
      throw new Error("Something went wrong with animateUnmountReducer");
  }
}

export function useAnimatedUnmount(initialContent, delayTime) {
  const [shouldAnim, setShouldAnim] = useState(false);
  const [state, dispatch] = useReducer(animateUnmountReducer, {
    activeContent: initialContent,
    newContent: initialContent
  });

  useEffect(() => {
    let timeoutId;

    if (state.activeContent !== state.newContent) {
      setShouldAnim(true);
      timeoutId = setTimeout(() => {
        dispatch({
          type: "updateActiveContent",
          activeContent: state.newContent
        });
        setShouldAnim(false);
      }, delayTime);
    }
    return () => clearTimeout(timeoutId);
  }, [shouldAnim, state.activeContent, state.newContent, delayTime]);
  return [shouldAnim, state.activeContent, dispatch];
}
```