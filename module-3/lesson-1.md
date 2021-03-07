# Lesson 1

There are two parts to our project:

-   the API
-   the frontend

The API we are going to use Wordpress REST API.

And we'll use React for the frontend, of course.

These are two entirely separate entities. Do not combine the code for each.

## Configuring JWT in Wordpress

In order to complete certain API calls like creating or deleting posts or fetching a list of media uploads, we need to be authorised.

The authorisation options for Wordpress REST API include Basic Auth, JSON Web Tokens and OAuth.

We'll use (JWT) for authorisation and set it up with <a href="https://wordpress.org/plugins/jwt-authentication-for-wp-rest-api/" target="_blank">this plugin</a>.

Install and activate the plugin and configure the .htaccess and wp-config.php files as indicated in the docs.

<iframe src="https://player.vimeo.com/video/520281229" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/520281229/d3f527f43e" target="_blank">Watch on Vimeo</a>

---

## The packages we'll use

We're going to use <a href="https://github.com/axios/axios" target="_blank">Axios</a> to make all our API requests instead of fetch as the syntax is shorter and we can create an instance of it with our JWT token attached so we don't have to manually set it on every request. More on that later.

These are all the packages we'll use:

```
npm i axios react-hook-form @hookform/resolvers yup react-router-dom
```

All styling will be kept to a minimum and we won't use a UI library in order to keep the code as readable as possible.

---

## Environment variables in Create React App

> The code in the lesson sections is on different branches of the repo.

> <a href="https://github.com/NoroffFEU/react-crud" target="_blank">This section's branch</a>.

Environment variables allow us to configure variables for different environments, rather than hard coding them and having to manually change them when moving your code from your local machine

So you may want a different base API URL for when you are running your app locally as opposed to on your own server or through a service like Netlify.

We'll create a .env.development` file to hold our API URL for when we are running locallly:

```js
REACT_APP_BASE_URL = "http://localhost:8080/wordpress-api/wp-json/";
```

We can access the current environment like this:

```js
process.env.NODE_ENV;
```

In a Create React App, when you run `npm run start` you will be in `development` mode.

We can access environment variables like this (`src/constants/api.js`):

```js
export const BASE_URL = process.env.REACT_APP_BASE_URL;
```

More production mode values you can use a `.env.production` file but you should consult the service you use for hosting. <a href="https://docs.netlify.com/configure-builds/environment-variables/" target="_blank">Here</a> are the relevant Netlify docs.

To test a production build you can first install `serve` globally:

```
npm install -g serve
```

Run this to create the production build:

```
npm run build
```

And then this to serve the build:

```
serve -s build
```

You can read more about using environment variables in a Create React App <a href="https://create-react-app.dev/docs/adding-custom-environment-variables/" target="_blank">here</a>.

---

In this video the environment API variable is created and initial routes set up.

<iframe src="https://player.vimeo.com/video/520302542" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/520302542/fdc250ec48" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/react-crud" target="_blank">Code from the video</a>

---

## Login form

> <a href="https://github.com/NoroffFEU/react-crud/tree/2-login-form" target="_blank">This section's branch</a>.

The Wordpress JWT plugin we installed above provides an endpoint we can call with a POST request

```
/wp-json/jwt-auth/v1/token
```

We'll send in a username and password and if accepted we'll be logged in and receive a token back.

<img src="/images/react-crud-1.png" style="max-width:600px" />

In this video we'll create the <a href="https://github.com/NoroffFEU/react-crud/blob/2-login-form/src/components/login/LoginForm.js" target="_blank">login form</a> using React Hook Form and yup for validation and make a request to the URL to try and login.

We'll use axios to make the request but fetch could be used just as easily.

<iframe src="https://player.vimeo.com/video/520324392" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/520324392/3c99f86c15" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/react-crud/tree/2-login-form" target="_blank">Code from the video</a>

---

## The Context API

> <a href="https://github.com/NoroffFEU/react-crud/tree/3-useContext" target="_blank">This section's branch</a>.

Once we've successfully logged in we want to update the Nav component and change the menu links based on the logged in status.

But there is no relationshop between the LoginForm component and the Nav component, one is not the child of the other so we can't use props to pass values in.

<img src="/images/react-crud-2.png" style="max-width:600px" />

In a case like this we can use the <a href="https://reactjs.org/docs/context.html" target="_blank">Context API</a>, which will provide us a kind of global state, somewhere to store variable and functions.

-First we create a context by using React's `createContext`.

-   Then we create the variables and functions we want to make available to components. We pass these into the context's `Provider`.
-   We wrap all the components we want to give access to this context to in the Provider.
-   Finally we use the `useContext` hook to access the context's variables and functions inside the components

<img src="/images/react-crud-3.png" style="max-width:600px" />

In this video we'll create a context for the auth state and access it using useContext in both the LoginForm and Nav components.

We'll also use the <a href="https://reactrouter.com/web/api/Hooks/usehistory" target="_blank">useHistory hook</a> from React Router to programatically navigate to routes when we log in and log out.

<iframe src="https://player.vimeo.com/video/520365138" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/520365138/733d688e3f" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/react-crud/tree/3-useContext" target="_blank">Code from the video</a>

---

## Storing auth state in local storage

> <a href="https://github.com/NoroffFEU/react-crud/tree/4-context-in-local-storage" target="_blank">This section's branch</a>.

When we want to share functionality across components we can create our own hooks, then import and call them.

Hooks are just functions, but we need to begin the name of our custom hook's with the word "use".

Currently in our app if we reload the page our auth state will disappear and we will be in a logged out state. So instead of using `useState` to hold the `auth` variable we'll create a custom hook called `useLocalStorage` to store the auth variable. That way it will be maintained across page refreshes.

And we say "create a custom hook" we really mean copy the code from <a href="https://usehooks.com/useLocalStorage/" target="_blank">this website</a>.

<iframe src="https://player.vimeo.com/video/520388545" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/520388545/eaf6af89a0" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/react-crud/tree/4-context-in-local-storage" target="_blank">Code from the video</a>

---

<!-- ## Lesson Task

There is a practice question in the master branch of <a href="https://github.com/NoroffFEU/lesson-task-workflow2-module3-lesson1" target="_blank">this repo</a>. -->

---

[Go to lesson 2](2)

---
