---
title: Styled Components
tags:
  - react
  - typescript
emoji: 📔
link: https://styled-components.com/docs/basics
---

## Extending Any Component

The styled method works perfectly on all of your own or any third-party component, as long as they attach the passed className prop to a DOM element.

```jsx
// This could be react-router-dom's Link for example
const Link = ({ className, children }) => (
  <a className={className}>
    {children}
  </a>
);

const StyledLink = styled(Link)`
  color: palevioletred;
  font-weight: bold;
`;

render(
	<div>
		<Link>Unstyled, boring Link</Link>
		<br />
		<StyledLink>Styled, exciting Link</StyledLink>
    </div>
  );
```


