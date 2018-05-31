# REACT/REDUX COURSE

## Section 1: Intro to React

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

## Section 2: Ajax Requests with React

> "Downwards data flow":  
> Only most parent component should be responsible for fetching data (from API or Flux, etc).

`index.js` is our most parent component, and all its children will need to use the fetched data.

_Use of state:_  
Whenever a user enters some search input, we will conduct a new search and set the results of the search on state.

> ### ES6: 
> Whenever we have a key/value that is the same string, we can condense it to just the name of the variable. Example:  
>```
> YTSearch({key: API_KEY, term: 'surfboards'}, (videos) => {
>   this.setState({ videos: videos });
> });
>```
>becomes:
>```
>YTSearch({key: API_KEY, term: 'surfboards'}, (videos) => {
>    this.setState({ videos });
>});
>```
### VideoList Component
Video_list will be a _functional component_:
* Uses Bootstrap class names in markup
  * In JSX we need to specify `className` attribute
* When importing, the `from` is a path, so start relative to current directory (e.g. `'./'`);

`App` is the parent to `VideoList` and it needs the list of videos from `App` state. Some data will need to pass from parent to the child&mdash;by defining a property on the child JSX tag in the parent: `<VideoList videos={this.state.videos}>`

This will be a `prop` that is passed to `VideoList`.  
Any time the component renders it will get the news list as well. 

With a functional component, the `props` object will arrive as an argument to the function: `const VideoList = (props) => {`

When reloading the page, the property has no value at first render, so if we output `props.videos.length` to the screen, it will briefly display `0` before being updated to `5`.

>### In a _functional_ component, the `props` object is an argument.   
>### In a _class_ component, `props` are available in any method as `this.props`.

### Lists
Making lists with React is easy: loop over array and generate a new item!  
Stay away from `for` loops if possible... use built-in iterators like `.map`:

>### ES6:
>`array.map` returns a function that will be run on each item in the array.
>```
>var array = [1, 2, 3];
>array.map(function(number) { return number * 2 });
>```
>This will return `[2,4,6]`
>```
>array.map(function(number) { return '<div>' + number + '</div>'});
>```
>This will return `["<div>1</div>","<div>2</div>","<div>3</div>"]`

React is clever&mdash;but almost _too_ clever. When we are building an array of the same type, React assumes it is a list, and has internal optimizations for rendering lists.

So when updating a list, it will update a list item by ID (`key`) if possible, otherwise it has to rebuild the entire list.

In our data result from YouTube, each video has an `etag` property that we can use as the key for each item in our `VideoList`.

>### ES6: 
>```
>const VideoListItem = (props) => {
>   const video = props.video;
>   ...
>```
> can be refactored to:
>```
>const VideoListItem = ({video}) => {
>   ...
>```
>In other words, "the first object in the arguments has a property called `video`. Please gather it and declare a new variable called `video`." 
>
>This can be done with more than one argument:
>```
>const VideoListItem = (props) => {
>   const video = props.video;
>   const onVideoSelect = props.onVideoSelect;
>   ...
>```
> can be refactored to:
>```
>const VideoListItem = ({video, onVideoSelect}) => {
>   ...
>```

### Ask Yourself:  
#### _"Do I need this component to maintain any kind of state?"_

Our video detail only needs to care about URL, title, thumbnail, which are all props that will be passed down. Therefore it can be a simple _functional_ component.

>### ES6:
> _Template String/Template Literal_:  
> Wrap a string in `` ` `` (instead of ``'`` or ``"``) for string interpolation:   
> `'https://www.youtube.com/embed/' + videoId;`
>becomes
> `` `https://www.youtube.com/embed/${videoId}`; ``

**NOTE:** React wants to render immediately. Some parent objects can't fetch fast enough to satisfy a child object. So null/undefined checks are important.

>### LODASH:
>`debounce()` Has ability to throttle existing callback: Will take a function and return a new function that can only be called every _`x`_ milliseconds (300 in our example).

If we pass the `debounced` callback into `SearchBar`, it will only run every 300ms, meaning we will only trigger a new search once every 300ms.