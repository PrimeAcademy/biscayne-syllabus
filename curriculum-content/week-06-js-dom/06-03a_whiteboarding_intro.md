# Whiteboard Interview Introduction

This is often a part of a technical interview. The same, Who, How, What, Where, and Why still apply.

## Fundamentals of Whiteboard Problem:

- Attempt every problem. 
- Ask for clarification
- Draw a few input outputs (“Yellow” -> “Blue”) to confirm
- Think out loud.
- Making mistakes is better than doing nothing.
- Admit when you don’t know things. “I’m not really sure, but this is how I might start…”
- Ask questions when you get stuck or if you start making up functions, “I think strings have a .split() function that does these nice things for me? Is it ok to use this?”.
- Be honest about your strengths and weaknesses (I feel pretty comfortable with creating a node server. I don’t have a lot of experience with C# and Java, but I finished a code school tutorial, started building an application in C#, and I’m excited to learn more.)
- When you are done, test your solution, walk through it, try to think of edge cases that would break it.
- Don't stop thinking out loud.

### Example Google Whiteboard Interview
- The first 11 minutes of this is gold:
  - https://www.youtube.com/watch?v=XKu_SEDAykw

### Reverse a String (instructor example)

The question:

> Write a function that reverses a string.

Example clarifying questions:

- JavaScript has a `.reverse()` method, may I use that? Not for this problem.
- Will it ever receive anything other than a string? You may assume it always takes in a string.

Example test cases:

```
"cat" -> "tac"
hello world -> "dlrow olleh"
"What up?" -> "?pu tahW"
```

Lessons:

- When asked to build something out in a `for` loop, instantiate outside the loop.
- Iterate all the way down to `0` when iterating in reverse

Example Solution:

```JavaScript
const stringReverser = originalString => {
    let reversedString = "";
    for (let i = originalString.length - 1; i >= 0; i--) {
        reversedString += originalString[i];
    }
    return reversedString;
}
```
