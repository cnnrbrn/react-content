# Lesson 1 - Web Application Bundlers. Webpack, Parcel and ejecting from Create React App

## Introduction

Web application bundlers combine multiple JavaScript files into one file, translate and compile things like Sass into CSS, and allow module imports and exports in browsers that don't support it.

Using a bundler will result in better performance for your frontend app, with fewer calls to the server to download the multiple files you'd need without your files being bundled.

One of the most widely used bundlers is Webpack, used by the Create React App tool.

When using a tool like Create React App, the bundling tools and configuration settings are part of what gets created by the app scaffolding, so you don't have to install the bundling packages or create the config settings yourself.

---

## Exiting a Create React App project

This video looks at at how we can "eject" from a project created with Create React App to see all the packages and config settings that using the tool creates for us.

<iframe src="https://player.vimeo.com/video/506532274" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/506532274/b367b3faab" target="_blank">Watch on Vimeo</a>

It's not recommended to eject from Create React App unless there is a tool you'd like to add to the project that isn't available through Create React App.

---

## Introduction to Webpack

This video looks at configuring a frontend project to use Sass and TypeScript using Webpack.

Settings are taken from the official docs, found <a href="https://webpack.js.org/loaders/sass-loader/#root" target="_blank">here</a> and <a href="https://webpack.js.org/guides/typescript/#root" target="_blank">here</a>.

<iframe src="https://player.vimeo.com/video/506700240" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/506700240/5e2a77fb4b" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/introduction-to-webpack" target="_blank">Code from the video</a>

---

### Introduction to Parcel

Using <a href="https://parceljs.org/" target="_blank">Parcel</a> as a bundler is simpler than using Webpack.

Here we look at adding TypeScript and Sass support to a frontend project using Parcel.

<iframe src="https://player.vimeo.com/video/506733052" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/506733052/66f45e6d9e" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/introduction-to-parcel" target="_blank">Code from the video</a>

---

### Babel

<a href="https://babeljs.io/" target="_blank">Babel</a> is a JavaScript transcompiler that converts JavaScript code into backwards compatible versions.

In the next lesson we'll use it to allow us to use ES6 modules in Jest tests.

---

[Go to lesson 2](2)

---
