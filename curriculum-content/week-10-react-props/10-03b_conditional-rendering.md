# Create-React-App


## Objectives

- understand the term "conditional rendering"
- why it's needed
- See some of the options in React/JSX for conditional rendering

## What is Conditional Rendering

Our component's are displaying (rendering) data from their local state or props onto the DOM. You don't always want to display everything all the time. The advantage of breaking things down into components is that each one can show or not show things based on logic and it won't affect the rest of the application.

## Example

> Example repo: [Heroes Review App](https://github.com/PrimeAcademy/react-review-heroes-hooks)

This application displays a list of super heroes and allows a user to toggle if they can see the hero's real name or not. 

In plain JS and jQuery, we would simply use `if/else` syntax in our appending/rendering function(s):

### JavaScript/jQuery

```
function renderItems(heroArray) {
    $('#heroContainer').empty();
    for(let i = 0; i < heroArray.length - 1; i++) {
        
        // "conditional" to determine what we "render"
        if(heroArray[i].revealIdentity) {
            // show the real name
            $('#heroContainer').append(heroArray[i]).realName);
        }

    }
}
```


### JSX

The logic will need to be implemented in JSX and this has some different syntax than what we're used to. We can't just drop `if/else` into our JSX code.

There are several options for us to use inside the `return` of ` the component:

1. Call a function. this function can use JS if/else syntax as long as it returns us valid JSX
2. Use the Logical AND operator `&&`
    - useful to just show a single thing or not
3. Use a Ternary operator
    - useful for if/else where you want to show one thing or another thing


```jsx
import  { useState } from 'react';

function SuperHeroItem ({hero}) {

    const [mask, setMask] = useState(true);

    // toggles if we show the name or not
    toggleMask = () => {
        console.log('clicked a button');
        // set state
        setUnMask(!mask);
    }


    // determine if we should render the on or off duty message
    onOrOffDuty = () => {
        if(hero.onDuty) {
            // need to return JSX
            return <p>ON DUTY</p>;
        } else {
            // need to return JSX
            return  <p>SLEEPING</p>;
        }
    }

        // JSX on the DOM

        return (
            <div className="hero">
                <h2>{hero.superheroName}</h2>
                <p>Power: {hero.power}</p>

                {/* conditionally render the hero's real name */}
                { !mask && 
                    <p>Alias: {hero.alias}</p>
                }
                
                {/* conditionally render Duty status */}
                {/* call a class method/function and have it return us JSX */}
                { onOrOffDuty() }

                <button onClick={toggleMask}>Toggle Alias</button>
                <button onClick={avengersAssemble}>Assemble!</button>

                {/* using Logical AND operator */}
                {/* if first part is true, show the second thing */}
                { hero.onDuty && <p>ON DUTY</p> }

                {/* using a ternary operator /*}
                (/* if/else show one thing or another */}            
                {
                    hero.onDuty ? /* if() */
                        <p>ON DUTY</p> :     /* true */
                        <p>SLEEPING</p>      /* false */
                }
                
            </div>
        );
    
}

export default SuperHeroItem;

```
