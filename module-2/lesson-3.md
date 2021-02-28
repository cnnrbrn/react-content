# Lesson 3

## PropTypes

We can use PropTypes (property types) to check that we are passing in the correct value types to props so that we don't, for example, call strimg methods on non-string values, try to do arithmetic on strings or print objects. This is a design time warning only, PropTypes won't stop you passing incorrect values as props.

> The code from this example is in <a href="https://github.com/NoroffFEU/PropTypes-examples" target="_blank">this repo</a>. Run the example, open the browser console and change or remove the prop values to see the warnings.

In this example we importing 3 components in `App.js`.

In `src/components/Heading.js`:

```js
import PropTypes from "prop-types";

export default function Heading({ title }) {
	return <h1>{title}</h1>;
}

Heading.propTypes = {
	title: PropTypes.string,
};
```

We're importing `PropTypes` from the `prop-types` package.

We've added a `propType` (lower case p) property to the component, and inside that object we are setting the `title` prop to be string value.

Now if you try pass in a non-string value as the title prop value (you pass in non-string values to props inbetween curly braces):

```jsx
<Heading title={56} />
```

You will see an error like this:

<img src="/images/proptypes-1.png" style="max-width: 700px" />

### Children props

The `children` prop is what is passed into a component inbetween its opening and closing tags:

```js
<Paragraph>This is the content</Paragraph>
```

The children could be anything that can be rendered (strings, numbers, arrays or elements). We can use the node property as the prop type;

```js
import PropTypes from "prop-types";

function Paragraph({ children }) {
	return <p>{children}</p>;
}

Paragraph.propTypes = {
	children: PropTypes.node.isRequired,
};

export default Paragraph;
```

### Required props

In the above example we are marking the children prop as required using `isRequired`. After adding this not providing this prop will produce a warning.

### Default prop values

In `src/components/WithTax.js` we are providing default values for both props:

```js
import PropTypes from "prop-types";

function WithTax({ amount, taxRate }) {
	const total = amount + amount / taxRate;
	return <div>{total}</div>;
}

WithTax.propTypes = {
	amount: PropTypes.number,
	taxRate: PropTypes.number,
};

WithTax.defaultProps = {
	amount: 0,
	taxRate: 10,
};

export default WithTax;
```

If either of those prop values are omitted they will be assigned the default values set in the `defaultProps` object.

---

## Passing a URL param and making an API request to fetch a single result

In this section we want to:

-   fetch a list of results and display each as a link that passes a parameter value in the URL to a details page
-   on the details page fetch parameter value and make another API call with it, this time to fetch a single result not a list
-   display the single result

We'll use React Router again.

> The code from this example is in <a href="https://github.com/NoroffFEU/react-router-url-params-and-single-resource-requests" target="_blank">this repo</a>.

In `src/App.js` we have two routes created with React Router.

```jsx
<Route path="/" exact>
    <BookList />
</Route>
<Route path="/detail/:id">
    <BookDetail />
</Route>
```

-   "/" - renders the BookList component
-   "/detail/:id" - renders the BookDetail component. We are creating a URL parameter with this route called "id". We can call this anything. In BookDetail we will retrieve this parameter.

The code in `src/components/books/BookList.js` is very similar to the code from the previous lesson.

The only differences are inside the return:

```js
return (
	<div className="books">
		{books.map(function (book) {
			const { id, title, published } = book;
			return <BookItem key={id} id={id} title={title} published={published} />;
		})}
	</div>
);
```

Here we retrieving the `id`, `title` and `published` properties from the `book` object using object destructuring and then passing them in as props to a component called `BookItem`.

So the map method will produce an array of BookItem components that will be rendered inside the div with the class of "books".

Let's look inside `src/components/books/BookItem.js`:

```js
import PropTypes from "prop-types";
import { Link } from "react-router-dom";

function BookItem({ id, title, published }) {
	return (
		<Link to={`detail/${id}`}>
			<h5>{title}</h5>
			<p>{published}</p>
		</Link>
	);
}

BookItem.propTypes = {
	id: PropTypes.number.isRequired,
	title: PropTypes.string.isRequired,
	published: PropTypes.string.isRequired,
};

export default BookItem;
```

We are using `Link` from React Router to create links that go to the details page. The `id` prop is added to the URL path, eg. `detail/5`. The `BookDetail` component is rendered the this path is navigated to.

We have added PropType checks for each prop.

The code in `src/components/books/BookDetail.js` is similar to `BookList` with some noticeable differences:

```js
import { useState, useEffect } from "react";
import { useParams, useHistory } from "react-router-dom";
import { API } from "../../constants/api";

function BookDetail() {
	const [book, setBook] = useState(null);
	const [loading, setLoading] = useState(true);
	const [error, setError] = useState(null);

	let history = useHistory();

	const { id } = useParams();

	if (!id) {
		history.push("/");
	}

	const url = API + "/" + id;

	useEffect(
		function () {
			async function fetchData() {
				try {
					const response = await fetch(url);

					if (response.ok) {
						const json = await response.json();
						console.log(json);
						setBook(json);
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
		},
		[url]
	);

	if (loading) {
		return <div>Loading...</div>;
	}

	if (error) {
		return <div>An error occured: {error}</div>;
	}

	return (
		<div className="book-detail">
			<h1>{book.title}</h1>
			<p>{book.description}</p>
		</div>
	);
}

export default BookDetail;
```

We are importing both the `useParams` and `useHistory` hooks from React Router.

On line 10 we call the `useHistory` hook and assign it to the `history` variable.

On line 12 we retrieve the `id` from the URL. This is the paramater name created here in App.js:

```js
<Route path="/detail/:id">
```

If we had done this instead, calling it `name` instead of `id`:

```js
<Route path="/detail/:name">
```

we'd need to get the parameter like this

```js
const { name } = useParams();
```

On lines 14 - 16 we check if `id` exists (if that parameter is in the URL) and if it doesn't exist we use the `push` method on `history` to redirect to the home page. We do this as we need `id` to have a value in order to make the API call to fetch a single result. If `id` doesn't exist we don't want to continue with the rest of the code.

One line 18 we are creating the `url` variable we will use in the fetch request.

On line 29 we call `setBook` and pass in `json` not `json.data`. There is no `data` property in the result of this call.

> Always check the json that is returned from an API call. Do not make assumptions about what is returned.

In the return beginning on line 52 we don't use the map method because this API call does not return an array of results, it returns only one result.

We're going to note this again because it keeps happening:

> Always check the json that is returned from an API call. Do not make assumptions about what is returned.

---

---

[Go to lesson 4](4)

---
