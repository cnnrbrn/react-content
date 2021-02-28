# Lesson 1

## Fragments

When returning JSX from a component, we need to always return one node, one element.

If you are returning multiple elements and you don't want to add an unecessary element like a parent div around the elements, use a fragment.

Fragments look like empty tags: `<></>`.

Use them like this:

```js
function ReturnMultiple() {
	return (
		<>
			<p>Paragraph 1</p>
			<p>Paragraph 2</p>
			<p>Paragraph 3</p>
		</>
	);
}
```

---

## Array destructuring

Array destructuring is a method of extracting multiple values from an array and assigning them to variables.

You will use array destructuring when using the `useState` hook in React function components.

Take this code:

```js
const [animal1, animal2, animal3] = ["dog", "cat", "elephant"];
console.log(animal1); //dog
console.log(animal2); //cat
console.log(animal3); //elephant
```

The code above is:

-   assiging the first item in the array ("dog") to the variable `animal1`
-   assiging the second item in the array ("cat") to the variable `animal2`
-   assiging the third item in the array ("elephant") to the variable `animal3`

Basically it's doing this

```js
const animal1 = "dog";
const animal2 = "cat";
const animal3 = "elephant";
```

This works the same if the array is assigned to a variable:

```js
const animals = ["dog", "cat", "elephant"];
const [animal1, animal2, animal3] = animals;
console.log(animal1); //dog
console.log(animal2); //cat
console.log(animal3); //elephant
```

This is the equivalent of doing this:

```js
const animals = ["dog", "cat", "elephant"];
const animal1 = animals[0];
const animal2 = animals[1];
const animal3 = animals[2];
console.log(animal1); //dog
console.log(animal2); //cat
console.log(animal3); //elephant
```

The same applies to an array of variables and/or functions

```js
// create two functions
function A() {
	console.log("I am function A");
}

function B() {
	console.log("I am function B");
}

// place the two functions in an array
const functions = [A, B];

// deconstruct the array of functions
const [firstFunction, secondFunction] = [functions];
firstFunction(); // "I am function A"
secondFunction(); // "I am function B"
```

## The useState hook

According to the official docs, "hooks are functions that let you “hook into” React state and lifecycle features from function components."

Function components in React use the `useState` hook to create and update local state variables.

Let's look at a rudimentary version of the `useState` hook to try get a basic understanding of how it works.

```js
function useState(initialValue) {
	let state = initialValue;

	function getState() {
		return state;
	}

	function setState(newValue) {
		state = newValue;
	}

	return [getState, setState];
}
```

We've called the function `useState` and it receives one argument.

That argument gets assigned to be the value of the variable `state`.

So when we call the `useState` function we'll pass in a initial value and that will be become the value of `state`.

The `getState` function just returns the `state` variable.

The `setState` function receives one argument and assigns it to the `state` variable.

Both `getState` and `setState` are returned from the function in an array.

So if we called useState like this

```js
useState("Mari");
```

the `state` variable would be assigned the value "Blobby".

We can use array destructuring to extract `getState` and `setState` to variables, and because they are variables we can give them any names we want

```js
const [name, setName] = useState("Mari");
```

Now if we call `name` it will return the value of `state`:

```js
name();
// Mari
```

We can change the value of `state` using `setName`:

```js
setName("Jon");
```

Calling `name` again will give us the new value of `state`

```js
name();
// Jon
```

The functions created and exported from their parent function have access to the variables they are "enclosed" with, in this case they both can access the `state` variable even when called from outside the `useState` function.

This is an example of a closure. You can read more about closures <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures" target="_blank">here</a>.

> The example above uses a function to return the state variable, which is different from how React's useState actually works where a variable is returned not a function. The above is a simplified version meant to give an idea of what is going on when using useState.

## Using useState

Using `useState` in a React component takes the following form:

```js
const [name, setName] = useState("Mari");
```

We're creating a state variable called `name` that we can use anywhere within the component.

We're creating a function called `setName` that we can use anywhere in the component to update the value of `name`.

We're using array destruction to create these.

We we call useState we pass in what will be the default value of `name`.

```js
setName("Jon");
```

`name` and `setName` are variables. They can be called whatever you want. It is common to prefix the function that updates the variable with the word "set".

If you wanted to create a state variable called `count` you could use:

```js
const [count, setCount] = useState(0);
```

This gives `count` an initial value of `0`.

## Multiple useState calls

We can use `useState` multiple times in a component:

```js
function Example() {
	const [name, setName] = useState("Mari");
	const [count, setCount] = useState(0);
	const [products, setProducst] = useState([]);
}
```

The third useState call there creates a variable called `products` with an initial value of an empty array.

When making GET requests in a components we might see the following:

```js
function Results() {
	const [loading, setLoading] = useState(true);
	const [error, setError] = useState(null);
	const [results, setResults] = useState([]);
}
```

We'll use state variables like this in the next lesson.

### useState first example

Let's see useState in action.

> The repo for these examples is <a href="https://github.com/NoroffFEU/useState-examples" target="_blank">here</a>.

We'll create a component that returns a button. The button displays the current value of the `name` variable. When the button is clicked `setName` is called and the value of `name` is changed.

Whenever a state variable is updated the component re-renders, it returns its JSX and the UI is updated.

---

Create a new React app.

Import an render the components below in `src/App.js`.

In `src/components/NameButton.js` add:

```js
import { useState } from "react";

function NameButton() {
	const [name, setName] = useState("Mari");

	function handleClick() {
		setName("Jon");
	}

	return <button onClick={handleClick}>{name}</button>;
}

export default NameButton;
```

First we need to import `useState` from React.

We're creating a state variable called `name` and a function to update it called `setName`.

`name` is being used as the text value of the button.

When the button is clicked (note we use `onClick` not `onclick`) the `handleClick` function is called which in turn calls the `setName` function and the value of `name` is updated to "Jon".

We could leave out the `handleClick` function and rewrite like this

```js
function NameButton() {
	const [name, setName] = useState("Mari");

	return <button onClick={() => setName("Jon")}>{name}</button>;
}
```

which is the arrow function version of this

```js
function NameButton() {
	const [name, setName] = useState("Mari");

	return (
		<button
			onClick={function () {
				setName("Jon");
			}}
		>
			{name}
		</button>
	);
}
```

All the above options are fine to use.

You cannot do this:

```jsx
return <button onClick={setName("Jon")}>{name}</button>;
```

as this will execute the `setName` function immediately. It needs to be wrapped in a function so that it is only executed in the click event.

---

## useState count example

This is similar to the example in the <a href="https://reactjs.org/docs/hooks-state.html" target="_blank">React docs</a>.

```js
import { useState } from "react";

function CountButton() {
	const [count, setCount] = useState(0);

	function increment() {
		setCount(count + 1);
	}

	return <button onClick={increment}>{count}</button>;
}

export default CountButton;
```

We create a state variable called `count`, give it a default value of `0` and use it as the button's text value.

The onClick event calls the `increment` function which in turns calls the `setCount` function which updates count's value by 1.

Updating count causes the component to re-render and the new value of count is displayed inside the button.

We could remove the `increment` function and write it like this:

```js
return <button onClick={() => setCount(count + 1)}>{count}</button>;
```

---

## useState nav example

In this example we'll use a state variable called `open` to decide whether the element with a class of `menu` also has a class of `closed`.

```js
import { useState } from "react";

function Nav() {
	const [open, toggleOpen] = useState(false);

	function toggle() {
		toggleOpen(!open);
	}

	return (
		<nav>
			<div>
				Menu
				<button onClick={toggle}>{open ? "Close" : "Open"}</button>
			</div>
			<div className={`menu ${open ? "" : "closed"}`}>
				<a href="/">Home</a>
				<a href="/">About</a>
				<a href="/">Contact</a>
			</div>
		</nav>
	);
}

export default Nav;
```

We're using a ternary to either add an empty string or the string "closed" to the menu element.

```js
${open ? "" : "closed"}
```

This means: if open is true, render an empty string, else render the string "closed".

Similar code is being used for the value of the button:

```js
{
	open ? "Close" : "Open";
}
```

This means: if open is true, render the string "Close", else render the string "Open".

The `.closed` class in the CSS file simply adds a `display: none` to the element.

---

## Activities

## Read

The official docs:

-   <a href="https://reactjs.org/docs/fragments.html" target="_blank">Fragments</a>
-   <a href="https://reactjs.org/docs/hooks-intro.html" target="_blank">Introducing Hooks</a>
-   <a href="https://reactjs.org/docs/hooks-overview.html" target="_blank">Hooks at a Glance</a>
-   <a href="https://reactjs.org/docs/hooks-state.html" target="_blank">Using the State Hook</a>

---

## Lesson Task

The lesson task is in <a href="https://github.com/NoroffFEU/lesson-task-js-frameworks-module2-lesson1" target="_blank">this repo</a>, with an example answer on the answer branch.

---

[Go to lesson 2](2)

---
