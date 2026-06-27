# 34. What is `useEffect`?

## Simple Definition

`useEffect` is a **React Hook** that lets you perform **side effects** in Functional Components.

In simple words,

> `useEffect` allows React to perform work **after the component has been rendered and displayed on the screen.**

These tasks are things that happen **outside of rendering the UI**.

---

# What is a Side Effect?

A **side effect** is any operation that is **not directly responsible for displaying the UI**.

Examples include:

- Calling an API
- Starting a timer
- Updating the browser title
- Reading from Local Storage
- Saving data to Local Storage
- Adding event listeners
- Opening a WebSocket connection
- Subscribing to notifications

These tasks happen **after React finishes rendering**.

---

# Real-Life Analogy: Opening an Office

Imagine you arrive at your office.

Your workday looks like this:

```
Reach Office

↓

Switch On Lights

↓

Start Computer

↓

Open Email

↓

Begin Work
```

The office first becomes ready.

Then you perform additional tasks.

Similarly,

React first renders the UI,

then `useEffect` runs.

---

# Without `useEffect`

Imagine trying to fetch data while React is still rendering.

```
Render UI

↓

Fetch API

↓

Render Again
```

This would make rendering unpredictable.

React keeps rendering separate from side effects.

---

# With `useEffect`

```
Render UI

↓

Display Screen

↓

Run useEffect()

↓

Fetch Data

↓

Update UI
```

This is cleaner and more predictable.

---

# Basic Syntax

```jsx
import { useEffect } from "react";

function App() {

  useEffect(() => {

    console.log("Hello");

  });

  return <h1>React</h1>;

}
```

`useEffect` accepts a function.

React executes that function **after rendering**.

---

# Visual Flow

```
Component Renders

↓

Browser Updates Screen

↓

useEffect Runs
```

---

# Example 1: Display a Message

```jsx
import { useEffect } from "react";

function App() {

  useEffect(() => {

    console.log("Component Loaded");

  }, []);

  return <h1>Hello</h1>;

}
```

Output

```
Hello

↓

Component Loaded
```

The UI appears first.

Then the effect runs.

---

# Example 2: Calling an API

```jsx
useEffect(() => {

  fetch("/api/users")
    .then(res => res.json())
    .then(data =>
      console.log(data)
    );

}, []);
```

Flow

```
Component Loads

↓

Render UI

↓

Call API

↓

Receive Data

↓

Update State

↓

Re-render UI
```

This is one of the most common uses of `useEffect`.

---

# Example 3: Updating the Browser Title

```jsx
import { useEffect, useState } from "react";

function Counter() {

  const [count, setCount] =
    useState(0);

  useEffect(() => {

    document.title =
      `Count: ${count}`;

  }, [count]);

  return (
    <button
      onClick={() =>
        setCount(count + 1)
      }
    >
      {count}
    </button>
  );
}
```

Every time `count` changes,

the browser title updates.

---

# Example 4: Timer

```jsx
useEffect(() => {

  const timer =
    setInterval(() => {
      console.log("Running");
    }, 1000);

  return () => {
    clearInterval(timer);
  };

}, []);
```

When the component is removed,

the cleanup function stops the timer.

---

# Dependency Array

The second argument to `useEffect` is called the **dependency array**.

It controls **when the effect runs**.

---

## No Dependency Array

```jsx
useEffect(() => {

});
```

Runs after **every render**.

---

## Empty Dependency Array

```jsx
useEffect(() => {

}, []);
```

Runs only once after the component mounts.

---

## One Dependency

```jsx
useEffect(() => {

}, [count]);
```

Runs:

- After the initial render
- Whenever `count` changes

---

## Multiple Dependencies

```jsx
useEffect(() => {

}, [count, user]);
```

Runs whenever:

- `count` changes
- `user` changes

---

# Cleanup Function

`useEffect` can return a function.

```jsx
useEffect(() => {

  return () => {

    console.log("Cleanup");

  };

}, []);
```

The cleanup function runs:

- Before the component unmounts.
- Before the effect runs again when dependencies change.

It is commonly used to:

- Stop timers
- Remove event listeners
- Close WebSocket connections
- Cancel subscriptions

---

# Lifecycle Comparison

| Class Component | Functional Component |
|-----------------|----------------------|
| `componentDidMount()` | `useEffect(() => {}, [])` |
| `componentDidUpdate()` | `useEffect(() => {}, [dependencies])` |
| `componentWillUnmount()` | Cleanup function returned from `useEffect()` |

---

# Summary

- `useEffect` is a Hook used to perform side effects after rendering.
- It helps with API calls, timers, subscriptions, browser updates, and other non-UI tasks.
- The dependency array controls when the effect runs.
- The cleanup function prevents memory leaks by releasing resources.

---

# 35. Why Do We Need `useEffect`?

## Simple Answer

React components should focus on **describing the UI**.

Tasks like:

- Calling APIs
- Starting timers
- Accessing browser APIs
- Adding event listeners

are **not part of rendering**.

`useEffect` gives React a dedicated place to perform these tasks safely after rendering.

---

# Real-Life Analogy: Restaurant

Imagine you're running a restaurant.

Step 1

```
Customer Orders Food
```

Step 2

```
Chef Cooks Food
```

Step 3

```
Waiter Serves Food
```

Each person has a different responsibility.

Similarly,

React separates:

```
Rendering UI

↓

Running Side Effects
```

This separation makes applications easier to understand and maintain.

---

# Why Can't We Put Side Effects Directly in the Component?

Imagine this code.

```jsx
function App() {

  fetch("/api/users");

  return <h1>Hello</h1>;

}
```

Problem:

Every time the component renders,

the API is called again.

Flow

```
Render

↓

API Call

↓

State Changes

↓

Render Again

↓

API Call Again

↓

Render Again
```

This can lead to unnecessary network requests or even infinite loops.

---

# Correct Way

```jsx
useEffect(() => {

  fetch("/api/users");

}, []);
```

Now the API is called only once after the component mounts.

---

# Common Reasons to Use `useEffect`

## 1. Fetch Data from an API

```jsx
useEffect(() => {

  fetchUsers();

}, []);
```

Example:

- Load products
- Load users
- Load weather information

---

## 2. Start Timers

```jsx
useEffect(() => {

  const timer =
    setInterval(() => {
      console.log("Tick");
    }, 1000);

  return () =>
    clearInterval(timer);

}, []);
```

---

## 3. Listen to Browser Events

```jsx
useEffect(() => {

  window.addEventListener(
    "resize",
    handleResize
  );

  return () => {
    window.removeEventListener(
      "resize",
      handleResize
    );
  };

}, []);
```

---

## 4. Update the Browser Title

```jsx
useEffect(() => {

  document.title =
    "Dashboard";

}, []);
```

---

## 5. Synchronize with External Systems

Examples:

- WebSocket connections
- Analytics
- Third-party libraries
- Maps
- Charts

---

# Real-World Examples

## Weather App

```
Open App

↓

Show Loading Screen

↓

Fetch Weather

↓

Display Weather
```

---

## Chat Application

```
Open Chat

↓

Connect to Server

↓

Receive Messages

↓

Leave Chat

↓

Disconnect
```

---

## Music Player

```
Play Song

↓

Start Timer

↓

Pause Song

↓

Stop Timer
```

---

# What Happens Without `useEffect`?

Imagine a component that starts a timer.

```jsx
setInterval(() => {

}, 1000);
```

Every render creates another timer.

After several renders:

```
Timer 1

Timer 2

Timer 3

Timer 4
```

This wastes memory and causes unexpected behavior.

With `useEffect`, you create the timer once and clean it up properly.

---

# Benefits of `useEffect`

## 1. Separates Rendering from Side Effects

Keeps components focused on describing the UI.

---

## 2. Avoids Duplicate Work

The dependency array helps control exactly when an effect runs.

---

## 3. Prevents Memory Leaks

Cleanup functions stop timers, remove event listeners, and close subscriptions.

---

## 4. Keeps Code Organized

Related setup and cleanup logic stays together.

---

## 5. Replaces Lifecycle Methods

One Hook handles mounting, updating, and cleanup.

---

# Common Interview Questions

## 1. What is `useEffect`?

**Answer:**

`useEffect` is a React Hook used to perform side effects in Functional Components. It runs after React has rendered the component.

---

## 2. Why Do We Need `useEffect`?

**Answer:**

We use `useEffect` to perform tasks that should happen after rendering, such as API calls, timers, event listeners, subscriptions, and interactions with external systems.

---

## 3. Does `useEffect` Run Before or After Rendering?

**Answer:**

`useEffect` runs **after** React renders the component and updates the DOM.

---

## 4. What Is the Dependency Array?

**Answer:**

The dependency array tells React when to re-run the effect.

- `[]` → Run once after mounting.
- `[value]` → Run after mounting and whenever `value` changes.
- No array → Run after every render.

---

## 5. What Is the Cleanup Function?

**Answer:**

The cleanup function is the function returned from `useEffect`. It runs before the component unmounts and before the effect runs again when its dependencies change.

---

# Easy Memory Trick

Imagine watering a plant.

```
Plant Exists

↓

Water It

↓

Take Care of It

↓

Remove Dead Leaves

↓

Plant Continues Growing
```

The plant is like your component.

The maintenance tasks are like `useEffect`.

The cleanup is like removing the dead leaves.

---

# Summary

- `useEffect` is a Hook used for side effects in Functional Components.
- Side effects include API calls, timers, browser events, subscriptions, and updates to external systems.
- React runs `useEffect` after rendering the UI.
- The dependency array determines when an effect should execute.
- Cleanup functions help prevent memory leaks by releasing resources.
- `useEffect` replaces most lifecycle methods from Class Components while keeping related logic together in one place.