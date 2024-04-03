# Solo Project Front End and Auth 
>>Work from the back-end auth lecture that makes pets?
## High Level
All auth needs to happen on the back end. Remember, any route can be hit with postman. So, Auth on the front end is actually for user experience. 
In the front end, most often we're concerned with people seeing pages that would otherwise break if they are not logged in. I can't see my messages unless I'm logged in, so I shouldn't be able to see the messages page unless I'm logged in, for example.

We have some tools and structure to help answer that question with you.

## New Redux File Structure
```
    src
    ├── components
    │   └── ...
    ├── redux
    |   ├── store.js
    │   └── reducers
    │   │   ├── bookReducer.js
    │   │   └── _root.reducer.js
    │   └── sagas
    │       ├── bookSagas.js
    │       └── _root.saga.js
    └── index.js
  ```

`store.js` -- contains all the configuration for our reducers and sagas, shouldn't need to change things here. `index.js` imports it.

## New Sagas
`_root.saga.js` -- a file that is the root saga! You will need to import a saga, and run it inside the yield all.
```js
export default rootSaga* () {
    yield all([
        saga1(),
        saga2()
    ])
}
```



`fetchUser*` -- Will make a call to `/api/user` and retrieve logged in persons info, if any. Will save in userReducer.


## New Reducers
`_root.reducer.js` -- this is where we combine all the reducers. You will need to import reducers and put them in the combineReducers.

As a point of clarification, `combineReducers()` smashes your reducers together. 

You can control state key names by using different keys for the reducers in the passed object. For example, you may call 
```
combineReducers(
    { todos: myTodosReducer, counter: myCounterReducer }
 )
``` 
for the state shape to be `{ todos, counter }`.

A popular convention is to name reducers after the state slices they manage, so you can use ES6 property shorthand notation: 
```
combineReducers(
    { counter, todos }
)
```
This is equivalent to writing `combineReducers({ counter: counter, todos: todos })`.


You may call combineReducers at any level of the reducer hierarchy. 
It doesn't have to happen at the top. In fact you may use it again to split the child reducers that get too complicated into independent grandchildren, and so on.





`userReducer` -- this reducer is storing locally the logged in user. It will be an empty object if the user is logged out.

## New Components
`ProtectedRoute` -- we made this. It behaves similarly to the regular `Route` component.  If a user is not logged in, you will get a `Login` page, regardless of where they are. If they are logged in, then the correct component will display.


```jsx
         {/* For protected routes, the view could show one of several things on the same route.
            Visiting localhost:3000/user will show the UserPage if the user is logged in.
            If the user is not logged in, the ProtectedRoute will show the LoginPage (component).
            Even though it seems like they are different pages, the user is always on localhost:3000/user */}
          <ProtectedRoute
            // logged in shows UserPage else shows LoginPage
            exact
            path="/user"
          >
            <UserPage />
          </ProtectedRoute>

```

What about the reverse? What if I don't want them to see something if they ARE logged in? We can use the `Redirect` Component:

```jsx
<Route
            exact
            path="/login"
          >
            {user.id ?
              // If the user is already logged in, 
              // redirect to the /user page
              <Redirect to="/user" />
              :
              // Otherwise, show the login page
              <LoginPage />
            }
          </Route>
```


Front-end auth requires that we know a few things about our user. For example:

- Are they logged in?
- What is their "auth level" (regular user vs. admin)
- What is their name / username, etc.

Our API gives us information about the logged in user via the `GET /api/user` endpoint:

```
GET /api/user

Example JSON Response:
{
  "id": 17,
  "username": "AuthQueen37",
  "authLevel": "ADMIN"
}
```

Most of our React components will need this user data, so we'll request it right when the app loads, inside our `App` component:

```jsx
// in App.jsx
function App() {
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch({
      type: 'FETCH_USER'
    });
  }, []);
}
```

This will trigger a `fetchUser` saga (already setup for us):

```js
// src/redux/sagas/user.saga.js
function* fetchUser() {
  // Request info about the logged-in user from the API
  const response = yield axios.get('/api/user');

  // ...and send the user data to our redux store
  yield put({ type: 'SET_USER', payload: response.data });
});
```

We now have a `user` property in our redux store, which we can access from any component. So let's update our `Pet` component to use that data:

```jsx
function Pets() {
  // Grab user data from redux store
  const user = useSelector(store => store.user);

  return (
    <>
      {
        // If we have a `user.id` property, we know the user is
        // logged in. 
        // Conditionally render a welcome message for the logged-in user
        user.id ?
          <h3>Welcome {user.username}</h3>
          :
          <h3>No user logged in</h3>
      }
    </>
  )
}
```

### Pitfall: Don't manually send your user id
Since we have the `user` in a reducer, you'll perhaps desire to send it on requests, something like:
```axios.get(`/pets/${userId}`)``` trying to get this users pets.

But this is a trap! You want  to use the the cookie and `req.user` on the server! (for most use cases, there may be reasons to send it. ) 
If you are sending the user info back and forth, it's probably a liability and definitely making it harder on yourself!



### Conditional rendering, based on user data

Sometimes different users will see different views in your app. For example, an admin user may see a link to your `/admin` page in the `Nav`, which regular users will never see.

Good ol' fashioned conditional rendering will get the job done here:

```jsx
// Nav.jsx

// Grab user from redux store
const user = useSelector(store => store.user);

// Only show "admin" Link for ADMIN level users
{user.access_level === 'ADMIN' && 
  <Link className="navLink" to="/admin">
    Admin
  </Link>
}
```

## Key Take-Aways:

- Front-end auth is _not_ security
- Take advantage of the pre-built `<LoginForm>` and `<RegistrationForm>` components (and all the related sagas/reducers)
- Use `<ProtectedRoute />`s to force users to log-in, before viewing a page
- Use conditional rendering to control who sees what.


