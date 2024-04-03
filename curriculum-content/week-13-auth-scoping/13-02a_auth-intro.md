# Auth Intro

## Goal

* Discuss the need for authentication within our applications.
* Introduce the concepts of concern and what we need to plan for when considering authentication in our application.


## Authentication

Authentication is the act of identifying yourself and proving that you are
who you claim to be. It's a yes/no question. Are you allowed here or not.

*ACTION:* Ask students how this might be useful for a Web application.
NOTE: Facebook, online banking, etc are some examples.


## Authorization

Authorization is determining how much or what access an Authenticated user can do. Compared to Authentication, Authorization asks "What do you have access to?"


### Need for authentication in our web applications

For Web applications, we often need to know who the user is

* to provide them with data that is relevant to them
* to provide them with data that is private to them
* to prevent them from seeing data that is another user's private data

We want to answer the question: When the server receives an HTTP request, how do we determine who it came from?

### Types of authentication

* local - storing the username and password on our server
* oauth/2 - logging in via some other services credentials (we don't store passwords, but we do store tokens)


## Adding authentication to our applications

Our applications will need to

1. Store authentication credentials (e.g. username and password)
2. Authenticate initial requests based on those credentials
3. Authenticate subsequent requests via cookies
4. Check that the data we are serving is authorized for that user

To facilitate these changes, we will learn new concepts

1. What is a `User` in an application
2. How to store passwords securely using **salting and hashing**
3. How to use **cookies** to determine which user made a request to our server
4. How to use **sessions** to store and retrieve user data
5. Methods for **authorizing** what data a user can access
 