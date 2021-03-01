# Lesson 2 - map, useEffect and GET requests

## Map method

We are going to use the `map` method to render the results of API calls in React components.

Before using it inside a component, let's see how it works.

Map returns a new when it is called on an existing array. The new array consists of the results of a function being called on each item in the existing array.

### First map example

Take this array of numbers:

```js
const numbers = [1, 2, 3, 4, 5];
```

We'll call the map method on it and multiply each number by 3.

```js
const multipliedNumbers = numbers.map(function (number) {
	return number * 3;
});
```

This will result in a new array containing the an array numbers like this

```js
[3, 6, 9, 12, 15];
```

We could rewrite the map method using an arrow function like this:

```js
const multipliedNumbers = numbers.map((number) => number * 3);
```

So each item in the existing array is passed in one by one to the function inside the map method. The return value of the function becomes an item in the new array.

### Second map example

Let's wrap each of these animal names in a div:

```js
const animals = ["dog", "cat", "elephant"];
```

We'll call the map method on the animals array, pass each animal string value into the function, and return a string with the animal wrapped in a div:

```js
const animalDivs = animals.map(function (animal) {
	return `<div>${animal}</div>`;
});
```

or

```js
const animalDivs = animals.map((animal) => `<div>${animal}</div>`);
```

This will return

```js
["<div>dog</div>", "<div>cat</div>", "<div>elephant</div>"];
```

## useEffect

The `useEffect` hook lets you perform side effects in function components.

The "effect" is the function you pass in to the useEffect hook.

You can think of useEffect as componentDidMount, componentDidUpdate, and componentWillUnmount, from class components, combined. So the function passed in to useEffect will be executed when the component is first mounted in the DOM and when any of it's state variables or props are updated. Basically whenever the component is rendered, the function will be called.

A function returned from useEffect will be called to clean up after the last render before the next render and when the component is removed from the DOM.

## useEffect example

> The code from this example is in <a href="https://github.com/NoroffFEU/useEffect-example" target="_blank">this repo</a>. Run the example and open the console to see the logs from the useEffect hook.

In this example we have three components.

-   <a href="https://github.com/NoroffFEU/useEffect-example/blob/master/src/App.js" target="_blank">src/App.js</a> is exactly the same as the component we used in the class example. It uses React Router to render either the First or Second component.
-   <a href="https://github.com/NoroffFEU/useEffect-example/blob/master/src/components/Second.js" target="_blank">src/components/Second.js</a> simply renders a div with some text.
-   <a href="https://github.com/NoroffFEU/useEffect-example/blob/master/src/components/First.js" target="_blank">src/components/First.js</a> is where we're using `useEffect`:

```js
import { useState, useEffect } from "react";

function First() {
	const [greeting, setGreeting] = useState("Hello");

	useEffect(
		function () {
			console.log("called on render");
			setGreeting("Goodbye");

			return function () {
				console.log("called when cleaning up");
			};
		},
		[greeting]
	);

	return <div className="App">First component: {greeting}</div>;
}

export default First;
```

We're importing both `useState` and `useEffect`.

We create a state variable called `greeting` and give it an initial value of "Hello".

The function inside the useEffect hook is what will be run on each render.

When `First` component is mounted, the console logs the following:

```js
called on render
called when cleaning up
called on render
```

When the component is first mounted "called on render" is logged.

Then `setGreeting` is called and the state variable is updated so the component is re-rendered.

Before re-rendering the function returned from the function (lines 11 - 13) is called so "called when cleaning up" is logged, because that function is executed to clean up after the last render before running the next effect.

Then "called on render" is logged again.

On line 15, there is an array passed as the second argument to useEffect. This array holds the values that when changed the effect should respond to.

In this case the effect will only be run after changes to the `greeting` variable.

If you passed an empty array in (`[]`) the effect would not respond to any changes and would only run once when the component is first mounted.

If you navigate to the Second component you will see "called when cleaning up" gets logged again - similar to `componentWillUnmount` in class components.

---

## Making a GET request in a component with useEffect

> The code from this example is in <a href="https://github.com/NoroffFEU/making-a-GET-request-in-a-component" target="_blank">this repo</a>. Download the repo to follow along.

First we'll create a folder called `constants` that will hold all our constant variables.

We'll place our API path in `src/constants/api.js:

```js
export const API_URL = "https://t9jt3myad3.execute-api.eu-west-2.amazonaws.com/api/books";
```

Calling this URL will return an array of books.

Because we are dealing with books, we'll place our component in a folder called books. BookList seems like a good name for a component which fetches and renders a list of books

In `src/components/books/BookList.js`:

```js
import { useState, useEffect } from "react";
import { API } from "../../constants/api";

function BookList() {
	const [books, setBooks] = useState([]);
	const [loading, setLoading] = useState(true);
	const [error, setError] = useState(null);

	useEffect(function () {
		async function fetchData() {
			try {
				const response = await fetch(API);

				if (response.ok) {
					const json = await response.json();
					console.log(json);
					setBooks(json.data);
				} else {
					setError("An error occured");
				}
			} catch (error) {
				setError(error.toString());
			} finally {
				setLoading(false);
			}
		}
		fetchData();
	}, []);

	if (loading) {
		return <div>Loading...</div>;
	}

	if (error) {
		return <div>ERROR: An error occured</div>;
	}

	return (
		<>
			{books.map(function (book) {
				return <div key={book.id}>{book.title}</div>;
			})}
		</>
	);
}

export default BookList;
```

The video below looks at this code in detail, but we'll discuss it here too.

On lines 5 - 7 we are creating 3 state variables and giving them default values.

The first argument to the `useEffect` hook is a function that will run every time the component renders, unless we pass in an empty array of dependencies as the second argument like we are doing on line 28. Using an empty array there means the effect will run only once, when the component is mounted.

Inside the function we create an async function so that we can use async/await to make the API call.

If the API call is a success, we place results in the `books` state variable using the `setBooks` function.

If there is an error we set the value of the `error` state variable using the `setError` function.

In the finally block on line 24 we set `loading` to false. Because this is being done in the finally block, it will run regardless of whether the fetch request was successful or not.

On lines 30 - 32 we check if `loading` is true. If it is we return some JSX to indicate that the API call is still in progress. Nothing will execute in a function after a return statement is encountered so this is all the component will render.

On lines 34 - 36 we check if `error` has a value. If it does we return an error message.

One lines 38 - 44 we map over the books array and return some JSX for each item. Because the map method returns a new array, an array of JSX will be rendered.

One line 41 we provied a value for the `key` prop. Each parent item returned in an array like this needs a unique key value.

---

<iframe src="https://player.vimeo.com/video/517815053" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/517815053/67234c3040" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/making-a-GET-request-in-a-component" target="_blank">Code from the video</a>

---

## Lesson Task

The lesson task is in <a href="https://github.com/NoroffFEU/lesson-task-js-frameworks-module2-lesson2" target="_blank">this repo</a>, with an example answer on the answer branch.

---

## Activities

## Read

The official docs:

-   <a href="https://reactjs.org/docs/hooks-effect.html" target="_blank">Using the effect hook</a>

---

[Go to lesson 3](3)

---
