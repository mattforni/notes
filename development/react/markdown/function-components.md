# Function Components
## Stateless Functional Components
Not all components need to be defined as *class components* in React. Components can also be defined as JavaScript functions. These types of components are called *function components* to differentiate them from *class components*. Function components can do everything that class components can and are often a more elegant, concise way of creating React components.

The most basic *function components* need only define the JSX expression that would normally be in the `render()` method of a class component. Nice and easy.

```
export const MyFunctionComponent = () => {
  return <h1>Hello world!</h1>;
}
```

Function components can receive a `props` JavaScript object just like a class component, and the `props` attribute can be accessed directly, without the use of the `this` keyword.

## The State Hook
React hooks are functions that let enable the management of internal state of components and handle post-rendering side effects directly from function components. Hooks do *NOT* inside class components. **NOTE** function components and React Hooks do *NOT* completely replace class components, but rather provide alternative tools for achieving similar results.

React offers a number of hooks, including `useState()`, `useEffect()`, `useContext()`, `useReducer()` and `useRef()` in addition to [a few more that are documented here](https://reactjs.org/docs/hooks-reference.html).

The most common hook is the `useState()` hook, which is a named export from the React library.

```
import React, { useState } from 'react';
```

When called, the `useState()` function returns an array with two values:

1. *current state* - the current value of this state
2. *state setter* - a function that can be used to update the value of this state

These two values are returned in an array and can be assigned to local variables with any names.

```
const [toggle, setToggle] = useState();
```

The *state setter* (e.g. `setToggle`) function provides an easy way to update state without having to bind functions to class instances, working with constructors or binding the keyword `this`. Using the state setter function also triggers a re-render of the associated function component without having to create classes. The state setter function can be used with JavaScript event listeners by calling it with an arrow method.

```
<button onClick={() => setToggle(true)}>
  Toggle On
</button>
```
### Initial State
The state hook can be used to manage the value of any primitive data type, along with data collections like arrays and objects. The initial value of the state that is being used should be passed as an argument to the `useState()` function. If not value is passed, the state will be `undefined` during the first render. It is best practice to explicitly initialize state, even if the necessary value is not present. In this case it is better to explicitly pass `null` instead of allowing the value to implicitly be set to `undefined`.

### State Setter Hygiene
It is very common define event handlers outside of JSX within the body of the function component. This is most often done by defining an event handler like so:

```
const handleChange = (event) => {
  setEmail(event.target.value);
};
```

Or even more simply by leveraging [destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

```
const handleChange = ({target}) => {
  setEmail(target.value);
};
```

### Using Previous State
The state setter function can also receive a callback function as an argument (instead of a value) in which case the *state setter callback function* takes the *previous value* as an argument and returns the *next value*.

When updated an array in state best practice is to replace the entire array with a brand new array instead of just appending the new value to the previous array. This means that any information that needs to be copied over needs to be done explicitly, which is where [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) comes into play.

### Object in State
When working with a set of related variables it is good practice to group them into an object. When using an object, the state setter callback updates the state based on the previous value, and the spread syntax is exactly the same as arrays (e.g. `{...oldObject, newKey: newValue }`). The event handler can be reused across multiple change events by using the `name` attribute to identify which field was changed.

```
const handleChange = ({ target }) => {
  const { name, value } = target;
  setFormState((prev) => ({
    ...prev,
    [name]: value
  }));
};

<form>
  <input
    value={profile.firstName || ''}
    onChange={handleChange}
    name-"firstName"
    type="text"
  />
</form>
```

Notice that the above state setter callback function wraps the curly brackets in parentheses, which indicates to JavaScript that these curly brackets refer to a new object to be returned. Also notice that the `name` parameter is wrapped in square brackets, which allows JavaScript to use the string value stored in the `name` variable as a *key* for the object via a [computed property](http://eloquentcode.com/computed-property-names-in-javascript).

### Separate State
While it can be advantageous to store related data in a data collection, it is also helpful to keep the data model as simple as possible and to separate disparate data into different object with different state hooks.
