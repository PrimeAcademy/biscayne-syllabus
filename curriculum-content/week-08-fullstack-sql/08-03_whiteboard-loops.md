# Whiteboard problems with loops

Lessons:

- To keep track of (and add to) a value, it should be outside of the loop

> NOTE: The first two problems, Addinator & Total Positive Numbers are *almost* identical. Only Odds is a different variations - making a array to return.

## Addinator

Prompt:

> Write a function that takes in an array of numbers and returns the sum of all the numbers in the array.

Example clarifying questions:

- Will they always be positive integers? Yes.
- May I use built in methods like `.reduce()`? Yes
- Will the array always have numbers? It may be empty, if it is, return 0.

Example test cases:

```
[1,4,8,2] -> 15.
[0,1,0,0,0,0,0,1] -> 2
[4,8,2] -> 14
[0] -> 0
[] -> 0
```

### Example Solutions

```JavaScript
const addinator = arrayToSum => {
    let sum = 0;
    for (let i = 0; i < arrayToSum.length; i++) {
        sum += arrayToSum[i];
    }
    return sum;
}
```

Another Example Solution:

```JavaScript
const addinator = arrayToSum => {
    return arrayToSum.reduce((accumulator, currentValue) => accumulator + currentValue);
}
```

## Total positive numbers 

Prompt:

> Write a function that takes an array and returns the sum of all the positive numbers in the array.

Clarifications:

- Assume the array will only contain numbers
- If the array is empty, should return 0
- Numbers less than 0 should be ignored

Example test cases:

- `[ 0, 1, 4, 5 ] -> 10`
- `[ 0, 5, -3, 2 ] -> 7`
- `[ ] -> 0`

#### Example solution: 

```JavaScript
function totalPositives(numbers)  {
   let sum = 0;

   for (let i = 0; i < numbers.length; i++) {
      if (numbers[i] > 0) {
         sum += numbers[i];
      }
   }

   return sum;
}
```

## Only Odds

Prompt: 

> Write a function that takes in an array of numbers and returns a new array with only the odd numbers.

Clarifications:

- Assume the array will contain only numbers
- If the array is empty or there are no odd numbers, return an empty array
- The numbers should be in the order in which they appeared in the original array.

Example test cases:

- `[ 0, 1, 3, 4, 5 ] -> [1, 3, 5]`
- `[ 0, 6, 8] -> [ ]`
- `[ ] -> [ ]`

### Example solution

```
function onlyOdds(array) {
    let result = [ ];
    for (let num of array) {
        if (num % 2 === 1) {
            result.push(num);
        }
    }
    return result;
}
```


## Find Divisible By

Write a function which takes two arguments and returns all numbers which are evenly divisible by the given divisor. First argument is an array of numbers and the second is the divisor.

Example(Input1, Input2 --> Output)
- `[1, 2, 3, 4, 5, 6], 2 --> [2, 4, 6]`
- `[3, 6, 7, 12], 3 --> [3, 6, 12]`

### Example Solution

```
function divisibleBy(numbers, divisor) {
  let newArr = [];
  for (let i = 0; i < numbers.length; i++) {
    if (numbers[i] % divisor === 0) {
      newArr.push(numbers[i]);
    } 
  }
return newArr;
}
```
