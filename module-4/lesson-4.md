# Lesson 4

## Image optimisation

Next provides an `Image` component that provides automatic image optimisation.

To convert a reuglar `img` element to an `Image` component, we only need to

```jsx
<img src="/path/to/picture.jpg" width="200" height="100" alt="My image">
```

becomes

```jsx
import Image from 'next/image'

<Image src="/path/to/picture.jpg" width="200" height="100" alt="My image">
```

Optimisations provided by the component include

-   lazy loading (images are only loaded when close to being viewed)
-   enforced image dimensions which prevents content jumping up and down the screen
-   preloading for images that are in the initial viewport
-   serving images in WebP format to browsers that support it. WebP are around 30% smaller in file size than jpegs.

---

## Sass

Next has built-in Sass support, but you do need to install the `sass` package before using it.

```
npm install sass
```

---

## The public folder

Static images, for example ones whose URLs don't come from an API call, can be placed in the `public` folder.

Their `src` attribute then begins with `/` and exludes the word "public"

```jsx
<img src="/images/my-image.png" />
```

You an also place files like `robot.txt` and favicons in the `public` folder.

From the offical docs:

> Note: Don't name the public directory anything else. The name cannot be changed and is the only directory used to serve static assets.

---

## Deploying a Next app

Two of the most popular ways of deploying a Next app are using Vercel and Netlify.

#### Vercel

<a href="https://vercel.com/" target="_blank">Vercel</a> (formerly Zeit) is the company that makes Next so it provides the easiest method of deployment. The steps are documented <a href="https://nextjs.org/docs/deployment" target="_blank">here</a>.

#### Netlify

The <a href="https://netlify.com" target="_blank">Netlify</a> steps are:

-   Login with your Github account
-   Select the "Authorise netlify" button
-   Select the "New site from Git" button
-   Select Github from the list of providers
-   Decide if you want Netlify to have access to all your repositories or select repositories, the hit the "Install" button
-   In the "Basic build settings", set the Build command to `next build && next export` and the Publish directory to `out`
-   Hit the "Deploy site" button

---

## Activities

The official docs contain an <a href="https://nextjs.org/learn/basics/create-nextjs-app" target="_blank">interactive tutorial</a> that covers the basics of Next.

---

<!-- ---

[Go to the MA](ma)

--- -->
