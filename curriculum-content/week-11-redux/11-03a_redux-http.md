# Async Redux

[Starter Code](https://github.com/PrimeAcademy/fs-redux-books)

The starter code has redux setup with TODO's. 
 - There is a `bookList` reducer setup in `index.js` and the `BookList` is setup to map over that as an array.
 - The `BookForm` is already setup w/ `handleSubmit` and `onChange` event handlers, but there is a TODO in the `handleSubmit` for the axios request.

## Redux cannot handle Async!

Redux is by nature synchronous -- your state change cannot be pending anything.

So, without some help, your dispatch has to be AFTER the async stuff happens -- 

```
// Make the async request
axios.get('/stuff').then( (response) => {
    // THEN when response comes back, update redux
    dispatch({
      type: 'SET_STUFF',
      payload: response.data
    })
}) 
```

Eventually, we will be able to handle async in a better way, but for now, start with this.

## GET Books from server

We want to get the books from the server when the application loads & when a new book is added. 

Update `App.js`:
```JavaScript
  const dispatch = useDispatch();

  useEffect(() => {
    fetchBookList();
  }, [])

  const fetchBookList = () => {
    axios.get('/api/books')
      .then( response => {
        dispatch({ type: `SET_BOOK_LIST`, payload: response.data });
      })
      .catch( error => {
        console.log(error);
        alert(`Could not get books at this time. Try again later.`);
      })
  }
```

> Note: You may see a warning in *this* repo:
>   "React Hook useEffect has a missing dependency: 'fetchBookList'. Either include it or remove the dependency array  react-hooks/exhaustive-deps"
> This does not seem to occur in other repos but we're also not sure why or 
> how to get rid of it here. It's fine to ignore this for now.

Update the `bookList` reducer in `index.js`:
```JavaScript
const bookList = (state = [], action) => {
  if (action.type === `SET_BOOK_LIST`) {
    // this will replace the book list, payload is array of all books
    // Dont need spread -- dont care about old value
    return  action.payload 
  }
  return state;
}
```

## Add a book

When we add a new book, need to send it to the server. 

Update `handleSubmit` in `BookForm`:

```JavaScript
  const handleSubmit = (event) => {
    event.preventDefault();
    console.log(`Adding book`, newBook);
    axios.post(`/api/books`, newBook)
      .then(response => {
        // Clear the form inputs
        setNewBook({
            title: '',
            author: ''
        });
      })
      .catch( error => {
        console.log(error);
        alert(`Sorry, couldn't add book at this time. Try again later`);
      });
  }
```

> Note should not be putting form inputs into redux as they are entered. It is only needed inside this component, not shared. OK to still have local state w/ Redux.

When we get the response back, *then* we need to fetch the books again (need to pass in as props from App.js):

Pass `fetchBookList` as a prop from `App` to `BookForm`

```jsx
// In App.jsx
<BookForm fetchBookList={fetchBookList}>
```

```js
// InBookForm.jsx
function BookForm({ fetchBookList }) {
```

Then call `fetchBookList` after `POST /api/books` is complete

```js
// In BookForm.jsx, in handleSubmit()
axios.post(`/api/books`, newBook)
  .then(response => {
    // Fetch the latest book list data from the server
    fetchBookList(); 

    // Clear form inputs
    setNewBook({
      title: '',
      author: ''
    });
  });
```
