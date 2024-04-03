# React Testing Lecture

Jest comes baked into Create-React-App! Use the Solo Project Starter branch ["unit-test"] (https://github.com/PrimeAcademy/prime-solo-project/tree/unit-test)

**NOTE**
Do not install Jest into CRA. It's already there, and if you do CRA will yell at you and error out.

If students have a package.json somewhere up the tree with JEST installed, CRA will yell. Find and remove the package.json/

## Topics
- How tests are part of the industry. Jobs and job interviews.
- Automated tests: unit vs integration. Not 'Test Automation'.
- Behavior-driven development (BDD)
- Test-driven development (TDD)


## Tools

Jest:
[JEST](https://jestjs.io/docs/en/getting-started.html)

Supertest:
[Supertest](https://github.com/visionmedia/supertest)


## Run Client Tests

```
npm run test
```


# Testing Code

Testing applications help to ensure that they function correctly and meet project requirements. Automated testing helps to provide peace of mind when making enhancements to an existing project. 

We'll be covering **unit testing** and **integration testing** in these lecture notes. 

### Prerequisites

[JEST](https://jestjs.io/docs/en/getting-started.html) is required to run the tests, but we get it from `create-react-app`.

  
  
## Unit Testing

Unit testing is used to test a small isolated piece of code (usually a single function or method). 


### Example Unit Tests

**loginModeReducer.js**

Located at `src/redux/reducers/loginModeReducer.js`

**loginModeReducer.test.js**

Create this file `src/redux/reducers/loginModeReducer.test.js`

```JavaScript
import loginReducers from './loginModeReducer.js';

describe('Testing loginReducers', () => {
  test('should have its correct initial state', () => {
    let action = {};
    let returnedState = loginReducers(undefined, action);
    expect(returnedState).toEqual('login')
  })
})
```

Run with `npm run test` -- what do you see?

### Test Driven Development (TDD)

Test Driven Development is the process of writing your tests *before* you write your code. This requires you to think about your code in advance and leads to improved code.

[More details](https://codeutopia.net/blog/2015/03/01/unit-testing-tdd-and-bdd/)

Lets write a test for a reducer that hasn't yet been written!

At this point, one test should pass and one should fail. This means that we did not write the function to match the requirements and must update the reducer.

