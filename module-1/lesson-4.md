# Lesson 4 - Class components and Component Methods

## Introduction

Using function components with hooks is currently the preferred method of creating components in React.

In this lesson we are going to briefly look at class components for two reasons:

-   they give us a clearer understanding of the lifecycle of a component
-   you are likely to encounter class components in other people's code or in tutorials

## The Component Lifecycle

React class components have several "“lifecycle methods” that you can override to run code at particular times in the process."

You can see details of all the methods in the <a href="https://reactjs.org/docs/react-component.html" target="_blank">official docs</a>, but we are only going to look at the following methods:

-   constructor
-   render
-   componentDidMount
-   componentDidUpdate
-   componentWillUnmount

---

### Constructor

In a regular JavaScript class, the constructor is called when a new instance of a class is instantiated with the `new` keyword.

In React, the constructor method is called before the component is mounted (inserted into the DOM). It is the first method called.

This is where state variables can be declared.

You should call super(props) before any other statement in the constructor.

---

### Render

This is the only required method in a class component, you must return something from a render function even if it is just `null`.

The render method is called every time the state variables are changed, so don't change state variables in the render method. Rather do all the work in componentDidMount.

---

### componentDidMount

This is called immediately after a component is mounted (inserted into the DOM).

This is a good place to change state variables, make API calls, etc.

---

### componentDidUpdate

This is called immediately after updating occurs.

---

### componentWillUnmount

This is called immediately before a component is unmounted and destroyed.

---

## State

You can think of state as variables that are private to a component and completely controlled by the component.

This is in contrast to props, which are variables passed in to the component.

We can create and initialise state variables in the constructor like this:

```js
constructor(props) {
    super(props)
    this.state = {
        name: "",
        age: 0
    }
}
```

That initialises two state variables, `name` and `age`.

To update state variables we use `this.setState()`.

This will update both the name and age variables:

```js
this.setState({ name: "Blob", age: 4 });
```

> Don't call setState in the render method, and don't just update the state object using `this.state = {name: "Blob", age: 4}`.

---

## Creating a class component

> The code below and be found <a href="https://github.com/NoroffFEU/react-class-component" target="_blank">here</a>. Download and run the repo, open the console and watch for the lifecycle method's console logs being executed. componentWillUnmount will only fire when you navigate to the second link.

To see the lifecycle methods and state in action, we are going to create a class component with the methods mentioned above.

In order to see componentWillUnmount being called, we will use React Router to unmount the class component by mounting a different component in the DOM.

We'll create a new app

```
npx create-react-app react-class-component
```

And install React Router

```
npm install react-router-dom
```

In `src/App.js` we'll replace the default contents with the following

```js
import { BrowserRouter as Router, Switch, Route, Link } from "react-router-dom";
import First from "./components/First";
import Second from "./components/Second";
import "./App.css";

function App() {
	return (
		<Router>
			<div>
				<nav>
					<Link to="/">First</Link> | <Link to="/second">Second</Link>
				</nav>

				<Switch>
					<Route path="/" exact>
						<First />
					</Route>
					<Route path="/second">
						<Second />
					</Route>
				</Switch>
			</div>
		</Router>
	);
}

export default App;
```

Here we have two simple links, and two routes.

-   "/" renders <First /> component
-   "/second" renders <Second /> component

Next we'll create the class component.

In `src/components/First.js`:

```js
import { Component } from "react";

class First extends Component {
	constructor(props) {
		super(props);
		console.log("constructor");
		this.state = { myVar: 0 };
	}

	componentDidMount() {
		console.log("componentDidMount");
		setTimeout(() => {
			this.setState({ myVar: 1 });
		}, 1000);
	}

	componentDidUpdate() {
		console.log("componentDidUpdate");
	}

	componentWillUnmount() {
		console.log("componentWillUnmount");
	}

	render() {
		console.log("render");
		return <div>First. State: {this.state.myVar}</div>;
	}
}

export default First;
```

First we import `Component` from "react" as our class must extend this in order to make the class a React component.

```js
import { Component } from "react";

class First extends Component {
```

First we have the `constructor` method:

```js
constructor(props) {
    super(props);
    console.log("constructor");
    this.state = { myVar: 0 };
}
```

First `super(props)` is called. This calls the class's parent constructor.

We then create a state variable called `myVar` with an initial value of 0.

Once the constructor is done executing, `render` is called.

```js
render() {
    console.log("render");
    return <div>First. State: {this.state.myVar}</div>;
}
```

This method is where our JSX and variables are returned.

Here we are rendering the value of the `myVar` state variable: `{this.state.myVar}`.

The next method to be called will be `componentDidMount`:

```js
componentDidMount() {
    console.log("componentDidMount");
    setTimeout(() => {
        this.setState({ myVar: 1 });
    }, 1000);
}
```

In this method we update the value of the `myVar` variable using `this.setState`.

We are using `setTimeout` to delay the update by 1s to simulate asynchronous code execution like an API call, to delay the console log calls.

Once the state has been updated, the render method will be called again (the render method is called each time the state updates) and then `componentDidUpdate` will be called.

In our componentDidUpdate method we just have a console log to notify us it was called.

The final method, `componentWillUnmount` will only execute when this component is removed from the DOM.

To do this we'll use React Router to load a different component, which will remove the `First` component.

The `Second` component needn't do anything except render a message.

In `src/components/Second.js`:

```js
export default function Second() {
	return <div>Second</div>;
}
```

Now we can click on "Second" link, this component will be rendered, `First` will be unmounted (removed from the DOM) and `componentWillUnmount` will be called.

Remember to watch the console to see the lifeccyle methods console logs being executed.

---

<iframe src="https://player.vimeo.com/video/515704511" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/515704511/9ba3540396" target="_blank">Watch on Vimeo</a>

<a href="github.com/NoroffFEU/react-class-component" target="_blank">Code from the video</a>

---

## Conclusion

That is the very basics of state variables and lifecycle methods.

We will be using only function components going forward, but the lifecycle methods should probably help you understand what is happening when hooks like `useEffect` are used in function components.

---

[Go to the module assignment](ma)

---
