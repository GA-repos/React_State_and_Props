![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)

# React State

In React, we can't change the values in props object.  They are meant to be **immutable**. So, if we can't change the value in a props object how do we update our page?  The answer is: **State**.

## Learning Objectives

- Review passing data to a React component via props.
- Identify state in a React app.
- Modify the state of a React component through events.
- Distinguish container and presentational components.

## Framing

In computer science terms, we descibe systems as stateful if they are designed to remember preceding events or user interactions; the remembered information is called the **state** of the system.

So far in this program, we used an imperative model of programming.  React uses a declarative style of programming.  **_Declarative programming_** is like describing a picture, whereas **_imperative programming_** is like a set of instructions for painting that picture. React enables us to design views for each **state** in our application, and it handles efficiently updating and rendering views when our data changes.

## Class-based Components

In order to use state in a React component, we need to use a different syntax for our component.  Fortunately, it's a syntax that you've seen before.  Stateful components use ES6 **class** syntax.

Let's change up the state of our App component so it can be a stateful component together.

### We Do: Class Components

We're going to change the App from a stateless function component to a class component, we need to:

1. Change the function declaration to a class declaration.
1. Use `extends` to inherit from `React.Component`.
1. Wrap the return statement in a `render` method.

```jsx
import React from 'react';
import './App.css';
import { movies } from './Data';
import Header from './Header';
import Movies from './Movies';

class App extends React.Component {
  render() {
    return (
      <>
        <Header />
        <Movies movies={movies} />
      </>
    );
  }
}

export default App;
```

Let's break this down:

#### `import React from 'react'`

This imports React methods from the React library just like we did in our stateless function components.

#### `class App`
This is the component we're creating. In this example, we are creating a component and calling it "App."

#### `extends React.Component`
We inherit from the Component React library class to create our component definitions. Here, we are creating a new Component subclass called App.

Because it extends (inherits from) Component, our App class gets to reuse code and capabilities from `React.Component`.

#### `render()`
Every component has a render method. The render method is what renders the component to the screen, so it controls what is displayed for this component. From this function, we return what we want to display, just like we did in the body of our stateless function components.

#### `export default App`
This exposes the App class to other files, in this case, our index.js file.  Here again, there's no difference from the export used previous when App was a stateless function component.

Make sure that your app still renders!

## Adding State

In React, we define the state of our component inside the constructor.  Let's add that now:

```js
constructor(props) {
  super(props);
  this.state = {
    movies: movies
  };
}
```

We've defined our state object and we've added a property for movies.  We're setting the initial value for the movies property to equal the movies array we imported earlier that's stored in memory.

Check the results in the browser and make sure nothing has changed.

## Listening for User Interactions

In the past when we wanted to listen for an event, we created an event listener and attached it to an element in the DOM.  Remember though, we're not interacting directly with the DOM in React.  That's React's job.

With React's declarative programming model, when we want to listen for an event on an element we'll just attach the event handler directly to the element.  Let's add a button to the App component so we can see how this works.

```jsx
<button onClick={() => alert('you clicked me')}>Click me</button>
```

We can find a comprehensive list of all of the events React supports in the [docs](https://reactjs.org/docs/events.html#supported-events).

Lets create method that will filter out any movies that the audience did not rate 60 or higher.

```js
filterMovies = () => {
 const filteredMovies = this.state.movies.filter(
   movie => movie.audience_score >= 60
 );
 console.log(filteredMovies);
};

```

Cool! Now instead of just logging the value to the console, let's update the state!  To do this we **must** use a special method provided by React called `setState()`.  This method takes an object with the property to update set to the new value.  Replace the `console.log` in your `filteredMovies` method with:

```js
this.setState({ movies: filteredMovies });
```

When the state is changed using `setState()`, React automatically kicks off a process that renders any of the affected components and their children! Thanks to the Virtual DOM it will only update the actual DOM when the data that is changed affects the specific component element.

## State Summary

- Stateful components must use class syntax.
- State is updated by calling `setState()`.
- State is locally scoped (within the class component where it is defined), but can be passed as props.

## Setting State from a Child Component

So this is really cool, but it'd be so cool if we could filter the movies when a user clicks on the **Must See Movies** in the Header component.

We can do that by passing the `filterMovies` method to the Header component as a prop.

```jsx
<Header filterMovies={this.filterMovies} />
```

Now in our Header component let's use the method.

```jsx
function Header(props) {
  return (
    <header>
      <h1>Reelz: The Movie App</h1>
      <Welcome name="Jen" firstTime={false} />
      <div>
        <ul>
          <li>Now Playing</li>
          <li onClick={props.filterMovies}>Must See Movies</li>
        </ul>
      </div>
    </header>
  );
}
```

## You Do: Show All Movies

Create a function that uses `setState()` to set the movies property in state back to the movies that we have in memory.

Test it inside of App first by making the "Click me" button reset the movie list.  Then, once it's working pass it as a prop to the Header component and make it so that when a user clicks on the "Now Playing" link, it displays all of the movies again.

## Showing and Hiding Elements

Let's make a toggle so that we can show or hide our movie list. We need to create a new bit of state for this, so add to our state object in App:

```jsx
this.state = {
  movies: movies,
  showMovieList: true
}
```

Now we'll wrap our Movies component in a JavaScript expression.

```jsx
{this.state.showMovieList && <Movies movies={this.state.movies} />}
```

Check the browser to see if anything changed.  Now manually change the value of the `showMovieList` property to false and check the browser again.

## You Do: Show and Hide Toggle

Create a method called `toggleMovies` so that it updates the state of `showMovieList` by toggling the value between `true` and `false`.

## Additional Resources

- [Visual Guide to State in React | Dave Ceddia](https://daveceddia.com/visual-guide-to-state-in-react/)
- [Component State | Codecademy](https://www.codecademy.com/courses/react-101/lessons/this-state)
- [React State | React Docs](https://reactjs.org/docs/state-and-lifecycle.html)
- [Thinking in React | React Docs](https://reactjs.org/docs/thinking-in-react.html)
- [Imperative vs. Declarative Javascript](http://www.tysoncadenhead.com/blog/the-state-of-javascript-a-shift-from-imperative-to-declarative#.VxgGxZMrKfQ)
- [Styling in React](http://survivejs.com/webpack_react/styling_react/)

## [License](LICENSE)

All content is licensed under a CC­BY­NC­SA 4.0 license.

All software code is licensed under GNU GPLv3. For commercial use or alternative licensing, please contact legal@ga.co.
