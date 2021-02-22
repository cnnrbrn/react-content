# Lesson 1 - Introduction, Components and Props

## Introduction

React is a JavaScript library meant to ease front-end development. It involves creating small, reusable components to build user interfaces.

The easiest way to get started with a new React app is to use the <a href="https://create-react-app.dev/docs/getting-started/">Create React App</a> tool.

You simply run this command at a terminal or command line:

```
npx create-react-app my-app
```

Change `my-app` to whatever you want to call your project.

Then change into the newly created directory

```
cd my-app
```

To start the app, run

```
npm start
```

This will start your React app on your localhost port 3000 (or another port if 3000 is already in use).

You can open that link in a browser: `http://localhost:3000`

#### Creating an app in the current directory

If you want to create a new app in the current directory (folder) rather than creating a new one, you can use this command:

```
npx create-react-app .
```

The folder you are in must comply with npm naming restrictions:

-   no capital letters
-   must be URL friendly (no spaces or special characters - you can use hyphens and underscores)

---

### Configuring CRA

Underneath, Create React App (CRA) uses Babel and Webpack, but this is not something you need to configure yourself. If you look at the source code in the app, you won't even see the config files for those two tools.

It's possible to use the "eject" command to eject from CRA and configure everything manually. This can't be undone so be sure this is the action you want to take.

#### CRACO

An alternative to ejecting is to use <a href="https://github.com/gsoft-inc/craco" target="_blank">CRACO</a> - Create React App Configuration Override.

From their README:

> Get all the benefits of create-react-app and customization without using 'eject' by adding a single craco.config.js file at the root of your application and customize your eslint, babel, postcss configurations and many more.
>
> All you have to do is create your app using create-react-app and customize the configuration with a craco.config.js file.

If you want to use Tailwind CSS with a Create React App you will need to use CRACO.

---

## Components

Components are the building blocks of React. They can receive variables (known as props), create and store their own variables (known as state variables) and return JSX (discussed below).

There are two types of React components: function components (which are now the preferred method to use for all components and which we will mostly use) and class components which we will briefly look at.

When creating components, create a `components` folder inside the `src` folder. You can create sub-folders and keep related components in them. For example, all components related to products should go in a `src/components/products` folder.

<img src="/images/react-intro-1.png" style="max-width:600px" />

#### Naming components

A common pattern is to use `camelCase` for your folder names and `PascalCase` for your component names. So you folders will begin with a small letter, and your components with a capitalised letter.

It's not necessary to include the word "component" in a component's name.

Components can be created with either a `.js` or `.jsx` file extension.

---

## Introduction video

This video looks at creating a new React app using CRA and creating our first (function) components.

> Since React version 17, it's no longer necessary to import the React library at the top of a component

<iframe src="https://player.vimeo.com/video/433956978" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/433956978/927a00e23e" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/react-introduction" target="_blank">Code from the video</a>

---

## JSX

The component in `src/App.js` and the ones created in the components folder in the video all return JSX.

JSX is a syntax created by the React team and looks similar to HTML, though it produces React "elements".

JSX is different from the more traditional templating used in other libraries or frameworks like Vue and Angular, and mixes JavaScript and UI (HTML and CSS), something developers new to React often balk at.

You can read more about the React team's decisions to create JSX in the <a href="https://reactjs.org/docs/introducing-jsx.html" target="_blank">offical docs</a>.

## Setting up your editor

Details about configuring the tools below can be found in the <a href="https://create-react-app.dev/docs/setting-up-your-editor" target="_blank">CRA docs</a>.

### ESLint

Create React App includes ESLint, a tool that will give you feedback on your code in the browser console, the terminal/command line where you started the app, or directly in your editor if we install the <a href="https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint" target="_blank">VSCode ESLint extension</a>.

_Always_ check the ESlint output if you run into any problems. The cause of most problems or bugs can be understood and solved by reading ESlint or normal JavaScript error messages in the console or wherever they are emitted.

### Debugging

The video below looks at setting up debugging using the <a href="https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome" target="_blank">VS Code Chrome Debugger</a> extension.

<iframe src="https://player.vimeo.com/video/514286307" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/514286307/b1ede077e6" target="_blank">Watch on Vimeo</a>

---

## React Developer Tools

React Developer Tools is an extension that allows inspection of theReact component hierarchy in the Chrome and Firefox Developer Tools.

An overview of the tools can be found on the browser-specific pages:

-   <a href="https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en" target="_blank">Chrome</a>
-   <a href="https://addons.mozilla.org/en-US/firefox/addon/react-devtools/" target="_blank">Firefox</a>

---

## Using images in JSX

The default content in `src/App.js` in a new app contains an example of using an image in JSX.

First the image is imported

```js
import logo from "./logo.svg";
```

Then used as the value of the `src` attribute in the `img` element

```jsx
<img src={logo} />
```

#### Storing images

Images are not components and images you import and use directly in components shouldn't be stored in the components folder.

One place to store them would be their own images folder.

<img src="/images/react-intro-2.png" style="max-width:600px" />

---

## Props

Props are the values that get passed in to components.

You pass props into components using attribute-like syntax.

In the code below two props called `title` and `subtitle` are being passed into a component called `Heading`.

```jsx
<Heading title="Introduction" subtitle="This is the subtitle">
```

Props are received in components similar to how arguments are in normal JavaScript functions.

```jsx
function Heading(props) {
	console.log(props);
}
```

The `props` variable above will be an object with two properties, `title` and `subtitle`.

```js
{
    title: "Introduction",
    subtitle: "This is the subtitle"
}
```

You can also use object destructuring to retrieve each property from the object in the parenthesis like this:

```jsx
function Heading({ title, subtitle }) {
	console.log(title);
	console.log(subtitle);
}
```

Or inside the function like this:

```jsx
function Heading(props) {
	const { title, subtitle } = props;

	console.log(title);
	console.log(subtitle);
}
```

Here is a video on object destructuring

<iframe src="https://player.vimeo.com/video/460712855" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/460712855/91150dade5" target="_blank">Watch on Vimeo</a>

---

This video looks at using props

<iframe src="https://player.vimeo.com/video/434690419" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/434690419/12a2f07306" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/react-props" target="_blank">Code from the video</a>

---

## Lesson Task

The lesson task is in <a href="https://github.com/NoroffFEU/lesson-task-workflow2-module1-lesson1" target="_blank">this repo</a>, with an example answer on the answer branch.

<!-- There are example answers in the <a href="https://github.com/NoroffFEU/lesson-task-workflow2-module1-lesson1/tree/answers" target="_blank">answers branch</a>. -->

---

## Activities

### Read

-   The <a href="https://reactjs.org/docs/introducing-jsx.html" target="_blank">offical documentation</a> on JSX.
-   The <a href="https://reactjs.org/docs/components-and-props.html" target="_blank">offical documentation</a> on Components and Props.

---

[Go to lesson 2](2)

---
