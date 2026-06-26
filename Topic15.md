# 23. What is Component Composition?

## Simple Definition

**Component Composition** means **building a large component by combining multiple smaller components**.

Instead of creating one huge component that does everything, React encourages you to build many **small, reusable components** and combine them to create complex user interfaces.

Think of it as **building with LEGO blocks**.

---

# Real-Life Analogy: Building a Car

Imagine a car.

A car is not built as one giant piece.

It is made up of many smaller parts.

```
Car
│
├── Engine
├── Wheels
├── Doors
├── Seats
├── Steering Wheel
└── Dashboard
```

Each part has its own responsibility.

When all the parts are assembled together, they form a complete car.

React applications are built in exactly the same way.

---

# Another Analogy: Building a House

A house consists of:

```
House
│
├── Living Room
├── Kitchen
├── Bedroom
├── Bathroom
└── Balcony
```

Each room serves a different purpose.

Together, they create a complete house.

Similarly, a React application is built from many small components.

---

# Why Do We Need Component Composition?

Imagine building an Amazon homepage.

You could write everything inside one component.

```
App.jsx

↓

10,000 lines of code
```

This would be:

- Difficult to read
- Difficult to debug
- Difficult to maintain
- Difficult for multiple developers to work on

Instead, React encourages breaking the UI into smaller components.

---

# Amazon Homepage Example

```
Amazon App

│

├── Header
│      ├── Logo
│      ├── Search Bar
│      ├── Navigation
│      └── Cart
│
├── Banner
│
├── Product List
│      ├── Product Card
│      ├── Product Card
│      ├── Product Card
│      └── Product Card
│
└── Footer
```

Notice that each part is an independent component.

The entire page is created by composing these smaller pieces.

---

# Simple Example

Instead of writing

```jsx
function App() {
  return (
    <>
      <header>Header</header>

      <main>Main Content</main>

      <footer>Footer</footer>
    </>
  );
}
```

Create separate components.

Header

```jsx
function Header() {
  return <header>Header</header>;
}
```

Main

```jsx
function Main() {
  return <main>Main Content</main>;
}
```

Footer

```jsx
function Footer() {
  return <footer>Footer</footer>;
}
```

Now compose them.

```jsx
function App() {
  return (
    <>
      <Header />
      <Main />
      <Footer />
    </>
  );
}
```

---

# Visual Representation

```
App

│

├── Header

├── Main

└── Footer
```

Each small component contributes to the complete application.

---

# Components Can Contain Other Components

Composition can happen at multiple levels.

```
App

│

├── Header
│      ├── Logo
│      ├── Search
│      └── Login
│
├── Product Section
│      ├── Product Card
│      ├── Product Card
│      └── Product Card
│
└── Footer
```

A component can contain many child components.

Those child components can contain even more components.

---

# Example: Blog Website

```
Blog App

│

├── Navbar

├── Hero Section

├── Articles

│      ├── Article Card

│      ├── Article Card

│      └── Article Card

├── Sidebar

└── Footer
```

Each section is developed independently.

Later they are combined.

---

# Example: Dashboard

```
Dashboard

│

├── Sidebar

├── Top Navigation

├── Statistics

├── Charts

├── Recent Orders

└── Footer
```

Each widget is its own component.

---

# Component Reusability

Suppose you have a button component.

```jsx
function Button() {
  return <button>Buy Now</button>;
}
```

Instead of writing buttons everywhere,

reuse the same component.

```jsx
<Button />

<Button />

<Button />
```

One component.

Multiple uses.

---

# Composition with Props

Composition becomes even more powerful when combined with Props.

Button Component

```jsx
function Button({ text }) {
  return (
    <button>{text}</button>
  );
}
```

Reuse it.

```jsx
<Button text="Login" />

<Button text="Register" />

<Button text="Buy Now" />
```

Output

```
Login

Register

Buy Now
```

Same component.

Different data.

---

# Composition with Children

React has a special Prop called `children`.

It allows one component to wrap other components or elements.

Example

```jsx
function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}
```

Use it like this.

```jsx
<Card>
  <h2>Laptop</h2>
  <p>₹50,000</p>
</Card>
```

Output

```
+----------------------+
| Laptop               |
| ₹50,000              |
+----------------------+
```

Here:

- `Card` provides the layout.
- The content inside `<Card>...</Card>` is passed as the `children` prop.

This is a common and powerful composition pattern in React.

---

# Benefits of Component Composition

## 1. Reusability

Write once.

Use many times.

Example

```
Button

↓

Login

Register

Buy
```

---

## 2. Easy Maintenance

Need to change the Header?

Update only the Header component.

Every page using it updates automatically.

---

## 3. Better Readability

Instead of one file with 5,000 lines,

you have many small files.

```
Header.jsx

Footer.jsx

Navbar.jsx

ProductCard.jsx

App.jsx
```

Each file has one clear responsibility.

---

## 4. Easier Testing

You can test each component independently.

For example:

- Test the Login component.
- Test the Product Card.
- Test the Search Bar.

Without testing the whole application.

---

## 5. Team Collaboration

One developer builds the Header.

Another builds the Footer.

Another builds the Product Card.

Later, all components are composed into one application.

---

# Bad Design

One huge component.

```
App

↓

10,000 lines

↓

Everything
```

Problems:

- Hard to understand
- Hard to debug
- Hard to reuse

---

# Good Design

```
App

│

├── Header

├── Sidebar

├── Dashboard

├── Footer
```

Each component has a single responsibility.

---

# Real-World Examples

## YouTube

```
YouTube

│

├── Navbar

├── Sidebar

├── Video Card

├── Comments

└── Footer
```

---

## Netflix

```
Netflix

│

├── Header

├── Banner

├── Movie Row

├── Movie Card

└── Footer
```

---

## Instagram

```
Instagram

│

├── Header

├── Story

├── Post

├── Comment

└── Profile
```

Each feature is a separate component.

---

# Component Composition vs Inheritance

React recommends **Composition over Inheritance**.

Instead of creating complex class hierarchies,

React encourages combining small components.

Example

```
App

↓

Header

↓

Logo

↓

Search

↓

Login
```

This is simpler and more flexible than deep inheritance trees.

---

# Common Interview Questions

## 1. What is Component Composition?

**Answer:**

Component Composition is the process of building complex user interfaces by combining smaller, reusable React components.

---

## 2. Why Is Component Composition Important?

**Answer:**

It improves code reusability, readability, maintainability, testing, and collaboration by breaking large UIs into small, focused components.

---

## 3. What Is the `children` Prop?

**Answer:**

`children` is a special React prop that represents the content placed between a component's opening and closing tags. It allows wrapper components (like `Card`, `Modal`, or `Layout`) to display nested content.

Example:

```jsx
<Card>
  <h2>Product</h2>
</Card>
```

Inside `Card`, that `<h2>` is available as `children`.

---

## 4. Does React Prefer Composition or Inheritance?

**Answer:**

React recommends **Composition over Inheritance** because composition leads to more flexible, reusable, and easier-to-maintain components.

---

# Easy Memory Trick

Imagine building with LEGO blocks.

```
LEGO Blocks

↓

House

↓

Castle

↓

Bridge

↓

Car
```

The same small blocks can create many different structures.

React Components work the same way.

Small components are combined to build large applications.

---

# Summary

- Component Composition means building larger components by combining smaller ones.
- It follows the principle of creating small, reusable, and focused components.
- Components can contain other components, forming a tree-like structure.
- The `children` prop is a powerful composition feature that allows components to wrap and display nested content.
- Composition improves reusability, readability, maintainability, testing, and teamwork.
- React strongly recommends **Composition over Inheritance** for building scalable applications.