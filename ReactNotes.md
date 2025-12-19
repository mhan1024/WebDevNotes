# React 
- A JavaScript library used to build dynamic and interactive UIs.
- React creates a virtual DOM to perform changes before applying those changes to the browser's DOM.
- React is also component-based, which means that the UI is broken down into reusable components, like functions or classes.

## Components
- Independent and reusable code
- Returns HTML
- Syntax:
  ```javascript
  function Foo() {
    return (<h1>Hello World</h1>);
  }
  ```
- Lifecycle: the stages that a component goes through
  1. Mounting: creating and inserting a component into the DOM for the first time
  2. Updating: re-rendering a component due to changes in its state or props
  3. Unmounting: removing the component from the page
  
## Props
- Props: properties that pass information from one component to another, and allow a parent component to send data to its child components
- Strictly for reading data and cannot be modified by the receiving component
- Can be updated when the parent component's state changes
- Syntax:
  ```javascript
  function Foo(props) {
    return (<h1>Hello {props.name}</h1>);
  }
  ```
- Destructuring: limit the properties that a component receives
  ```javascript
  function Foo(props) {
    const {brand, model} = props;
    return(<h1>Brand: {brand}, Model: {model}</h1>);
  }

  // When you don't know how many properties to receive
  function Foo ({...rest}) {
    return(<h1>Brand: {rest.brand}, Model: {rest.model}</h1>);
  }
  ```
- Setting a default value
  ```javascript
  function Foo({color="blue", brand}) {
    return(<h1>Brand: {brand}, Color: {color}</h1>);
  }
  ```
  
## State Management
- State: a component's personal data storage
  - Use state to keep track of component data that will change over time
- Process of controlling how dynamic data flows and updates across components without excessive re-renders

## React Hooks
- Hooks: functions that let you "hook into" React state and lifecycle features
- General rules:
  1. Hooks can only be called inside React function components
  2. Hooks can only be called at the top level of a component
  3. Hooks can't be conditional
- Syntax:
  ```javascript
  import { useState } from 'react';
  
  function FavoriteColor() {
    const [color, setColor] = useState("red");
  
    return (
      <>
        <h1>My favorite color is {color}!</h1>
        <button
          type="button"
          onClick={() => setColor("blue")}
        >Blue</button>
        <button
          type="button"
          onClick={() => setColor("red")}
        >Red</button>
        <button
          type="button"
          onClick={() => setColor("pink")}
        >Pink</button>
        <button
          type="button"
          onClick={() => setColor("green")}
        >Green</button>
      </>
    );
  }
  ```
- useState: a Hook that allows us to track the state in a function component
- Syntax: accepts an initial state and returns 2 values, the current state and a function that updates the state
  ```javascript
  import { useState } from "react";

  function FavoriteColor() {
    const [color, setColor] = useState("red");

    // useStates can also be used for objects
    const [car, setCar] = useState({
      brand: "Ford",
      model: "Mustang",
      year: "1964",
    });

    // Update the state using spread operator
    const updateModel = () => {
      setCar(previousState => {
        return { ...previousState, model: "Jeep" }
      });
    }

    // Update the state using a button
    return (
      <>
        <h1>My favorite color is {color}!</h1>
        <button
          type="button"
          onClick={() => setColor("blue")}
        >Blue</button>
        <p>
          It is a {car.color} {car.model} from {car.year}.
        </p>
      </>
    )
  }
  ```
- useReducer: a better alternative to useState for complex state-building or when the next state value depends on the previous value
- Syntax:
  ```javascript
  const [state, dispatch] = useReducer(reducer, initialArgs, init);
  // reducer: a function that updates the state
  // initialArgs: initial state value
  // init (optional): a function to lazily initialize the state
  ```
- useEffect: allows you to perform side effects, like fetching data or updating the DOM or timers
- Syntax: accepts 2 arguments, function and dependency, and the second argument is optional
  ```javascript
  import { useState, useEffect } from 'react';

  function Timer() {
    const [count, setCount] = useState(0);
  
    useEffect(() => {
      setTimeout(() => {
        setCount((count) => count + 1);
      }, 1000);
    }, []);
  
    return <h1>I've rendered {count} times!</h1>;
  }
  ```
## Context
- Context: a way to share data globally across components without passing props down the DOM (prop drilling)
- Syntax:
  ```javascript
  // Create context
  import { createContext } from "react";

  export const ThemeContext = createContext();

  // Provide the context
  import { ThemeContext } from "./ThemeContext";

  function App() {
    return (
      <ThemeContext.Provider value="dark">
        <Dashboard />
      </ThemeContext.Provider>
    );
  }

  // Using the context
  import { useContext } from "react";
  import { ThemeContext } from "./ThemeContext";
  
  function Button() {
    const theme = useContext(ThemeContext);
  
    return <button className={theme}>Click me</button>;
  }
  ```

## Events
- Actions performed based on user events
- Synax: events are written in camelCase and event handlers are written inside curly braces
  ```javascript
  <button onClick={shoot}>Take the Shot!</button>
  ```

## Lists
- Lists are rendered with a loop, mostly commonly the map()
- Syntax:
  ```javascript
  function MyCars() {
    const cars = ['Ford', 'BMW', 'Audi'];
    return (
      <>
        <h1>My Cars:</h1>
        <ul>
          {cars.map((car) => <li>I am a { car }</li>)}
        </ul>
      </>
    );
  }
  ```

## React Router
