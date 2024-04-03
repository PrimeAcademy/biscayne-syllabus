# Troubleshooting Code

**Context**

- Fixing bugs can take up _most_ of your as a software engineer! For every line of code you write, you'll spend a good chunk of time testing it, and fixing bugs.
- The better you get at troubleshooting, the more efficient you'll be as an engineer, and the more fun you'll have. Invest the time now in learning methods and tools for smart debugging!


## A Framework for Debugging

First: What is a "bug" 🐛? We often think of bugs as JS errors, but we're talking about any time code is not working the way you want it to. Can be:
- JS Errors
- In-progress code that is behaving incorrectly
  - eg. a function isn't receiving the arguments it expects
- Does nothing
- Does the wrong thing
- Works sometimes
- "Works on my machine"
- CSS / styling bugs

What do you do when you encounter a bug? Here's a framework for approaching, thinking about, and discussing bugs:

1. **Describe**
2. **Isolate**
3. **Fix**

### 1. Describe

> **Key question:** 
> What do you want to happen? What is actually happening?

Don't move on until you can answer this question! If you can't articulate what you want to happen, how can you know if your bug is fixed?

It's also very difficult to ask for help, if you can't articulate what you're trying to do in this way. If your asking a peer or a manager to help you solve a bug, it's your due diligence to understand and clearly describe the bug.

Examples:

- I want to see a list of products and prices. Right now, the browser is showing nothing. 
- I don't want to see any errors in my console. But I am!
- I want to see a console log that shows an array with the strings "hello" and "world" in it. But instead I'm seeing a console log with an empty array. 
- When I click this button, I want to see a console log that says "click". But when I click the button, I don't see any logs.

This may sound obvious, but it's a step we often skip past! It's easy to get lost in your code, and lose sight of what you're trying to do. And without describing your bug, it's like trying to do a puzzle in a pitch black room. 

> Pro tip: If you're having time articulating this step, ***don't keep going!*** That is your brain telling you to ***take a break.***


### 2. Isolate

> **Key question:** 
> Where exactly is the problem happening?

Start big, then narrow down:
- Where in our code are things breaking?
- Is it JS or HTML or CSS? what file? what function? what line? what variable/expression?
- How do you know that's where the bug is? Don't guess! Use tools 👉

Tools to help isolate bugs:
- Error messages and stacks
  - How to read error messages?
  - How to read stacks?
  - How to trace error messages to a line
- Elements panel (for HTML stuff): what's on the DOM? What do you want to be on the DOM
- `console.log()` does two things:
  - Evaluates variables/express: eg. you want the `food` variable to be `= "tacos"`, but it's actually `undefined`
  - Asks: "is this code even running?". eg. add `console.log('in the eatTacos())` function. If you're not seeing that log, that function is not being called! - Don't try to fix code that isn't running 😉, figure out _why_ it isn't running (eg. fn isn't being called)
- Debugger (Chrome or VSCode debugger), (separate lecture)
  - Allows you to "walk through" the code line by line, see which lines of code are run, and the value of each variable. Like `console.log()` on steroids!
  - See VSCode Browser Debugging documentation: [https://code.visualstudio.com/docs/nodejs/browser-debugging](https://code.visualstudio.com/docs/nodejs/browser-debugging)


**Example**

**Describe:** I want to see a console log for every album in my collection, but I'm not seeing any logs.

**Isolate:** I think the problem is with the `for` loop in my `showCollection` function, because that's where the `console.log()` statements are

```js
function showCollection (album) {
  for (let i = 0; i < collection; i++) {
    // I should be seeing these logs but I'm not. 
    // So I can infer that the error lives around here!
    console.log(`Album title is ${collection[i].title}`);
  }
}
```

**Use a Tool:** Use console logs, to see if the code is running as expected

```js
function showCollection(collection) {
  // Is this function even being called?
  // If it is, we'll see this log 👇
  console.log('inside show collection');

  // Are we getting the correct argument?
  // I expect collection to be an array of 3 objects, each with `title` properties
  console.log('collection is:', collection); 

  for (let i = 0; i < collection; i++) {
    // Is this for loop running?
    // I expect to see this log 3 times (once for each album)
    console.log('inside for loop');

    console.log(`Album title is ${collection[i].title}`);
  }
}
```


Again, **don't move on to the next step, until you've isolated your bug!** 

### 3. Fix

- Change **one thing**, and try again!
  - If you change too many things at once, you may have fixed your code, and then broken it again! Or fixed one thing and broke another.
  - Then go back to your description: does it do what you want, now?
- Google search (best for specific errors, or questions)
- Ask a peer for help
- Escalate (to your instructors, senior engineers, or bosses)
  
> Pro Tip: New errors are good errors! You might fix a bug, but still have other errors in your code. Don't undo your fix! Instead, start the debugging process over from the top (describe, isolate, fix).

## [Bug Report](../supporting-documentation/bug-report.md)

Next time you run into a bug, use this Bug Report to help you troubleshoot

[**Bug Report**](../supporting-documentation/bug-report.md)
