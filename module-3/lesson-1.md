# Lesson 1 - Creating an NPM package

## Introduction

You've used several NPM packages created by other developers like `node-sass` and `react`.

In this lesson we'll create our own package, a rather silly/brilliant one that selects input elements by a specific class and turns them into inputs that change their background colour on every key up event.

<img src="/images/npm-package-1.gif" alt="Package example" style="max-width: 700px">

---

## The steps we require

1. Create a package.json a file and enter the name of our package
2. Compile the source code for the package to ES5 so that the package can be used in all browsers. We'll use Babel for this step.
3. Create an NPM account
4. Login to NPM via the terminal console
5. Publish the package

---

The repo for this lesson is <a href="https://github.com/NoroffFEU/creating-an-npm-package" target="_blank">here</a>, the master branch contains the starting source code.

The final code is on <a href="https://github.com/NoroffFEU/creating-an-npm-package/tree/final" target="_blank">this branch</a>.

There are comments in the code explaining it, but basically it does the following:

-   selects all input elements with a certain selector string (`.silly-input` if another one is not passed in)
-   loops through the inputs, styles them and adds a `keyup` event listener to each that changes the background colour

---

## Creating package.json

You can run `npm init -y` to have all default values filled in automatically, or `npm init` to fill the values in interactively.

We'll change the `name` field to `silly-input`. This value is what users will enter when they run:

```bash
npm install <name-of-package>
```

## Compiling the source code to ES5

The <a href="https://github.com/NoroffFEU/creating-an-npm-package/blob/master/src/index.js" target="_blank">source code</a> uses ES6/ES2015, so we need to compile down to ES5 to make sure our package works in all (older) browsers.

We'll use Babel for this so we'll install the following as dev dependencies:

```bash
npm install babel-cli babel-preset-es2015 -D
```

We'll also create a `.gitignore` file and add the `node_modules` folder.

<img src="/images/npm-package-2.png" alt=".gitignore" style="max-width: 700px">

We'll add `.babelrc` file with the `presets` value set to `es2015`:

<img src="/images/npm-package-3.png" alt=".babelrc" style="max-width: 700px">

And create a `build` script entry in package.json:

<img src="/images/npm-package-4.gif" alt="Build script" style="max-width: 700px">

When run, that will compile the code in the `src` folder into the `dist` folder.

Now we can run `npm run build`:

<img src="/images/npm-package-5.gif" alt="npm run build" style="max-width: 700px">

The compiled code in the `dist` folder is what the package will consist of.

We need to add an `index.js` in the root of our project that exports this code:

<img src="/images/npm-package-6.png" alt="index.js" style="max-width: 700px">

---

## Creating an NPM account and logging in

<a href="https://www.npmjs.com/" target="_blank">npmjs.com</a> is NPM's home.

Create an account at <a href="https://www.npmjs.com/signup" target="_blank">npmjs.com/signup</a>

Once created, use `npm login` from a terminal/command line to log in. On Mac (and maybe Windows too) your password won't show up when typing it.

<img src="/images/npm-package-7.gif" alt="npm login" style="max-width: 700px">

## Publishing the package

Before we publish the package, there is a final file to add, an `.npmignore`. We'll exclude the `src` and `node_modules` folders from the package:

<img src="/images/npm-package-8.png" alt=".npmignore" style="max-width: 700px">

Then we can publish the package with the `npm publish` command:

<img src="/images/npm-package-9.gif" alt="npm publish" style="max-width: 700px">

Once published, the package is listed in our NPM profile.

<img src="/images/npm-package-10.png" alt=".npmignore" style="max-width: 700px">

### Adding a README file

We need to add a README file with instructions for users of our package.

<img src="/images/npm-package-11.png" alt="README.md" style="max-width: 700px">

We can't use the same version number when updatin a package, so in package.json we'll update the version to `1.0.1`, and update the package by running `npm publish` again.

You can click through from the admin panel to the <a href="https://www.npmjs.com/package/silly-input" target="_blank">public URL</a> for the package.

<img src="/images/npm-package-12.gif" alt="Published package" style="max-width: 700px">

---

### Using the package

We've added instructions on how to use the package in a project in the package's README file.

```
npm install silly-input
```

Add a `.silly-input` class to any input element we want to convert to a Silly Input

```html
<input class="silly-input" />
```

And then in a script file, import and call the `sillyInput` function:

```js
import sillyInput from "silly-input";

sillyInput();
```

To import the function from an NPM package, we will need to use a bundler like Parcel or Webpack.

<a href="https://github.com/NoroffFEU/using-our-custom-npm-package" target="_blank">Here</a> is an example project using the package.

### Unpublishing a package

You can use the following command to completely unpublish a package:

```
npm unpublish -f
```

\*Note: Once a package has been completely unpublished, you will need to wait 24 hours to republish it with the same name.

---

## Lesson Task

There is a practice question in the master branch of <a href="https://github.com/NoroffFEU/lesson-task-workflow2-module3-lesson1" target="_blank">this repo</a>.

---

[Go to lesson 2](2)

---
