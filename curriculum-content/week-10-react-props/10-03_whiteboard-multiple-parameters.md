## Move odd numbers back 

Prompt

> Write a function that takes in an array of numbers and either the word 'odd' or 'even', and then returns a new array containing the same numbers, in the same order, but with all odd or even numbers (based on the word passed in) moved to the end of the array.

Clarifications:

- Don't modify the input array!
- Assume the array will only contain numbers
- If the array is empty, should return an empty array
- Numbers moved to the end should be in the order they originally appeared

Example test cases:

- `[ 0, 6, 3, 1, 4 ], even -> [ 3, 1, 0, 6, 4 ]`
- `[ 0, 6, 3, 1, 4 ], odd -> [ 0, 6, 4, 3, 1 ]`
- `[ ], odd -> [ ]`

#### Example solution:
```JavaScript
function moveNumbersBack(list, word) {
  let odds = [];
  let evens = [];
  for (num of list) {
    if (num%2 === 0) {
      evens.push(num);
    } else {
      odds.push(num);
    }
  }
  if (word === 'odd') {
    return evens.concat(odds);
  } else {
    return odds.concat(evens);
  }
}
```

## Word Counter

Prompt:

> Write a function that takes in two arguments, an array of strings and a string. The function should return the number of times the string is found in the array.

Example clarifying questions:

- Will they always be array of strings and a string? Yes.
- May I use built in methods like `.toLowercase()`? Yes
- Will the array always have strings? It may be empty, if it is, return 0.
- Does capitalization matter? No. You may assume all characters are lowercase.

Example test cases:

```
["apple", "pizza", "orange", "apple", "sour apple"], "apple" -> 2
["apple", "pizza", "orange", "apple", "sour apple"], "pizza" -> 1
["apple", "pizza", "orange", "apple", "sour apple"], "sour" -> 0
["sour", "soiree", "Sour", "sour", "sour"], "sour" -> 3
[], "banana" -> 0
```

> Interviewer Note: To complicate the problem, allow letters to match even if one is capitalized. Example:

```
["sour", "soiree", "Sour", "sour", "sour"], "sour" -> 4
```

Example Solution:

```JavaScript
const countWordMatches = (arrayToCheck, wordToCheck) => {
    let wordCount = 0;
    for (let i = 0; i < arrayToCheck.length; i++) {
        if (arrayToCheck[i] == wordToCheck) {
            wordCount++;
        }
    }
    return wordCount;
}
```

## Add Up the Numbers

Prompt:

> Write a function that takes in two numbers. Return the sum of all the numbers from the first number passed in to the second number passed in. For example, if we pass in 1 and 4, we'd return 1 + 2 + 3 + 4 = 10.

Example clarifying questions:

- Can the number passed in be negative? No.
- Can the number be zero? No.
- Will it always be a number? Yes.
- Will the second number always be larger than the first number? Yes.

Example test cases:

```
addItUp(1, 4) -> 10
addItUp(2, 6) -> 20
addItUp(10, 11) -> 21
```

Example solution:

```js
function addItUp(startNum, endNum) {
  let sum = 0;

  for (let i = startNum; i <= endNum; i++) {
    sum += i;
  }
  
  return sum;
}
```
