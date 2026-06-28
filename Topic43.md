# 64. How React Batches Updates?

## Simple Definition

**Batching** means React **groups multiple state updates together and performs a single re-render instead of multiple re-renders.**

In simple words,

> **Instead of updating the UI after every `setState()`, React collects multiple updates and processes them together.**

This improves performance by reducing unnecessary renders.

---

# Real-Life Analogy: Shopping Cart

Imagine you're shopping online.

Without batching,

every item you add immediately places a separate order.

```
Add Phone

↓

Place Order
```

```
Add Laptop

↓

Place Order
```

```
Add Headphones

↓

Place Order
```

Three separate orders.

---

With batching,

you add everything first.

```
Phone

Laptop

Headphones

↓

Checkout Once
```

Only one order is placed.

React behaves similarly.

---

# Why Does React Batch Updates?

Suppose you write:

```jsx
setName("Saurabh");

setAge(30);

setCity("Bangalore");
```

Without batching:

```
Update Name

↓

Render

↓

Update Age

↓

Render

↓

Update City

↓

Render
```

Total

```
3 Renders
```

Very inefficient.

---

With batching:

```
Update Name

Update Age

Update City

↓

One Render
```

Only one re-render occurs.

---

# Example

```jsx
function App() {

  const [count, setCount] =
    useState(0);

  const [name, setName] =
    useState("");

  function handleClick() {

    setCount(1);

    setName("React");

  }

}
```

When the button is clicked,

React batches both updates.

Flow

```
setCount()

↓

setName()

↓

One Re-render
```

---

# Visual Representation

Without Batching

```
setState()

↓

Render

↓

setState()

↓

Render

↓

setState()

↓

Render
```

---

With Batching

```
setState()

↓

setState()

↓

setState()

↓

Single Render
```

---

# Does React Update State Immediately?

No.

React schedules the updates.

That's why this code can surprise beginners:

```jsx
setCount(count + 1);

console.log(count);
```

Console

```
Old Value
```

The state update has been scheduled but the component hasn't re-rendered yet.

---

# Multiple Updates to the Same State

Example

```jsx
setCount(count + 1);

setCount(count + 1);
```

Suppose

```
count = 0
```

Both lines calculate:

```
0 + 1
```

Result

```
count = 1
```

Not

```
2
```

Why?

Both updates used the same old value.

---

# Correct Way

Use a functional update.

```jsx
setCount(previous =>

  previous + 1

);

setCount(previous =>

  previous + 1

);
```

Flow

```
0

↓

1

↓

2
```

Each update receives the latest state.

---

# Why Is Functional Update Better?

Because React processes updates in order.

Each update receives the newest value rather than the stale value captured when the event handler started.

---

# Real-Life Analogy

Imagine a bank account.

Wrong approach

```
Balance = 100

↓

Deposit 10

↓

Deposit 10
```

If both transactions mistakenly use the original balance,

you end up with:

```
110
```

Correct approach

```
100

↓

110

↓

120
```

Each transaction uses the updated balance.

---

# Summary

- React batches multiple state updates together.
- Batching reduces unnecessary re-renders.
- Functional updates should be used when the next state depends on the previous state.

---

# 65. What is Automatic Batching?

## Simple Definition

**Automatic Batching** is a feature introduced in **React 18** that automatically batches state updates from **more places than before**, resulting in fewer re-renders.

In simple words,

> **React now automatically groups multiple state updates together, even when they happen asynchronously in many common scenarios.**

---

# Before React 18

Automatic batching happened mainly inside React event handlers.

Example

```jsx
function handleClick() {

  setCount(1);

  setName("React");

}
```

Result

```
One Render
```

Good.

---

But consider

```jsx
setTimeout(() => {

  setCount(1);

  setName("React");

}, 1000);
```

Before React 18

```
setCount()

↓

Render

↓

setName()

↓

Render
```

Two renders.

---

# After React 18

The same code

```jsx
setTimeout(() => {

  setCount(1);

  setName("React");

}, 1000);
```

Now becomes

```
setCount()

↓

setName()

↓

One Render
```

React automatically batches these updates.

---

# Another Example with Promises

```jsx
fetchData().then(() => {

  setUser(user);

  setLoading(false);

});
```

### Before React 18

```
setUser()

↓

Render

↓

setLoading()

↓

Render
```

---

### React 18

```
setUser()

↓

setLoading()

↓

One Render
```

---

# Common Places Where Automatic Batching Works

React 18 automatically batches updates in many situations, including:

- React event handlers
- `setTimeout`
- `setInterval`
- Promise callbacks (`then`, `catch`, `finally`)
- `async/await`
- Many native asynchronous callbacks managed through React's scheduling

---

# Visual Comparison

Before React 18

```
Async Update

↓

Render

↓

Async Update

↓

Render
```

---

React 18

```
Async Update

↓

Async Update

↓

Single Render
```

---

# Why Was Automatic Batching Introduced?

Because applications often perform multiple related updates.

Example

Loading user data.

```
Update User

↓

Stop Spinner

↓

Show Dashboard
```

Instead of rendering after each step,

React waits and performs one render.

Benefits:

- Faster UI
- Better performance
- Less unnecessary work

---

# Can You Disable Batching?

Usually, you should let React batch updates.

In rare cases where you need the DOM to update immediately (for example, integrating with some third-party libraries), React provides APIs such as `flushSync()`.

Example

```jsx
import { flushSync } from "react-dom";

flushSync(() => {

  setCount(1);

});
```

This forces React to process that update immediately.

Use it sparingly.

---

# Side-by-Side Comparison

| Feature | React Batching | Automatic Batching |
|----------|----------------|--------------------|
| Purpose | Group multiple updates into one render | Expand batching to many more situations |
| Introduced | Earlier React versions | React 18 |
| React event handlers | ✅ Yes | ✅ Yes |
| `setTimeout` | ❌ Not automatically (before React 18) | ✅ Yes |
| Promise callbacks | ❌ Not automatically (before React 18) | ✅ Yes |
| `async/await` | ❌ Not automatically (before React 18) | ✅ Yes |
| Benefit | Fewer renders | Even fewer renders across synchronous and asynchronous updates |

---

# Real-Life Example

Imagine sending emails.

Without batching

```
Write Email

↓

Send

↓

Write Another

↓

Send
```

Lots of work.

With batching

```
Write All Emails

↓

Send Together
```

More efficient.

---

# Common Interview Questions

## 1. What Is Batching?

**Answer:**

Batching is the process of grouping multiple state updates together so React performs a single re-render instead of one render for each update.

---

## 2. Why Does React Batch Updates?

**Answer:**

To improve performance by reducing unnecessary component re-renders and DOM work.

---

## 3. What Is Automatic Batching?

**Answer:**

Automatic batching, introduced in React 18, extends batching to many asynchronous scenarios such as timers, Promise callbacks, and `async/await`, resulting in fewer renders.

---

## 4. Why Doesn't This Increment Twice?

```jsx
setCount(count + 1);

setCount(count + 1);
```

**Answer:**

Both updates use the same captured value of `count`. To increment twice, use functional updates.

---

## 5. How Do You Correctly Increment Twice?

**Answer:**

```jsx
setCount(previous =>

  previous + 1

);

setCount(previous =>

  previous + 1

);
```

Each update receives the latest state.

---

## 6. When Would You Use `flushSync()`?

**Answer:**

Only in rare situations where you need React to apply updates immediately before continuing, such as certain integrations with third-party libraries or imperative browser APIs.

---

# Easy Memory Trick

Imagine washing clothes.

Without batching

```
Wash One Shirt

↓

Dry

↓

Wash Another

↓

Dry
```

Very inefficient.

With batching

```
Collect All Clothes

↓

Wash Together

↓

Dry Together
```

React follows the same idea.

- **Batching = Group updates together.**
- **Automatic Batching = React 18 automatically does this in many more situations, including common asynchronous code.**

---

# Summary

- **Batching** groups multiple state updates into a single re-render.
- Batching improves performance by reducing unnecessary rendering work.
- When updating the same state multiple times, use **functional updates** if each update depends on the previous value.
- **Automatic Batching** was introduced in React 18.
- React 18 automatically batches updates from event handlers, timers, Promise callbacks, `async/await`, and many other asynchronous contexts.
- In rare cases where immediate updates are required, `flushSync()` can be used to opt out of batching for a specific update.