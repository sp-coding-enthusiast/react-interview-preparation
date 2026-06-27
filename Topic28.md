# 38. What is `useRef`?

## Simple Definition

`useRef` is a **React Hook** that allows you to:

1. **Store a value that persists between renders without causing a re-render**, or
2. **Access a DOM element directly**.

In simple words,

> **`useRef` is like a private storage box that React remembers, but changing it doesn't update the screen.**

---

# Real-Life Analogy: A Locker

Imagine you have a personal locker in your office.

```
You

↓

Locker

↓

Store Notebook
```

You can:

- Put something inside
- Take it out
- Change what's inside

But changing the contents of the locker doesn't change your office.

Similarly,

`useRef` stores data,

but changing it does **not** make React re-render the component.

---

# Why Do We Need `useRef`?

Sometimes we want to store information that React should remember,

but we **don't want the UI to update**.

Examples:

- Timer IDs
- Previous values
- DOM elements
- Input references
- Scroll positions

This is where `useRef` is useful.

---

# Basic Syntax

```jsx
import { useRef } from "react";

function App() {

  const inputRef =
    useRef(null);

}
```

React returns an object like this:

```jsx
{
  current: null
}
```

The actual value is stored inside:

```jsx
ref.current
```

---

# Visual Representation

```
useRef()

↓

Returns

↓

{
  current: value
}
```

---

# Example 1: Accessing an Input Box

```jsx
import { useRef } from "react";

function App() {

  const inputRef =
    useRef(null);

  function focusInput() {

    inputRef.current.focus();

  }

  return (
    <>
      <input ref={inputRef} />

      <button
        onClick={focusInput}
      >
        Focus
      </button>
    </>
  );
}
```

Flow

```
Click Button

↓

Focus Input

↓

Cursor Appears
```

No State is required.

---

# Real-Life Analogy

Imagine pressing a TV remote.

```
Remote

↓

TV

↓

Turn On
```

The remote directly controls the TV.

Similarly,

`useRef` lets you directly access a DOM element.

---

# Example 2: Storing a Counter Without Re-rendering

```jsx
import { useRef } from "react";

function App() {

  const countRef =
    useRef(0);

  function increment() {

    countRef.current++;

    console.log(countRef.current);

  }

  return (
    <button
      onClick={increment}
    >
      Click
    </button>
  );

}
```

Output

```
1

↓

2

↓

3
```

The value changes,

but the UI doesn't re-render.

---

# Example 3: Keeping Previous Value

```jsx
import {
  useEffect,
  useRef
} from "react";

function App({ count }) {

  const previousCount =
    useRef();

  useEffect(() => {

    previousCount.current =
      count;

  }, [count]);

  return (
    <>
      Current: {count}

      Previous:
      {previousCount.current}
    </>
  );

}
```

`useRef` remembers the previous value across renders.

---

# Common Uses of `useRef`

## Access DOM Elements

```jsx
inputRef.current.focus();
```

---

## Store Timer IDs

```jsx
const timerRef =
  useRef(null);

timerRef.current =
  setInterval(...);
```

---

## Store Previous Values

```jsx
previousValue.current =
  value;
```

---

## Store Mutable Data

```jsx
cache.current =
  data;
```

---

# Does Updating `useRef` Cause a Re-render?

No.

```jsx
ref.current = 100;
```

React remembers the new value,

but the component does **not** re-render.

---

# Visual Flow

```
ref.current Changes

↓

React Stores Value

↓

No Re-render
```

---

# Summary

- `useRef` stores a value that persists between renders.
- The value is available through `ref.current`.
- Updating `ref.current` does not trigger a re-render.
- `useRef` is commonly used for DOM access, timers, previous values, and other mutable data.

---

# 39. `useRef` vs `useState`

## Simple Difference

Both `useState` and `useRef` store values.

The difference is:

- **`useState` updates the UI.**
- **`useRef` does not update the UI.**

---

# Real-Life Analogy: Notice Board vs Personal Notebook

Imagine an office.

### Notice Board

Everyone sees it.

Whenever something changes,

everyone notices.

This is like **State**.

---

### Personal Notebook

Only you use it.

Updating it doesn't affect anyone else.

This is like **Ref**.

---

# Example Using `useState`

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

1

↓

2

↓

3
```

Every update changes the UI.

---

# Example Using `useRef`

```jsx
import { useRef } from "react";

function Counter() {

  const countRef =
    useRef(0);

  function increase() {

    countRef.current++;

    console.log(countRef.current);

  }

  return (
    <button
      onClick={increase}
    >
      Click
    </button>
  );

}
```

Console

```
1

↓

2

↓

3
```

The screen doesn't update because changing a ref does not trigger a re-render.

---

# Visual Comparison

## `useState`

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

## `useRef`

```
Button Click

↓

Ref Changes

↓

No Re-render

↓

UI Stays the Same
```

---

# When to Use `useState`

Use `useState` when the value affects what the user sees.

Examples:

- Counter
- Theme
- Login status
- Form values
- Shopping cart
- Search text

---

# When to Use `useRef`

Use `useRef` when the value should persist but **doesn't need to update the UI**.

Examples:

- Input focus
- Timer IDs
- Previous values
- Scroll position
- DOM elements
- Mutable caches

---

# Example: Input Focus

```jsx
const inputRef =
  useRef(null);

inputRef.current.focus();
```

`useState` cannot directly focus an input.

This is a perfect use case for `useRef`.

---

# Example: Timer

```jsx
const timerRef =
  useRef(null);

useEffect(() => {

  timerRef.current =
    setInterval(() => {

    }, 1000);

  return () => {

    clearInterval(
      timerRef.current
    );

  };

}, []);
```

The timer ID doesn't need to be displayed on the screen, so a ref is appropriate.

---

# Side-by-Side Comparison

| Feature | `useState` | `useRef` |
|---------|------------|----------|
| Stores data | ✅ Yes | ✅ Yes |
| Persists between renders | ✅ Yes | ✅ Yes |
| Causes re-render when updated | ✅ Yes | ❌ No |
| Used for UI data | ✅ Yes | ❌ Usually No |
| Access DOM elements | ❌ No | ✅ Yes |
| Stores mutable values | Limited (via setter) | ✅ Yes |
| Has a setter function | ✅ Yes (`setState`) | ❌ No (update `ref.current` directly) |

---

# Real-World Examples

## Counter

```jsx
const [count, setCount] =
  useState(0);
```

The count is displayed on the screen.

Use **State**.

---

## Input Focus

```jsx
const inputRef =
  useRef(null);
```

You only need a reference to the input element.

Use **Ref**.

---

## Previous Search Term

```jsx
const previousSearch =
  useRef("");
```

The previous value is stored internally.

The user doesn't need to see it update immediately.

Use **Ref**.

---

## Dark Mode

```jsx
const [darkMode, setDarkMode] =
  useState(false);
```

The UI changes.

Use **State**.

---

# Common Interview Questions

## 1. What is `useRef`?

**Answer:**

`useRef` is a React Hook that stores a mutable value which persists across renders. It is commonly used for accessing DOM elements and storing values that should not trigger re-renders.

---

## 2. Does Updating `useRef` Cause a Re-render?

**Answer:**

No.

Changing `ref.current` updates the stored value, but React does not re-render the component.

---

## 3. When Should You Use `useRef` Instead of `useState`?

**Answer:**

Use `useRef` when the value needs to persist but does not affect the rendered UI, such as timer IDs, DOM references, previous values, or mutable caches.

---

## 4. Can `useRef` Store Any Type of Value?

**Answer:**

Yes.

Just like `useState`, it can store numbers, strings, arrays, objects, functions, or DOM elements.

---

## 5. Can `useRef` Replace `useState`?

**Answer:**

No.

If a value affects what is displayed on the screen, use `useState`.

If a value is only needed internally and should not trigger a re-render, use `useRef`.

---

# Easy Memory Trick

Imagine driving a car.

### Speedometer

```
Speed Changes

↓

Dashboard Updates
```

That's like **`useState`**.

The UI changes.

---

### Glove Box

```
Put Documents Inside

↓

Car Keeps Driving
```

Nothing visible changes.

That's like **`useRef`**.

It stores information without updating the UI.

---

# Summary

- `useRef` stores mutable values that persist across renders without causing re-renders.
- The stored value is accessed through `ref.current`.
- `useRef` is commonly used for DOM access, timer IDs, previous values, scroll positions, and other internal data.
- `useState` should be used for values that affect the rendered UI.
- `useRef` should be used for values that React needs to remember but that don't require updating the screen.
- A good rule of thumb is:
  - **If the user should see the change → `useState`.**
  - **If only the code needs the value → `useRef`.**