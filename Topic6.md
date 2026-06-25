# 9. What is JSX?

## Simple Definition

**JSX (JavaScript XML)** is a syntax extension for JavaScript that allows us to write HTML-like code inside JavaScript.

Instead of creating UI using complicated JavaScript statements, JSX allows us to write code that looks very similar to HTML.

---

# Why Was JSX Introduced?

Imagine building a UI using pure JavaScript.

Without JSX:

```javascript
const element = React.createElement(
  "h1",
  null,
  "Hello World"
);
```

This works, but it becomes difficult to read when the UI grows.

Now with JSX:

```jsx
const element = <h1>Hello World</h1>;
```

Much easier to understand.

---

# Real-Life Analogy

Imagine you want to build a house.

### Without JSX

You explain every detail manually:

```text
Create wall
Create window
Create door
Attach window to wall
Attach door to wall
```

Very verbose.

---

### With JSX

You simply show the blueprint:

```text
House
 ├── Door
 └── Window
```

Much easier.

JSX acts like the blueprint of the UI.

---

# Basic JSX Example

```jsx
function App() {
  return <h1>Hello React</h1>;
}
```

Looks like HTML.

But it is actually JavaScript.

---

# JSX Can Contain JavaScript

Use curly braces `{}`.

```jsx
const name = "John";

function App() {
  return <h1>Hello {name}</h1>;
}
```

Output:

```html
<h1>Hello John</h1>
```

---

# JSX with Expressions

```jsx
const age = 25;

function App() {
  return <h1>Age: {age + 5}</h1>;
}
```

Output:

```html
<h1>Age: 30</h1>
```

Anything inside `{}` is treated as JavaScript.

---

# JSX with Functions

```jsx
function greet() {
  return "Welcome";
}

function App() {
  return <h1>{greet()}</h1>;
}
```

Output:

```html
<h1>Welcome</h1>
```

---

# JSX with Attributes

HTML:

```html
<img src="logo.png" alt="Logo">
```

JSX:

```jsx
<img src="logo.png" alt="Logo" />
```

Looks almost identical.

---

# Difference Between HTML and JSX

## HTML

```html
<label for="name">Name</label>
```

## JSX

```jsx
<label htmlFor="name">Name</label>
```

Because `for` is a reserved JavaScript keyword.

---

## HTML

```html
<div class="container"></div>
```

## JSX

```jsx
<div className="container"></div>
```

Because `class` is a JavaScript keyword.

---

# JSX Must Have One Parent Element

❌ Invalid

```jsx
return (
  <h1>Hello</h1>
  <p>Welcome</p>
);
```

React does not know how to return two separate elements.

---

✅ Correct

```jsx
return (
  <div>
    <h1>Hello</h1>
    <p>Welcome</p>
  </div>
);
```

---

# React Fragment

Instead of adding unnecessary divs:

```jsx
return (
  <>
    <h1>Hello</h1>
    <p>Welcome</p>
  </>
);
```

React Fragment creates no extra HTML element.

---

# JSX is NOT HTML

Many beginners think:

```text
JSX = HTML
```

Wrong.

JSX only looks like HTML.

Under the hood:

```text
JSX
   ↓
JavaScript Function Calls
   ↓
React Elements
   ↓
Virtual DOM
   ↓
Real DOM
```

---

# Benefits of JSX

### Readability

Easy to understand.

```jsx
<h1>Hello</h1>
```

instead of:

```javascript
React.createElement("h1", null, "Hello");
```

---

### Easier UI Development

HTML-like syntax feels natural.

---

### JavaScript Integration

Can use variables, loops, functions, conditions.

---

### Better Developer Experience

Cleaner and maintainable code.

---

# Interview Answer (Short Version)

**JSX (JavaScript XML) is a syntax extension for JavaScript used in React that allows developers to write HTML-like code inside JavaScript. JSX makes UI code easier to read and write. It is not understood directly by browsers and is transformed into JavaScript function calls before execution.**

---

# 10. How is JSX Transformed Internally?

One of the most common React interview questions is:

> "If browsers understand only JavaScript, how does JSX work?"

The answer is:

**Browsers do NOT understand JSX directly.**

JSX must first be converted into regular JavaScript.

This conversion is performed by tools like:

- Babel
- TypeScript Compiler
- React Build Tools

---

# High-Level Flow

```text
JSX Code
    ↓
Babel Compilation
    ↓
React.createElement()
    ↓
React Element Object
    ↓
Virtual DOM
    ↓
Real DOM
```

---

# Example 1

JSX:

```jsx
const element = <h1>Hello World</h1>;
```

Babel converts it to:

```javascript
const element = React.createElement(
  "h1",
  null,
  "Hello World"
);
```

React then creates a React Element object.

---

# What Does React.createElement Return?

It returns a JavaScript object.

Example:

```javascript
{
  type: "h1",
  props: {
    children: "Hello World"
  }
}
```

This object becomes part of the Virtual DOM.

---

# Step-by-Step Internal Working

## Step 1: Developer Writes JSX

```jsx
<h1>Hello React</h1>
```

---

## Step 2: Babel Compiles JSX

```javascript
React.createElement(
  "h1",
  null,
  "Hello React"
);
```

---

## Step 3: React Creates Element Object

```javascript
{
  type: "h1",
  props: {
    children: "Hello React"
  }
}
```

---

## Step 4: Virtual DOM Creation

React stores this object in memory.

```text
Virtual DOM
```

---

## Step 5: Real DOM Rendering

React converts it into:

```html
<h1>Hello React</h1>
```

and displays it in the browser.

---

# Example 2: Nested JSX

JSX:

```jsx
<div>
  <h1>Hello</h1>
  <p>Welcome</p>
</div>
```

---

Babel converts it to:

```javascript
React.createElement(
  "div",
  null,
  React.createElement(
    "h1",
    null,
    "Hello"
  ),
  React.createElement(
    "p",
    null,
    "Welcome"
  )
);
```

Notice how nested JSX becomes nested function calls.

---

# Modern React Transformation (React 17+)

Older React versions required:

```javascript
import React from "react";
```

because JSX became:

```javascript
React.createElement(...)
```

---

React 17 introduced the new JSX Transform.

JSX:

```jsx
<h1>Hello</h1>
```

can become:

```javascript
import { jsx } from "react/jsx-runtime";

jsx("h1", {
  children: "Hello"
});
```

Now importing React solely for JSX is no longer required.

---

# Real-Life Analogy

Imagine JSX is English.

```text
Build a red house with two windows.
```

Browser understands only machine instructions.

A translator converts:

```text
English
   ↓
Machine Instructions
```

Similarly:

```text
JSX
   ↓
Babel
   ↓
JavaScript
```

Babel acts as the translator.

---

# Complete Internal Flow Example

Developer writes:

```jsx
function App() {
  return <h1>Hello React</h1>;
}
```

---

Compiler transforms:

```javascript
function App() {
  return React.createElement(
    "h1",
    null,
    "Hello React"
  );
}
```

---

React creates:

```javascript
{
  type: "h1",
  props: {
    children: "Hello React"
  }
}
```

---

Stored as:

```text
Virtual DOM
```

---

Rendered as:

```html
<h1>Hello React</h1>
```

---

# Why Is This Important?

Understanding JSX transformation helps explain:

- How React works internally
- Virtual DOM creation
- Rendering process
- Reconciliation process
- React interview questions

Many senior-level React interviews ask:

> "What happens internally when JSX is written?"

The answer starts with:

```text
JSX
  ↓
Babel
  ↓
React.createElement()
  ↓
React Element
  ↓
Virtual DOM
  ↓
Real DOM
```

---

# Interview Answer (Short Version)

**JSX is not understood by browsers directly. During the build process, Babel (or TypeScript) transforms JSX into JavaScript function calls such as `React.createElement()` (or `jsx()` in React 17+). These function calls create React Element objects, which React uses to build the Virtual DOM. React then compares Virtual DOM changes and efficiently updates the Real DOM.**