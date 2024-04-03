## Conditionals and Mod Operator

### Fizz-Buzz

The question:

> Write a function that takes in a number. If the number is divisible by 3, return "Fizz". If the number is divisible by 5, return "Buzz". If the number is divisible by both 3 and 5, return "FizzBuzz". Otherwise, just return the number that was passed into the function.

Example clarifying questions:

- Will they always be positive integers? Yes.
- Should it always return a string? No. It should return a number unless it is returning "Fizz", "Buzz", or "FizzBuzz".

Example test cases:

```
1 -> 1
2 -> 2
3 -> Fizz
4 -> 4
5 -> Buzz
6 -> Fizz
10 -> Buzz
15 -> FizzBuzz
```

Lessons:

- The Mod operator `%` returns the remainder
- Start with the most specific check first (the three and five) and then go to the other checks. The problem is presented in the opposite order to confuse.

Example Solution:

```JavaScript
const fizzBuzz = number => {
    if(number % 15 === 0) {
        return "FizzBuzz";
    } else if (number % 3 === 0) {
        return "Fizz";
    } else if (number % 5 === 0) {
        return "Buzz";
    } else {
        return number;
    }
}
```

### Leap Year

> Write a program that takes in a year (as an integer) and returns true if the year is a leap year. Years that are divisible by 4 are leap years, but years that are divisible by 100 are not leap years, but years that are divisible by 400 are leap years.


Example clarifying questions:

- Will they always be positive integers? Yes.
- How will years BC be indicated? 0 is the smallest year you need to worry about.
- Should it always return a boolean? Yes.

Example test cases:

```
1999 -> false (normal year)
2004 -> true (divisible by 4)
2100 -> false (divisible by 100)
2000 -> true (divisible by 400)
```

Lessons:

- The Mod operator `%` returns the remainder
- Start with the most specific check first (the 400) and then go to the other checks.

Example Solution:

```JavaScript
const leapYear = year => {
    if(number % 400 === 0) {
        return true;
    } else if (number % 100 === 0) {
        return false;
    } else if (number % 4 === 0) {
        return true;
    } else {
        return false;
    }
}
```

### Is it a BLT?

> Write a function that accepts an array of ingredients, and determines whether they make a BLT sandwich. An array is a sandwich if the first and last items are both "bread", and the middle contains "bacon", "lettuce" and "tomatoes".


Example clarifying questions:

- Will I always receive an array of string? Yes.
- Can the sandwich have additional ingredients? Yes, as long as they are inside the "bread"
- Can the ingredients be in any order? Yes, any order inside the "bread"

Example test cases:

```
["bread", "bacon", "lettuce", "blue cheese", "tomatoes", "bread"] --> true
["bread", "lettuce", "bacon", "tomatoes"] --> false (missing "bread" at end)
["bread", "lettuce", "tomatoes", "bacon", "bread"] --> true
["bread", "lettuce", "tomatoes", "bacon", "bread", "cheese"] --> false ("cheese" is outside the "bread")
```

Example Solution:

```JavaScript
const isBLT = ingredients => {
    // Check that we have bread on both sides
    const hasBreadOnBothSides = (
      ingredients[0] === "bread" &&
      ingredients[ingredients.length - 1] === "bread"
    );
    
    // We'll loop through the array looking for each of these ingredients
    // To start, assume we don't have them
    let hasBacon = false;
    let hasLettuce = false;
    let hasTomatoes = false;
    
    for (let item of ingredients) {
       if (item === "bacon") {
         hasBacon = true;
       }
       else if (item === "lettuce") {
         hasLettuce = true;
       }
       else if (item === "tomatoes") {
         hasTomatoes = true;
       }
    }
    
    // It's a BLT if it meets all these conditions
    return (
      hasBreadOnBothSides &&
      hasBacon &&
      hasLettuce &&
      hasTomatoes
    );
}
```

