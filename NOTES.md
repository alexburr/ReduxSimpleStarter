# REACT/REDUX COURSE

## Section 1: Intro to React
----------------------------

React: Components/Views

**Components**
- Snippets of JS code that produce HTML
- Multiple components are nest inside one another

```javascript
const App = function() {
    return <div>Hi!</div>; }
```

`const` is **ES6** syntax. `<div>Hi!</div>` is **JSX**: Looks like HTML but is really JS; gets transpiled

JS **Modules**:  
Code is siloed and has no contact with/access to other code unless specifically set 
- ie: `import React from 'react';`
- This means 'Assign library to a `var` called "React"' (will transpile)

When we create a component, we create a _class_ not an _instance_.  
In the code for the course, `App` is a class (not an instance) and needs to be _instantiated_.  
Using a component name in JSX turns it into a component _instance_, such as `<App></App>`.

ReactDom `render()` takes a second argument for a target node, ie our `.container` div element. 

> ### ES6 Concept: 
> `() =>` (fat arrow):  
> This is the same as `function()`, but affects the value of `this`

#### Video Player App Components:
* 1 for search bar
* 1 for player/detail
* 1 for a single preview
* 1 for a list of previews
* 1 for app container that holds other components

> ### RULE: __1 Component per file!__

`package.json` file: All npm dependencies.  
`npm install --save` will write to this.

React components can show other components!

To render search box component in index, index needs a reference.  
That's where `export` comes in!

> `export default SearchBar;`
>
> Any file that imports will get that `SearchBar` component

Any code we write (ie not a package) must be imported by Path (eg `import foo from './ex/foo';`)

 ### Functional Component:
 * One function
 * Some argument goes in, one object comes out
 ### Class Component:
 * Some record-keeping around its own state
 * Uses ES6 class: JS object w/ props and methods
 * `render` method must exist, must return JSX

> ### ES6 Tip:
> Instead of writing `extends React.Component` you can `import React { Component }` and write `extends Component`.  
> This is equivalent to `const Component = React.Component`

When a user users a web app, the trigger _events_:
* Move mouse
* Click button
* Type into input field

2 steps to handling events:
1. Declare event handler (function to call on event)
   * Name it something like `handle` or `on` followed by element and event type  
   eg: `onInputChange`
1. Pass handler to element to monitor 

All browser events are called with an _event_ object, so pass it to any handlers (eg `onInputChange(event)`) to get access to value of event.

> ### ES6: 
> Can pass single-line function directly instead of as a method

### React State
State is a plain JS object used to record and react to events. Each class component has its own state. Whenever state is changed, the component re-renders, _as well as all its children_.

Functional components do _not_ have state.

State object must be initialized in constructor:  
`this.state = {};`  
(New instances call constructor automatically)

`super(props)`: `Component` has a constructor method, so it is called by calling `super`.

> ### Updating state is different than initializing.
> To init we set state by assigning it, but _ONLY_ in constructor.  
> _Everywhere else uses `this.setState()`_  
> Pass it an object for the new state.  
>   Component will automatically re-render -- So updates to state will be reflected in `render()` as written.

__Controlled form element/Controlled field:__  Element whose value is set by state, instead of the other way around.

A controlled component's value can ONLY change when state changes. So if there is an event handler to set state on change, the value of the input and the state prop will be bound, instead of set independently. Typing in the field would _NOT_ actually set the field's `value` _unless_ it was rendered as a Controlled element.