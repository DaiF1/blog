---
title: Setting up a simple website
summary: |
    How to setup a simple website using ViteJS and GitHub Pages
date: 2024-09-27
tags: ["website", "tutorial"]
authors:
    - "daif.en"
---

I was recently asked by a friend how I was able to set up my different websites.
So here is a tutorial on the subject.

## Creating a website using ViteJS

### Initial setup

We'll setup our website using *ViteJS*. It is a very fast framework for making
websites.

You can create a project using the following command:

```bash
npm create vite@latest
```

A prompt will ask you for your project name, and create a directory for it.
It will also ask for the language and framework used in the project.
For this tutorial, I'll be using Vanilla Javascript.

When the command finished running, you can enter the directory and run
`npm install`.

To run the project on localhost, simply type `npm run dev`.
You should open up on a page with a simple click counter.

### Directory overview

Now that we made that website working, it's time to check out the content of our
newly created directory.

The smallest website you can create follows this architecture:

```
project/
├── index.html
├── main.js
├── package.json
├── public/
│   └── Here are all of your images
└── style.css
```

The `index.html` contains your page's content and `style.css` all of it's style.
The website's entry point is the file `main.js`. This is the first javascript
file called on load.

### Hello World!

Let's try populating our website a little. Add the following code inside the 
`div id="app"` tag.

```html
<h1>Hello World!</h1>
<p>This is my first website</p>

<button id="click-me-btn">Click Me!</button>
```

We'll try displaying the text "Good job!" when clicking on the "Click Me!"
button.

Here is the Javascript code written in `main.js` to make it possible:

```js
import './style.css'

let button = document.getElementById("click-me-btn");
let app = document.getElementById("app");

button.addEventListener("click", () => {
    app.innerHTML += "<p>Good Job!</p>"
})
```

{{< alert "circle-info" >}}
The first line in `main.js` is here to import the associated CSSfor the script.
This is imported like so because *ViteJS* is often used with component based
libraries like *React* or *Vue*, for which each script corresponds to a
different object on the page.
{{< /alert >}}

If you try running `npm run dev` now, the button should append the text 'Good Job'
to the page's content.

## Deploy it on GitHub Pages

### Installing `gh-pages`

To deploy our website, we'll use a very useful package called `gh-pages`.
It's a package that deploys automatically your website on a branch for
GitHub Pages.

Install it on the project with the following command:

```bash
npm install gh-pages --save-dev
```

Once the installation is done, add the 2 following rules in the `script` section
of your `package.json`:

```json
{
    "predeploy": "npm run build",
    "deploy": "gh-pages -d dist",
}
```

### Making it live!

Now you can type `npm run deploy` and the script will create a branch called
`gh-pages` with the website's pages built.

To finish everything, go to your repository's settings, and in the "Pages"
section.
Fill the settings under "Build and deployment" like so:

{{< figure src="./pages-settings.png" >}}

Once the pipeline is done running, your website should be deployed at
`https://your-username.github.io/`.

{{< alert "circle-info" >}}

Each time you want to deploy a new version of your website, you simply have to
type `npm run deploy` in your terminal.

{{< /alert >}}
