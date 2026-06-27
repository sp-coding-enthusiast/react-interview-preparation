# 31. How does `useEffect` Replace Lifecycle Methods?

## Simple Definition

`useEffect` is a **React Hook** that lets Functional Components perform **side effects** such as:

- Calling APIs
- Starting timers
- Updating the document title
- Adding event listeners
- Cleaning up resources

Before Hooks, these tasks were handled using lifecycle methods in **Class Components**.

After Hooks were introduced, **`useEffect` became the modern replacement for most lifecycle methods**.

---

# What is a Side Effect?

A **side effect** is any operation that happens **outside of rendering the UI**.

Examples:

- Fetching data from an API
- Saving data to local storage
- Starting a timer
- Listening for keyboard or mouse events
- Opening a WebSocket connection
- Updating the browser title

Rendering JSX is **not** a side effect.

---

# Real-Life Analogy: Opening a Shop

Imagine you own a shop.

When you open the shop:

```
Open Shop

↓

Turn On Lights

↓

Start Computer

↓

Open Cash Counter
```

While the shop is open:

```
Customers Arrive

↓

Update Inventory

↓

Print Bills
```

When closing the shop:

```
Turn Off Lights

↓

Lock Door

↓

Go Home
```

This is exactly how `useEffect` works.

```
Mount

↓

Update

↓

Unmount
```

---

# Before Hooks (Class Components)

React had three major lifecycle methods.

| Lifecycle Method | Purpose |
|------------------|----------|
| `componentDidMount()` | Runs after the component is first displayed |
| `componentDidUpdate()` | Runs after State or Props change |
| `componentWillUnmount()` | Runs before the component is removed |

---

# After Hooks

React replaced most lifecycle logic with a single Hook.

```
useEffect()
```

Depending on how you use it,

it can behave like all three lifecycle methods.

---

# Visual Representation

```
Class Component

↓

componentDidMount()

↓

componentDidUpdate()

↓

componentWillUnmount()
```

becomes

```
Functional Component

↓

useEffect()
```

One Hook.

Different behaviors.

---

# 1. Replacing componentDidMount()

## Class Component

```jsx
class App extends React.Component {

  componentDidMount() {
    console.log("Mounted");
  }

  render() {
    return <h1>Hello</h1>;
  }
}
```

Runs only once.

---

## Functional Component

```jsx
import { useEffect } from "react";

function App() {

  useEffect(() => {
    console.log("Mounted");
  }, []);

  return <h1>Hello</h1>;
}
```

The empty dependency array (`[]`) tells React:

```
Run only once after mounting.
```

Equivalent to:

```
componentDidMount()
```

---

# Visual Flow

```
Component Appears

↓

useEffect()

↓

Runs Once
```

---

# 2. Replacing componentDidUpdate()

Suppose you have a counter.

---

## Class Component

```jsx
componentDidUpdate() {
  console.log("Updated");
}
```

Runs after updates.

---

## Functional Component

```jsx
useEffect(() => {
  console.log("Updated");
}, [count]);
```

Whenever `count` changes,

the effect runs.

---

# Visual Flow

```
count Changes

↓

useEffect()

↓

Runs Again
```

Equivalent to:

```
componentDidUpdate()
```

---

# Example

```jsx
import { useState, useEffect } from "react";

function App() {

  const [count, setCount] =
    useState(0);

  useEffect(() => {
    console.log("Count Changed");
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

Output

```
0

↓

Click

↓

1

↓

Count Changed
```

---

# 3. Replacing componentWillUnmount()

Before Hooks,

cleanup happened here.

```jsx
componentWillUnmount() {
  clearInterval(timer);
}
```

---

## Functional Component

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

The function returned from `useEffect` is called the **cleanup function**.

It runs before the component is removed from the DOM.

Equivalent to:

```
componentWillUnmount()
```

---

# Visual Flow

```
Component Removed

↓

Cleanup Function

↓

Timer Stopped
```

---

# One Hook, Three Behaviors

## Mount Only

```jsx
useEffect(() => {

}, []);
```

Runs once.

Equivalent to:

```
componentDidMount()
```

---

## Update When Dependency Changes

```jsx
useEffect(() => {

}, [count]);
```

Runs whenever `count` changes.

Equivalent to:

```
componentDidUpdate()
```

---

## Cleanup on Unmount

```jsx
useEffect(() => {

  return () => {

  };

}, []);
```

Runs the cleanup function when the component unmounts.

Equivalent to:

```
componentWillUnmount()
```

---

# What Happens If There Is No Dependency Array?

```jsx
useEffect(() => {
  console.log("Runs");
});
```

This runs:

- After the first render
- After every re-render

Flow

```
Render

↓

useEffect()

↓

Render

↓

useEffect()

↓

Render

↓

useEffect()
```

This is **not exactly the same** as `componentDidUpdate()` because it also runs after the initial mount.

---

# Dependency Array Explained

## Empty Array

```jsx
useEffect(() => {

}, []);
```

Runs once after mounting.

---

## One Dependency

```jsx
useEffect(() => {

}, [count]);
```

Runs:

- After mounting
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

## No Dependency Array

```jsx
useEffect(() => {

});
```

Runs after every render.

---

# Real-Life Example: Weather App

When opening the app:

```
Open App

↓

Fetch Weather
```

```jsx
useEffect(() => {

  fetchWeather();

}, []);
```

---

When the city changes:

```
City Changed

↓

Fetch New Weather
```

```jsx
useEffect(() => {

  fetchWeather();

}, [city]);
```

---

When leaving the app:

```
Close App

↓

Stop Location Tracking
```

```jsx
useEffect(() => {

  startTracking();

  return () => {
    stopTracking();
  };

}, []);
```

---

# Complete Example

```jsx
import { useState, useEffect } from "react";

function App() {

  const [count, setCount] =
    useState(0);

  useEffect(() => {

    console.log("Mounted");

    return () => {
      console.log("Unmounted");
    };

  }, []);

  useEffect(() => {

    console.log("Count Changed");

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

This demonstrates:

- Mounting
- Updating
- Cleanup

using `useEffect`.

---

# Class Components vs Functional Components

| Class Component | Functional Component |
|-----------------|----------------------|
| `componentDidMount()` | `useEffect(() => {}, [])` |
| `componentDidUpdate()` | `useEffect(() => {}, [dependencies])` |
| `componentWillUnmount()` | Cleanup function returned from `useEffect()` |

---

# Why Is `useEffect` Better?

## Before Hooks

Lifecycle logic was split across different methods.

```
componentDidMount()

↓

componentDidUpdate()

↓

componentWillUnmount()
```

Related code could be scattered.

---

## With Hooks

Everything related to one feature can stay together.

```jsx
useEffect(() => {

  startTimer();

  return () => {
    stopTimer();
  };

}, []);
```

Setup and cleanup are in one place.

This makes the code easier to understand and maintain.

---

# Common Interview Questions

## 1. How Does `useEffect` Replace Lifecycle Methods?

**Answer:**

`useEffect` replaces most lifecycle methods in Functional Components.

- `useEffect(() => {}, [])` behaves like `componentDidMount()`.
- `useEffect(() => {}, [dependency])` behaves like `componentDidUpdate()` for that dependency.
- The cleanup function returned from `useEffect` behaves like `componentWillUnmount()`.

---

## 2. Why Does `useEffect(() => {}, [])` Run Only Once?

**Answer:**

The empty dependency array tells React that the effect does not depend on any changing values, so it only runs after the initial mount.

---

## 3. What Is the Cleanup Function?

**Answer:**

The cleanup function is the function returned from `useEffect`. It runs before the component unmounts and also before the effect runs again if its dependencies change.

Example:

```jsx
useEffect(() => {

  return () => {
    console.log("Cleanup");
  };

}, []);
```

---

## 4. Does `useEffect` Run Before or After Rendering?

**Answer:**

`useEffect` runs **after React has rendered the component and updated the DOM**. This ensures the UI is visible before side effects are executed.

---

## 5. Can One Component Have Multiple `useEffect` Hooks?

**Answer:**

Yes.

In fact, it is often recommended to separate unrelated side effects.

Example:

```jsx
useEffect(() => {
  fetchUsers();
}, []);

useEffect(() => {
  document.title =
    `Count: ${count}`;
}, [count]);
```

Each effect has a single responsibility.

---

# Easy Memory Trick

Imagine your daily office routine.

```
Reach Office

↓

Start Computer

(Mount)

↓

Work All Day

(Updates)

↓

Leave Office

↓

Shut Down Computer

(Unmount)
```

`useEffect` can handle all of these stages.

---

# Summary

- `useEffect` is the modern way to handle side effects in Functional Components.
- It replaces most uses of `componentDidMount()`, `componentDidUpdate()`, and `componentWillUnmount()`.
- An empty dependency array (`[]`) runs the effect only after the component mounts.
- A dependency array (e.g., `[count]`) runs the effect whenever those dependencies change.
- The cleanup function returned from `useEffect` is used to release resources when the component unmounts or before the effect runs again.
- Using multiple `useEffect` Hooks helps organize related logic and keeps components easier to read and maintain.