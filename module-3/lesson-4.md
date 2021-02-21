# Lesson 4 - Introduction to GraphQL

## Introduction

<a href="https://graphql.org/" target="_blank">GraphQL</a> provides a different type of API to REST APIs.

In a REST API you have multiple endpoints all returning different data or performing different requests.

With GraphQL, there is only one endpoint and you send this endpoint a query telling the API what you want to do and what you want returned.

The video below looks at making a request (the equivalent of a REST API GET request) using the Apollo client and the <a href="https://graphqlzero.almansi.me/" target="_blank">GraphQLZero</a> fake GraphQL API.

<iframe src="https://player.vimeo.com/video/509843576" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/509843576/1b63d4d0dc" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/introduction-to-graphql" target="_blank">Code from the video</a>

---

## The result of making a REST or a GraphQL call is the same

Whether you use a REST API or a GraphQL API, you are going to get a JSON response back.

Take this REST GET request:

```js
async function makeRestCall() {
	const restUrl = "https://jsonplaceholder.typicode.com/posts";
	const response = await fetch(restUrl);
	const json = await response.json();
	console.log(json);
}

makeRestCall();
```

When the `json` variable is logged, you will see an array of 100 objects that you would loop through and do something with. All fields/properties that the developers of the API decided to return are returned.

Now look at this GraphQL query:

```js
import ApolloClient, { gql } from "apollo-boost";

async function makeGQLCall() {
	const gqlUrl = "https://graphqlzero.almansi.me/api";

	const client = new ApolloClient({
		uri: gqlUrl,
	});

	const json = await client.query({
		query: gql`
			{
				posts {
					data {
						id
						title
					}
				}
			}
		`,
	});
	console.log(json.data.posts.data);
}

makeGQLCall();
```

It's a completely different way of making the API call, but the result is similar: 100 objects, though this time only the fields we requested are returned, ie. `id` and `title`.

In both methods we are assigning the result of the API call (the JSON returned by the call) to a variable called `json`.

Once assigned to that variable, we would manipulate it exactly the same way.

```js
for (let i = 0; i < json.length; i++) {
	// log each object
	console.log(json[i]);
}
```

---

## Activity

### Read

<a href="https://www.apollographql.com/blog/graphql-vs-rest-5d425123e34b/" target="_blank">GraphQL vs. REST</a>

---

[Go to the MA](ma)

---
