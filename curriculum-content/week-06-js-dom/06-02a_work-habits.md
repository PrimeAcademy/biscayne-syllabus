# Work Habits

## Write a line of code. Test a line of code. Repeat.

The first rule of debugging code: 

**Don't write too much code, before you test it!**

Every line of code has a dozen possible ways it can fail. So if you write 100 lines of code, you have thousands of possible bugs 🐛🐛🐛🐛🐛🐛🐛🐛

Don't write more than a couple lines of code before testing it. That way, if there's a bug, you know exactly what caused it.

### Example: Find value in nested array

Let's work through an example problem together, and practice:

- writing **one line of code at a time**
- **testing**, before moving on

**The Problem**

> Given a list of people, find which people are wearing a certain item of clothing

Here's the list of people:

```js
let people = [
    {
        name: 'John',
        relationship: 'friend',
        wearing: ['watch', 'stylish hat', 'ascot tie', 'gloves']
    },
    {
        name: 'Sarah',
        relationship: 'unknown',
        wearing: ['gloves', 'cardigan', 'scarf', 'shoes']
    },
    {
        name: 'Naseem',
        relationship: 'Uncle',
        wearing: ['shoes', 'jacket', 'plain hat', 'watch']
    }
];
```

This may feel like a big problem at first. Don't try to solve it all at once!

**Iteration 1: Setup the function**

```js
// Setup a function, make sure we can call it:
function whoIsWearing(listOfPeople, itemToFind) {
  // Test 1: We should get the arguments we expect:
  console.log('listOfPeople', listOfPeople);    // should be our `people` array 
  console.log('itemToFind', itemToFind);        // should be "watch"

  // "Hardcode" the result for now, to make sure it works
  return ['John', 'Naseem'];
}

// Test 2: we should be able to call the function, and get back the return value
let result = whoIsWearing(people, 'watch');
console.log(result);    // should be ['John', 'Naseem']
```

**Iteration 2: Loop through the people**

```js
function whoIsWearing(listOfPeople, itemToFind) {
  for (let person of listOfPeople) {
    // Test: we should see a log for each person in the list
    // Note the data type! It's an object, with a `wearing` property
    console.log('person:', person);
  }

  return ['John', 'Naseem'];
}
```

**Iteration 3: What are they wearing?**

How do we access the `wearing` array for each person

```js
function whoIsWearing(listOfPeople, itemToFind) {
  for (let person of listOfPeople) {
    // Test: we should see an array, with what each person is wearing
    console.log('person:', person, 'is wearing:', person.wearing);
  }

  return ['John', 'Naseem'];
}
```

**Iteration 4: Loop through the `wearing` array**

```js
function whoIsWearing(listOfPeople, itemToFind) {
  for (let person of listOfPeople) {
    console.log('person:', person, 'is wearing:', person.wearing);

    for (let item of person.wearing) {
      // Test: we should see a log for each item 
      // that each person is wearing
      console.log('item:', item);
    }
  }

  return ['John', 'Naseem'];
}
```

**Iteration 5: Does the item of clothing match**

```js
function whoIsWearing(listOfPeople, itemToFind) {
  for (let person of listOfPeople) {

    for (let item of person.wearing) {
      
      if (item === itemToFind) {
        // Test: We should see this log, if the item matches
        console.log('We found an item!', item, 'from person:', person);
      }
    }
  }

  return ['John', 'Naseem'];
}
```

**Iteration 6: Save the matches to an array**

```js
function whoIsWearing(listOfPeople, itemToFind) {
  let matches = [];

  for (let person of listOfPeople) {

    for (let item of person.wearing) {

      if (item === itemToFind) {
        // Save the person's name to the `matches` array
        matches.push(person.name);
      }
    }
  }

  // Test: matches should be 'John' and 'Naseem' 
  console.log('matches', matches);

  return matches
}

// Test: result should be ['John', 'Naseem']
let result = whoIsWearing(people, 'watch');
console.log('result', result);
```

**Iteration 7: Test with other arguments**

To make sure it works in multiple cases!

```js
// Test: result should be ['John', 'Naseem']
let watchWearers = whoIsWearing(people, 'watch');
console.log('watchWearers', watchWearers);

// Test: Should be John and Sarah
let gloveWearers = whoIsWearing(people, 'gloves');
console.log('gloveWearers', gloveWearers);

// Test: should be empty array
let tuxedoWearers = whoIsWearing(people, 'tuxedo');
console.log('tuxedoWearers', tuxedoWearers);
```