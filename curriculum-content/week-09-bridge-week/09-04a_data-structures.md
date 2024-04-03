# Data Structures

## Objectives

- Review **syntax for accessing values** within data structures
  - Object dot notation
  - Object bracket notation
  - Nested loops
- Practice working with **deep-nested data structures** 

## Overview

- "Data Structures" === organized data (variables/values)
- In JS, our data structures are mostly built using **arrays** and **object**

### Nested data structures

Arrays and object can be "_nested_" inside of other object, creating complex structures. 

```js
// We often see arrays of objects:
let instructors = [
    { 
        name: 'Edan', 
        title: 'instructor' 
    },
    { 
        name: 'Liz',
        title: 'instructor' 
    }
]

// But we can also put arrays and objects inside of arrays and objects
let instructors = [
    { 
        name: 'Edan', 
        title: 'instructor',
        cohorts: [
            {
                name: 'Tarjan',
                dates: {
                    started: '06-13-2020',
                    graduated: '11-25-2020',
                } 
            },
            {
                name: 'Gaiman',
                dates: {
                    started: '03-28-2022',
                    graduated: '10-12-2022',
                }
            }
        ]
    },
    { 
        name: 'Liz',
        title: 'instructor',
        cohorts: [
            {
                name: 'Jemisin',
                dates: {
                    started: '05-02-22',
                    graduated: '09-16-22'
                }
            }
        ]
    }
]
```

Use **dot** notation to access deep-nested values:

```js
// Read from left to right!
let graduationDate = instructors[0].cohorts[0].dates.graduated

// Break it down:

// list of instructors (array)
instructors;     

// first instructor in the list (object)
instructors[0];  

// list of cohorts for the first instructor (array)
instructors[0].cohorts;

// First cohort for the first instructor (object)
instructors[0].cohorts[0];

// Dates of the first cohort (object)
instructors[0].cohorts[0].dates;

// Graduation date of the first cohort (string)
instructors[0].cohorts[0].dates.graduation;
```

### Looping through nested arrays

We can loop through nested arrays just like any other array.

```js
// Loop through cohorts of the first instructor
for (let cohort of instructors[0].cohorts) {
    console.log(`${cohort.name} will graduate on ${cohort.dates.graduated}`);
}

// Or, we can loop through all the instructors, and then each cohort for that instructor
for (let instructor of instructors) {
    // `instructor` is now assigned to
    // a single instructor object!
    for (let cohort of instructor.cohorts) {
        console.log(`${instructor.name} is teaching ${cohort.name}`);
    }
}
```

## Object Bracket Syntax

We usually use "dot notation" to access object properties, but there is an alternate "bracket notation":

```js
let dates = {
    started: '06-13-2020',
    graduated: '11-25-2020',
};

// Dot notation
let startDate = dates.started;

// Bracket notation
let startDate = dates['started'];

// This is powerful, because we can pass in any value that resolves to a string
let dateType = 'started';
let startDate = dates[dateType]

// Or 
dates[dateType] = '06-13-2020';
```

Object keys are _strings_. We can pass in a string to access an object property. 

An example where this is useful:

```js
function getDate(type) {
    return dates[type];
}

let startDate = getDate('started');
let graduationDate = getDate('graduated');
```

You can use this to set properties, too:

```js
function setDate(type, val) {
    dates[type] = val;
}

setDate('started', '1978-09-21');
setDate('graduated', '1979-02-14');
```
