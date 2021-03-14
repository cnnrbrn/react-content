# Lesson 1 - Introduction to Next.js

> The code from this lesson is <a href="https://github.com/NoroffFEU/next-introduction" target="_blank">here</a>.

---

# Introduction

<a href="https://nextjs.org/" target="_blank">Next.js</a> is a React framework that provides, amongst other things, pre-rendered pages (which will improve your site's SEO), and easy routing and optimised images.

If you are writing a public-facing site and SEO is a concern, it is a good idea to use a solution like Next when creating a React app. <a href="https://www.gatsbyjs.com/" target="_blank">Gatsby</a> is a similar option which we won't be covering.

---

To create a new Next app, we can use a tool called `create-next-app`. You call this the same way you do `create-react-app`:

```
npx create-next-app name-of-app
```

> To create a new Next app directly in the current directory use `npx create-next-app .`

Always check the `scripts` section of package.json for the commands you can run. In a Create Next app the command to start the dev server is

```
npm run dev
```

The suggestions in the Next console use <a href="https://yarnpkg.com/" target="_blank">yarn</a> as the runner

<img src="/images/next-intro-1.png" style="max-width: 400px" />

You can use `yarn` or `npm` to install packages, just make sure you only have one of `yarn.lock` or `package-lock.json` otherwise you may run into versioning errors. (These lock files are created when you run installations (`npm install`or`yarn`), not when running commands like `start`or`dev`.

If you have another app running on port 3000, you can set the port number with a `-p` switch:

```
npm run dev -- -p 3001
```

---

## App structure

Create Next app creates three folders for us:

-   `components` - all our components apart from our page components will go in here.
-   `pages` - this will hold the components that will serve as the pages in our app.
-   `static` or `public` - if your version of create-next-app calls this folder "static", please rename it to "public". This is where things like images and favicons will go.

You can delete components/nav.js.

---

## Routing

Every file in the `pages` folder, such as `about.js` or `contact.js`, will become a route we can navigate to without having to use a library like `react-router`.

The file names must match the browser url you want. If you want your About Us page to have the path `/about-us`, the file in pages must be called `about-us.js`.

Open `pages/index.js`, this is the default page, served at the root path: `http://localhost:3000`

Delete everything inside `index.js` and replace it with the following:

```js
import Link from "next/link";
import Head from "../components/head";

export default function Index() {
	return (
		<>
			<Head title="Next Intro" />

			<Link href="/about">
				<a>About</a>
			</Link>
		</>
	);
}
```

Page components are normal React components.

`Head` is a component provided by `create-next-app` that we can use on each page to create the contents of the HTML `<head>` tags, such as the `title` tag and `description` meta tag. Above we are using the `title` prop to set the title of the index page to "Next Intro". Because Next provides pre-rendered page generation, these values are easier found by search bots, improving your site's SEO.

`Head` utilises a component imported from `next/head` to append elements to the `head` tag. We'll use this same component shortly.

`Link` is a component provided by Next we can use to navigate around our app, similar to `Link` or `NavLink` in React Router. We are setting the `href` prop to `/about` which means clicking the link will navigate to `http://localhost:3000/about`. If you vist that page now you'll be greeted by Next's built-in 404 component.

---

## Adding a page

Create the file `pages/about.js` and add the following:

```js
import Link from "next/link";
import Head from "../components/head";

export default function About() {
	return (
		<>
			<Head title="About | Next Intro" />

			<Link href="/">
				<a>Home</a>
			</Link>
		</>
	);
}
```

We've changed the title in the `Head` component and the `Link` component will take us back to the `index` page. (You may have to reload to see the changes when adding a new page).

---

## Adding a layout component

We want to wrap all our pages in a component that contains a `nav` and other elements so that we don't need to add these to each page individually.

Create `components/layout/Layout.js` and add the following:

```js
import Link from "next/link";

export default function Layout({ children }) {
	return (
		<>
			<nav>
				<Link href="/">
					<a>Home</a>
				</Link>
				<Link href="/about">About</Link>
			</nav>

			<div className="container">{children}</div>
		</>
	);
}
```

The important things there are the `nav` and that we are rendering the `children` prop. Eveything passed inbetween `<Layout>` and `</Layout>` tags will be rendered there.

To see this work import Layout in both `index` and `about`, replace the fragments with `Layout` and remove the `Link` components in both pages - the links will appear on both pages as we are wrapping both pages in `Layout`.

`pages/index.js` now looks like this:

```js
import Head from "../components/head";
import Layout from "../components/layout/Layout";

export default function Index() {
	return (
		<Layout>
			<Head title="Next Intro" />
			<p>Home content</p>
		</Layout>
	);
}
```

---

## The \_app.js page

Next.js uses the App component to initialize pages and we can override it and control the page initialization. Doing this will allow us to, among other things, add global styles.

We currently have no styling in the app.

First let's create some basic global styles in `styles/style.css`:

```css
body {
	font-family: sans-serif;
	margin: 0;
}

nav {
	background: darkred;
	padding: 1rem;
}

nav a {
	color: white;
	margin-right: 10px;
}

.container {
	max-width: 1000px;
	margin: 0 auto;
	padding: 1rem;
}
```

Then create `pages/_app.js` and add:

```js
import "../styles/style.css";

export default function MyApp({ Component, pageProps }) {
	return <Component {...pageProps} />;
}
```

Stop and restart the app to see the styles being applied.

---

[Go to lesson 2](2)

---
