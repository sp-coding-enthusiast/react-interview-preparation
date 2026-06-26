# 12. What are Components?

## Simple Definition

A **Component** is a **small reusable piece of UI (User Interface)** that performs one specific job.

Think of it like **LEGO blocks**.

Instead of building an entire house as one huge piece, you build it using many small blocks.

React works exactly the same way.

Instead of writing one huge webpage, we create many small components and combine them together.

---

## Real Life Example: Building a House

Imagine you're building a house.

A house contains:

- Door
- Windows
- Kitchen
- Bedroom
- Bathroom
- Roof

Each of these is built separately.

Later, they are assembled into one complete house.

```
House
│
├── Door
├── Window
├── Kitchen
├── Bedroom
└── Bathroom
```

React applications are built exactly like this.

```
App
│
├── Header
├── Sidebar
├── Navbar
├── Product Card
├── Shopping Cart
├── Footer
```

Every item above is a separate component.

---

# Example: Amazon Homepage

Instead of writing one giant HTML page...

```
Entire Amazon Page
```

React divides it into components.

```
Amazon App
│
├── Header
│     ├── Logo
│     ├── Search Bar
│     ├── Login Button
│     └── Cart
│
├── Banner
│
├── Product List
│     ├── Product Card
│     ├── Product Card
│     ├── Product Card
│     └── Product Card
│
└── Footer
```

Notice something?

The **Product Card** is repeated many times.

Instead of writing it 100 times...

React creates **one component**.

Then reuses it.

---

# Without Components

Imagine writing this manually.

```
Product 1

Image
Name
Price
Button

Product 2

Image
Name
Price
Button

Product 3

Image
Name
Price
Button

Product 4

Image
Name
Price
Button

...
100 products
```

Thousands of duplicate lines.

Very difficult to maintain.

---

# With Components

Create one Product component.

```
<Product />

<Product />

<Product />

<Product />

<Product />
```

Same code.

Different data.

Very clean.

---

# Real Life Example: Restaurant Menu

A restaurant has one menu card design.

```
------------------
Pizza

Price

Buy
------------------
```

They don't redesign the menu for every pizza.

Instead they reuse the same design.

```
Pizza

Burger

Pasta

Sandwich
```

Same layout.

Different data.

That's exactly how React Components work.

---

# Example Component

```jsx
function Header() {
  return <h1>Welcome</h1>;
}
```

This creates a Header component.

Use it anywhere.

```jsx
<Header />
```

React displays

```
Welcome
```

---

# Another Example

```jsx
function Footer() {
  return <p>Copyright 2026</p>;
}
```

Use it

```jsx
<Footer />
```

Output

```
Copyright 2026
```

---

# Combining Components

```
App
│
├── Header
├── Main Content
├── Product List
└── Footer
```

Code

```jsx
function App() {
  return (
    <>
      <Header />
      <ProductList />
      <Footer />
    </>
  );
}
```

Output

```
---------------------
Welcome
---------------------

Products

---------------------

Copyright
```

---

# Components Can Contain Components

Just like a car contains many parts.

```
Car
│
├── Engine
├── Wheels
├── Seats
└── Dashboard
```

React also allows nesting.

```
App
│
├── Header
│      ├── Logo
│      ├── Search
│      └── Login
│
├── Main
│
└── Footer
```

Small components combine into larger ones.

---

# Why Components Are Powerful

## 1. Reusable

Write once.

Use everywhere.

```jsx
<Button />

<Button />

<Button />
```

---

## 2. Easy Maintenance

Need to change button color?

Only change one component.

Everything updates automatically.

---

## 3. Cleaner Code

Instead of

```
5000 lines
```

You split them.

```
Header.jsx

Navbar.jsx

Product.jsx

Footer.jsx
```

Much easier to understand.

---

## 4. Team Work

One developer builds Header.

Another builds Footer.

Another builds Login.

Later they combine everything.

---

# Components Are Like Functions

Functions

```
Input

↓

Processing

↓

Output
```

Components

```
Data

↓

UI Generation

↓

HTML
```

Very similar concept.

---

# Think of Components Like...

🏠 House → Rooms

🚗 Car → Engine + Wheels

📱 Mobile → Camera + Screen + Battery

🧱 LEGO → Individual Blocks

🍔 Burger → Bun + Patty + Cheese

React Applications

```
Application

↓

Many Components

↓

Complete UI
```

---

# Component Naming Rules

React components must start with a capital letter.

Correct

```jsx
function Header() {}
```

Wrong

```jsx
function header() {}
```

Why?

Because React treats lowercase names as HTML elements.

```
<div>

<span>

<p>
```

Uppercase names are considered React components.

```
<Header />

<Navbar />

<Product />
```

---

# Component File Structure

```
src

│

├── App.jsx

├── components

│      ├── Header.jsx

│      ├── Footer.jsx

│      ├── Navbar.jsx

│      └── Product.jsx
```

This organization keeps projects manageable.

---

# Summary

- A Component is a reusable piece of UI.
- Components help break large applications into smaller parts.
- They make code cleaner, reusable, and easier to maintain.
- Components can be nested inside other components.
- React applications are built by combining many components.
- Component names always start with a capital letter.

---

# 13. Functional vs Class Components

## Introduction

React components can be created in two ways:

1. Functional Components
2. Class Components

Today, **Functional Components** are the standard and recommended approach. Class Components are mainly encountered in older React codebases.

---

# Analogy: Writing a Letter

Imagine you need to write a letter.

### Old Method

Write everything by hand.

Slow.

More work.

```
Dear...

Write...

Sign...
```

### Modern Method

Use a computer.

Type.

Edit.

Save.

Faster and simpler.

Functional Components are like using a computer.

Class Components are like writing by hand.

Both work.

One is much easier.

---

# Functional Component

A Functional Component is simply a JavaScript function that returns JSX.

```jsx
function Welcome() {
  return <h1>Hello</h1>;
}
```

Use it like this.

```jsx
<Welcome />
```

Output

```
Hello
```

---

# Arrow Function Version

```jsx
const Welcome = () => {
  return <h1>Hello</h1>;
};
```

Both versions are equivalent.

---

# Class Component

Before Hooks were introduced, React used classes.

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello</h1>;
  }
}
```

Notice the extra syntax:

- `class`
- `extends`
- `render()`
- `this`

This makes class components more verbose.

---

# Comparing the Same Component

## Functional Component

```jsx
function Header() {
  return <h1>My Website</h1>;
}
```

Very straightforward.

---

## Class Component

```jsx
class Header extends React.Component {
  render() {
    return <h1>My Website</h1>;
  }
}
```

Same output.

More code.

---

# Why Functional Components Became Popular

Originally, Functional Components could only display UI.

They couldn't manage state or lifecycle behavior.

Class Components were required for those features.

React later introduced **Hooks**, allowing Functional Components to do everything Class Components could, with much simpler syntax.

Example using state:

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <>
      <h2>{count}</h2>

      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </>
  );
}
```

No class.

No `this`.

No `render()`.

---

# Code Size Comparison

## Functional

```jsx
function App() {
  return <h1>Hello</h1>;
}
```

Only a few lines.

---

## Class

```jsx
class App extends React.Component {
  render() {
    return <h1>Hello</h1>;
  }
}
```

More boilerplate for the same result.

---

# Real Life Analogy

Imagine two ways to make tea.

### Functional Component

```
Boil water

Add tea

Serve
```

Simple.

---

### Class Component

```
Create kitchen

Create utensils

Create pot

Create water

Boil

Serve
```

Same tea.

More setup.

---

# Key Differences

| Feature | Functional Component | Class Component |
|----------|----------------------|-----------------|
| Syntax | Simple JavaScript function | ES6 class |
| Boilerplate | Minimal | More verbose |
| State | Uses Hooks (`useState`) | Uses `this.state` |
| Lifecycle | Uses Hooks (`useEffect`) | Lifecycle methods (`componentDidMount`, etc.) |
| `this` keyword | Not required | Required |
| Performance | Comparable in modern React | Comparable |
| Learning curve | Easier | Harder |
| Current recommendation | ✅ Preferred | Mostly legacy code |

---

# When Will You See Class Components?

Even though new projects use Functional Components, you may still encounter Class Components when:

- Maintaining older React applications.
- Working on enterprise projects that haven't been fully modernized.
- Preparing for interviews, where understanding both approaches is expected.

---

# Interview Question

**Can Functional Components replace Class Components?**

**Answer:**

Yes. With the introduction of Hooks, Functional Components can manage state, handle side effects, access context, optimize rendering, and perform nearly all tasks previously handled by Class Components. This is why most modern React applications are built using Functional Components.

---

# Summary

- React supports Functional and Class Components.
- Functional Components are plain JavaScript functions that return JSX.
- Class Components are based on ES6 classes and use a `render()` method.
- Hooks enable Functional Components to manage state and lifecycle behavior.
- Functional Components are simpler, easier to read, and are the recommended approach for modern React development.
- Class Components are still important to understand because many existing enterprise applications continue to use them.