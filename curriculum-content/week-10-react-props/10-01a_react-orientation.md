# React Orientation

## Objectives

Work with a vite bootstrapped app:

- file structure, files provided
    - import/export syntax
    - components
    - JSX
    - compiling
    - make first component
    - render

## Starting Repo

[React Orientation](https://github.com/PrimeAcademy/react-orientation)

## Vite Tooling

Vite (French word for "quick", pronounced /vit/, like "veet") is a build tool that aims to provide a faster and leaner development experience for modern web projects.
It comes with a lot built in under the hood, some we don't need to worry about, and some we should at least be aware of!

Let's look at our first react app!

### Background

This code is based on running this command:

```shell
`npm create vite@latest react-orientation -- --template react`
```

- Our project will be called `react-orientation`.
- It will create a new project, files, folders, and install react, among other libraries.
- We will need to NPM install.

~ Time passes ~

Once its ready, open in VSCode with the command:

```shell
code react-orientation
```

## How to Start

Run this command:

```shell
npm start
```

This will fire up our dev server via vite and get your application running in the browser. There's a lot going on here, but the big thing to focus on is you can command click the link to open in the browser.

## Folder Structure

- `src` folder
    - this is where we'll do most of our work.

- `public` folder
    - we wont really be working in this.
  
- `root` folder
    - this is where the index.html is, amongst other config things.

## Import/Export

Open up `App.jsx`.

Look at the first lines. What does that look like?

Look at the last line. What does that look like?

Client | Server
import ... from | require()
export default ... | module.exports

Don't mix these. import/export is only client side.

## Hello Component

Find the text in the project that is being shown on the screen. Change it.

Questions: What happened? What did you not have to do? Did you notice anything when you finished typing in the terminal?

It restarts your client every time you change code, and refreshes the page!

## Compiling

Every time you make a change, we need to compile the code, as the stuff you are writing needs to be transformed into HTML, CSS, JS. `esbuild` is what is making this happen. Check out [esbuild](https://esbuild.github.io/) to show what compiling means. To inspect readable code, you have to access the webpack option in the browser sources.

## Build Tool

This is accomplished because, behind the scenes, we've got a build tool- This helps recombobulate our code!
The build tool in use is vite. Others could be tools like webpack, Gulp, Grunt. They all are trying to make your life easier.

### Basic understanding of Vite

Vite copies, combines and converts our JavaScript files. We will be doing our work in the `src` folder. Vite merges our code with the HTML in the `public` folder and produces output in the we see in the browser. Unlike with vanilla js, there is no **deployable** code until we run the command:

```shell
npm run build
```

This packages everything up for deployment and puts the files in a `build` folder.

## JSX

We need to compile because we're using JSX (JavaScript extension) which LOOKS like HTML, with some noticeable differences.
JSX needs to compile into HTML -- even though it looks like HTML, its still in a .js file.

| JSX | HTML |
|--|--|
|className | class |
| htmlFor | for |

## Component syntax

Whats going on with our `function App (){...}` ?
What do you think this function does?

App is our Component. It returns our JSX!

In React, everything we make is a function. It adheres to the concept of `functional programming` -- a paradigm, or a way of designing code. The main goal of functional programming is that our functions are `pure` -- meaning same input gives us same output. Programs are made by calling functions and composing others.

Our Component Functions here are what make our program. They call other Component Functions.
