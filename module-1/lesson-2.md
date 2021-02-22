# Lesson 2 - Styling a React app

## Introduction

In an app created with Create React App, regular CSS is used for styling.

In `src/App.js`, there is an example of the CSS being imported.

```js
import "./App.css";
```

The styles imported here will apply globally to the app. If you wanted to import styles that would only apply locally to the component in which they are imported, you can CSS Modules (discussed below).

This lesson looks at three other methods for styling a React app:

-   Sass
-   CSS Modules
-   Styled Components

The currently popular Tailwind CSS is also briefly mentioned.

> You don't need to be proficient in all of these methods. You can style your React apps using your preferred method and come back to the other methods another time.

## Adding Sass to a Create React App

This is very simple.

Just install `node-sass`

```
npm install node-sass
```

And then you can change your `.css` extensions to `.scss` on both the file names themselves and the imports.

```js
import "./App.scss";
```

The CRA docs for using Sass are <a href="https://create-react-app.dev/docs/adding-a-sass-stylesheet/" target="">here</a>.

---

<iframe src="https://player.vimeo.com/video/434108216" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/434108216/b291429de4" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/react-introduction/tree/add-sass" target="_blank">Code from the video</a>

---

## CSS Modules

CSS Modules allow you to write styles that are locally scoped, meaning they only apply to the component they are imported in.

When using CSS or Sass you create global styles so need to worry about class name clashes, and should consider using naming conventions like BEM.

When using locally scoped styles, there is no need to worry about name clashes.

<iframe src="https://player.vimeo.com/video/437826097" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/437826097/e679370d87" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/css-modules-introduction" target="_blank">Code from the video</a>

---

## Styled Components

Styled Components is an example of a CSS-in-JS library, where JavaScript is used to style components. <a href="https://medium.com/the-non-traditional-developer/beginners-guide-to-tagged-template-literals-in-javascript-202d1805e862" target="_blank">Tagged template literals</a> are used to apply styles in the library.

Similar to CSS Modules, Styled Components results in local rather than global styles, though the library does provide for global styles as well as theming. Both are covered in the video below.

<a href="https://styled-components.com/docs">Here</a> are the official docs.

A big benefit of using Styled Components is being able to pass in a prop to the styled component (it is a component after all) and change styles based on the value of that prop.

A downside of using Styled Components is that you need to come up with even more names for components.

To install the Styled Components package run:

```
npm install styled-components
```

<iframe src="https://player.vimeo.com/video/437837847" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/437837847/46a27178c9" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/styled-components-introduction" target="_blank">Code from the video</a>

---

## Tailwind CSS

Tailwind is popular at the moment and is basically a library of utility classes that you add to your HTML elements or React components.

Depending on how you like to work, it can be an efficient way to style projects though you do end up with a lot of classes in your markup/JSX.

It will take a little while to get to know the class names, but once you do know them you should be able to style your components quickly wihout having to write too much of your own CSS.

<a href="https://tailwindcss.com/docs/guides/create-react-app" target="_blank">Here</a> are the official docs on adding Tailwind to a Create React App.

You will need to use <a href="https://github.com/gsoft-inc/craco" target="_blank">CRACO</a> to add the extra configuration (this is covered in the docs).

---

## Activities

### Read (if you're going to use Styled Components)

-   About <a href="https://medium.com/the-non-traditional-developer/beginners-guide-to-tagged-template-literals-in-javascript-202d1805e862" target="_blank">tagged template literals</a>.
-   About the <a href="https://styled-components.com/docs/basics" target="_blank">basics of Styled Components</a>.

<!-- ---

[Go to lesson 3](3)

--- -->
