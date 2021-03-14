# Lesson 2 - Pre-rendered pages

> The code in this lesson follows that in the previous lesson. The final code is on <a href="https://github.com/NoroffFEU/next-introduction/tree/2-pre-rendered-pages" target="_blank">this branch</a>.

---

## Logging server-side errors

We can't look for server-side errors in the browser console as the code is not being executed in the browser.

When using Next, check for API errors in your VSCode console or any other console you are running the dev command from

```
npm run dev
```

---

## Pre-rendered pages

If you select View Page Source (not inspect element) on a page created with Next, you will see the HTML elements created inside the components. This is because Next generates the initial content on the server and then sends it to the browser.

This is not the case with regular React apps like those generated with Create React app.. With those, only a very small amount of HTML is send to the browser and the rest is filled in by JavaScript.

This is an issue for SEO and pre-rendered apps will improve the ability of search engines to index your content.

## Static Generation and Server-side Rendering

Next provides two different kinds of pre-rendering: static generation and server-side rendering.

### Static rendering

Static rendering happens when the site is built.

All pages that a user can visit must be known and the build process will fetch the data (usually from some kind of API) and build out the HTML with this data.

Static rendering results in very fast delivery times for your content as there is no server processing necessary when a user requests a page - the page has already been built and is simply delivered to the browser.

This kind of rendering is suitable if your content doesn't change that often.

Apart from occuring when your frontend source code changes, builds can be triggered when the data in your API changes.

### Server rendering

Server rendering happens when a user requests a page, not ahead of time. This is is similar to how sites built with a server-side language or framework like PHP and ASP.NET work.

This allows for dynamic content to be produced and is suitable if you have a lot of user generated content, for example reviews or a rating system where the voting totals are constantly being updated.

---

## API calls in Next

The <a href="https://nextjs.org/docs/basic-features/pages" target="_blank">offfical Next docs</a> recommend using static generation for pre-rendering, so we'll use that in this lesson.

We want to store our API URL as a constant. In `constants/api.js`:

```js
export const BASE_URL = "https://t9jt3myad3.execute-api.eu-west-2.amazonaws.com/api/old-games";
```

Because Next pages are pre-rendered on the server, we need an HTTP library we can use in both the server and client (browser) environments. The built-in `fetch` method in JavaScript only works in the browser.

Install axios:

```js
npm install axios
```

For each page in a Next app you can choose whether to staticly generate it or server-side render it.

For static generaion you use the `getStaticProps` function.

For server-side rendering you use the `getServerSideProps` function.

To use it we need to export it from the same file our page function is in.

In `pages/index.js` replace the existing code with this:

```js
import Head from "../components/head";
import Layout from "../components/layout/Layout";
import axios from "axios";
import { BASE_URL } from "../constants/api";

export default function Index(props) {
	// the log here will happen in the browser console
	console.log(props);

	return (
		<Layout>
			<Head title="Next Intro" />

			{props.games.map((game) => {
				return <h3 key={game.slug}>{game.name}</h3>;
			})}
		</Layout>
	);
}

export async function getStaticProps() {
	// in case there is an error in the API call
	// we'll send an empty array in as the prop
	let games = [];

	try {
		const response = await axios.get(BASE_URL);
		// the log here will happen on the server, you can check the console in your editor
		console.log(response.data);
		// the array is on the response.data.data property
		games = response.data.data;
	} catch (error) {
		console.log(error);
	}

	// the props object we return here will become the props in the component
	return {
		props: {
			games: games,
		},
	};
}
```

There are two main parts to this.

On lines 6 - 19 we have a normal component. This component receives props, loops through the games prop and renders the name of each game in the map function.

Any values console logged here will be logged in the browser as usual.

After that component comes the server-side call, the async `getStaticProps` function.

Inside this `getStaticProps` function we are using `axios` to make the API call.

The return value from the API can be found on the `response.data` property. But the array we want to use is in turn on the `.data` property. So it's the `response.data.data` property we want to use from this call, that's the array of games.

Logging values in this function will display the results in your editor's terminal, not the browser. This is because this call is happening on the server, not in the browser.

We return an object from the function with a property of `props` that will become the `props` object that gets passed into our `Index` component.

<img src="/images/next-api-calls-1.png" style="max-width: 800px" />

The object returned from `getStaticProps` has a `props` property. This props property becomes the props object passed in to the Index component.

It's essentially the same result as doing this

```jsx
<Index games=[arrayofGames] />
```

It's passing a prop called `games` in to the Index component.

When using Next and the `getStaticProps` method, the API call will happen at build time, not run time.

## Building a Next app

Run

```
npm run build
```

Then check the `.next/server/pages folder in your editor.

Open `index.html` and you'll see the HTML for the each game there. It's this file that is the pre-rendered HTML that Next sends to browsers.

This results in much higher page speeds as there is no server-side processing, the browser just gets sent the `index.html` file.

<!-- ---

[Go to lesson 3](3)

--- -->
