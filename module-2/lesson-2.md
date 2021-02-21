# Lesson 2 - An introduction to Jest

## Introduction

<a href="https://jestjs.io/" target="_blank">Jest</a> is a tool that allows us to test JavaScript code.

In this brief introduction, we'll install Jest and use it to test the return value of a function.

The value of testing lies in being certain that changes you make to your code won't break other parts of your program.

You will probably come across the term TDD or Test-Driven Development.

This method of development involves writing tests before you write your actual code. At first the test(s) will fail, and as you implement your code the test(s) will pass.

TDD won't be covered in this course.

---

## A first Jest test

To add Jest to our project, we'll follow the docs found <a href="https://jestjs.io/docs/en/getting-started" target="_blank">here</a>.

> Remember to only use NPM or Yarn on a project, not both. If you find a project has both a package-lock.json and a yarn.lock file, it means both package managers have been used. Delete both lock files.

First we'll create a package.json file:

```
npm init -y
```

Then we'll install Jest as a dev dependency

```
npm install --save-dev jest
```

We're going to use ES6 modules (imports and exports) so we also need to install the following Babel packages:

```
npm install --save-dev babel-jest @babel/core @babel/preset-env
```

Then we'll create a `babel.config.js` file with the following config:

```js
module.exports = {
	presets: [["@babel/preset-env", { targets: { node: "current" } }]],
};
```

In the scripts section of `package.json`, set the `test` command to run `jest`:

```json
 "scripts": {
    "test": "jest"
},
```

With that all set up we can start to run tests:

```
npm run test
```

Tests should be in files ending with a `.test.js` extentsion (or `.test.ts` if using TypeScript) or in a `__tests__` folder (double underscores).

---

Details of writing your first test are in the video below.

<iframe src="https://player.vimeo.com/video/506827581" height="500" frameborder="0" allow="autoplay; fullscreen; picture-in-picture" allowfullscreen></iframe>

<a href="https://vimeo.com/506827581/8202fd20a8" target="_blank">Watch on Vimeo</a>

<a href="https://github.com/NoroffFEU/introduction-to-jest" target="_blank">Code from the video</a>

---

## Matchers

Jest users matchers to test for values.

In the video above we used the `toBe` matcher.

If we wanted to test for an object value we would use the `toEqual` matcher.

Consult the <a href="https://jestjs.io/docs/en/using-matchers" target="_blank">official docs</a> when writing your tests to find an appropriate matcher.

---

## Using Jest with TypeScript

To add TypeScript support, install the following Babel package:

```
npm install --save-dev @babel/preset-typescript
```

We'll also add the types for Jest:

```
npm install --save-dev @types/jest
```

Then add the preset to `babel.config.js`

```js
module.exports = {
	presets: [
		["@babel/preset-env", { targets: { node: "current" } }],
		// this preset is the new addition
		"@babel/preset-typescript",
	],
};
```

Now you can use Jest to run tests in TypeScript files.

---

[Go to lesson 3](3)

---
