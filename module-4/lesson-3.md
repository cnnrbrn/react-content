<!-- # Lesson 3 - Dynamic routes

In this lesson we want to render a game page for each game, according to its `slug` property.

Often we fetch an invididual resource by its `id` property, but this time we are using the slug property.

---

## Dynamic routes

We want each individual game to appear at a path like this: `http://localhost:3000/game/slug-for-game`

To set up a dynamic route for this we create a path with square brackets, a folder called `game` and then a file in the game folder called `[slug].js`

<img src="/images/next-dynamic-routes-1.png" style="max-width: 400px">

---

## Creating the list of games index.js

```js
export default function Index(props) {
	return (
		<Layout>
			<Head title="Next Intro" />

			{props.games.map((game) => {
				return (
					<a key={game.slug} href={`game/${game.slug}`}>
						{game.name}
					</a>
				);
			})}
		</Layout>
	);
}
```

We are updating the component to return a list of links pointing to a `game/slug` path.

---

## [slug].js

The code in `[slug].js` is going to have 3 parts:

-   The page component which will receive a `game` prop and render whatever properties we want from it
-   There will again be a `getStaticProps` function. This will make a call to fetch an invidual game
-   A `getStaticPaths` function. This will produce an array of paths we want to create

If you look at the first two objects returned in the base API call they have these two slug properties:

-   `capitalism-2`
-   `jackie-chan-adventures`

So we want to create paths like paths:

-   `game/capitalism-2`
-   `game/jackie-chan-adventures`

<img src="/images/next-dynamic-routes-2.png" style="max-width: 900px">

```js
export default function Game({ game }) {
	return (
		<Layout>
			<Head title={game.name} />
			<h1>{game.name}</h1>
			<img src={game.image} />
		</Layout>
	);
}

export async function getStaticPaths() {
	try {
		// the same API call we used in `index.js`
		// we want to get all the slugs from the array of games
		// so first we need to fetch the games
		const response = await axios.get(BASE_URL);
		// the log here will happen on the server, you can check the console in your editor
		console.log(response.data);
		// the array is on the response.data.data property
		const games = response.data.data;

		// Get the paths we want to pre-render based on the slugs in the games
		const paths = games.map((game) => ({
			params: { slug: game.slug },
		}));

		console.log(paths);

		return { paths: paths, fallback: false };
	} catch (error) {
		console.log(error);
	}
}

export async function getStaticProps({ params }) {
	const url = `${BASE_URL}/${params.slug}`;

	let game = null;

	try {
		const response = await axios.get(url);
		// the value we want is on response.data here, not response.data.data
		game = response.data;
	} catch (error) {
		console.log(error);
	}

	// we are sending a prop called game in to the Game component up above
	return {
		props: { game: game },
	};
}
```

Lines 1 - 9 is the page component that receives the `game` prop.

One line 16 - 20 in the `getStaticPaths` an API call to the same URL we called in `index.js` is made. We want to get all the slug properies from the array of games so first we need to get that games array.

On lines 23 - 25 we create a an array of objects with a params property.

Look at the output of the console.log on line 17 (in the terminal/command line not the browser):

```
{ params: { slug: 'capitalism-2' } },
{ params: { slug: 'jackie-chan-adventures' } },
...
```

The `paths` array is used to create all the static pages.

---

[Go to lesson 2](4)

--- -->
