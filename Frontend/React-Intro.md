# 🚀 React Quick Start Notes

These notes summarize the **React Quick Start** guide and cover the concepts you’ll use most often.

---

## ✅ What You’ll Learn
- How to create and nest components  
- How to add markup and styles  
- How to display data  
- How to render conditions and lists  
- How to respond to events and update the screen  
- How to share data between components  

---

## 🔹 Creating and Nesting Components

React apps are built out of **components**.  
A component is a piece of UI with its own logic and appearance.

```jsx
function MyButton() {
  return (
    <button>I'm a button</button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />
    </div>
  );
}
```

➡️ Note: Component names must start with a capital letter (MyButton) while HTML tags are lowercase.

## 🔹 Writing Markup with JSX
JSX lets you write markup inside JavaScript.
It’s stricter than HTML:

All tags must be closed/

Components must return one parent element

```jsx

function AboutPage() {
  return (
    <>
      <h1>About</h1>
      <p>Hello there.<br />How do you do?</p>
    </>
  );
}
```

## 🔹 Adding Styles
Use className instead of class:

```jsx

<img className="avatar" />
CSS file:

css

.avatar {
  border-radius: 50%;
}
```

## 🔹 Displaying Data
Use curly braces { } to embed variables or expressions:

```jsx

const user = {
  name: 'Hedy Lamarr',
  imageUrl: 'https://i.imgur.com/yXOvdOSs.jpg',
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Photo of ' + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize
        }}
      />
    </>
  );
}
```

## 🔹 Conditional Rendering
React uses normal JS conditions:

```jsx

if (isLoggedIn) {
  return <AdminPanel />;
}
return <LoginForm />;
```

Using ternary operator:

```jsx

<div>
  {isLoggedIn ? <AdminPanel /> : <LoginForm />}
</div>
```

Using logical AND:

```jsx

<div>
  {isLoggedIn && <AdminPanel />}
</div>
```

## 🔹 Rendering Lists
Use map() to render lists. Each child needs a unique key:

```jsx

const products = [
  { title: 'Cabbage', isFruit: false, id: 1 },
  { title: 'Garlic', isFruit: false, id: 2 },
  { title: 'Apple', isFruit: true, id: 3 },
];

export default function ShoppingList() {
  const listItems = products.map(product =>
    <li
      key={product.id}
      style={{ color: product.isFruit ? 'magenta' : 'darkgreen' }}
    >
      {product.title}
    </li>
  );

  return (
    <ul>{listItems}</ul>
  );
}
```

## 🔹 Responding to Events
Attach event handlers like onClick:

```jsx

function MyButton() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```
➡️ Don’t call the function with (). Just pass the reference: onClick={handleClick}.

## 🔹 Updating the Screen with State
Use the useState Hook to remember data:

```jsx

import { useState } from 'react';

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```
Each component instance has its own state.

## 🔹 Sharing Data Between Components (Lifting State Up)
Move state up to the nearest common parent and pass it as props:

```jsx

import { useState } from 'react';

export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update together</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}
```

This is called lifting state up.

## ⚡ Key Takeaways
Components are functions returning JSX.

JSX lets you mix HTML-like markup with JS logic.

className replaces class.

State (useState) makes components dynamic.

Props let data flow from parent to child.

Lift state up to share data across components.