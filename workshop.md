# [Click Here to Get Started](https://codesandbox.io/s/ljg0t8?file=/App.js&utm_medium=sandpack)
This link sets up an online environment on a site called CodeSandbox for a React workspace. For this workshop we will be using the online workspace, but you can also choose to work on your own machine

# Intro

This is the Intro to React workshop created by the UC CEAS Library. In this workshop, you will learn what React is all about, and build a simple tic-tac-toe game to demonstrate the capabilities. 

## What is React?
React.js is a JavaScript Libarary created by Facebook. It is currently a very popular front-end web library. Its main purpose is to build components that are reusable and repeatable.

## Why use React?
In a large project, you are bound to have many moving parts. Many of which are repetitive or have complicated behaviors. React helps developers by providing a library that helps compartmentalize repetitive and complicated parts of the UI into componenets. 

# Inspecting the starting code

## App.js
- This is our first component
- In React, a component is a piece of reusable code that represents a part of a user interface. 
- Components are used to render, manage, and update the UI elements in your application.
- Components are expressed as a function, and what they return is what the component will look like.

## styles.css
- Stylesheets don't change in behavior in React
- .square defines the style of the square you see

## index.js
- This file isn't something we will edit but it allows us to use React in the project
- Imports the React DOM which lets React "talk" to the browser
- Also inserts App.js (Root Component)

# Making the Board

- We are going to need a 3x3 grid of smaller squares to create a larger one. 
- Change App.js to the following:

```javascript
export default function Board() {
  return (
    <>
      <div className="board-row">
        <button className="square">1</button>
        <button className="square">2</button>
        <button className="square">3</button>
      </div>
      <div className="board-row">
        <button className="square">4</button>
        <button className="square">5</button>
        <button className="square">6</button>
      </div>
      <div className="board-row">
        <button className="square">7</button>
        <button className="square">8</button>
        <button className="square">9</button>
      </div>
    </>
  );
}
```

- In our return, we wrap our whole element with ()
- Components must return exactly one root element at a time. Meaning, it cannot have several elements at the same hierarchial level. This is why we use fragments (the <> and </>) to wrap the whole thing.
- Within, we have 9 buttons for each square. But we use a div to separate the three rows. The `board-row` class separates each row instead of making one long row.

# Making our Second Component and Using Props

- To play the game, we will need to individually manage the state of each square
- We should make each square its own component, and make the whole board their parent component (the component that sits above another component, allowing the child component to exist)
- Make a new function called `Square` and move the button from the `Board` into `Square`'s return. 

```javascript
function Square(){
    return <button className="square">1</button>;
}
```

- Then, change `Board`'s render to use `Square` instead of the button tag from before.

```javascript
export default function Board() {
  return (
    <>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
    </>
  );
}
```
## Props 

- Each of the squares should show "1". We are going to need to identify each square separately.
- Change the function parameters of Square to include `{ value }`. 
    - This tells React that this component is going to accept a prop called value
    - A prop is property that you can pass into a component when rendering it to change its behavior
    - With value, inside of `Square`'s return we can refer to value by wrapping with {}

```javascript
function Square({ value }){
    return <button className="square">{value}</button>;
}
```

- To pass a prop, in the parent component (Board), add an attribute to the declaration of the component

```javascript
export default function Board() {
  return (
    <>
      <div className="board-row">
        <Square value="1" />
        <Square value="2" />
        <Square value="3" />
      </div>
      <div className="board-row">
        <Square value="4" />
        <Square value="5" />
        <Square value="6" />
      </div>
      <div className="board-row">
        <Square value="7" />
        <Square value="8" />
        <Square value="9" />
      </div>
    </>
  );
}
```

# Event Handling and States

- When we click a square, we should see an X appear
    - Of course, we will need to alternate between X and O but that comes later
- To do this, we need to craete a function which will be the "handler" when the square is clicked and we need to then pass this function to the button using the `onClick` attribute.

```javascript
function Square({ value }) {
  function handleClick() {
    console.log('clicked!');
  }

  return (
    <button
      className="square"
      onClick={handleClick}
    >
      {value}
    </button>
  );
}
```

- Next we will change how the component looks when we click on it!
- We store properties of a component in its state.

```javascript
import { useState } from 'react';

function Square() {
  const [value, setValue] = useState(null);

  function handleClick() {
    //...
```

- We have created two things:
    - `value` is a variable that we can use to store information
    - `setValue` is a function that we can use to update `value`
- We will use these together to place an X when we click on a square!

```javascript
function Square() {
  const [value, setValue] = useState(null);

  function handleClick() {
    setValue('X');
  }

  return (
    <button
      className="square"
      onClick={handleClick}
    >
      {value}
    </button>
  );
}
```
- **Remove** the passed prop from the `Board` component and make the above changes to `Square`
- In the browser try clicking on squares. They should fill up with an X!

# Making a playable game

- Next, let's add alternating Xs and Os for each click.
- Currently, each square manages its own state separately. Thus, it is difficult for us to know where we are in the game from a single place.
- We will return to our previous setup where the `Board` passed a prop into each child component. We will then move the state of the game into `Board.

```javascript
import { useState } from 'react';

function Square({ value }) {
  return <button className="square">{value}</button>;
}

export default function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));
  return (
    <>
      <div className="board-row">
        <Square value={squares[0]} />
        <Square value={squares[1]} />
        <Square value={squares[2]} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} />
        <Square value={squares[4]} />
        <Square value={squares[5]} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} />
        <Square value={squares[7]} />
        <Square value={squares[8]} />
      </div>
    </>
  );
}
```

- When we click on a `Square`, we should be update to update the state of `Board` then. This cannot be done directly, so we pass `handleClick` as a prop into each child square then.

```javascript
export default function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));

  function handleClick(i) {

    const nextSquares = squares.slice();
    nextSquares[i] = 'X';
    setSquares(nextSquares);
  }

  return (
    <>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}
```
- We use `handleClick` as a wrapper for `setSquares` to update the state of the squares. Also note that we do not directly pass `handleClick` to a `Square`. We pass a function that calls `handleClick`. This is because without doing this, when we try to pass the function we would be actually calling the function, which is not the intention. We want to only call the function when the square is clicked.

# Taking turns
From here, we need to go from just placing Xs, to Os as well, and eventually finding out who wins.

We have to keep track of whose turn it is using state. We add the following to the state of `Board`:
```javascript
export default function Board() {
  const [xIsNext, setXIsNext] = useState(true);
  const [squares, setSquares] = useState(Array(9).fill(null));
  //...
}
```

`handleClick` then needs some updates:
- Add a check for the state of `squares[i]`. If it's been selected, return the function and take no action.
- Then, add an if/else block for each of the `xIsNext` states. This determines which letter gets placed in the box.
- Lastly, update `xIsNext` with the mutator `setXIsNext`

```javascript
function handleClick(i) {
    if (squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    setSquares(nextSquares);
    setXIsNext(!xIsNext);
  }
```

Now, X and O can take turns. And they cannot override an existing square.

# Declaring a winner

Given our board state, we should use this info to detect if a player has won. Here is the code; don't focus too much on the logic as it is not important to React itself.

```javascript
function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

What matters is more is how we will use it. 

```javascript
function handleClick(i) {
  if (squares[i] || calculateWinner(squares)) {
    return;
  }
  const nextSquares = squares.slice();
  //...
}
```
Here, we use it to prevent players from updating the board once a winner has been found.

```javascript
export default function Board() {
  // ...
  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = "Winner: " + winner;
  } else {
    status = "Next player: " + (xIsNext ? "X" : "O");
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        // ...
  )
}
```
This adds a line of text that shows the players the current game status. 
- If there is a winner, display the name of the winner
- If not, show who is next
    - `(xIsNext ? "X" : "O")` is an example of a ternary operator, a relatively newer feature in javascript. 
    - This is an if/else statement all in one. The part left of the ? is the condition, the part left of the : is the result if `true` and the last part is if `false`.

# Conclusion

With that, we have a working tic-tac-toe game in React.