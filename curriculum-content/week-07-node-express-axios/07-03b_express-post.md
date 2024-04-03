# Express POST Requests

Earlier we looked at how to request data from our server. Now we'll look at how to add new data to the server.

![full stack diagram](../images/full-stack-post-diagram.png)

## JSON and Server setup

We need to do some extra configuration with our server to *parse* data sent from the client. When Axios bundles up our object, (to send it over the internet) it turns it into *[JSON](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON) (JavaScript Object Notation)*.  This process is called *serialization*.  JSON looks a lot like a JavaScript object.  It's actually a really big string, with some more strict rules applied. (one example: It adds double quotes around the property names.)  Express needs to know that.  We need a way to tell Express how the data it's going to recieve is formatted, so it can *deserialize* it (transform it from a JSON string, back into an object in JavaScript).  To do this, we'll extend Express' capabilty with the following command: 

```JavaScript
app.use(express.json()); //Used to parse JSON bodies
```

Example data as a JavaScript object:
```JavaScript
let quote = {
  text: `You may say I'm a dreamer, but I'm not the only one. I hope someday you'll join us. And the world will live as one.`,
  author: 'John Lennon'
}
```

To turn it into JSON ourselves, we could do:
```JavaScript
console.log(JSON.stringify(quote));
```

That would give us the same data, serialized as JSON data.
```json
{
  "text": "You may say I'm a dreamer, but I'm not the only one. I hope someday you'll join us. And the world will live as one.",
  "author": "John Lennon"
}
```
It also show us that it's one big string!
```javaScript
console.log(typeof JSON.stringify(quote));
// outputs: string
```

## Create a Post Route

The *method* name sets the server up to listen for specific types of requests.  Earlier we setup a route to listen for `GET` requests. 

Now we will setup a route to listen for POST requests.

```JavaScript
app.post('/', (req,res) => {
    // The data (body) sent from the client is saved for us
    // in `req.body`
    // Note that without bodyParser setup, req.body will be undefined!
    console.log(`Get a POST request!`, req.body);

    // Grab the new quote from the request body
    let quote = req.body.quoteToAdd;

    // Push the quote into our array
    console.log(`Adding new quote: `, quote)
    quoteList.push(quote);

    // Send back a status code of 201
    res.sendStatus(201);
});
```

But wait! We could test the GET request in the browser by typing in the URL. We can't do that for a POST request. How do we test this? Postman to the rescue!
