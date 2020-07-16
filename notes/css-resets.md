---
title: CSS Resets
tags:
  - css
emoji: 👋
link:
---

## Box-sizing reset

[CSS-Tricks](https://css-tricks.com/box-sizing/)

```css
html {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
*, *:before, *:after {
  -webkit-box-sizing: inherit;
  -moz-box-sizing: inherit;
  box-sizing: inherit;
  }
```
