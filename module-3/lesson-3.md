# Lesson 3 - Running tests with Github Actions

## Introduction

In this lesson we are going to look at an example CI (Continous Integration) flow.

In our project we'll have a `main` and a `dev` branch. We'll create a pull request that will fix a broken test in the dev branch.

Pull requests are a way for code to be reviewed before getting merged into a major branch of a repo.

Using Github actions we will automate the running of our tests - in our case just one test. You wouldn't merge a pull request that had failing tests into a major branch.

---

The repo from the lesson is <a href="https://github.com/NoroffFEU/github-actions-tests" target="_blank">here</a>.

---

We are going to use the command line for most of the git operations (except creating and merging the pull request), but it doesn't matter if you use the command line, Github Desktop or VSCode's git tools.

---

### Steps

1. Create a new React app with Create React App, create a repo for the app, link the app to the repo with the default branch set to `main` and push the local code to Github.
2. Using Github's website, create a workflow that will install the packages and run the tests in the project on any push or pull request to the dev branch. We'll use the test in `App.test.js` as the only test. We'll need to pull the workflow file to our local version.
3. Create a new branch called `dev`. Change the contents of `App.js` so that the test will fail when it's run. Push the dev branch branch and see the test fail when run by the action
4. Create a new branch off dev called `test-fix` and fix the `App.js` so that the test will pass. Push this branch to Github and create a pull request.
5. Merge the pull request - this will merge the test-fix branch into dev branch.

#### Step 1

We'll create a new React app with the following command:

```
npx create-react-app github-actions-tests
```

We'll create a new repo in Github called `github-actions-tests`, and link our new app to the repo after change the default branch to main.

You can find similar instructions on Github when createing a new repo. Make you are in the correct folder when running the commands - the easiest way to do that is to run the commands from a VSCode terminal

```
cd github-actions-tests
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/NoroffFEU/github-actions-tests-.git
git push -u origin main
```

#### Step 2

After pushing the files to Github, we'll go to the actions tab and create a workflow file.

We'll change the branch that will respond to pushes and pull-requests to `dev` and change the steps to:

```yml
- uses: actions/checkout@v2

- name: Install modules
  run: npm install
- name: Run tests
  run: npm run test
```

These steps will checkout the repo code, install the modules and run the tests.

<img src="/images/actions-test-1.gif" alt="Create the workflow" style="max-width: 800px">

We created the workflow file in Github, so we need to pull it in to our local version of the repo:

```
git pull origin main
```

<img src="/images/actions-test-2.gif" alt="Pull origin" style="max-width: 800px">

---

#### Step 3

We'll create and checkout a new branch called `dev` with the following command:

```
git checkout -b dev
```

VSCode will indicate you're now on the dev branch in the bottom left.

<img src="/images/actions-test-3.gif" alt="Create dev branch" style="max-width: 800px">

The test found in `App.test.js` looks for the text `learn react`. Remove the word `learn` from the `App.js` file so the test will fail, then commit and push the changes.

```
git add .
git commit -m "Change App.js"
git push origin dev
```

<img src="/images/actions-test-4.gif" alt="Push to dev" style="max-width: 800px">

Go to the Actions tab in Github to see the action first install the packages, then run the tests.

The test will fail.

<img src="/images/actions-test-5.png" alt="Failed test" style="max-width: 800px">

---

#### Step 4

Now we're going to create a pull request with a fix for `App.js` that will make the test pass.

To create a pull request we first need a branch. We'll call the branch `test-fix`.

```
git checkout -b test-fix
```

We'll add the word "Learn" back to `App.js` then commit and push the branch:

```
git add .
git commit -m "Fix App.js"
git push origin test-fix
```

To create the pull request do the following:

-   Change to the test-fix branch
-   Select the `pull-request` link
-   Change the destination brange for the merge from `main` to `dev`
-   Add a comment for the pull request
-   Click `Create pull request`

<img src="/images/actions-test-6.gif" alt="Create pull request" style="max-width: 800px">

Because our workflow is set to respond to pull requests on the dev branch, the action will run and the test will run.

This time our test will pass.

<img src="/images/actions-test-7.png" alt="Passed test" style="max-width: 800px">

---

#### Step 5

The final thing to do is merge the pull request.

<img src="/images/actions-test-8.gif" alt="Merge the pull request" style="max-width: 800px">

---
