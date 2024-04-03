# HTML, CSS, and DOM Concepts

**Instructor Notes:**
- The goal is to cover the HTML/CSS/DOM stuff that's essential for event-handling.
- Create a repo named `${cohortName}-html-css-dom`. Use this as a way to again model:
  - `touch index.html style.css`

**Conceptual Goals:**
- Understand these foundational HTML/CSS/DOM concepts:
  - The Box Model
  - `block` vs `inline` vs `inline-block`
  - CSS Selectors
  - CSS Specificity
  - The DOM is a data structure that the browser renders
  - The DOM can be manipulated

---

## Essential HTML Concepts

### The Box Model: How a Single Element Takes Up Space

**Every HTML element is a rectangle**.

Every HTML element follows a small list of *box model* rules (CSS properties) that dictate how much space it takes up. If you inspect a given element with your browser, you can see precisely what these rules are:
- `content` - The content of the box, where text and images appear.
- `padding` - Area *outside* the content but *inside* the element's border.
- `border` - Border that goes around the padding and content.
- `margin` - Additional space outside of the border.

<img src="./images/box-model.png" width="600" alt="The Box Model" />

### CSS Display Types: How Multiple Elements Share Space

`display` is a CSS property that determines how HTML elements take up space *in relation to other elements*.

**Every HTML element has a default display type**. Nearly all HTML elements are `block` or `inline` *by default*.

These three `display` types are foundational:
- `display: block` elements take up the entire width of the page, even if the actual size of the box model *does not* take up the width of the page.
- `display: inline` elements are as wide as they need to be
    - You *cannot* use padding, margin, width, or height!
- `display: inline-block` elements are as wide as they need to be
    - You *can* use padding, margin, width, height!

More on:
- [block and inline](https://www.w3schools.com/css/css_display_visibility.asp)
- [inline-block](https://www.w3schools.com/css/css_inline-block.asp)

---

## Essential CSS Concepts

### CSS Selectors

CSS works by *targeting* HTML elements on the DOM, and applying style rules to them. In order to target HTML elements, we use **CSS selectors**.

Here are the most common CSS selectors, along with links to W3 schools that go into more detail:

Selector | Example | Description
--- | --- | ---
element | div | Selects all elements with that tag
[.class](https://www.w3schools.com/cssref/sel_class.asp) | .intro | Selects all elements with class="intro"
[#id](https://www.w3schools.com/cssref/sel_id.asp) | #firstName | Selects the element with id="firstName"
 [*](https://www.w3schools.com/cssref/sel_all.asp) | * | Selects all elements
[element](https://www.w3schools.com/cssref/sel_element.asp) | p | Selects all <p> elements
[element within another element](https://www.w3schools.com/cssref/sel_element_element.asp) | div p | Selects all <p> elements inside <div> elements
| [multiple elements](https://www.w3schools.com/cssref/sel_element_comma.asp) | h1, h2, h3 | select all these elements

### CSS Specificity

CSS actually stands for *Cascading Style Sheets*. Rules can override other rules. Actually, any time you apply your own CSS, you are *overriding the browser's default CSS rules* for a given element.

Know that there is a surprising amount of depth to this topic. That said, the essential concepts about how CSS rules override each other are:
- A `class` selector overrides an `element` selector.
- An `id` selector overrides a `class` selector.
- If two CSS rules are applied via the same type of selector, the *most recently applied* style rule wins.

---

## Essential DOM Concepts

### It's a Data Structure

The DOM is a **heirarchical data structure** that represents the current state of HTML elements in a browser.

- To be specific, the DOM is a **tree** data structure.
- Elements may be nested within other elements.
- Elements may have attributes, text content, and child elements.

Here's a visualization of the DOM:
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/DOM-model.svg/800px-DOM-model.svg.png" width="600" alt="DOM Visualization" />

**The DOM is not your index.html file.** The `index.html` file is just a set of instructions that a web browser follows to *create the DOM*. A web browser then uses this data structure to draw/paint/render the actual content in the browser window.


### The DOM Can be Manipulated

We can use Chrome Dev Tools to inspect an element. While we're there, we can actually manipulate the DOM! Check it out:
<img src="./images/dom-manipulation.gif" width="600" alt="DOM Manipulation via Dev Tools" />

In this 👆 gif, we changed a value stored inside the DOM data structure. And so, because the DOM changed, the browser re-rendered to reflect it.

We can also use JS to manipulate the DOM. *This is actually the purpose that JS was created for*.

---

## On Events

A webpage being loaded by a web browser is *a process*.

We need to be very specific this process, though. It has implications on how we'll use JS to manipulate the DOM.

We use the language of "events" to talk about this process. There are **two events** that we need to understand:
- **EVENT #1**: Browser receives the `index.html` file and **begins** constructing the DOM.
- **EVENT #2**: DOM construction is **finished**.

It's crucial to understand that these 👆 are *two separate events*.

In order of us to use JS to manipulate the DOM, the DOM has to exist. This means that we need a way to guarantee that any DOM-manipulating JS code **will not be executed until the DOM construction is finished**. 

Think of it this way: We need to wait for **EVENT #2** before we attempt to run JS code that interfaces with the DOM.
