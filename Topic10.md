# 15. What is State?

## Simple Definition

**State** is a component's **own data that can change over time**.

Whenever the State changes, React automatically updates the UI (User Interface).

Think of State as the **memory of a component**.

---

# Real-Life Analogy: TV Remote

Imagine you're watching TV.

You press the volume button.

```
Volume = 20

↓

Press +

↓

Volume = 21

↓

Press +

↓

Volume = 22
```

The volume changes.

The TV screen immediately shows the new volume.

Here:

- TV = React Component
- Volume = State
- Pressing the button = Updating State
- Screen update = React Re-render

---

# Another Analogy: Bank Account

Imagine your bank balance.

Initially

```
₹10,000
```

Salary credited

```
₹15,000
```

Electricity bill paid

```
₹13,500
```

Online shopping

```
₹11,000
```

The balance keeps changing.

That changing value is like **State**.

---

# Another Analogy: Mobile Battery

```
100%

↓

80%

↓

45%

↓

10%
```

Battery percentage changes continuously.

Whenever it changes, your phone immediately updates the display.

React State works exactly like that.

---

# Why Do We Need State?

Imagine a Counter App.

Without State

```jsx
function Counter() {
  let count = 0;

  return (
    <>
      <h1>{count}</h1>
      <button>Increment</button>
    </>
  );
}
```

The value never changes on the screen.

Even if you modify the variable, React doesn't know it changed.

---

# With State

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <>
      <h1>{count}</h1>

      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </>
  );
}
```

Output

```
0

↓

Click

↓

1

↓

Click

↓

2

↓

Click

↓

3
```

React automatically updates the screen.

---

# Understanding useState()

```jsx
const [count, setCount] = useState(0);
```

Looks confusing?

Let's break it down.

```
count
```

Current value.

```
setCount()
```

Function used to update the value.

```
0
```

Initial value.

---

# Visual Representation

```
useState(0)

↓

count = 0

↓

Click Button

↓

setCount(1)

↓

count = 1

↓

React Re-renders

↓

Screen Updates
```

---

# Example: Light Switch

Imagine a room light.

Initially

```
OFF
```

Click Switch

```
ON
```

Click Again

```
OFF
```

React Example

```jsx
import { useState } from "react";

function Light() {
  const [isOn, setIsOn] = useState(false);

  return (
    <>
      <h2>{isOn ? "ON" : "OFF"}</h2>

      <button
        onClick={() => setIsOn(!isOn)}
      >
        Toggle
      </button>
    </>
  );
}
```

Output

```
OFF

↓

ON

↓

OFF

↓

ON
```

---

# Example: Like Button

Initially

```
❤️ 0 Likes
```

Click

```
❤️ 1 Like
```

Click

```
❤️ 2 Likes
```

React

```jsx
import { useState } from "react";

function LikeButton() {
  const [likes, setLikes] = useState(0);

  return (
    <>
      <h2>{likes}</h2>

      <button
        onClick={() => setLikes(likes + 1)}
      >
        Like
      </button>
    </>
  );
}
```

---

# Example: Login Status

Initially

```
Logged Out
```

Click Login

```
Logged In
```

```jsx
import { useState } from "react";

function Login() {
  const [loggedIn, setLoggedIn] =
    useState(false);

  return (
    <>
      <h2>
        {loggedIn
          ? "Logged In"
          : "Logged Out"}
      </h2>

      <button
        onClick={() =>
          setLoggedIn(true)
        }
      >
        Login
      </button>
    </>
  );
}
```

---

# State Can Store Different Data Types

## String

```jsx
const [name, setName] =
  useState("John");
```

---

## Number

```jsx
const [age, setAge] =
  useState(25);
```

---

## Boolean

```jsx
const [isDarkMode, setIsDarkMode] =
  useState(false);
```

---

## Array

```jsx
const [fruits, setFruits] =
  useState(["Apple", "Banana"]);
```

---

## Object

```jsx
const [user, setUser] =
  useState({
    name: "John",
    age: 25
  });
```

---

# How State Updates the UI

Imagine a digital scoreboard.

```
Score

0

↓

Goal

↓

1

↓

Goal

↓

2
```

The display updates instantly.

React does the same.

```
State Changes

↓

React Detects Change

↓

Component Re-renders

↓

UI Updates
```

---

# State is Private

Each component has its own State.

Example

```
Counter A

0

Counter B

0
```

Click Counter A

```
Counter A

1

Counter B

0
```

They don't affect each other because each component has its own private State.

---

# Rules of State

## Always Use the Setter Function

Wrong

```jsx
count = count + 1;
```

Correct

```jsx
setCount(count + 1);
```

React only knows about changes made using the setter function.

---

## Don't Modify State Directly

Wrong

```jsx
user.name = "Rahul";
```

Correct

```jsx
setUser({
  ...user,
  name: "Rahul"
});
```

Always create a new value instead of modifying the existing one.

---

# Real-Life Examples of State

Think about these apps:

### Instagram

```
❤️ Likes

Comments

Followers
```

These numbers change.

State.

---

### WhatsApp

```
Online

Offline

Typing...
```

Changes frequently.

State.

---

### Amazon

```
Cart Items

0

↓

1

↓

2
```

Cart count is State.

---

### Weather App

```
Today

28°C

↓

Refresh

↓

31°C
```

Temperature changes.

State.

---

# Common Interview Questions

## What is State?

**Answer:**

State is a component's internal data that can change over time. When the State changes, React automatically re-renders the component and updates the UI.

---

## Why is State Needed?

Because React needs a way to track changing data and refresh the UI automatically when that data changes.

---

## Can We Update State Directly?

No.

Always use the setter function (`setState` in class components or the function returned by `useState` in functional components).

---

## Is State Private?

Yes.

Each component manages its own State unless it is explicitly shared through Props or another state management solution.

---

# Summary

- State is a component's own data.
- State can change over time.
- Changing State automatically updates the UI.
- `useState()` is the Hook used to create State in Functional Components.
- Always use the setter function to update State.
- State can store strings, numbers, arrays, objects, booleans, and more.
- Every component has its own private State.

---

# 16. Props vs State

## Introduction

Props and State are two of the most important concepts in React.

Many beginners confuse them because **both hold data**.

The key difference is **who owns the data and who can change it**.

---

# Simple Analogy: School Report Card

Imagine your school report card.

Your teacher gives you the marks.

```
Math = 90

Science = 85

English = 88
```

You can read the marks.

You cannot change them yourself.

These are like **Props**.

Now imagine your notebook.

You can write notes.

Erase them.

Add more notes.

Those notes are like **State** because you control and update them.

---

# Another Analogy: Restaurant

Customer places an order.

```
Pizza

Large

Extra Cheese
```

Chef receives the order.

The chef doesn't change the customer's order.

This is like **Props**.

Now the chef tracks the cooking progress.

```
Preparing

↓

Cooking

↓

Ready

↓

Served
```

This changing status is like **State**.

---

# Visual Representation

```
Parent Component

↓

Props

↓

Child Component

↓

Uses Data
```

State lives inside the component itself.

```
Component

↓

State

↓

Changes

↓

UI Updates
```

---

# Example Using Props

Parent Component

```jsx
function App() {
  return (
    <Student name="Rahul" />
  );
}
```

Child Component

```jsx
function Student({ name }) {
  return <h2>{name}</h2>;
}
```

Output

```
Rahul
```

The child simply displays the value received.

---

# Example Using State

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] =
    useState(0);

  return (
    <>
      <h2>{count}</h2>

      <button
        onClick={() =>
          setCount(count + 1)
        }
      >
        +
      </button>
    </>
  );
}
```

Output

```
0

↓

1

↓

2

↓

3
```

The component updates its own data.

---

# Combining Props and State

A parent passes the starting value as a Prop.

```jsx
function App() {
  return <Counter initialValue={10} />;
}
```

The child uses that Prop to initialize its own State.

```jsx
import { useState } from "react";

function Counter({ initialValue }) {
  const [count, setCount] =
    useState(initialValue);

  return (
    <>
      <h2>{count}</h2>

      <button
        onClick={() =>
          setCount(count + 1)
        }
      >
        Increment
      </button>
    </>
  );
}
```

Flow:

```
Parent

↓

initialValue = 10 (Prop)

↓

Child

↓

State = 10

↓

Button Click

↓

State = 11

↓

UI Updates
```

The Prop remains unchanged, while the State changes.

---

# Detailed Comparison

| Feature | Props | State |
|----------|--------|--------|
| Full Form | Properties | State |
| Owner | Parent Component | Current Component |
| Purpose | Pass data to child | Store changing data |
| Can it change? | No (Read-only) | Yes |
| Who updates it? | Parent Component | Component itself |
| Triggers UI update? | Yes, when the parent passes new Props | Yes, whenever State changes |
| Mutable? | ❌ No | ✅ Yes (through setter functions) |
| Shared with other components? | Yes | No (unless passed as Props or managed globally) |
| Example | Product Name, Price | Counter Value, Likes, Login Status |

---

# Easy Way to Remember

## Props

Think:

> "Someone else gives me this data."

```
Parent

↓

Props

↓

Child
```

---

## State

Think:

> "This data belongs to me."

```
Component

↓

State

↓

Component Updates Itself
```

---

# Real-World Examples

## Props

- User Name passed from Parent
- Product Price
- Profile Picture URL
- Theme Color
- Company Logo

These values are usually provided by another component.

---

## State

- Shopping Cart Count
- Login Status
- Search Box Text
- Dark Mode Toggle
- Like Count
- Timer Value
- Form Input Values

These values change while the user interacts with the application.

---

# Interview Questions

## 1. What is the difference between Props and State?

**Answer:**

Props are read-only values passed from a parent component to a child component. State is internal data owned by a component that can change over time using setter functions.

---

## 2. Can Props Change?

Yes, but **only when the parent component passes new values**.

The child component cannot modify its Props.

---

## 3. Can State Be Passed to Another Component?

Yes.

A component can pass its State to a child as Props.

Example:

```
Parent State

↓

Props

↓

Child
```

---

## 4. Which Should Be Used for Dynamic Data?

State.

Dynamic values like counters, form inputs, login status, and likes belong in State because they change over time.

---

# Quick Memory Trick

Imagine you're borrowing a book from the library.

📚 **Props**

The library gives you the book.

You can read it.

You cannot rewrite the library's copy.

📒 **State**

Your personal notebook.

You can write, erase, and update it whenever you want.

---

# Summary

- Props are used to pass data from a parent component to a child component.
- Props are read-only and cannot be modified by the child.
- State is owned and managed by the component itself.
- State can change over time and automatically updates the UI.
- Props help components communicate with each other.
- State helps components remember and manage changing information.
- Modern React applications use **Props** and **State** together to build dynamic and interactive user interfaces.