# 32. What is `useState`?

## Simple Definition

`useState` is a **React Hook** that allows a **Functional Component** to store and update data (called **State**).

In simple words,

> `useState` gives a component its own **memory**.

Without `useState`, a Functional Component cannot remember values between renders.

---

# Real-Life Analogy: Whiteboard

Imagine you're teaching in a classroom.

Without a whiteboard,

everything you write disappears immediately.

```
Teacher

↓

Speaks

↓

Students Forget
```

Now imagine you have a whiteboard.

```
Teacher

↓

Writes on Whiteboard

↓

Information Stays
```

The whiteboard stores information until you erase or change it.

`useState` works exactly like that whiteboard.

It stores values that React remembers.

---

# Why Do We Need `useState`?

Suppose you want to build a Counter.

```
Count

0
```

When the user clicks:

```
+

↓

1
```

React must remember the current count.

Normal JavaScript variables cannot do this across renders.

That's why we use State.

---

# Without `useState`

```jsx
function Counter() {

  let count = 0;

  function increment() {
    count++;
    console.log(count);
  }

  return (
    <>
      <h1>{count}</h1>

      <button onClick={increment}>
        +
      </button>
    </>
  );
}
```

You might expect:

```
0

↓

1

↓

2

↓

3
```

But the UI doesn't update correctly.

Why?

Because changing a normal variable does **not** tell React to re-render the component.

---

# With `useState`

```jsx
import { useState } from "react";

function Counter() {

  const [count, setCount] =
    useState(0);

  return (
    <>
      <h1>{count}</h1>

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

React updates the UI automatically.

---

# Understanding the Syntax

```jsx
const [count, setCount] =
  useState(0);
```

Let's break it down.

---

## `useState(0)`

```
useState(0)
```

The value inside the parentheses is the **initial state**.

Here,

```
count = 0
```

Initially.

---

## Array Destructuring

```jsx
const [count, setCount] =
  useState(0);
```

React returns an array with two values.

```
[
  Current State,
  Update Function
]
```

So,

```
count

↓

Current Value
```

and

```
setCount

↓

Function to Update State
```

---

# Visual Representation

```
useState(0)

↓

Returns

↓

[
  count,
  setCount
]
```

---

# How State Changes

Initially

```
count = 0
```

Click Button

```
setCount(1)
```

React updates State.

```
count = 1
```

Click Again

```
setCount(2)
```

Now

```
count = 2
```

---

# Real-Life Example: Water Bottle

Imagine a water bottle.

Initially

```
500 ml
```

Drink some water.

```
300 ml
```

Drink again.

```
150 ml
```

The bottle remembers how much water is left.

Similarly,

State remembers the latest value.

---

# Another Example: Light Switch

Initially

```
OFF
```

Click

```
ON
```

Click Again

```
OFF
```

The switch remembers its current position.

React State behaves the same way.

---

# Multiple State Variables

A component can have many State variables.

```jsx
const [name, setName] =
  useState("");

const [age, setAge] =
  useState(25);

const [city, setCity] =
  useState("Bangalore");
```

Each State variable is independent.

---

# Example

```jsx
function User() {

  const [name, setName] =
    useState("Saurabh");

  return (
    <>
      <h1>{name}</h1>

      <button
        onClick={() =>
          setName("Rahul")
        }
      >
        Change
      </button>
    </>
  );
}
```

Output

Initially

```
Saurabh
```

After Click

```
Rahul
```

---

# State Can Store Any Type

## Number

```jsx
const [count, setCount] =
  useState(0);
```

---

## String

```jsx
const [name, setName] =
  useState("");
```

---

## Boolean

```jsx
const [isLoggedIn, setIsLoggedIn] =
  useState(false);
```

---

## Array

```jsx
const [users, setUsers] =
  useState([]);
```

---

## Object

```jsx
const [user, setUser] =
  useState({
    name: "",
    age: 0
});
```

---

# Updating State

Wrong

```jsx
count = 10;
```

React won't know about this change.

Correct

```jsx
setCount(10);
```

Always use the setter function.

---

# Why Use the Setter Function?

Because the setter:

- Updates the State.
- Tells React the State has changed.
- Triggers a re-render.
- Updates the UI.

---

# Flow

```
Button Click

↓

setCount()

↓

State Changes

↓

React Re-renders

↓

Updated UI
```

---

# Summary

- `useState` is a Hook that adds State to Functional Components.
- It stores values between renders.
- It returns the current State and a setter function.
- Always update State using the setter function.
- Updating State causes React to re-render the component.

---

# 33. How Does `useState` Work Internally?

## Simple Answer

Internally,

React stores every State value in memory and **associates it with the specific component instance**.

When State changes,

React:

1. Saves the new value.
2. Re-runs the component function.
3. Compares the new Virtual DOM with the previous one.
4. Updates only the parts of the real DOM that changed.

---

# Real-Life Analogy: Locker System

Imagine a gym.

Every customer gets a locker.

```
Customer A

↓

Locker 101
```

```
Customer B

↓

Locker 102
```

Each person's belongings stay in their assigned locker.

Similarly,

each component gets its own place where React stores its State.

---

# Step 1: Component Renders

```jsx
function Counter() {

  const [count, setCount] =
    useState(0);

  return <h1>{count}</h1>;

}
```

When React runs the component for the first time,

it sees

```jsx
useState(0)
```

React stores:

```
count = 0
```

---

# Internal Memory (Conceptual)

Think of React maintaining something like this:

```
Component

↓

State Storage

↓

0
```

This is a simplified illustration. React's real implementation is more sophisticated.

---

# Step 2: User Clicks Button

```jsx
setCount(1);
```

React stores:

```
count = 1
```

The old value is replaced.

---

# Step 3: React Re-runs the Component

React executes the component function again.

```jsx
function Counter() {

  const [count, setCount] =
    useState(0);

}
```

You might wonder:

> Why doesn't `count` become `0` again?

Because React ignores the initial value after the first render.

It already has the stored value.

Instead of returning `0`,

React returns the latest value:

```
1
```

---

# Visual Flow

First Render

```
useState(0)

↓

Store

↓

0
```

Second Render

```
setCount(1)

↓

Store

↓

1
```

Third Render

```
setCount(2)

↓

Store

↓

2
```

---

# Important Point

The initial value is used **only during the first render**.

```jsx
useState(0)
```

After that,

React always returns the latest stored State.

---

# Why Does the Component Run Again?

When State changes,

React calls the component function again.

```
Component()

↓

Creates New JSX

↓

Creates New Virtual DOM

↓

Compares with Previous Virtual DOM

↓

Updates Only the Changed Parts
```

This process is called **Reconciliation**.

---

# Example

Initially

```jsx
<h1>0</h1>
```

After clicking

```jsx
<h1>1</h1>
```

React compares:

Old

```
0
```

New

```
1
```

Only the text changes.

The `<h1>` element itself is reused.

---

# Functional State Updates

Sometimes the new value depends on the previous value.

Instead of:

```jsx
setCount(count + 1);
```

Prefer:

```jsx
setCount(prevCount =>
  prevCount + 1
);
```

Why?

Because React may batch multiple State updates together.

Using the functional form ensures each update uses the latest available State.

Example:

```jsx
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

Result:

```
+2
```

This is safer than relying on the current `count` variable in situations with multiple queued updates.

---

# Multiple `useState` Calls

```jsx
const [name, setName] =
  useState("");

const [age, setAge] =
  useState(25);

const [city, setCity] =
  useState("Bangalore");
```

React keeps these State values separate and returns them in the same order on every render.

> **Important Rule:** Never call Hooks conditionally or inside loops. Hooks must always be called in the same order so React can correctly match each stored State to the correct `useState` call.

---

# State Update Flow

```
User Clicks

↓

setState()

↓

React Stores New State

↓

Component Function Runs Again

↓

New JSX Created

↓

Virtual DOM Compared

↓

Real DOM Updated
```

---

# Common Interview Questions

## 1. What is `useState`?

**Answer:**

`useState` is a React Hook that allows Functional Components to store and update State. It returns the current State value and a setter function.

---

## 2. Why Doesn't React Use Normal Variables?

**Answer:**

Normal variables are recreated every time the component function runs, and changing them does not trigger a re-render. State persists between renders and tells React when to update the UI.

---

## 3. Does `useState` Re-render the Component?

**Answer:**

Yes.

Calling the setter function (such as `setCount`) schedules a re-render so React can display the latest State.

---

## 4. Is the Initial Value Used on Every Render?

**Answer:**

No.

The initial value passed to `useState` is used only during the first render. On later renders, React returns the current stored State.

---

## 5. Why Should We Use Functional Updates?

**Answer:**

Functional updates use the previous State value provided by React, making them the safest choice when the next State depends on the previous one or when multiple updates may be queued.

Example:

```jsx
setCount(prev => prev + 1);
```

---

# Easy Memory Trick

Imagine a notebook.

```
Page 1

↓

Write 10
```

Next day

```
Open Notebook

↓

Still 10
```

Update it

```
15
```

The notebook remembers the latest value.

`useState` is React's notebook.

It remembers values between renders.

---

# Summary

- `useState` is a Hook that adds State to Functional Components.
- It returns an array containing the current State and a setter function.
- The initial value is used only on the first render.
- Calling the setter updates the stored State and causes React to re-render the component.
- Internally, React keeps State associated with each component instance and returns it in the same Hook order on every render.
- Functional updates (`setState(prev => ...)`) are the preferred approach when the next value depends on the previous State.