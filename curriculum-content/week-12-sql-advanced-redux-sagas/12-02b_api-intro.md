# Using APIs

## Key Vocabulary

- API (Application Programming Interface)
- Query String

## Skills

- Reading Documentation
- Using Postman
- Query Strings

## Review

- What is an API? 
- Why do we use APIs?
- What do they do? 
- How do we interact with them? 
- How is that similar/different to interacting with your Node servers? 
- Have you made an API?


> An API is a set of routes that have been made available to others. Some API's are public and others are private. Here is a [list of Public API's](https://github.com/toddmotto/public-apis).


## Giphy API 

Not all public APIs allow you to just access them -- sometimes you need to have some sort of authentication or an account with the API provider.

We're going to explore Giphy [https://developers.giphy.com/]

First you'll need an API key, so go ahead and sign up for an account.

## Read docs

There are usually *Getting Started* docs for public API's. This is a great place to find answers to questions like 


- What can I do with this API?
- Do I need a key?
- If so, how do I send that key on a request?

In the Giphy Getting Started docs, they suggest a few things you might try...

Let's see what's [trending](https://developers.giphy.com/docs/#operation--gifs-trending-get) on Giphy. 

We can use Postman to try making a request and see what we get back for a response.

Send a request to `api.giphy.com/v1/gifs/trending`.  

Notice that we get a response with a status code of 401 and an error message:
```
{
    "message": "No API key found in request"
}
```

Notice the docs for `/trending` say that the `api_key` is a required request parameter. 

If we add our `api_key` in Postman's Params, we should get a successful response.  

Wow... It's a lot of data!!!

That's pretty common for public API requests. They often send back very large objects that contain a lot of information.

We can add another param to our request too! Let's make sure we keep it PG13 -- add a key of rating and a value of PG-13.
This key is optional!

> Note  You can *collapse* the objects within Postman using the little arrows on the left margin.

The Giphy API docs for `/trending` shows what a successful response should contain. Notice that it says it will be an __object[]__ and says what type those objects will be.  We're seeing [GIF Objects](https://developers.giphy.com/docs/#gif-object).

Sometimes you can tell what the values in the objects you get back are based on the name and value, but many times you can't. __Check the docs!__ They will help you understand the data that you are getting back.

### Parts of a URL

Notice that the URL that is getting sent to Giphy is formatted to send the "parameters" on the URL following a `?`. They are key/value paired after the `?`. 

[Talk like a Googler: parts of a url](https://www.mattcutts.com/blog/seo-glossary-url-definitions/)
   

