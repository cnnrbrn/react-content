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

-   `pages` - this will hold the components that will serve as the pages in our app. It also holds a special file called `_app.js` and an `api` folder, both of which will be discussed later.
-   `styles` - to hold css or scss files
-   `public` - This is where things like images, favicons and robot.txt will go.

Create a new folder called `components`. This is where we'll create all our non-page components.

---

## Styles

The `styles` folder contains 2 files:

-   `globals.css` - this is being imported in `pages/_app.js` and the styles from there are being applied to the entire site
-   `Home.module.css` - a CSS Module file that is being imported in `pages/index.js` and is only being applied to that page/component.

Open `pages/index.js` and you will see the `Home.module.css` import and the styles from it being applied, for example:

```jsx
<div className={styles.container}>
```

It is not necessary to use CSS Modules with Next.js.

---

## Routing

Every file in the `pages` folder, such as `about.js` or `contact.js`, will become a route we can navigate to without having to use a library like `react-router`.

The file names must match the browser url you want. If you want your About Us page to have the path `/about-us`, the file in pages must be called `about-us.js`.

Open `pages/index.js`, this is the default page, served at the root path: `http://localhost:3000`

Change the contents of `index.js` to this:

```js
import Link from "next/link";
import Head from "next/head";

export default function Home() {
	return (
		<>
			<Head>
				<title>Create Next App</title>
				<link rel="icon" href="/favicon.ico" />
			</Head>

			<Link href="/about">
				<a>About</a>
			</Link>
		</>
	);
}
```

Page components are normal React components.

We've simplified the JSX and removed the CSS module. We'll just use the global CSS file.

The `Head` component from "next/head" allows us to create the contents of the HTML `<head>` tags, such as the `title` tag and add a favicon link. Because Next provides pre-rendered page generation, values like the title and description are easier found by search bots, improving your site's SEO.

`Link` is a component provided by Next we can use to navigate around our app, similar to `Link` or `NavLink` in React Router. We are setting the `href` prop to `/about` which means clicking the link will navigate to `http://localhost:3000/about`. If you visit that page now you'll be greeted by Next's built-in 404 component.

---

## Adding a page

Create the file `pages/about.js` and add the following:

```js
import Link from "next/link";
import Head from "next/head";

export default function About() {
	return (
		<>
			<Head>
				<title>About | Create Next App</title>
			</Head>

			<Link href="/">
				<a>Home</a>
			</Link>
		</>
	);
}
```

We've changed the title in the `Head` component but we didn't add a favicon link, so now the about page won't have a favicon. We don't want to repeat ourselves by adding the same favicon code on every page so this is a good time to add a reusable component.

The `Link` component will take us back to the `index` page. (You may have to reload to see the changes when adding a new page).

---

## Creating our own Head component

In `components/layout/Head.js`:

```js
import NextHead from "next/head";

export default function Head({ title = "" }) {
	return (
		<NextHead>
			<title>
				{title}
				{title ? " | " : ""}Create Next App
			</title>
			<link rel="icon" href="/favicon.ico" />
		</NextHead>
	);
}
```

We're importing the `Head` component from "next/head" as `NextHead` because we are naming our own component `Head`. We can't have two Heads. Since Head is a default export from "next/head" we can import it as any name we like.

We have a `title` prop with a default empty string value and on line 8 we check if the title is empty. If it is we don't add the `|` to the title.

Now we only have to pass the left side of the title (left of the |) in to the Head component.

This component could easily be extended to include things like a description meta tag.

> For brevity we have not included Prop Type checks but your code should include them.

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

The important things there are the `nav` and that we are rendering the `children` prop. Everything passed inbetween `<Layout>` and `</Layout>` tags will be rendered there.

To see this work import Layout in both `index` and `about`, replace the fragments with `Layout` and remove the `Link` components in both pages - the links will appear on both pages as we are wrapping both pages in `Layout`.

`pages/index.js` now looks like this:

```js
import Head from "next/head";
import Layout from "../components/layout/Layout";

export default function Home() {
	return (
		<Layout>
			<Head />

			<div className="container">
				<h1>Home page</h1>
			</div>
		</Layout>
	);
}
```

Everything between `<Layout>` and `</Layout>` becomes the `children` prop in the Layout component.

We're not passing a `title` prop into the `<Head>` so it will contain on the default value in the title tag.

---

## The \_app.js page

Next.js uses the App component to initialize pages and we can override it and control the page initialization. Doing this will allow us to, among other things, add global styles.

Open `pages/_app.js`.

We're already importing our global CSS file.

```js
import "../styles/globals.css";
```

We're going to replace the styles in that file with:

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

Our nav is at least moderately styled now.

---

## Activities

### Read

<a href="https://nextjs.org/docs/basic-features/built-in-css-support" target="_blank">The offfical docs</a> on Next's CSS support.

---

[Go to lesson 2](2)

---
