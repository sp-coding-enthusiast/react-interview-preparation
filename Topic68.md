# 96. What is Automatic Batching?

## Simple Definition

**Automatic Batching** is a React 18 feature where **React groups multiple state updates together and performs only one re-render instead of multiple re-renders.**

In simple words,

> **Instead of repainting the UI after every state update, React waits for all related updates to finish and then updates the UI only once.**

This makes React applications **faster and more efficient**.

---

# Why Do We Need Automatic Batching?

Imagine you're painting a room.

Without batching

```
Paint One Wall

↓

Clean Brushes

↓

Paint Second Wall

↓

Clean Brushes

↓

Paint Third Wall

↓

Clean Brushes
```

Lots of unnecessary work.

---

With batching

```
Paint All Walls

↓

Clean Brushes Once
```

Much more efficient.

React follows the same idea.

---

# Real-Life Analogy

Imagine ordering food.

Without batching

```
Order Burger

↓

Kitchen Starts Cooking

↓

Order Fries

↓

Kitchen Starts Again

↓

Order Juice

↓

Kitchen Starts Again
```

Three separate cooking sessions.

---

With batching

```
Burger

+

Fries

+

Juice

↓

Kitchen Prepares Everything Together
```

One efficient process.

---

# Before React 18

Suppose you have:

```jsx
function App() {

  const [count, setCount] = useState(0);

  const [name, setName] = useState("");

  function handleClick() {

    setCount(c => c + 1);

    setName("John");

  }

}
```

### React batches updates inside React event handlers.

```
Button Click

↓

setCount()

↓

setName()

↓

One Re-render
```

This was already supported.

---

### But outside React event handlers...

Example

```jsx
setTimeout(() => {

  setCount(c => c + 1);

  setName("John");

}, 1000);
```

In **React 17 and earlier**:

```
setCount()

↓

Render

↓

setName()

↓

Render Again
```

Two renders.

---

# React 18 Automatic Batching

In React 18:

```jsx
setTimeout(() => {

  setCount(c => c + 1);

  setName("John");

}, 1000);
```

Now React batches them.

```
setCount()

↓

setName()

↓

One Render
```

Much more efficient.

---

# Where Automatic Batching Works

React 18 automatically batches updates from many sources, including:

- React event handlers
- `setTimeout()`
- `setInterval()`
- Promises
- `async/await`
- Native browser event listeners
- Fetch callbacks

---

# Example with Promise

```jsx
fetch("/api/users")

.then(() => {

  setLoading(false);

  setUsers(data);

});
```

React 17

```
setLoading()

↓

Render

↓

setUsers()

↓

Render
```

---

React 18

```
setLoading()

↓

setUsers()

↓

One Render
```

---

# Example with async/await

```jsx
async function loadUsers() {

  const users = await fetchUsers();

  setUsers(users);

  setLoading(false);

}
```

React batches both updates.

---

# What Happens Internally?

Without batching

```
State Update

↓

Render

↓

State Update

↓

Render

↓

State Update

↓

Render
```

Three renders.

---

With Automatic Batching

```
State Update

↓

State Update

↓

State Update

↓

One Render
```

Only one render.

---

# Performance Benefits

Automatic batching provides:

- Fewer renders
- Less DOM work
- Faster UI updates
- Better CPU usage
- Improved responsiveness

---

# Does React Delay the Updates?

Not in a way users notice.

React waits until the current task finishes,

then processes all batched updates together.

The UI still feels immediate.

---

# Can Automatic Batching Be Disabled?

Sometimes you need the UI to update immediately.

React provides:

```jsx
import { flushSync } from "react-dom";
```

Example

```jsx
flushSync(() => {

  setCount(c => c + 1);

});
```

`flushSync()` forces React to process the update immediately instead of waiting for batching.

Use it **sparingly**, because it reduces batching benefits.

---

# Visual Timeline

Without Automatic Batching

```
Click

↓

setCount()

↓

Render

↓

setName()

↓

Render

↓

setTheme()

↓

Render
```

Three renders.

---

With Automatic Batching

```
Click

↓

setCount()

↓

setName()

↓

setTheme()

↓

One Render
```

One render.

---

# Automatic Batching vs Manual Batching

Before React 18, developers sometimes needed manual batching for updates outside React event handlers.

React 18 performs this batching automatically in most common scenarios.

---

# Real-World Example

Imagine an e-commerce website.

User clicks

```
Checkout
```

Application updates:

- Cart
- Order Status
- Loading State
- Payment Status

Without batching

```
Cart

↓

Render

↓

Loading

↓

Render

↓

Payment

↓

Render
```

Four renders.

---

With batching

```
Cart

↓

Loading

↓

Payment

↓

Order

↓

One Render
```

Much more efficient.

---

# Common Interview Questions

## 1. What Is Automatic Batching?

**Answer:**

Automatic Batching is a React 18 feature that groups multiple state updates into a single render, reducing unnecessary re-renders and improving performance.

---

## 2. Why Was Automatic Batching Introduced?

**Answer:**

To improve application performance by reducing the number of renders when multiple state updates occur close together.

---

## 3. What Changed in React 18?

**Answer:**

Before React 18, batching mainly occurred inside React event handlers.

React 18 extends batching to asynchronous contexts such as:

- Promises
- `setTimeout`
- `async/await`
- Native event listeners

---

## 4. Does Automatic Batching Change State Values?

**Answer:**

No.

It changes **when React renders**, not **what state values become**.

All state updates are still applied correctly.

---

## 5. How Can You Force an Immediate Render?

**Answer:**

Use:

```jsx
flushSync(() => {

  setState(...);

});
```

This forces React to process the update immediately.

Use it only when necessary.

---

## 6. Does Automatic Batching Make React Faster?

**Answer:**

Yes.

By reducing unnecessary renders and DOM updates, React performs less work, leading to better overall performance.

---

# Senior Interview Tip ⭐

If you're asked:

> **"What is Automatic Batching in React 18?"**

A strong answer is:

> "Automatic Batching is a React 18 optimization where multiple state updates are grouped into a single render. Earlier versions mainly batched updates inside React event handlers, but React 18 extends batching to asynchronous operations like promises, `setTimeout`, `async/await`, and native event listeners. This reduces unnecessary renders and improves performance."

This demonstrates both historical context and current behavior.

---

# Easy Memory Trick

Imagine sending packages.

Without batching

```
Package 1

↓

Truck Leaves

----------------

Package 2

↓

Truck Leaves

----------------

Package 3

↓

Truck Leaves
```

Three trips.

---

With batching

```
Package 1

+

Package 2

+

Package 3

↓

One Truck

↓

One Delivery
```

React behaves the same way.

- **State Updates = Packages**
- **Render = Truck Delivery**
- **Automatic Batching = One Delivery Instead of Many**

---

# Summary

- **Automatic Batching** is a React 18 feature that combines multiple state updates into a **single render**.
- It reduces unnecessary rendering work and improves application performance.
- React 18 automatically batches updates from:
  - React event handlers
  - `setTimeout()`
  - `setInterval()`
  - Promises
  - `async/await`
  - Native browser events
- Automatic Batching changes **when React renders**, not the resulting state values.
- Use **`flushSync()`** only when an immediate render is required, as it bypasses batching and should be used sparingly.
- Automatic Batching is one of the key improvements built on React's **concurrent rendering** architecture.