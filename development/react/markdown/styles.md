# Styles
## Inline Styles
An inline style is a style that’s written as an *attribute*, like this:

```
<h1 style={{ color: 'red' }}>Hello world</h1>
```

In the above example the outer curly braces inject JavaScript into JSX while the inner curly brace defines a JavaScript object literal. The object being passed to the `style` attribute can contain any number of styles to apply.

## Style Object
Applying inline styles can be cumbersome though, so it is often better to define a style object and store it in a *variable* to be passed to the `style` attribute. While style names are written in hyphenated-lowercase in regular JavaScript, they are expressed in camelCase when written in React.

## Style Value Syntax
In regular JavaScript, style values are almost always strings, even if they represent a number. In React, if a style value is written as a number then the "px" is implied. To use a unit other than `px` (like `rem`) then the value needs to be a string.

## Sharing Styles
One way to make styles reusable is to keep them in a separate JavaScript file that exports all of the styles to be used by other files via `import`. Those styles can then be used across different components.
