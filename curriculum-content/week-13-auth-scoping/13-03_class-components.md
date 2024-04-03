# Class Components

Function Components work great! You can use them for all of your components in your react applications, but we want to show another way: Class Components.

What function components return can be thought of as just the `render` part of a class component. Function components can have props, the biggest difference is they do not have local state by default - with the addition of React Hooks, this is technically false, but it used to be true.

Here is a classic function component with hooks:

```JSX
import {useState} from 'react';
import logo from './logo.svg';

const Header = ({title}) => {
  const [isHidden, setIsHidden] = useState(false);

  const handleClick = () => {
    setIsHidden(!isHidden)
  }

  return (
  <div className="App">
    <header onClick={handleClick} className="App-header">
      <img src={logo} className="App-logo" alt="logo" />
      {isHidden && (
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
      )}
      <a
        className="App-link"
        href="https://reactjs.org"
        target="_blank"
        rel="noopener noreferrer"
      >
        {title}
      </a>
    </header>
  </div>
);
}

export default Header;

```

Now lets see the same component written as a Class:

```JSX
import {Component} from 'react';
import logo from './logo.svg';

class Header extends Component {

  state = {
    isHidden : false
  }

  handleClick = () => {
    this.setState({isHidden: !isHidden})
  }

  render () {
    return (
      <div className="App">
        <header onClick={this.handleClick} className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          {this.state.isHidden && (
            <p>
              Edit <code>src/App.js</code> and save to reload.
            </p>
          )}
          <a
            className="App-link"
            href="https://reactjs.org"
            target="_blank"
            rel="noopener noreferrer"
          >
            {this.props.title}
          </a>
        </header>
      </div>
)}

export default Header;

```

The the `render()` method part of the Class component is the same as the return in a Function Component. 

Props are accessed on `this`, along with methods, and state. In order to change state, you use `this.setState` which is provided by the `extends Component`

Classes are more verbose, and use `this` a lot. React is slowly moving away from classes, but you will see and run into them. 

That's it!