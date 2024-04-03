# Managing Events

**Instructor Notes**
- Starter repo:
  - https://github.com/PrimeAcademy/event-management-starter
- Immediately after the lecture:
  1. Push the repo with completed code up to GitHub.
  2. Instruct students to fork/clone and make it so that 🦄 emojis are deleted when clicked.

**Conceptual Goals**
- Understand the need for event object.
- Use `event.target` to answer the question of, "Which thing got clicked?"

## We Need to Delete Potatoes, But...

We need to make it so that when a potato gets clicked, it's specifically removed from the DOM.

*Slight wrinkle, we do NOT want to delete unicorns. Nor other potatoes. Don't you dare.*

We've learned how to attach event handlers to the html. So we can start there. Add an onclick to the `p` tag! But this time, let's take a look at the event thats happening:

```js
function makePotato() {
  console.log('whoa, makePotato() got clicked.');

  // Select the magicalDiv element on the dom.
  const magicalDiv = document.querySelector('#magicalDiv');
  // When the potatoButton is clicked, add a new element to the magicalDiv element.
    magicDiv.innerHTML +=  <p onclick="deletePotato(event)">'🥔'</p>;
}
```


This will pass the relevant "click" event to our `deletePotato` function as the first parameter.

```js
function deletePotato(event) {
  console.log('Event: ', event);
}
```

This will log the whole Event object to our console, which has _many_ properties, right down to x and y coordinates of the cursor when the button was clicked on.

Note that one such property is the `event.type` which in this case is "click". This is is where the attribute name "onclick" comes from. It is triggered _on_ click events. Every other type of event has its own "on" attribute, like "onblur", "oninput", "onmouseover", etc.

Cool. We can click on a potato! Now we want to delete the specific potato we clicked on...



### Event Target

One particularly useful Event property is `event.target`. This provides us with the actual [Element](https://developer.mozilla.org/en-US/docs/Web/API/Element) that was clicked on. This is useful if you want to modify the item that was clicked. Which we do!

```js
function deletePotato(event) {
  console.log('Event: ', event);
  event.target.remove(); //remove the thing where this event came from!

}
```



## Your Turn

Nevermind about not deleting unicorns. Now it's your turn to make it so that clicking on a 🦄 results in it being removed from the DOM.
