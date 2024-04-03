# Integration Testing



## Integration Testing

Integration Testing is the next step up from Unit Testing. 

Unit Testing makes sure, for example, a lock works. 
Integration Testing checks to make sure the door and lock work together.



### Running the Tests

Jest comes with Create-React-App, but CRA can't work for us outside of the `src` folder. So we need to configure it for our server.

This time, let's add a different test script to our `package.json` file. When running asynchronous tests, `--forceExit` is required or jest will run forever.

```JSON
"scripts": {
    "testServer": "jest server/test/*.test.js --forceExit",
}
```

Run the tests using `npm run testServer`.


For integration testing, we'll be adding [Supertest](https://github.com/visionmedia/supertest) to make requests to our server. The first step is to make a simple server and export the app.

```
npm install supertest --save-dev
```

**server.js**

Located at `server/server.js`

Add, at the end of the file
```JavaScript
module.exports = app;
```

### Asynchronous Tests

When running asynchronous tests (and integration tests commonly are!), we have to handle the time that takes place to make a request. A simple way to do this is `async/await`. We could also use returned promises, or `done()`. 

`async` describes the function, like a generator. `await` is very much like `yield` -- it will pause and wait for the async action to return, and then progress as usual.

**server.test.js**

Create this file `tests/server.test.js`

```JavaScript
let app = require('../server')
let testServer = require('supertest')


describe('Test the root path', () => {
  test('It should respond 200 the LOGOUT route', async () => {
    const response = await testServer(app).post('/api/user/logout');
    expect(response.statusCode).toBe(200);
  });

  test('It should protect the /USER route', async () => {
    const response = await testServer(app).get('/api/user');
    expect(response.statusCode).toBe(403);
  });

// your db must have a user made that you are testing!
  // agent allows us to reuse cookies!
  test('/user route should return user info when authenticated', async () => {
    
    let agent = testServer.agent(app);
    const response = await agent
                            .post('/api/user/login')
                            .send({username: 'dane', password: '1234'});
    expect(response.statusCode).toBe(200);
    
    const userResponse = await agent.get('/api/user');
    expect(userResponse.statusCode).toBe(200);
    console.log('here is userResponse', userResponse)
    

  });
})
```


## Summary

Creating tests helps to ensure your code works as expected. They provide early warning signs to bugs and require you to think through your logic. The more types of testing you can incorporate into your project, the more reliable the code will be. 


### Additional Resources

- [http://www.albertgao.xyz/2017/05/24/how-to-test-expressjs-with-jest-and-supertest/](http://www.albertgao.xyz/2017/05/24/how-to-test-expressjs-with-jest-and-supertest/)
- [https://create-react-app.dev/docs/running-tests/](https://create-react-app.dev/docs/running-tests/)
- [https://redux.js.org/recipes/writingtests](https://redux.js.org/recipes/writingtests)
- [https://codeutopia.net/blog/2015/04/11/what-are-unit-testing-integration-testing-and-functional-testing/](https://codeutopia.net/blog/2015/04/11/what-are-unit-testing-integration-testing-and-functional-testing/)
- [https://buddy.works/guides/how-automate-nodejs-unit-tests-with-mocha-chai](https://buddy.works/guides/how-automate-nodejs-unit-tests-with-mocha-chai)
- [https://alexanderpaterson.com/posts/how-to-start-unit-testing-your-express-apps](https://alexanderpaterson.com/posts/how-to-start-unit-testing-your-express-apps)

- [https://semaphoreci.com/community/tutorials/getting-started-with-node-js-and-mocha](https://semaphoreci.com/community/tutorials/getting-started-with-node-js-and-mocha)

- [https://semaphoreci.com/community/tutorials/a-tdd-approach-to-building-a-todo-api-using-node-js-and-mongodb](https://semaphoreci.com/community/tutorials/a-tdd-approach-to-building-a-todo-api-using-node-js-and-mongodb)
