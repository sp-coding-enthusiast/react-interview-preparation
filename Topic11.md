# 17. What is One-Way Data Binding?

## Simple Definition

**One-Way Data Binding** means that **data flows in only one direction** in a React application.

The flow is always:

```
Parent Component

↓

Child Component

↓

User Interface (UI)
```

Data moves **from Parent to Child** using **Props**.

The child component **cannot directly change the parent's data**.

---

# Layman Analogy: Water Tank

Imagine a water tank connected to a tap.

```
Water Tank

↓

Pipe

↓

Tap
```

Water flows only in one direction.

It does **not** flow back into the tank.

React data works the same way.

```
Parent

↓

Props

↓

Child
```

The data flows downward only.

---

# Another Analogy: Teacher and Students

Imagine a classroom.

The teacher writes on the board.

```
Teacher

↓

Instructions

↓

Students
```

Students receive the information.

They cannot directly change what is written on the board.

If something needs to change, the **teacher** changes it.

In React:

- Teacher = Parent Component
- Students = Child Components
- Instructions = Props

---

# Why Does React Use One-Way Data Binding?

Imagine a large application with hundreds of components.

If every component could change every other component's data directly, it would become:

- Hard to understand
- Difficult to debug
- Unpredictable
- Error-prone

One-way data flow makes applications easier to reason about because data has a clear direction.

---

# Visual Representation

```
Parent

↓

Props

↓

Child

↓

UI
```

Notice:

The arrow only points downward.

---

# Basic Example

## Parent Component

```jsx
function App() {
  return <Welcome name="Saurabh" />;
}
```

---

## Child Component

```jsx
function Welcome({ name }) {
  return <h1>Hello {name}</h1>;
}
```

Output

```
Hello Saurabh
```

Flow

```
App

↓

name = "Saurabh"

↓

Welcome

↓

Hello Saurabh
```

The child only receives the data.

---

# Example: Product Card

Parent

```jsx
function App() {
  return (
    <>
      <Product
        name="Laptop"
        price={50000}
      />

      <Product
        name="Phone"
        price={20000}
      />
    </>
  );
}
```

Child

```jsx
function Product({ name, price }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>₹{price}</p>
    </div>
  );
}
```

Flow

```
App

↓

Product

↓

Displays Product
```

The Product component simply displays what it receives.

---

# Can the Child Change Props?

Suppose we try this.

```jsx
function Welcome(props) {
    props.name = "Rahul";
}
```

❌ Wrong

React does not allow this.

Props are **read-only**.

Only the parent component should decide what value to pass.

---

# If Data Needs to Change

Suppose the parent has this State.

```jsx
const [name, setName] =
  useState("John");
```

The parent passes it as a Prop.

```jsx
<Welcome name={name} />
```

Flow

```
State

↓

Props

↓

Child
```

If the parent changes the State:

```jsx
setName("David");
```

React automatically sends the updated Prop to the child.

```
John

↓

David
```

The child updates automatically.

---

# Real-Life Example: Company Hierarchy

Imagine a company.

```
CEO

↓

Manager

↓

Team Lead

↓

Developers
```

Instructions flow downward.

If a developer wants something changed, they don't rewrite the CEO's plan directly.

Instead, they communicate upward, and the decision-maker updates the plan.

React follows a similar pattern.

---

# How Does a Child Request a Change?

A child **cannot** change the parent's State directly.

Instead, the parent passes a **function** as a Prop.

Parent

```jsx
function App() {
  const [count, setCount] =
    useState(0);

  return (
    <Counter
      count={count}
      increment={() =>
        setCount(count + 1)
      }
    />
  );
}
```

Child

```jsx
function Counter({
  count,
  increment
}) {
  return (
    <>
      <h2>{count}</h2>

      <button onClick={increment}>
        Increment
      </button>
    </>
  );
}
```

Flow

```
Parent State

↓

Props

↓

Child

↓

Button Click

↓

Calls Parent Function

↓

Parent Updates State

↓

New Props Sent

↓

UI Updates
```

Notice:

The child **requests** the change by calling a function.

The parent still owns and updates the data.

---

# Why Is This Better?

Imagine every employee in a company could edit the CEO's documents directly.

Chaos!

Different people could overwrite each other's changes.

React avoids this by giving each component a clear responsibility.

- Parent owns the data.
- Child displays the data.
- Parent updates the data.

This makes applications easier to maintain and debug.

---

# One-Way vs Two-Way Data Binding

| One-Way Data Binding (React) | Two-Way Data Binding |
|------------------------------|----------------------|
| Data flows from Parent to Child | Data flows in both directions |
| Parent controls the data | Both UI and data can update each other automatically |
| Easier to debug | Can become harder to track in large applications |
| More predictable | Can be less predictable if not managed carefully |

---

# Does React Support Two-Way Data Binding?

React does **not** use two-way data binding by default.

However, when building forms, it may seem like two-way binding because:

- The input displays the State.
- Typing triggers an event.
- The event updates the State.
- React re-renders the input with the new value.

Example:

```jsx
import { useState } from "react";

function App() {
  const [name, setName] =
    useState("");

  return (
    <input
      value={name}
      onChange={(e) =>
        setName(e.target.value)
      }
    />
  );
}
```

What actually happens?

```
State

↓

Input Displays Value

↓

User Types

↓

onChange Event

↓

setName()

↓

State Updates

↓

React Re-renders

↓

Updated Input
```

Even here, React still follows **one-way data flow** because the State remains the single source of truth.

---

# Common Interview Questions

## 1. What is One-Way Data Binding?

**Answer:**

One-Way Data Binding means data flows in only one direction—from the parent component to child components through Props. Child components receive data but cannot directly modify it.

---

## 2. Why Does React Use One-Way Data Binding?

**Answer:**

It makes applications more predictable, easier to understand, easier to debug, and simpler to maintain because data always has a clear and consistent direction.

---

## 3. Can a Child Component Change Parent Data?

**Answer:**

No.

A child cannot directly change the parent's State.

Instead, the parent passes a callback function as a Prop, and the child calls that function to request a change.

---

## 4. Is React Completely One-Way?

**Answer:**

Yes.

React's data flow is always one-way. Even when users type into form fields, updates happen through events and State changes, not through automatic two-way synchronization.

---

# Easy Memory Trick

Imagine a television broadcast.

```
TV Station

↓

Signal

↓

Television
```

The television receives the signal.

It cannot send the program back and change what the TV station is broadcasting.

React works similarly.

```
Parent

↓

Props

↓

Child
```

Data flows downward.

The child displays the data but doesn't own it.

---

# Summary

- One-Way Data Binding means data flows in one direction only.
- In React, data flows from the **Parent Component** to the **Child Component** using **Props**.
- Child components cannot directly modify Props or the parent's State.
- If a child needs to request a change, it calls a callback function passed by the parent.
- One-way data flow makes React applications predictable, maintainable, and easier to debug.
- React forms still follow one-way data flow because State is the single source of truth and UI updates happen through explicit State changes.