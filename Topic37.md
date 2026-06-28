# 53. What are Stale Closures?

## Simple Definition

A **stale closure** happens when a function **remembers an old value of a variable instead of the latest one**.

In simple words,

> **A stale closure means a function is using outdated data because it "captured" the values from an older render.**

This is one of the most common React interview topics because it often causes unexpected bugs.

---

# Before Understanding Stale Closures

Let's first understand **Closure**.

## What is a Closure?

A closure is a JavaScript feature where a function **remembers the variables from the place where it was created**, even after that code has finished executing.

Example:

```jsx
function createCounter() {

  let count = 0;

  return function () {

    count++;

    console.log(count);

  };

}

const counter = createCounter();

counter();
counter();
counter();
```

Output

```
1

2

3
```

The inner function remembers `count`.

That's a closure.

---

# Real-Life Analogy

Imagine taking a photo.

```
Today

↓

Take Photo
```

The photo captures everything exactly as it is.

Tomorrow,

people may move,

but the photo still shows yesterday's scene.

A closure behaves similarly.

It remembers the values that existed when it was created.

---

# How React Creates Stale Closures

Remember:

Every time a component renders,

React creates **new variables**.

Example

```jsx
function App() {

  const [count, setCount] =
    useState(0);

}
```

Render 1

```
count = 0
```

Render 2

```
count = 1
```

Render 3

```
count = 2
```

Each render has its own version of `count`.

---

# Example of a Stale Closure

```jsx
function App() {

  const [count, setCount] =
    useState(0);

  useEffect(() => {

    setInterval(() => {

      console.log(count);

    }, 1000);

  }, []);

}
```

Expected

```
0

1

2

3
```

Actual

```
0

0

0

0
```

Why?

Because the effect runs only once.

The interval callback captures:

```
count = 0
```

It never sees later renders.

This is a **stale closure**.

---

# Visual Representation

```
Render 1

↓

count = 0

↓

Interval Created

↓

Closure Stores count = 0
```

Later

```
Render 2

↓

count = 1
```

The interval still remembers:

```
count = 0
```

---

# Another Example

```jsx
function App() {

  const [name, setName] =
    useState("John");

  function greet() {

    alert(name);

  }

}
```

Each render creates a new `greet()` function that closes over that render's `name`.

If an older callback is kept alive (for example, by a timer or event listener), it may still use the older value.

---

# Common Places Where Stale Closures Happen

- `setInterval`
- `setTimeout`
- Event listeners
- WebSocket callbacks
- Promise callbacks
- Missing dependencies in `useEffect`

---

# How to Fix Stale Closures

## Solution 1: Add Dependencies

Wrong

```jsx
useEffect(() => {

  console.log(count);

}, []);
```

Correct

```jsx
useEffect(() => {

  console.log(count);

}, [count]);
```

React recreates the effect when `count` changes.

---

## Solution 2: Functional State Update

Instead of

```jsx
setCount(count + 1);
```

Use

```jsx
setCount(previous =>

  previous + 1

);
```

This always uses the latest state value.

---

## Solution 3: Use `useRef`

```jsx
const countRef =
  useRef(0);

countRef.current =
  count;
```

Now asynchronous callbacks can read:

```jsx
countRef.current
```

which always contains the latest value.

---

# Real-Life Example

Imagine a cricket scoreboard.

```
Score

↓

50
```

A commentator receives a printed sheet.

Meanwhile,

the score becomes

```
120
```

The commentator still reads

```
50
```

because the sheet is old.

That's a stale closure.

---

# Why Do Stale Closures Happen?

Because JavaScript closures remember variables from **the render in which they were created**, not automatically from future renders.

---

# Summary

- A stale closure occurs when a callback keeps using values from an older render.
- It commonly affects timers, event listeners, and asynchronous callbacks.
- It can usually be fixed by using correct dependencies, functional updates, or `useRef`.

---

# 54. How Do Hooks Work Internally?

## Simple Answer

Internally,

React stores Hooks **in the order they are called**.

This is why the **Rules of Hooks** are so important.

---

# Real-Life Analogy: Lockers

Imagine a school.

Every student gets a locker.

```
Locker 1

Locker 2

Locker 3
```

Every day,

students use the same locker.

React does something similar.

Each Hook gets a fixed "slot."

---

# Visual Representation

Component

```jsx
function App() {

  useState();

  useEffect();

  useRef();

}
```

React internally keeps something conceptually like:

```
Hook 1

↓

State
```

```
Hook 2

↓

Effect
```

```
Hook 3

↓

Ref
```

On the next render,

React expects the Hooks to appear in the **same order**.

---

# First Render

```jsx
useState();

useEffect();

useRef();
```

React stores

```
Slot 1

↓

State
```

```
Slot 2

↓

Effect
```

```
Slot 3

↓

Ref
```

---

# Second Render

React encounters

```jsx
useState();

useEffect();

useRef();
```

Again.

Everything matches.

---

# What Happens If Order Changes?

Suppose

First render

```jsx
useState();

useEffect();

useRef();
```

Second render

```jsx
useEffect();

useRef();
```

Now

React expects

```
Slot 1

↓

State
```

But finds

```
Effect
```

Everything becomes misaligned.

This is why Hooks **must always be called in the same order**.

---

# Internal Concept

React maintains a list of Hook data for each component.

Think of it like this:

```
Component

↓

Hook List

↓

State

↓

Effect

↓

Memo

↓

Ref

↓

Reducer
```

During rendering,

React walks through this list in order.

> **Note:** Internally, React uses a linked-list-like structure attached to each component (Fiber), but you can think of it as an ordered list of Hook "slots." This mental model is accurate enough for understanding interviews and day-to-day development.

---

# Example with `useState`

```jsx
const [count, setCount] =
  useState(0);
```

Internally (conceptually)

```
Slot 1

↓

0
```

When you call

```jsx
setCount(1);
```

React updates:

```
Slot 1

↓

1
```

Then schedules a re-render.

---

# Example with Two States

```jsx
const [name, setName] =
  useState("");

const [age, setAge] =
  useState(20);
```

Internally

```
Slot 1

↓

name
```

```
Slot 2

↓

age
```

React relies on the order remaining unchanged.

---

# How `useEffect` Works Internally

```jsx
useEffect(() => {

}, [count]);
```

React stores:

- The effect callback
- The cleanup function (if one is returned)
- The dependency array

On the next render,

React compares the new dependency array with the previous one.

If something changed,

it schedules the effect to run.

---

# How `useMemo` Works Internally

```jsx
useMemo(() => {

  return total;

}, [items]);
```

React stores:

- The calculated value
- The dependency array

Next render

```
Dependencies Changed?

↓

No

↓

Return Stored Value
```

Otherwise,

React recalculates and stores the new value.

---

# How `useCallback` Works Internally

```jsx
useCallback(() => {

}, [user]);
```

React stores:

- The function reference
- The dependency array

If dependencies haven't changed,

React returns the same function reference.

---

# How `useRef` Works Internally

```jsx
const ref =
  useRef();
```

React creates an object similar to:

```jsx
{

  current: value

}
```

That object stays the same between renders.

Only `current` changes.

Changing `current` does **not** trigger a re-render.

---

# How React Knows Which Component Owns the Hooks

Each rendered component has an internal data structure called a **Fiber**.

That Fiber stores information such as:

- Hook data
- State
- Effects
- Props
- Other rendering metadata

Each component has its own Hook storage, so one component's Hooks never interfere with another's.

---

# Real-Life Analogy: Hotel Rooms

Imagine a hotel.

Each room belongs to one guest.

```
Room 101

↓

Guest A
```

```
Room 102

↓

Guest B
```

Each guest has their own belongings.

Similarly,

each React component has its own Hook storage.

---

# Why Rules of Hooks Exist

Wrong

```jsx
if (loggedIn) {

  useState();

}
```

Sometimes the Hook exists.

Sometimes it doesn't.

React loses track of the Hook order.

Correct

```jsx
useState();

if (loggedIn) {

  // Logic

}
```

The Hook order stays the same.

---

# Common Interview Questions

## 1. What Is a Stale Closure?

**Answer:**

A stale closure occurs when a callback uses values captured from an older render instead of the latest state or props.

---

## 2. Why Do Stale Closures Happen?

**Answer:**

Because JavaScript closures remember the variables from the render in which they were created, and asynchronous callbacks may continue using those old values.

---

## 3. How Can You Fix a Stale Closure?

**Answer:**

Common approaches include:

- Adding the correct dependency array.
- Using functional state updates.
- Using `useRef` to access the latest value when appropriate.

---

## 4. How Do Hooks Work Internally?

**Answer:**

React stores Hook information in the order Hooks are called for each component. During every render, React walks through those Hook slots in the same order to retrieve and update state, effects, refs, and other Hook data.

---

## 5. Why Can't Hooks Be Called Conditionally?

**Answer:**

Because React depends on Hooks being called in the same order on every render. Conditional Hook calls change that order and break React's ability to match Hook state correctly.

---

## 6. How Does React Know Which `useState` Belongs to Which Component?

**Answer:**

Each component instance has its own internal Fiber that stores its Hook data. React associates Hook calls with that component's Hook list, keeping state isolated between components.

---

# Easy Memory Trick

Imagine a library.

Each book has a fixed shelf number.

```
Shelf 1

↓

Book A
```

```
Shelf 2

↓

Book B
```

If you suddenly remove Shelf 1,

every other book shifts,

and the librarian can't find anything.

React Hooks work the same way.

Their order must never change.

---

# Summary

- A **stale closure** happens when a function continues to use values from an older render instead of the latest state or props.
- Stale closures commonly occur with timers, event listeners, and asynchronous callbacks.
- They can be fixed using correct dependencies, functional updates, or `useRef`.
- Internally, React stores Hook information in a fixed order for each component.
- React identifies Hooks by **their call order**, which is why the Rules of Hooks are essential.
- Every component has its own internal Hook storage, allowing each instance to manage its own state independently.