# Node Introduction

## Intro to Node

* What is Node?
* How we will use Node
* Modules

### What is Node?

Node.js is a JavaScript runtime environment (i.e. a running program) built on Chrome's V8 JavaScript
engine (i.e. a program or library that executes JavaScript code).

[Read more at the Node website.](https://nodejs.org/en/)

### How we will use Node

We will use Node to handle HTTP requests. We'll accept requests, do some logic, and return an HTTP response to our client (e.g. the browser).

### Example

Create a new folder and add a file named `intro.js`. Add some JavaScript code to the file. Log something to the console. Keep in mind that we do not have a DOM on the server!

Run the file by typing the following into terminal:

```
node intro.js
```
## Using multiple files, urls

You've had to use multiple files before -- in your `index.html` we had to get the script file and the style file connected. It may have looked something like this:

`<script src="./client.js"></script>`

The url to client js is called a `relative` url. That means that the path to the client.js is relative to the file we are currently writing in. The `./` means THIS FOLDER. If you wanted to go up a level to a parent folder, you'd type `../` -- similar to in the terminal you'd use `cd ..`

On the client, we do that work in the HTML file, but what about in Node?

## Node Modules

On the server, we don't have an index.html to use for sourcing multiple JS files. But we still need that ability. This is called `importing` and `exporting`. Almost every language has this concept.

Node modules are a way for us to break out code out into multiple files. It allows us to organize our code and utilize third party libraries. Without modules, our code would quickly become unmanageable (hundreds or thousands of lines of code in a single file). When you have more than ~200 lines of code in a single file, consider breaking it down into modules.

Many languages have the ability to connect multiple source files together for better organization. Modules is how we do this with Node. To make something available to other files, you need to use `module.exports`

**people.js**

```JavaScript
let people = ['Ally', 'Chris', 'Dane'];

// export is similar to a function return, it's what is returned from the module.
module.exports = people;
```
And to use it in a different file, you need to `require()` it.
**index.js**

```JavaScript
// Assign whatever is exported in the module to a variable named arrayOfPeople
let arrayOfPeople = require('./people.js');

console.log(arrayOfPeople); // logs out ['Ally', 'Chris', 'Dane'];
```



### What can I import/export?

You can import lots of things! Each file only gets ONE export. Not everything in the file needs to be exported, though!

For example you can declare variables in a module, use them, but not export them.

```js

const SECRET_CODE = 123;

function generateNewSecret(numberIn) {
  return SECRET_CODE * numberIn;
}

module.exports = generateNewSecret;
```

`SECRET_CODE` is not exported, only the function is. Whatever imports it only knows about the function, not the value of the code!

NOTE:
If you forget to use `module.exports` your code will behave oddly and probably error. The default is an empty object `{}`, so you'll likely get errors around that. Feel free to console log what you import!

`let codeGenerator = require('./modules/secret.js')`
`console.log(codeGenerator) // hoping for a function, not an object.


### Objects

You can also export an object, which itself can contain multiple keys/values.

```js
let pet = {
  type: 'dog',
  age: 4,
  breed = 'collie'
}

module.exports = pet;
```
