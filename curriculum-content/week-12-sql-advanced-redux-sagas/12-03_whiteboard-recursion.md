# Recursion

## Sum until One Digit

Recursion

The Question:

> Given a non-negative integer num, repeatedly add all its digits until the result has only one digit. For example: Given `num = 38`, the process is like: `3 + 8 = 11`, `1 + 1 = 2`. Since 2 has only one digit, return it.

Example clarifying questions:

- Will the input always be a positive integer? Yes.
- Can I return a number or string? A number please.

Example test cases:

```
38 -> 2
12 -> 3
102 -> 3
1000000002 -> 3
989 -> 8
```

Lessons:

- This is potentially a good case for recursion
- If you use `.toString()`, what happens if this is a really large number when you run it? At some point, it says `NaN` which is a result of the fact that `.toString()` will use scientific notation and you'll end up trying to add a `.` to your number. Here is an example on JSFiddle: https://jsfiddle.net/lukeschlangen/muq0jyk5/6/
- Can you rewrite it as a while loop?

Example Recursive Solution:

```JavaScript
const sumUntilOne = (number) => {
  const numberAsString = String(number);
  let newSumNumber = 0;
  for (let i = 0; i < numberAsString.length; i++) {
    newSumNumber += Number(numberAsString[i]);
  }
  
  if (newSumNumber > 9) {
    return sumUntilOne(newSumNumber);
  } else {
    return newSumNumber;
  }
}

console.log(sumUntilOne(3839));
```

Rewritten as a while loop:

```JavaScript
const sumUntilOne = (number) => {
  while (number > 9) {
    const numberAsString = String(number);
    number = 0;
    for (let i = 0; i < numberAsString.length; i++) {
      number += Number(numberAsString[i]);
    }
  }
  return number;
}

console.log(sumUntilOne(43568767765765677));
```

## Fibonacci

The Question:

> The Fibonacci number sequence is created by starting with 1, 1, and then adding the previous two numbers together. It's starts like:

```
1, 1, 2, 3, 5, 8, 13, 21, 34 ...
```

Write a program that can find the nth Fibonacci number.

Example clarifying questions:

- Will the input always be a positive integer? Yes.

Example test cases:

```
1 -> 1
2 -> 1
3 -> 2
4 -> 3
5 -> 5
6 -> 8
```

Lessons:

- Fibonacci is the classical example for recursion
- What happens if this is a really large number when you run it? At some point, it says `Uncaught RangeError: Maximum call stack size exceeded` which is really a result of the maximum stack size. Meaning, it has to call itself so many times that chrome or your environment says "No, this is too deep of a stack trace." Think, stack overflow. This is a potential issue with any recursive function: https://stackoverflow.com/a/7828803/3644991
- Here is an example of the recursive function that works for a small number, but errors out for a big one: https://jsfiddle.net/lukeschlangen/5huL6zsg/5/
- Here is an example of the looping function that works for any size number, but large numbers will take a long long time to run: https://jsfiddle.net/lukeschlangen/aegmLwe7/4/ Although, at a certain point, it will take forever to run or JavaScript will just return `Infinity`.

Example Recursive Solution:

```JavaScript
const recursiveFibonacci = (n) => {
  if (n <= 0) {
    return 0;
  } else if (n === 1) {
    return 1;
  } else {
    return recursiveFibonacci(n-1) + recursiveFibonacci(n-2);
  }
};

console.log(recursiveFibonacci(68897));
```

Rewritten as a for loop:

```JavaScript
const loopingFibonacci = (n) => {

 let fibo = 1;
 let fiboPrev = 1;
  if (n <= 0) {
    return 0;
  } else if (n === 1) {
    return 1;
  } else {
   for(let i = 2; i < n; i++){
    let temp = fibo;
    fibo += fiboPrev;
    fiboPrev = temp;
   }
   return fibo;
  }
}

console.log(loopingFibonacci(68897));
```


## Flatten Array

The Question:

> Create a function to flatten an array of numbers. So if you're given and array like `[1, [2, 3], [4, [5]], 6]` , your function should return a "flat" array like `[1, 2, 3, 4, 5, 6]`.

Example clarifying questions:

- Will the array only contain numbers and arrays of numbers? Yes.
- Is there a limit to how deeply nested the array can be? No.
- Can the array include empty arrays? Yes.
- Can I use the built in Array.flat() method? No.

Example test cases:

`[1, 2, 3, 4]` --> `[1, 2, 3, 4]`
`[1, [2, [3]], 4]` --> `[1, 2, 3, 4]`
`[1, [], [2, []], 3, 4]` --> `[1, 2, 3, 4]`
`[]` --> `[]`

Example Recursive Solution:

```js
const flatten = (arr) => {
  // Initialize a "flat" array
  let flatArray = [];
  // Loop through our input array
  for (let item of arr) {
    // If the item is an array...
    if (Array.isArray(item)) {
      // Flatten out the nested array into a list of items
      let flattenedItems = flatten(item);
      // and add those items to the flat array
      flatArray.push(...flattenedItems);
    }
    else {
      // Otherwise just add the item to the array
      flatArray.push(item);
    }
  }
  return flatArray;
}
```

Rewritten as a while loop:

```JavaScript
function flattenArray(arr) {
  const result = [];
  const stack = [...arr];

  while (stack.length) {
    const next = stack.pop();
    if (Array.isArray(next)) {
      stack.push(...next);
    } else {
      result.unshift(next);
    }
  }

  return result;
}
```
