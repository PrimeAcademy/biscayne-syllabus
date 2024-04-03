## Find the first repeat

Prompt:

> Given a string, find the first consecutively repeating character in it and return its index. If there are no repeating characters, return -1.

Example clarifying questions:

- Will they always be strings? Yes.
- Does capitalization matter? No. You may assume all characters are lowercase.
- Do spaces matter? No. "elite elephant" should return `-1` because there is a space between then (non-consecutive)
- May I use built in methods like `.indexof()`, `.replace()`, `.split()`, `.join()`, `.toLowercase()`, and `.reverse()`? Yes

Example test cases:

```
"hello" -> 3
"aaaaaaaw shucks!" -> 1 
"sweet" -> 3
"radical" -> -1
"elite elephant" -> -1
```

> Interviewer Note: To complicate the problem, allow letters to match even if there is a space between them and even if one is capitalized. Example: `Elite Elephant` or `elite elephant` should return `6`

Example Solution:

```JavaScript
const findFirstRepeat = stringToCheck => {
    for (let i = 0; i < stringToCheck.length; i++) {
        if (stringToCheck[i] == stringToCheck[i+1]) {
            return i + 1;
        }
    }
    return -1;
}
```

<details>
<summary>Added tip for the interviewer/observer:</summary>  
    
This one is insidious.  

The language of `find the first consecutively repeating character in it and return its index` is purposely vague.  

Depending on what someone assumes `its index` means they will either get the right answers, or be "off by one".  

Does the interviewee assume `its index` is referencing the first time a character that repeats shows up?  
Then the answers will be off by one from what we expect.  

Does the interviewee assume `its index` is referencing the first time a character repeats?  
Then the answers will be right.  

This highlights just how dang tricky and detailed talking about code can be, and really really highlights the need to ask clarifying questions __and verify your understanding with the interviewer in terms of inputs and expected outputs__.  

That said, don't be mean about it.  If they are off, and frustrated, help them out :slightly_smiling_face:
</details>

Lessons:

- Strings can be treated like arrays in JavaScript
- String manipulation is incredibly important for whiteboard problems.

## Max Consecutive Ones

Prompt:

> Given an array of 1s and 0s, find the maximum number of consecutive 1s in this array.

Example clarifying questions:

- Will they always be arrays of 1s and 0s as numbers? Yes.
- May I use built in methods like `.max()` or `.min()`? Yes
- Will the array always have numbers? It may be empty, if it is, return 0 (the number of 1s).

Example test cases:

```
[0,1,0,0,0,0,0,1] -> 1
[1,0,1,1,1,1,1,0,1,1] -> 5
[0,0,0] -> 0
[] -> 0
[1,1,1,0,1,1,1] -> 3
```

Example Solution:

```JavaScript
const countConsecutiveOnes = arrayToCheck => {
    let maxConsecutive = 0;
    let currentStreak = 0;
    for (let i = 0; i < arrayToCheck.length; i++) {
        if (arrayToCheck[i] == 1) {
            currentStreak++;
        } else {
            //check current against longest
            if (currentStreak > maxConsecutive) {
                maxConsecutive = currentStreak;
            }
            currentStreak = 0;
        }
    }
    return maxConsecutive;
};
```

## Compare arrays

Prompt:

> Given two arrays of strings, determine if any value from the first array is present in the second array.

Example clarifying questions:

- Will the arrays always contain strings? Yes.
- May I use `Array.includes()`? No
- Will the arrays ever be empty? Possibly, in that case return false
- Does capitalization matter? No. You may assume all characters are lowercase.

Example test cases:

```
["tacos", "pizza", "salamander"], ["potato", "orangutan", "pizza"] -> true
["tacos", "pizza", "salamander"], ["potato", "orangutan", "fluff-ball"] -> false
[], ["potato", "orangutan", "fluff-ball"] -> false
```

Example Solution:

```JavaScript
const compareArrays = (firstArray, secondArray) => {
  for (let firstArrayVal of firstArray) {
    for (let secondArrayVal of secondArray) {
      if (firstArrayVal === secondArrayVal) {
        return true;
      }
    }
  }

  return false;
}
```
