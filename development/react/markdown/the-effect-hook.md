# The Effect Hook
The Effect Hook is used to run JavaScript code after each render. This can be actions like:

1. Fetching data from a backend service
2. Subscribing to a stream of data
3. managing timers and intervals
4. reading from and making changes to the DOM

There are three key moments when the Effect Hook can be utilized:

1. When the component is first added (or *mounted*) to the DOM and renders
2. When the `state` or `props` change, causing the component to re-render
3. When the component is removed (or *unmounted*) from the DOM

## Importing the Effect Hook
The `useEffect` function is a named export in the React library and can be imported with:

```
import { useEffect } from 'react';
```

The first argument passed tot he `useEffect()` function is the callback function that React calls each time this component renders. This callback function is referred to as "the effect."

## Cleanup Effects
Some effects require cleanup. These are effects like adding event listeners to some element(s) in the DOM beyond the JSX in the component then it is important to remove those those even listeners when the component unmounts to avoid [memory leaks](https://auth0.com/blog/four-types-of-leaks-in-your-javascript-code-and-how-to-get-rid-of-them/). In the example below, if the effect does *not* also return a *cleanup function* then every time the component re-renders, a *new* event listener is added to the document!

```
useEffect(()=>{
  document.addEventListener('keydown', handleKeyPress);
  return () => {
    document.removeEventListener('keydown', handleKeyPress);
  };
})
```

This not only introduces bugs, but it also leads to decreased performance over time and eventually to application crashes if the application runs for long enough. Since effects run after every render, React calls the cleanup function before *each* re-render and before unmounting to clean up each effect call. If a call to `useEffect()` returns a function, that function is considered the *cleanup function* and is called before each re-render and before unmounting.

## Controlling When Effects are Called
Be default the first argument passed to `useEffect()` is called after *each time a component renders*. Sometimes that is not the desired behavior. It is very common to want to run an effect only the first time that a component renders, or when it mounts. The `useEffect()` function makes this very easy by passing in an *empty array* as the second argument. The second argument is called the *dependency array* and it tells the `useEffect()` method when to apply the effect, and when to skip it. The effect is *always* called after the first render, but is only called again if something in the dependency array has changed between renders. Passing an empty array of dependencies means effects will only be applied on mounting:

```
useEffect(() => {
  alert("component rendered for the first time");
  return () => {
    alert("component is being removed from the DOM");
  };
}, []);
```

| **Dependency Array**       | **When Affect is Called**           |
| -------------------------- | ----------------------------------- |
| `undefined`                | every re-render                     |
| Empty Array (`[]`)         | no re-renders                       |
| Non-empty Array (`[data]`) | when any value in the array changes |

## Fetching Data
When an effect is used to fetch data, it is important to pay close attention to when the effect is called. Unnecessary round trip calls between the React components and the servers that provide the data. If the data being fetched never changes, the effect can trigger the fetch and the state hook can store that data in the component when it is retrieved.

A non-empty dependency array tells the effect hook that it only need to be re-run when data in the dependency array has changed.

```
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // Only re-run the effect if the value stored by count changes
```

## Separate Hooks for Separate Effects
Similar to how it makes sense to use different hooks for managing different state, it is a best practice to use different hooks for different effects. By separating hooks for both state and effects it is much easier to logically group hooks to make it much clearer what the code is doing.

```
const [menu, setMenu] = useState(null);  
useEffect(() => {
  get('/menu').then((menuResponse) => {
    setMenu(menuResponse.data);
  });
}, []);

const [friends, setFriends] = useState(null);
useEffect(() => {
  get('/friends').then((friendsResponse) => {
    setFriends(friendsResponse.data);
  });
}, []);

...
```
