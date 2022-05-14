# `propTypes`
[`propTypes`](https://www.npmjs.com/package/prop-types) are useful for two reasons.

## Validation
The first reason is prop validation. *Validation* can ensure that `props` are doing what they are supposed to be doing by ensuring all expected `props` are present and match the expected types, printing a warning to the console when the expectations are not met.

## Documentation
The second reason *documentation* and is **arguably** the more useful of the two uses. Documenting `props` makes it much easier to understand a component at a glance, which becomes infinitely helpful as more components are introduced.

## Apply `propTypes`
If a component class expects a `prop`, then `propTypes` can be used for that component class! Using `propTypes`, requires importing the `'prop-types'` library.

```
import PropTypes from 'prop-types';
...
```

Then `propTypes` can be declared as a static property for a component after the component has been defined where `propTypes` is an object literal that maps properties to the appropriate types. Each property on the `propTypes` object is called a `propType`.

E.G.
```
import PropTypes from 'prop-types';

export class MessageDisplayer extends React.Component {
  render() {
    return <h1>{this.props.message}</h1>;
  }
}

// This propTypes object should have
// one property for each expected prop:
MessageDisplayer.propTypes = {
  message: PropTypes.string
};
```

Where each value in the `propTypes` object literal follow the pattern:

```
PropTypes.expected_data_type_goes_here
```

Additionally, each `propTypes` value can be required by appending `.isRequired` after the expected data type. If `.isRequired` is present for a `propType` a warning will be printed to the console if that component is created without the expected `prop`.

### `propTypes` for Function Components
Defining `propTypes` are just as easy to define for function components. To write `propTypes` for a function component just define a `propTypes` object as a property of the function component itself. The example above is represented as:

```
const MessageWriter = (props) => {
  return <h1>{props.message}</h1>;
}

MessageWriter.propTypes = {
  message: PropTypes.string.isRequired
};
```
