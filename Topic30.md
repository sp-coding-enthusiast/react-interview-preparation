# 42. What is `useCallback`?

## Simple Definition

`useCallback` is a **React Hook** that **remembers (memoizes) a function** so React doesn't create a new function on every render unless it's actually needed.

In simple words,

> **`useCallback` helps React reuse the same function instead of creating a new one every time the component re-renders.**

---

# Real-Life Analogy: Employee ID Card

Imagine you work in an office.

Every morning, instead of getting a brand-new ID card,

you keep using the same one.

```
Day 1

↓

Employee ID: 123
```

Next day

```
Still Employee ID: 123
```

No need to create a new card every day.

`useCallback` works the same way.

It reuses the same function whenever possible.

---

# Why Do We Need `useCallback`?

Every time a React component renders,

functions inside it are recreated.

Example

```jsx
function App() {

  function greet() {
    console.log("Hello");
  }

  return <button onClick={greet}>Click</button>;

}
```

Although the code looks the same,

React creates a **new `greet` function** on every render.

```
Render 1

↓

New Function
```

```
Render 2

↓

Another New Function
```

```
Render 3

↓

Another New Function
```

Usually this is fine.

However, it can become a performance issue when passing functions to memoized child components.

---

# Without `useCallback`

```jsx
function App() {

  const [count, setCount] =
    useState(0);

  function handleClick() {

    console.log("Clicked");

  }

  return (
    <>
      <button
        onClick={() =>
          setCount(count + 1)
        }
      >
        +
      </button>

      <Child
        onClick={handleClick}
      />
    </>
  );

}
```

Every render

```
New handleClick()

↓

Child Receives New Function

↓

Child May Re-render
```

---

# With `useCallback`

```jsx
const handleClick =
  useCallback(() => {

    console.log("Clicked");

  }, []);
```

Now

```
Render

↓

Same Function Reused

↓

Child Doesn't Receive
New Function
```

If the dependencies don't change,

React reuses the existing function.

---

# Basic Syntax

```jsx
const callback =
  useCallback(() => {

    // Function body

  }, [dependencies]);
```

The first argument is the function.

The second argument is the dependency array.

---

# Visual Representation

```
Function Created

↓

Store Function

↓

Dependencies Changed?

↓

No

↓

Reuse Function

↓

Yes

↓

Create New Function
```

---

# Example 1: Button Click

```jsx
import { useCallback } from "react";

function App() {

  const sayHello =
    useCallback(() => {

      console.log("Hello");

    }, []);

  return (
    <button
      onClick={sayHello}
    >
      Click
    </button>
  );

}
```

The same `sayHello` function is reused across renders.

---

# Example 2: Child Component

Imagine:

```
Parent

↓

Child
```

Parent passes a function.

```jsx
<Child
  onSave={handleSave}
/>
```

Without `useCallback`

```
Parent Re-renders

↓

New handleSave()

↓

Child Thinks Props Changed

↓

Child Re-renders
```

With `useCallback`

```
Parent Re-renders

↓

Same Function

↓

Child Props Unchanged

↓

Child Can Skip Re-render
```

This is especially useful when the child component is wrapped with `React.memo`.

---

# Real-Life Analogy: TV Remote

Imagine changing TV channels.

Without `useCallback`

```
Every Time

↓

Buy New Remote
```

With `useCallback`

```
Keep Same Remote

↓

Use Again
```

Much more efficient.

---

# When Should We Use `useCallback`?

Use it when:

- Passing functions to memoized child components.
- Preventing unnecessary child re-renders.
- A function is included in a dependency array of another Hook (such as `useEffect`).
- Creating the function repeatedly causes measurable performance issues.

---

# When NOT to Use `useCallback`

Don't wrap every function.

Wrong

```jsx
const greet =
  useCallback(() => {

    console.log("Hi");

  }, []);
```

If the function isn't causing performance issues,

`useCallback` adds unnecessary complexity.

---

# Summary

- `useCallback` memoizes a function.
- React recreates the function only when its dependencies change.
- It is mainly a performance optimization.
- It is commonly used with `React.memo` and dependency arrays.

---

# 43. `useMemo` vs `useCallback`

## Simple Difference

The biggest difference is:

- **`useMemo` remembers a value.**
- **`useCallback` remembers a function.**

---

# Easy Memory Trick

Imagine a bakery.

## `useMemo`

```
Bake Cake

↓

Store Cake

↓

Serve Same Cake
```

It stores the **result**.

---

## `useCallback`

```
Remember Recipe

↓

Bake Again When Needed
```

It stores the **recipe (function)**.

---

# What Does Each Hook Return?

## `useMemo`

```jsx
const total =
  useMemo(() => {

    return calculate();

  }, []);
```

Returns

```
Value
```

---

## `useCallback`

```jsx
const handleClick =
  useCallback(() => {

    console.log("Clicked");

  }, []);
```

Returns

```
Function
```

---

# Visual Representation

## `useMemo`

```
Function

↓

Execute

↓

Store Result

↓

Return Value
```

---

## `useCallback`

```
Function

↓

Store Function

↓

Return Same Function
```

---

# Example

## `useMemo`

```jsx
const total =
  useMemo(() => {

    return cart.reduce(
      (sum, item) =>
        sum + item.price,
      0
    );

  }, [cart]);
```

React stores:

```
2500
```

---

## `useCallback`

```jsx
const handleCheckout =
  useCallback(() => {

    console.log("Checkout");

  }, []);
```

React stores:

```
Function
```

---

# Internal Flow

## `useMemo`

```
Dependencies Changed?

↓

Yes

↓

Run Function

↓

Store Value
```

Otherwise

```
Return Old Value
```

---

## `useCallback`

```
Dependencies Changed?

↓

Yes

↓

Create Function

↓

Store Function
```

Otherwise

```
Return Old Function
```

---

# Relationship Between Them

Internally, you can think of `useCallback` as behaving similarly to this:

```jsx
const callback =
  useMemo(() => {

    return () => {

      console.log("Hello");

    };

  }, []);
```

The difference is that:

- `useMemo` memoizes **the returned value**.
- `useCallback` is a more convenient API for memoizing **functions**.

---

# Side-by-Side Comparison

| Feature | `useMemo` | `useCallback` |
|----------|-----------|---------------|
| Memoizes | Value | Function |
| Returns | Calculated value | Function |
| Used for | Expensive calculations | Stable function references |
| Helps avoid | Recalculating values | Recreating functions |
| Dependency array | ✅ Yes | ✅ Yes |
| Performance optimization | ✅ Yes | ✅ Yes |

---

# Real-World Examples

## Dashboard

Calculate revenue.

```jsx
const revenue =
  useMemo(() => {

    return calculateRevenue();

  }, [orders]);
```

Store:

```
Number
```

---

## Save Button

```jsx
const save =
  useCallback(() => {

    saveData();

  }, []);
```

Store:

```
Function
```

---

## Shopping App

Filter products.

```jsx
const filtered =
  useMemo(() => {

    return products.filter(
      p => p.stock > 0
    );

  }, [products]);
```

---

Handle Buy button.

```jsx
const buy =
  useCallback(id => {

    purchase(id);

  }, []);
```

---

# Common Mistakes

## Using `useCallback` for Every Function

Not every function needs memoization.

Use it when a stable function reference provides a measurable benefit.

---

## Forgetting Dependencies

Wrong

```jsx
const save =
  useCallback(() => {

    console.log(user);

  }, []);
```

If `user` changes,

the callback still uses the old value.

Correct

```jsx
const save =
  useCallback(() => {

    console.log(user);

  }, [user]);
```

---

## Confusing `useMemo` and `useCallback`

Wrong idea:

```
useMemo

↓

Stores Functions
```

Correct:

```
useMemo

↓

Stores Values
```

---

# Common Interview Questions

## 1. What is `useCallback`?

**Answer:**

`useCallback` is a React Hook that memoizes a function, returning the same function instance until one of its dependencies changes.

---

## 2. Why Do We Use `useCallback`?

**Answer:**

We use `useCallback` to avoid recreating functions on every render, especially when passing them to memoized child components or when functions are dependencies of other Hooks.

---

## 3. What Is the Difference Between `useMemo` and `useCallback`?

**Answer:**

`useMemo` memoizes the **result of a calculation (a value)**.

`useCallback` memoizes **the function itself**.

---

## 4. Does `useCallback` Improve Every Application?

**Answer:**

No.

It is a performance optimization and should be used only when recreating functions causes unnecessary work.

---

## 5. Can `useCallback` Replace `useMemo`?

**Answer:**

No.

They solve different problems.

- `useMemo` caches values.
- `useCallback` caches functions.

---

# Easy Memory Trick

Imagine cooking.

### `useMemo`

```
Cook Food

↓

Store Food

↓

Eat Later
```

You store the **finished dish**.

---

### `useCallback`

```
Write Recipe

↓

Keep Recipe

↓

Cook Again Later
```

You store the **recipe**.

---

# Summary

- `useCallback` memoizes a function and returns the same function reference until its dependencies change.
- It is mainly used to prevent unnecessary re-renders of memoized child components and to provide stable function references.
- `useMemo` memoizes a calculated **value**.
- `useCallback` memoizes a **function**.
- Both Hooks accept a dependency array and are intended as performance optimizations, not as features required for application correctness.
- A simple way to remember them is:
  - **`useMemo` → Remember the result.**
  - **`useCallback` → Remember the function.**