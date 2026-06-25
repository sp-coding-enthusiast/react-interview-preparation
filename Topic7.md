# What are React Elements?

React Elements are the **smallest building blocks of a React application**.

Think of them as **blueprints (instructions)** that tell React:

> "What should appear on the screen?"

A React Element is simply a JavaScript object that describes a UI element.

---

# Real-Life Analogy

Imagine you want to build a house.

The architect first creates a blueprint.

The blueprint contains:

- Number of rooms
- Kitchen location
- Bedroom location
- Window placements

The blueprint itself is **not the actual house**.

It only describes how the house should look.

Similarly:

- React Element = Blueprint
- Real DOM Element = Actual House

React uses React Elements to create and update the actual UI shown in the browser.

---

# Simple Example

```jsx
const element = <h1>Hello World</h1>;
```

React converts this into a JavaScript object similar to:

```javascript
{
  type: "h1",
  props: {
    children: "Hello World"
  }
}
```

This object is called a **React Element**.

---

# React Element vs HTML Element

HTML:

```html
<h1>Hello World</h1>
```

React:

```jsx
const element = <h1>Hello World</h1>;
```

In React:

1. JSX is converted into a React Element.
2. React stores the element in memory.
3. React eventually creates the actual DOM node.

---

# Creating React Elements Manually

JSX is just syntactic sugar.

This JSX:

```jsx
const element = <h1>Hello World</h1>;
```

is transformed into:

```javascript
const element = React.createElement(
  "h1",
  null,
  "Hello World"
);
```

`React.createElement()` creates a React Element object.

---

# Structure of a React Element

Example:

```jsx
const element = (
  <button className="save-btn">
    Save
  </button>
);
```

Internally:

```javascript
{
  type: "button",
  props: {
    className: "save-btn",
    children: "Save"
  }
}
```

---

# Important Properties

| Property | Description |
|-----------|-------------|
| type | HTML tag or React Component |
| props | Attributes and data |
| children | Nested content or elements |

---

# React Element Tree

Consider:

```jsx
function App() {
  return (
    <div>
      <h1>Welcome</h1>
      <p>Learning React</p>
    </div>
  );
}
```

React creates an element tree:

```text
div
├── h1
│   └── Welcome
└── p
    └── Learning React
```

This tree exists in memory and helps React determine what should appear on the screen.

---

# React Elements are Immutable

Once created, a React Element cannot be modified.

Example:

```jsx
const element = <h1>Hello</h1>;
```

This is not allowed:

```javascript
element.props.children = "Hi";
```

Instead, React creates a completely new element:

```jsx
const newElement = <h1>Hi</h1>;
```

---

# Why are React Elements Immutable?

Immutability makes comparison easy.

Old Element:

```jsx
<h1>Hello</h1>
```

New Element:

```jsx
<h1>Hi</h1>
```

React can quickly identify:

> Only the text changed.

As a result, React updates only the necessary part of the UI.

This is one reason React is fast.

---

# React Element vs React Component

These are different concepts.

## React Element

A JavaScript object describing UI.

```jsx
const element = <h1>Hello</h1>;
```

---

## React Component

A function or class that returns React Elements.

```jsx
function Welcome() {
  return <h1>Hello</h1>;
}
```

Here:

- `Welcome` = Component
- `<h1>Hello</h1>` = React Element

---

# Relationship Between Components and Elements

```text
Component
    ↓
Returns Elements
    ↓
React Builds Virtual DOM
    ↓
Updates Real DOM
    ↓
Browser Displays UI
```

---

# Example

```jsx
function User() {
  return (
    <div>
      <h2>John</h2>
      <p>Developer</p>
    </div>
  );
}
```

Flow:

```text
User Component
      ↓
Returns React Elements
      ↓
Virtual DOM Created
      ↓
DOM Updated
      ↓
UI Rendered
```

---

# React Elements and Virtual DOM

React Elements are the objects stored inside the Virtual DOM.

Example:

```jsx
<div>
  <h1>Hello</h1>
</div>
```

React creates:

```text
React Element Tree
        ↓
Virtual DOM
        ↓
Diffing Algorithm
        ↓
Real DOM Update
```

Every render creates a new React Element tree.

React compares the old and new trees to determine what changed.

---

# React Fragment Creates Elements Too

Example:

```jsx
function App() {
  return (
    <>
      <h1>Hello</h1>
      <p>Welcome</p>
    </>
  );
}
```

React creates fragment elements internally and avoids unnecessary DOM nodes.

---

# React Element Lifecycle

```text
JSX
 ↓
React.createElement()
 ↓
React Element Object
 ↓
Virtual DOM
 ↓
Diffing/Reconciliation
 ↓
Real DOM Update
 ↓
Browser UI
```

---

# Common Interview Questions

## Is a React Element a Real DOM Node?

No.

A React Element is a lightweight JavaScript object that describes the UI.

React later converts it into actual DOM nodes.

---

## Are React Elements Mutable?

No.

React Elements are immutable.

Whenever UI changes, React creates a new element tree and compares it with the previous one.

---

## Does JSX Create React Elements?

Yes.

JSX is transformed into `React.createElement()` calls, which create React Elements.

---

## Can Components Return Multiple Elements?

Yes.

Using Fragments:

```jsx
<>
  <h1>Hello</h1>
  <p>Welcome</p>
</>
```

---

## Where are React Elements Stored?

They are stored in the Virtual DOM as JavaScript objects.

---

# Senior-Level Interview Answer

A React Element is the smallest unit in React and represents an immutable JavaScript object that describes what should appear on the UI. JSX is compiled into `React.createElement()` calls, which create React Elements. React stores these elements in the Virtual DOM, compares old and new element trees during reconciliation, and updates only the changed portions of the Real DOM for efficient rendering.