# Tools for Debugging

Invest in your tools, and you can debug faster and better understand your code.

## Objectives

- Review the **available tools** for debugging
- Understand the **purpose of console logs** (did the code run, view the values of variables)
- Understand **how to read an error message**, including **the error stack**
- Understand **HTTP error responses**, what the codes mean, and how these errors differ from JS runtime errors


## Tools for debugging:

- Chrome console (`console.log()`)
- Error messages
- JS Debugger (Chrome and VSCode)
- Postman
- Chrome Network Panel
- Postico

## Console Logs

Console logs are the first go to for debugging code. They can tell us:

- Did this line of code ever run?
- What is the value of a variable?

This is most useful for the **Describe** phase of debugging. For example:

- "I want this function to run, but it is never being called!" (_and I can tell, because the `console.log` never showed up)
- "I want this variable to be an object with a `firstName` and `lastName` property, but it's actually an array: `[null]`" (_and I can see that value in the console_)

## Error Messages

- Practice reading error stacks
- Break it down:
  - Error type
  - Error message
  - Error stack trace
  - (Sometimes) Error object (like errors from `pg.query()` or `axios`)
- Messages:
  - Read your error messages, for goodness sake! 
  - You may need to scroll way up to find the message.
  - Slow down, take time to try to understand what the message is telling you.
- Stack traces
  - `at ... `, `at...`, `at...`
    - Keep scrolling: your error message is at the top of the `at`'s
  - History of function calls
  - Ignore lines from code that isn't yours (ie, from other libraries). Find the top-most value from a file that belongs to you!
  - Pay attention to file name and number: Isolate!

### HTTP Error Messages

You may see messages in the console like:

> GET http://localhost:5000/pets 404

or 

> POST http://localhost:5000/pets 500

These are **HTTP error responses**! These are not errors in your client side code, but errors being sent back to the client by the server.

The main two errors we see are:

**404 Not Found**

This means you are sending a request to a URL that has no corresponding endpoint on the server. 

Check:

- The spelling of your URL in both the AJAX request and the express endpoint
- The base URL used when you `app.use()`'d your router
- The you aren't duplicating a URL between your `app.use()` statement and your router, eg:
    ```js
    // server.js
    app.use('/pets', petsRouter);

    // pets.router.js
    // ❗ This should be `app.get('/', ...)
    // otherwise the URL is `/pets/pets`
    app.get('/pets', (req, res) => {
        res.send(pets);
    });
    ```
**500 Server Error**

This means something broke **on your server!** Stop looking at your client logs (Chrome), and start looking at your server logs (VSCode, or your terminal). 

If you aren't seeing any error logs from your server, make sure you have `.catch()`'es where needed, and that you are _logging your errors_ in those `catch()` functions.

## JS Debugger

Demo using VSCode to debug.

Using the `Debug: Open Link` command is the easiest way to get setup: https://code.visualstudio.com/docs/nodejs/browser-debugging
    - This may not work with React. 

Debugger configuration (works with React):
https://code.visualstudio.com/docs/nodejs/reactjs-tutorial#_debugging-react

- Allows you to "be the computer", and walk through the code line-by-line.
- Some additional setup, but much more powerful that `console.log()`
  - Helps you understand how your code is run (control flow)
  - Can see the value of every variable at once
  - Code is not littered with `console.log`'s

This is more advanced, but worth the effort!

## Postman

Postman is an HTTP Client.

- What does that mean? Show where it belongs in the stack (another client for the server, just like Postico is another client for the database).
- Allows testing your server side code in isolation
  - if you have a bug -- is it on the server? or can we rule that out?
  - Can start by coding the server, testing it, then moving on to the client.

Demo using Postman on an existing server, how it replicates the configuration we pass to `$.ajax()`.

> NOTE: `$.ajax()` uses the `x-www-form-urlencoded` content type for payloads by default. Postman supports this. You can also send data as JSON (which may be more intuitive), though it will behave slightly different (data types are maintained, can POST arrays, etc).

## Chrome Network Panel

- Allows you to see every request made by the client
- "The other side of Postman"
  - Postman tells you if your server can handle requests correctly
  - Network panel tells you if your client is sending the correct request
- Can see all HTTP attribute:
  - URL
  - Method
  - Request body (payload)
  - Response body
  - Status code

## Postico

- Use Postico to help you debug SQL issues in your server code
- Copy/Paste server code in postico, replace placeholder values if needed, and try
- Helps to isolate: is the problem with your SQL? Or with something else (eg, your `req.body` has bad values)