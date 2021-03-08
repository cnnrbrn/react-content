# Lesson 4 - DELETE requests

In this lesson we'll add a delete button to the post's edit form.

## Adding a delete button

> <a href="https://github.com/NoroffFEU/react-crud/tree/13-add-delete-buttons" target="_blank">This section's branch</a>.

<a href="https://developer.wordpress.org/rest-api/reference/posts/#delete-a-post" target="_blank">The API docs</a> tell us the URL to make the DELETE request to is:

```
/wp/v2/posts/<id>
```

<iframe src="https://player.vimeo.com/video/520716356" width="640" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/520716356/8cfb7df74f" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/react-crud/tree/13-add-delete-buttons" target="_blank">Code from the video</a>

---

## Adding a confirmation dialog

> <a href="https://github.com/NoroffFEU/react-crud/tree/14-adding-a-confirmation-dialog" target="_blank">This section's branch</a>.

At the moment the DELETE request runs as soon as the button is clicked.

We can ask for confirmation from the user before deleting by using a `window.confirm` dialog.

The `handleDelete` function inside `DeletePostButton` now looks like this:

```js
async function handleDelete() {
	const confirmDelete = window.confirm("Delete this post?");

	if (confirmDelete) {
		try {
			await http.delete(url);
			history.push("/dashboard/posts");
		} catch (error) {
			setError(error);
		}
	}
}
```

---

[Go to the MA](ma)

---
