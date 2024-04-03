# Using URL Route Params for a Detail View

A common pattern in web applications is to see a listing view with many things to choose from (like a list of movies). When clicking on one of them, we want to load up all of the details for that item, often in a new component and usually getting more information from the database, too.

You can store these details in Redux, but this is not ideal. We're storing more than needed in Redux just in case a user picks that item. Typically there are way more details in the database we want. Plus, Redux is lost upon a refresh.

## URL Parameters

Better is to pass the `id` of that item on the URL and use that number to get the details from the next component.

Let's add a new link:

```JSX
<li>
    <Link to='/animals/details/47'>Details for animal with id number 47</Link>
</li>
```

and a new route in `App.js`:

```JSX
<Route path='/animals/details/:id'>
   <AnimalDetails />
</Route>
```

In the `AnimalDetails` component, we can access the `:id` part of the dynamic URL with a new hook called `useParams`:

``` JSX
import { useParams, useEffect } from 'react-router-dom';

const AnimalDetail = () => {
    // useParams give us an object with matched URL params
    // (these are set up in the App.js route, with a path like /details/:id)
    const params = useParams();

    // You can also use object destructuring:
    // let { id } = useParams();

    // An example of doing something with the ID when the component
    // mounts, like doing an ajax request and setting state with
    // the response
    useEffect(() => {
        // Do something with our params.id information!
        console.log(`We could be doing an ajax call here!`);
        console.log(`ex: GET /animals/${params.id}`);
        console.log(`Or just a simple state lookup, redux hook, etc`);
    }, []);
    
    return (
        <div>
            <h1>Animal Details</h1>
            <p>params object: {JSON.stringify(params)}</p>
            <p>Here we could show details for an animal with id of {params.id}</p>
        </div>
    );
}

export default AnimalDetail;
```

Take a look at the object returned from the `useParams` hook. Dig into that object -- what's in it?. What could you do with that? Maybe make an Axios request to get information? This way when we refresh that page (or send the link to a friend), that information is still available from the URL.

## URL Search Parameters

In the `App.js` we'll create a link and route for our search

```JSX
<li>
    <Link to='/search?legs=1000&type=insect'>Search for insect with 1000 legs</Link>
</li>
```

And this is the only route we need

```JSX
<Route path='/search'>
   <Search />
</Route>
```

And now we're going to import a library called query string to pick the URL apart.

```
npm install query-string
```

This library takes the `?legs=1000&type=insect` part and converts it to a nice object like:

```JSX
{
    legs: '1000',
    type: 'insect',
}
```

Using it looks like this:

```JSX
import { useEffect } from 'react-router'dom';
import qs from 'query-string';
import { useHistory } from 'react-router-dom';

function Search() {
    // useHistory gives us the location object with the search string
    const history = useHistory();
    
    useEffect(() => {
        const searchObject = qs.parse(history.location.search);
        console.log(searchObject);
    }, []);

     return (
         <div>
             <h2>Search</h2>
             <h3>search string (useHistory)</h3>
             <pre>{JSON.stringify(history.location.search)}</pre>
             <h3>search object (useHistory + qs)</h3>
             <pre>{JSON.stringify(qs.parse(history.location.search))}</pre>
         </div>
     );
};

export default Search;
```