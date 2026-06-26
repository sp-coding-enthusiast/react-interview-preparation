# 26. Explain Component Lifecycle

## Simple Definition

A **Component Lifecycle** is the **journey of a React component from the moment it is created until it is removed from the screen**.

Just like humans have different stages of life:

```
Birth

↓

Childhood

↓

Adult

↓

Old Age

↓

Death
```

A React component also goes through different stages.

```
Created

↓

Displayed

↓

Updated

↓

Removed
```

These stages are called the **Component Lifecycle**.

---

# Real-Life Analogy: A Restaurant

Imagine a customer visits a restaurant.

### Step 1

Customer enters.

```
Customer Arrives
```

---

### Step 2

Customer orders food.

```
Food Prepared
```

---

### Step 3

Customer changes order.

```
Extra Cheese Please
```

---

### Step 4

Customer leaves.

```
Table Becomes Empty
```

Every customer goes through these stages.

A React component behaves similarly.

---

# The Four Lifecycle Phases

```
Component

↓

1. Mounting

↓

2. Updating

↓

3. Unmounting

↓

4. Error Handling
```

Let's understand each one.

---

# Phase 1: Mounting

## What is Mounting?

Mounting means:

> **The component is being created and shown on the screen for the first time.**

Think of it as the component's **birth**.

---

# Real-Life Analogy

Imagine opening YouTube.

```
Open App

↓

Home Screen Appears
```

The Home Screen has just been created.

This is Mounting.

---

# React Example

```jsx
function Welcome() {
  return <h1>Hello</h1>;
}
```

When React displays

```
Hello
```

for the first time,

the component has mounted.

---

# During Mounting

React:

- Creates the component.
- Creates the Virtual DOM.
- Renders the JSX.
- Updates the browser.

```
Create Component

↓

Generate JSX

↓

Virtual DOM

↓

Real DOM

↓

Screen
```

---

# useEffect During Mounting

```jsx
import { useEffect } from "react";

function App() {

  useEffect(() => {
    console.log("Mounted");
  }, []);

  return <h1>Hello</h1>;
}
```

Output

```
Mounted
```

The empty dependency array (`[]`) means this effect runs only once after the component first appears.

---

# Real Example

Opening WhatsApp

```
Open App

↓

Contacts Loaded

↓

Messages Loaded

↓

UI Displayed
```

These tasks typically happen during mounting.

---

# Phase 2: Updating

## What is Updating?

Updating happens whenever:

- State changes.
- Props change.
- (Or, less commonly, the component is forced to re-render.)

React updates the UI to reflect the latest data.

---

# Real-Life Analogy

Imagine a digital clock.

Initially

```
10:00
```

One minute later

```
10:01
```

Again

```
10:02
```

The clock updates continuously.

---

# React Example

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

Every click updates the State.

React re-renders the component.

---

# Updating Flow

```
Button Click

↓

State Changes

↓

React Re-renders

↓

UI Updates
```

---

# useEffect During Updating

```jsx
import { useEffect, useState } from "react";

function App() {

  const [count, setCount] =
    useState(0);

  useEffect(() => {
    console.log("Count changed");
  }, [count]);

  return (
    <button
      onClick={() =>
        setCount(count + 1)
      }
    >
      Click
    </button>
  );
}
```

Whenever `count` changes,

the effect runs again.

---

# Real Example

Instagram

```
Like

↓

Likes Increase

↓

Screen Updates
```

The Like Count is updated.

---

# Phase 3: Unmounting

## What is Unmounting?

Unmounting means:

> **The component is removed from the screen.**

Think of it as the component's retirement.

---

# Real-Life Analogy

Imagine closing a browser tab.

```
Tab Open

↓

Work Done

↓

Close Tab
```

The page disappears.

This is Unmounting.

---

# React Example

Suppose a popup is visible.

```
Popup

↓

Close Button

↓

Popup Disappears
```

The popup component has unmounted.

---

# Why Is Cleanup Important?

Some components create resources such as:

- Timers
- Event listeners
- WebSocket connections
- API subscriptions

When the component is removed,

those resources should also be cleaned up.

Otherwise,

they continue running and may waste memory or cause bugs.

---

# Cleanup Example

```jsx
import { useEffect } from "react";

function Timer() {

  useEffect(() => {

    const id =
      setInterval(() => {
        console.log("Running");
      }, 1000);

    return () => {
      clearInterval(id);
    };

  }, []);

  return <h1>Timer</h1>;
}
```

The function returned from `useEffect` is called the **cleanup function**.

It runs when the component unmounts (and before the effect runs again if dependencies change).

---

# Real-Life Example

Imagine turning on a fan.

```
Fan Running
```

Before leaving the room,

you turn it off.

```
Leave Room

↓

Turn Off Fan
```

The cleanup function is like turning off the fan.

---

# Phase 4: Error Handling

React applications can also encounter errors while rendering.

To prevent the entire application from crashing,

React provides **Error Boundaries** (implemented using class components).

They catch errors in parts of the component tree and display a fallback UI.

Example:

```
Product Page

↓

Error Occurs

↓

Show Error Message
```

Instead of crashing the whole application,

users see a friendly error screen.

> **Note:** Error Boundaries currently use class components. They are not implemented with Hooks.

---

# Complete Lifecycle Diagram

```
Component Created

↓

Mounting

↓

Displayed

↓

Updating

↓

Updating

↓

Updating

↓

Unmounting

↓

Removed
```

---

# Lifecycle in Class Components

Before Hooks,

React used lifecycle methods.

| Lifecycle Phase | Class Component Method |
|-----------------|------------------------|
| Mounting | `componentDidMount()` |
| Updating | `componentDidUpdate()` |
| Unmounting | `componentWillUnmount()` |

Example

```jsx
class App extends React.Component {

  componentDidMount() {
    console.log("Mounted");
  }

  componentDidUpdate() {
    console.log("Updated");
  }

  componentWillUnmount() {
    console.log("Removed");
  }

  render() {
    return <h1>Hello</h1>;
  }
}
```

---

# Lifecycle in Functional Components

Modern React uses **Hooks**.

| Lifecycle Phase | Hook |
|-----------------|------|
| Mounting | `useEffect(() => {}, [])` |
| Updating | `useEffect(() => {}, [dependencies])` |
| Unmounting | Cleanup function returned from `useEffect` |

Example

```jsx
useEffect(() => {

  console.log("Mounted");

  return () => {
    console.log("Unmounted");
  };

}, []);
```

---

# Real-World Examples

## YouTube

```
Open Video

↓

Video Loads

↓

Watch

↓

Close Video
```

Lifecycle:

```
Mount

↓

Update

↓

Unmount
```

---

## Amazon

```
Open Product

↓

Price Loaded

↓

Quantity Changed

↓

Leave Page
```

---

## WhatsApp

```
Open Chat

↓

Messages Loaded

↓

Receive New Message

↓

Close Chat
```

---

# Common Interview Questions

## 1. What is the Component Lifecycle?

**Answer:**

The Component Lifecycle is the sequence of stages a React component goes through from creation to removal. The main phases are Mounting, Updating, and Unmounting. React also supports Error Boundaries for handling rendering errors.

---

## 2. What Is Mounting?

**Answer:**

Mounting is the phase where a component is created and inserted into the DOM for the first time.

---

## 3. What Triggers Updating?

**Answer:**

A component updates when its State changes, its Props change, or it is explicitly re-rendered.

---

## 4. What Is Unmounting?

**Answer:**

Unmounting is the phase where a component is removed from the DOM. It is the right time to clean up timers, subscriptions, and event listeners.

---

## 5. Which Hook Is Used for Lifecycle Behavior?

**Answer:**

`useEffect()`.

Depending on how it is used, it can handle:

- Mounting
- Updating
- Cleanup during unmounting

---

# Easy Memory Trick

Think of a plant.

```
Seed

↓

Plant Grows

↓

Flowers Bloom

↓

Plant Removed
```

React components follow a similar journey.

```
Created

↓

Mounted

↓

Updated

↓

Unmounted
```

---

# Summary

- A Component Lifecycle describes the different stages of a React component's existence.
- The main phases are **Mounting**, **Updating**, and **Unmounting**.
- During Mounting, the component is created and displayed for the first time.
- During Updating, changes to State or Props cause React to re-render the component.
- During Unmounting, the component is removed, and cleanup tasks should be performed.
- Modern React uses the `useEffect` Hook to handle lifecycle-related logic instead of class lifecycle methods.
- Understanding the lifecycle helps developers manage data fetching, subscriptions, timers, and resource cleanup efficiently.