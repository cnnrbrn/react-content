# Lesson 3 - React Bootstrap and Routing

## Introduction

The best thing about React components is that they are reusable. Once written, we can simply import them and use them. We can also import components written by other developers.

> The <a href="https://github.com/NoroffFEU/react-with-bootstrap" target="_blank">repo</a> from the lesson.

---

## React UI Libraries

There are several React UI libraries of varying quality, with <a href="https://material-ui.com/" target="_blank">Material-UI</a>, <a href="https://ant.design/" target="_blank">Ant Design</a> and <a href="https://react-bootstrap.github.io/" target="_blank">React Bootstrap being three of the most popular ones.

We are going to look at React Boostrap as we are already familiar with Bootstrap and this library, while not the most exciting, is easy to set up and use.

---

## Using React Bootstrap

> Note on app names: don't call your React app the same name as an NPM package you are going to use, for example don't call an app `react-bootstrap`

We'll start with a new app:

```
npx create-react-app react-with-bootstrap
```

Then install the following

```
npm install react-bootstrap
```

In the public/index.html add a link to the latest Bootstrap CDN CSS file, just as you would for a non-React site.

> At the time of writing, React Bootstrap uses Bootstrap 4.6. Check in the <a href="https://react-bootstrap.github.io/getting-started/introduction" target="_blank">docs</a> for the latest versions.

```html
<link
	rel="stylesheet"
	href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css"
	integrity="sha384-B0vP5xmATw1+K9KRQjQERJvTumQW0nPEzvF6L/Z6nronJ3oUOFUFpCjEUQouq2+l"
	crossorigin="anonymous"
/>
```

The first component we want to use from React Bootstrap is the <a href="https://react-bootstrap.github.io/components/navbar/" target="_blank">Navbar</a>.

As <a href="https://react-bootstrap.github.io/getting-started/introduction#importing-components" target="_blank">stated in the docs</a>, you should import individual components like: react-bootstrap/Navbar rather than the entire library. Doing so pulls in only the specific components that you use, which can significantly reduce the amount of code you end up sending to the client.

```js
import Navbar from "react-bootstrap/Navbar";

// or less ideally
import { Navbar } from "react-bootstrap";
```

You'll be used to seeing Bootstrap code examples as HTML elements with classes. In this library all the elements will be React components.

First we'll use the Navbar and Nav components.

Create `src/components/layout/Layout.js`, import the components and add the following:

```js
import Navbar from "react-bootstrap/Navbar";
import Nav from "react-bootstrap/Nav";

function Layout() {
	return (
		<Navbar bg="dark" variant="dark" expand="lg">
			<Navbar.Brand href="/">React App</Navbar.Brand>
			<Navbar.Toggle aria-controls="basic-navbar-nav" />
			<Navbar.Collapse id="basic-navbar-nav">
				<Nav className="mr-auto">
					<Nav.Link href="/">Home</Nav.Link>
					<Nav.Link href="/about/">About</Nav.Link>
					<Nav.Link href="/contact/">Contact</Nav.Link>
				</Nav>
			</Navbar.Collapse>
		</Navbar>
	);
}

export default Layout;
```

We're using the Navbar's bg and variant props to change the style of the nav to a dark one. We've added About and Contact links.

We'll then replace everything in `src/App.js` with this

```jsx
import Layout from "./components/layout/Layout";

function App() {
	return <Layout />;
}

export default App;
```

We're importing and rendering the Layout component and this will give us a responsive Navbar.

## Routing

We have a link to an About page and a Contact page in our navigation, but no way to display that content.

We will create components to display content on each of those pages, but first let's create a Heading component which will receive a `title` prop and return an `h1` element.

Create `src/components/layout/Heading.js` and add the following:

```jsx
export default function Heading({ title }) {
	return <h1>{title}</h1>;
}
```

> This time we're exporting the component on the same line we create it.

Now let’s add three new components, `Home`, `About` and `Contact`.

Create the following files:

    - src/components/home/Home.js
    - src/components/about/About.js
    - src/components/contact/Contact.js

Import the Heading component in each and set a title value for the prop in each.

In src/components/home/Home.js:

```jsx
import Heading from "../layout/Heading";

export default function Home() {
	return <Heading title="Home" />;
}
```

In src/components/about/About.js:

```jsx
import Heading from "../layout/Heading";

export default function About() {
	return (
		<>
			<Heading title="About" />
			<p>This is the about page</p>
		</>
	);
}
```

In src/components/contact/Contact.js:

```jsx
import Heading from "../layout/Heading";

export default function Contact() {
	return <Heading title="Contact" />;
}
```

### React Router

Now we need something to navigate around the site. We are going to use React Router.

```
npm i react-router-dom
```

> i is shorthand for install

Now we can write code to navigate between these components.

In Layout.js, we need to add more imports and edit what is returned:

```jsx
import Navbar from "react-bootstrap/Navbar";
import Nav from "react-bootstrap/Nav";
import { BrowserRouter as Router, Switch, Route, NavLink } from "react-router-dom";
import Container from "react-bootstrap/Container";
import Home from "../home/Home";
import About from "../about/About";
import Contact from "../contact/Contact";

function Layout() {
	return (
		<Router>
			<Navbar bg="dark" variant="dark" expand="lg">
				<NavLink to="/" exact>
					<Navbar.Brand>React App</Navbar.Brand>
				</NavLink>
				<Navbar.Toggle aria-controls="basic-navbar-nav" />
				<Navbar.Collapse id="basic-navbar-nav">
					<Nav className="mr-auto">
						<NavLink to="/" exact className="nav-link">
							Home
						</NavLink>
						<NavLink to="/about" className="nav-link">
							About
						</NavLink>
						<NavLink to="/contact" className="nav-link">
							Contact
						</NavLink>
					</Nav>
				</Navbar.Collapse>
			</Navbar>
			<Container>
				<Switch>
					<Route path="/" exact component={Home} />
					<Route path="/about" component={About} />
					<Route path="/contact" component={Contact} />
				</Switch>
			</Container>
		</Router>
	);
}

export default Layout;
```

Apart from the new imports, we swapped out the Nav.Link Bootstrap components for NavLink components from React Router.

We added a Switch to contain our Routes.

Each Route points to a component we created above.

The content that the Routes point to will be displayed inside the Container component. This means all the content will have a maximum width, as the Container component has a max width. If you wanted a full-width container you can add the `fluid` prop:

```jsx
<Container fluid>
```

We wrapped the Navbar.Brand in a NavLink that also points to the home page. The / path is the home page. Finally, everything we return is contained in a Router component.

In the browser you will now be able to navigate between the Home, About and Contact components.

Note that navigating between "pages" doesn't cause the browser to reload. React Router decides based on the URL what to render:

-   If the path is "/", render the `Home` component.
-   If the path is "/about", render the `About` component.
-   If the path is "/contact", render the `Contact` component.

#### The exact prop

The `exact` prop in the first route is important.

```jsx
<Route path="/" exact component={Home} />
```

Without it, all routes will return the Home component as the router will encounter "/" and think it has found a route requires a component to be returned.

`exact` makes sure that only the URL/route exactly matchin "/" returns the Home component.

Try remove the `exact` prop and you'll see all routes render the Home component.

The `exact` prop in the first NavLink makes sure that only when the URL is exactly "/" should this link get an "active" class.

```jsx
<NavLink to="/" exact className="nav-link">
```

Try remove the `exact` prop and you'll see that even if the URL is "/about" or "/contact", the Home link also gets an "active" class.

---

<iframe src="https://player.vimeo.com/video/455457887" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/455457887/ad045422a5" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/react-router-dom" target="_blank">Code from the video</a>

---

### Optional videos

These videos are from the CSS Frameworks that you revisit if you want:

-   <a href="https://vimeo.com/434881041/d81ed48eda" target="_blank">Introduction to React Bootstrap</a>
-   <a href="https://vimeo.com/437446618/5959c93d64" target="_blank">Introduction to React Bootstrap Part 2</a>

---
