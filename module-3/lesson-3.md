# Lesson 3

In this lesson we'll:

-   create a list of posts that will link to an Edit Post page, passing the id of the post in the URL
-   update a post using a PUT request

---

## Creating a list of posts

In this section we make a GET request to fetch all the posts from Wordpress and display their titles in a list.

We'll link from each post to an Edit Post component, with the id of the post in the URL.

<iframe src="https://player.vimeo.com/video/520631327" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/520631327/bc5215a09e" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/react-crud/tree/10-post-list" target="_blank">Code from the video</a>

---

## Updating a post

When click on post from the list we'll navigate to the Edit Post component.

In this component we'll:

1. Get the id (this won't always be an id or called "id") from the URL
2. Create a URL with the id and make a GET request with the URL
3. Populate the form with values returned from the request
4. When the form is submitted make a PUT request to update the resource

<img src="/images/react-crud-4.png" style="max-width:400px" />

---

<iframe src="https://player.vimeo.com/video/520683383" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/520683383/9971e172bb" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/react-crud/tree/11-edit-post-form" target="_blank">Code from the video</a>

---
