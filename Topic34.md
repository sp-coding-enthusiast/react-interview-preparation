# 47. What are Custom Hooks?

## Simple Definition

A **Custom Hook** is a **JavaScript function** that uses one or more React Hooks to **reuse stateful logic** across multiple components.

In simple words,

> **A Custom Hook lets you write React logic once and reuse it anywhere.**

Instead of copying the same code into multiple components, you move it into a Custom Hook.

---

# Real-Life Analogy: Washing Machine

Imagine you wash clothes every day.

Without a washing machine:

```
Wash Shirt

↓

Wash Pants

↓

Wash Socks

↓

Repeat Daily
```

The same work is repeated.

Now imagine buying a washing machine.

```
Put Clothes In

↓

Press Start

↓

Everything Gets Washed
```

You reuse the same machine every time.

A **Custom Hook** is like that washing machine.

Write the logic once.

Reuse it everywhere.

---

# Why Do We Need Custom Hooks?

Imagine three components.

```
Home

Profile

Dashboard
```

All of them need to:

- Fetch users
- Show loading
- Handle errors

Without a Custom Hook,

each component repeats the same code.

```jsx
useState(...)

useEffect(...)

fetch(...)
```

Again...

```
Home

↓

Profile

↓

Dashboard
```

Lots of duplicate code.

---

# With a Custom Hook

Create one Hook.

```
useUsers()

↓

Home

↓

Profile

↓

Dashboard
```

All components reuse the same logic.

---

# Basic Syntax

A Custom Hook is simply a function whose name starts with **`use`**.

```jsx
function useSomething() {

  // Hooks

}
```

React recognizes it as a Hook because of the `use` prefix.

---

# Example 1: Counter Hook

Without Custom Hook

Component A

```jsx
const [count, setCount] =
  useState(0);
```

Component B

```jsx
const [count, setCount] =
  useState(0);
```

Duplicate code.

---

Better

```jsx
function useCounter() {

  const [count, setCount] =
    useState(0);

  function increment() {

    setCount(count + 1);

  }

  return {

    count,

    increment

  };

}
```

Use it

```jsx
function App() {

  const {

    count,

    increment

  } = useCounter();

}
```

Now every component can use the same counter logic.

---

# Example 2: Window Width

Suppose many pages need the browser width.

Without Custom Hook

Every page contains:

```jsx
useEffect(() => {

  window.addEventListener(...);

}, []);
```

Repeated everywhere.

---

Create one Hook.

```jsx
function useWindowWidth() {

  const [width, setWidth] =
    useState(window.innerWidth);

  useEffect(() => {

    function handleResize() {

      setWidth(window.innerWidth);

    }

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

  return width;

}
```

Now use it.

```jsx
const width =
  useWindowWidth();
```

Simple.

Reusable.

---

# Example 3: Fetch Users

Imagine many components fetch users.

Instead of repeating:

```jsx
useEffect(() => {

  fetch(...);

}, []);
```

Create:

```jsx
function useUsers() {

  // Fetch users

}
```

Now every page simply calls:

```jsx
const users =
  useUsers();
```

---

# Visual Representation

Without Custom Hook

```
Component A

↓

Fetch

↓

Component B

↓

Fetch

↓

Component C

↓

Fetch
```

Repeated.

---

With Custom Hook

```
useUsers()

↓

Component A

↓

Component B

↓

Component C
```

One implementation.

Many users.

---

# What Can a Custom Hook Use?

A Custom Hook can use:

- `useState`
- `useEffect`
- `useContext`
- `useReducer`
- `useMemo`
- `useCallback`
- Other Custom Hooks

It can combine multiple Hooks into one reusable unit.

---

# Naming Rule

Custom Hooks should always start with:

```
use
```

Good

```jsx
useUsers()

useCounter()

useTheme()
```

Bad

```jsx
counter()

theme()
```

The `use` prefix helps React and linting tools recognize it as a Hook.

---

# Benefits of Custom Hooks

- Reuse logic
- Reduce duplicate code
- Keep components smaller
- Easier testing
- Better organization
- Easier maintenance

---

# Real-Life Example: Authentication

Instead of:

```
Login Page

↓

Dashboard

↓

Settings

↓

Profile
```

Each checking login separately,

create:

```jsx
useAuth()
```

Now every page simply uses:

```jsx
const user =
  useAuth();
```

---

# Summary

- A Custom Hook is a reusable function that contains React Hook logic.
- It starts with the word `use`.
- It helps share logic between components without duplicating code.
- Custom Hooks improve code organization and maintainability.

---

# 48. Rules of Hooks

## Simple Definition

React Hooks have a few important rules that ensure React can correctly track state and effects.

In simple words,

> **Always call Hooks in the same order and only from React components or Custom Hooks.**

Breaking these rules can lead to bugs and errors.

---

# Rule 1: Only Call Hooks at the Top Level

Always call Hooks at the top of your component.

Correct

```jsx
function App() {

  const [count, setCount] =
    useState(0);

  useEffect(() => {

  }, []);

}
```

Wrong

```jsx
if (loggedIn) {

  useState(0);

}
```

---

# Why?

React identifies each Hook by **its position (order) in the component**.

Example

First render

```jsx
useState();

useEffect();

useRef();
```

React stores:

```
1

2

3
```

Second render

```jsx
useEffect();

useRef();
```

Now the first `useState()` is missing.

React gets confused because every Hook has shifted.

---

# Visual Example

Correct

```
Render 1

↓

State

↓

Effect

↓

Ref
```

Render 2

```
State

↓

Effect

↓

Ref
```

Same order.

Good.

---

Wrong

Render 1

```
State

↓

Effect

↓

Ref
```

Render 2

```
Effect

↓

Ref
```

Different order.

React cannot correctly match Hook state.

---

# Rule 2: Don't Call Hooks Inside Loops

Wrong

```jsx
for (let i = 0; i < 5; i++) {

  useState();

}
```

The number of Hook calls could change.

Always call Hooks once at the top level.

---

# Rule 3: Don't Call Hooks Inside Conditions

Wrong

```jsx
if (isAdmin) {

  useEffect(() => {

  });

}
```

Correct

```jsx
useEffect(() => {

  if (isAdmin) {

    // Do something

  }

}, [isAdmin]);
```

The Hook is always called.

The conditional logic is inside the Hook.

---

# Rule 4: Don't Call Hooks Inside Nested Functions

Wrong

```jsx
function handleClick() {

  useState();

}
```

Hooks should be called while React is rendering the component, not later in an event handler.

---

# Rule 5: Only Call Hooks from React Components

Correct

```jsx
function App() {

  useState();

}
```

---

Wrong

```jsx
function calculate() {

  useState();

}
```

`calculate()` is a regular JavaScript function, not a React component or Custom Hook.

---

# Rule 6: Only Call Hooks from Custom Hooks

Correct

```jsx
function useCounter() {

  useState();

}
```

A Custom Hook can call other Hooks.

---

# Visual Representation

Allowed

```
React Component

↓

Hooks
```

Or

```
Custom Hook

↓

Hooks
```

Not Allowed

```
Normal Function

↓

Hooks
```

---

# Real-Life Analogy: Recipe

Imagine baking a cake.

The recipe says:

```
Step 1

↓

Step 2

↓

Step 3
```

Every time you bake,

you follow the same order.

If one day you skip Step 2,

the result may not be correct.

React Hooks work the same way.

The order must remain consistent.

---

# Common Mistakes

## Hook Inside `if`

Wrong

```jsx
if (show) {

  useEffect(() => {

  });

}
```

Correct

```jsx
useEffect(() => {

  if (show) {

    // Logic

  }

}, [show]);
```

---

## Hook Inside Event Handler

Wrong

```jsx
function save() {

  useState();

}
```

Correct

```jsx
const [count, setCount] =
  useState(0);

function save() {

  setCount(count + 1);

}
```

---

## Hook Inside Loop

Wrong

```jsx
items.forEach(() => {

  useEffect();

});
```

Hooks must not be called in loops because the number and order of Hook calls could change.

---

# Rules Cheat Sheet

| Rule | Allowed? |
|------|----------|
| Top level of component | ✅ Yes |
| Top level of Custom Hook | ✅ Yes |
| Inside `if` | ❌ No |
| Inside `for` loop | ❌ No |
| Inside `while` loop | ❌ No |
| Inside nested function | ❌ No |
| Inside event handler | ❌ No |
| Inside normal JavaScript function | ❌ No |

---

# Common Interview Questions

## 1. What Is a Custom Hook?

**Answer:**

A Custom Hook is a reusable JavaScript function that uses one or more React Hooks to share stateful logic between components. Its name must start with `use`.

---

## 2. Why Do We Use Custom Hooks?

**Answer:**

We use Custom Hooks to avoid duplicate code, improve reusability, and keep components clean and maintainable.

---

## 3. Why Must Custom Hooks Start With `use`?

**Answer:**

The `use` prefix lets React and the React Hooks ESLint plugin recognize the function as a Hook and enforce the Rules of Hooks.

---

## 4. What Are the Rules of Hooks?

**Answer:**

- Call Hooks only at the top level.
- Never call Hooks inside loops, conditions, or nested functions.
- Call Hooks only from React function components or Custom Hooks.

---

## 5. Why Can't We Call Hooks Conditionally?

**Answer:**

React relies on Hooks being called in the same order on every render. Conditional Hook calls can change that order and cause React to associate state with the wrong Hook.

---

## 6. Can One Custom Hook Use Another Custom Hook?

**Answer:**

Yes.

Custom Hooks can call other Hooks, including other Custom Hooks, as long as they follow the Rules of Hooks.

---

# Easy Memory Trick

Imagine a school assembly.

Every morning,

students stand in the same order.

```
Class 1

↓

Class 2

↓

Class 3
```

If one class suddenly disappears,

everyone's position changes,

and attendance becomes confusing.

React expects Hooks to line up the same way on every render.

Always keep them in the same order.

---

# Summary

- A **Custom Hook** is a reusable function that encapsulates React Hook logic.
- Custom Hooks always start with `use` and can call other Hooks.
- They help eliminate duplicate logic and improve code organization.
- React Hooks must always be called:
  - At the top level of a React function component or Custom Hook.
  - In the same order on every render.
- Never call Hooks inside conditions, loops, nested functions, or regular JavaScript functions.
- Following the Rules of Hooks ensures React can correctly track state and effects across renders.