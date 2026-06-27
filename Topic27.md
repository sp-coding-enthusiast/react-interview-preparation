# 37. What is a Cleanup Function in React?

## Simple Definition

A **cleanup function** is a function that is **returned from `useEffect()`**.

Its job is to **clean up or remove anything that the effect created** before the component is removed or before the effect runs again.

In simple words,

> **If `useEffect` starts something, the cleanup function stops it.**

---

# Real-Life Analogy: Leaving a Hotel Room

Imagine you check into a hotel.

When you enter the room, you:

```
Turn On Lights

↓

Switch On AC

↓

Use TV
```

Before leaving the room, you should:

```
Turn Off Lights

↓

Turn Off AC

↓

Switch Off TV

↓

Lock Door
```

The cleanup function is like **checking out of the hotel properly**.

It ensures nothing is left running.

---

# Why Do We Need a Cleanup Function?

Imagine starting a timer.

```jsx
setInterval(() => {

  console.log("Hello");

}, 1000);
```

The timer keeps running forever.

Even if the component disappears,

the timer is still active.

This wastes:

- Memory
- CPU
- Battery

This problem is called a **memory leak**.

A cleanup function stops the timer.

---

# Basic Syntax

```jsx
useEffect(() => {

  // Setup

  return () => {

    // Cleanup

  };

}, []);
```

The function returned by `useEffect` is the cleanup function.

---

# Visual Flow

```
useEffect Starts

↓

Do Some Work

↓

Component Removed

↓

Cleanup Function Runs
```

---

# Example 1: Timer

Without cleanup

```jsx
useEffect(() => {

  setInterval(() => {

    console.log("Running");

  }, 1000);

}, []);
```

Problem

```
Component Closed

↓

Timer Still Running
```

Memory leak.

---

# Correct Version

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

Flow

```
Start Timer

↓

Component Removed

↓

Stop Timer
```

---

# Example 2: Window Resize Event

Imagine listening to window resize events.

```jsx
useEffect(() => {

  window.addEventListener(
    "resize",
    handleResize
  );

}, []);
```

Problem

Even after leaving the page,

the listener is still attached.

---

# Correct Version

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

Now the event listener is removed when the component is no longer needed.

---

# Example 3: WebSocket Connection

Suppose your chat application connects to a server.

```jsx
useEffect(() => {

  const socket =
    connect();

}, []);
```

Without cleanup,

the connection remains open.

Correct

```jsx
useEffect(() => {

  const socket =
    connect();

  return () => {

    socket.close();

  };

}, []);
```

Flow

```
Open Connection

↓

Chat

↓

Close Connection
```

---

# Example 4: Subscription

Imagine subscribing to notifications.

```
Subscribe

↓

Receive Updates

↓

Leave Page

↓

Unsubscribe
```

React cleanup performs the unsubscribe step.

---

# When Does the Cleanup Function Run?

There are **two situations**.

---

## 1. Before the Component Unmounts

Example

```
User Opens Page

↓

Timer Starts

↓

User Leaves Page

↓

Cleanup Runs

↓

Timer Stops
```

---

## 2. Before the Effect Runs Again

This surprises many beginners.

Consider

```jsx
useEffect(() => {

  console.log("Started");

  return () => {

    console.log("Stopped");

  };

}, [count]);
```

When `count` changes,

React does **not** immediately run the new effect.

Instead,

it performs these steps:

```
Cleanup Previous Effect

↓

Run New Effect
```

---

# Timeline Example

Initially

```
count = 0

↓

Effect Runs
```

User clicks

```
count = 1

↓

Cleanup Runs

↓

New Effect Runs
```

User clicks again

```
count = 2

↓

Cleanup Runs

↓

New Effect Runs
```

Finally

```
Component Removed

↓

Cleanup Runs
```

---

# Why Does React Clean Up First?

Imagine a music player.

Current song

```
Song A
```

User selects another song.

Should React play both songs?

No.

It should:

```
Stop Song A

↓

Play Song B
```

Cleanup prevents multiple copies of the same effect from running at the same time.

---

# Real-Life Example: Classroom

Teacher enters.

```
Projector On

↓

Teach
```

Class ends.

```
Projector Off

↓

Lights Off

↓

Lock Room
```

Next teacher arrives.

Everything starts fresh.

This is exactly how cleanup works.

---

# What Happens Without Cleanup?

Imagine this code.

```jsx
useEffect(() => {

  setInterval(() => {

    console.log("Hello");

  }, 1000);

});
```

Every render creates another timer.

```
Timer 1

↓

Timer 2

↓

Timer 3

↓

Timer 4
```

Soon,

the browser becomes slow.

---

# With Cleanup

```jsx
useEffect(() => {

  const timer =
    setInterval(() => {

      console.log("Hello");

    }, 1000);

  return () => {

    clearInterval(timer);

  };

});
```

Flow

```
Old Timer

↓

Stop

↓

Create New Timer
```

Only one timer remains active.

---

# Common Uses of Cleanup Functions

## Stop Timers

```jsx
clearInterval(timer);
```

---

## Remove Event Listeners

```jsx
window.removeEventListener(
  "resize",
  handleResize
);
```

---

## Close WebSockets

```jsx
socket.close();
```

---

## Cancel Subscriptions

```jsx
unsubscribe();
```

---

## Abort Pending Requests (when applicable)

```jsx
controller.abort();
```

This helps avoid updating state after a request is no longer needed.

---

# Things That Usually Don't Need Cleanup

Some effects perform one-time work and leave nothing running.

Example

```jsx
useEffect(() => {

  document.title =
    "Dashboard";

}, []);
```

Nothing needs to be cleaned up.

---

# Visual Representation

```
Component Mounts

↓

useEffect Runs

↓

Creates Timer

↓

Component Updates

↓

Cleanup Old Timer

↓

Create New Timer

↓

Component Unmounts

↓

Cleanup Runs Again
```

---

# Common Mistakes

## Forgetting Cleanup

Wrong

```jsx
useEffect(() => {

  setInterval(() => {

  }, 1000);

}, []);
```

The timer keeps running.

---

## Correct

```jsx
useEffect(() => {

  const timer =
    setInterval(() => {

    }, 1000);

  return () => {

    clearInterval(timer);

  };

}, []);
```

---

## Updating State in Cleanup

A cleanup function is mainly for **releasing resources**, not for starting new work.

In many cases, updating State inside cleanup is unnecessary and may cause confusing behavior.

---

# Common Interview Questions

## 1. What Is a Cleanup Function?

**Answer:**

A cleanup function is the function returned from `useEffect`. It releases resources created by the effect, such as timers, event listeners, subscriptions, or WebSocket connections.

---

## 2. When Does the Cleanup Function Run?

**Answer:**

The cleanup function runs:

- Before the component unmounts.
- Before React executes the effect again because one of its dependencies changed.

---

## 3. Why Do We Need Cleanup?

**Answer:**

Cleanup prevents memory leaks and unwanted behavior by stopping timers, removing event listeners, closing connections, and canceling subscriptions when they are no longer needed.

---

## 4. Does Every `useEffect` Need a Cleanup Function?

**Answer:**

No.

Only effects that create resources requiring cleanup (such as timers, event listeners, subscriptions, or connections) need one.

---

## 5. Can `useEffect` Return Anything Else?

**Answer:**

No.

A `useEffect` callback should either:

- Return nothing (`undefined`), or
- Return a cleanup function.

It should **not** return a Promise. If you need asynchronous work, define and call an async function inside the effect instead.

Example:

```jsx
useEffect(() => {

  async function loadUsers() {
    // Fetch data
  }

  loadUsers();

}, []);
```

---

# Easy Memory Trick

Imagine borrowing a library book.

```
Borrow Book

↓

Read Book

↓

Return Book
```

Borrowing is like the setup inside `useEffect`.

Returning the book is like the cleanup function.

Always return what you borrowed.

---

# Summary

- A cleanup function is returned from `useEffect`.
- It removes or stops anything created by the effect.
- It runs before the component unmounts and before the effect runs again when dependencies change.
- Common cleanup tasks include stopping timers, removing event listeners, closing WebSockets, canceling subscriptions, and aborting pending requests.
- Cleanup helps prevent memory leaks and keeps applications efficient.
- Not every effect requires cleanup—only those that create resources needing explicit release.