# Lesson 2 - Creating an Axios instance and GET and POST requests

In this lesson we will:

-   create a hook that returns an instance of Axios that will send the JWT in the header of every request
-   create a component that lists all the media uploads in the Wordpress admin section. This component will be used to a select a featured image for a new post
-   make a POST request to the Wordpress REST API to create a new post

---

## Creating an Axios instance

> <a href="https://github.com/NoroffFEU/react-crud/tree/5-axios-instance" target="_blank">This section's branch</a>.

Every time we make a call to the API that requires authorisation we need to send the token in the header.

We could manually add the token in the header for every call.

Using fetch it would look something like this:

```js
const options = {
	headers: { Authorization: `Bearer ${token}` },
};

fetch("some/url", options);
```

With axios it would look like this:

```js
const options = {
	headers: { Authorization: `Bearer ${token}` },
};

axios.get("some/url", options);
```

We'll instead create a custom hook that fetches the token from the context, then adds it to an Axios instance. That way we only have to add the token to the request headers in one place.

We'll also set the base URL so that we don't need to pass the full URL into every request.

In `hooks/useAxios`:

```js
import { useContext } from "react";
import axios from "axios";
import AuthContext from "../context/AuthContext";

const url = process.env.REACT_APP_BASE_URL;

export default function useAxios() {
	const [auth] = useContext(AuthContext);

	const apiClient = axios.create({
		baseURL: url,
	});

	apiClient.interceptors.request.use(function (config) {
		const token = auth.token;
		config.headers.Authorization = token ? `Bearer ${token}` : "";
		return config;
	});

	return apiClient;
}
```

On line 5 we go and ge the URL from the environment configuration file and on line 11 we set it as the base URL of all requests that use this Axios instance.

On line 16 we add the token to the headers object for each request. We get the token from the `auth` variable that we retrieve from the context on line 8.

<iframe src="https://player.vimeo.com/video/520531925" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/520531925/ed0ff8ce5e" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/react-crud/tree/5-axios-instance" target="_blank">Code from the video</a>

---

## Creating a dropdown of media uploads

> <a href="https://github.com/NoroffFEU/react-crud/tree/6-media-dropdown" target="_blank">This section's branch</a>.

Next we'll create a dropdown (select) of all the media uploads in the Wordpress media section.

The media-related API endpoints are listed in <a href="https://developer.wordpress.org/rest-api/reference/media/" target="_blank">the docs</a>.

Using the Axios instance we created above we'll make a GET request to the `/wp/v2/media` endpoint, which will be added to the base URL set in the `useAxios` hook.

Then we'll loop of the results and create an option for each.

<iframe src="https://player.vimeo.com/video/520542855" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/520542855/ee9192719f" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/react-crud/tree/6-media-dropdown" target="_blank">Code from the video</a>

---

# Layout changes and additions

> <a href="https://github.com/NoroffFEU/react-crud/tree/7-layout-changes" target="_blank">This section's branch</a>.

Now we're going to add two new routes for the PostList and AddPost components. We'll also update the Heading component to allow it to return different heading elements.

<iframe src="https://player.vimeo.com/video/520560169" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/520560169/6343becd69" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/react-crud/tree/7-layout-changes" target="_blank">Code from the video</a>

---

## Creating a new post

> <a href="https://github.com/NoroffFEU/react-crud/tree/8-create-post-form" target="_blank">This section's branch</a>.

Now we'll add a form in the AddPost component that will create a new post in Wordpress.

The post-related API endpoints are listed in <a href="https://developer.wordpress.org/rest-api/reference/posts/" target="_blank">the docs</a>.

<iframe src="https://player.vimeo.com/video/520579035" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/520579035/666571878e" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/react-crud/tree/8-create-post-form" target="_blank">Code from the video</a>

---

## Checking for an empty featured media selection

> <a href="https://github.com/NoroffFEU/react-crud/tree/9-empty-media-check" target="_blank">This section's branch</a>.

We have issue with our form - the POST request will fail if we don't select a Featured Media.

The `featured_media` property is an integer in the API, and the default value of the featured_media select box is an empty string.

We need to send in `null` instead `""` if no media is selected.

```js
if (data.featured_media === "") {
	data.featured_media = null;
}
```

<iframe src="https://player.vimeo.com/video/520614031" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/520614031/6ca3ae60a2" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/react-crud/tree/9-empty-media-check" target="_blank">Code from the video</a>

---

[Go to lesson 3](3)

---
