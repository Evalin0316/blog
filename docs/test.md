---
title: Title for search engines; can be different from the actual heading
description: A short description of this page
keywords: [keywords, describing, the main topics]
---

<!-- # Hello world

```jsx title="/src/components/HelloCodeTitle.js" showLineNumbers{2-3,5} 
function HelloCodeTitle(props) {
   let message = "Hello";
   let message = "Hello";
   let message = "Hello";
   let message = "Hello";
   let message = "Hello";
  return <h1>Hello, {props.name}</h1>;
}
```
:::note

Some **content** with _Markdown_ `syntax`. Check [this `api`](#).

:: -->

export const Highlight = ({children, color}) => (
  <span
    style={{
      backgroundColor: color,
      borderRadius: '2px',
      color: '#fff',
      padding: '0.2rem',
    }}>
    {children}
  </span>
);

<Highlight color="#25c2a0">Docusaurus green</Highlight> and <Highlight color="#1877F2">Facebook blue</Highlight> are my favorite colors.

I can write **Markdown** alongside my _JSX_!