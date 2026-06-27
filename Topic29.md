# 40. What is `useMemo`?

## Simple Definition

`useMemo` is a **React Hook** that **remembers (memoizes) the result of an expensive calculation** so React doesn't calculate it again unless it's actually needed.

In simple words,

> **`useMemo` helps React avoid doing the same expensive work repeatedly.**

It improves performance by **reusing a previously calculated value**.

---

# Real-Life Analogy: School Mathematics

Imagine your teacher asks:

```
125 × 478
```

You calculate it.

It takes time.

Now, one minute later, the teacher asks the exact same question.

Would you calculate it again?

No.

You simply look at your notebook.

```
Already Calculated

↓

Read Answer

↓

Done
```

That notebook is exactly what `useMemo` is.

It stores the previous result.

---

# Why Do We Need `useMemo`?

Imagine you have an application that calculates:

- Total sales
- Employee salaries
- Large reports
- Thousands of products
- Complex mathematical calculations

Some calculations may take noticeable time.

Without `useMemo`,

React performs the calculation on every render.

---

# Without `useMemo`

```jsx
function App() {

  const total =
    calculateBigReport();

  return <h1>{total}</h1>;

}
```

Flow

```
Render

↓

Calculate

↓

Render Again

↓

Calculate Again

↓

Render Again

↓

Calculate Again
```

Even if nothing changed,

the calculation repeats.

---

# With `useMemo`

```jsx
const total = useMemo(() => {

  return calculateBigReport();

}, []);
```

Now

```
First Render

↓

Calculate

↓

Store Result

↓

Next Render

↓

Reuse Stored Result
```

No unnecessary calculations.

---

# Basic Syntax

```jsx
const value = useMemo(() => {

  return expensiveCalculation();

}, [dependencies]);
```

`useMemo` accepts:

1. A function that returns a value.
2. A dependency array.

React remembers the returned value.

---

# Visual Representation

```
Expensive Calculation

↓

Store Result

↓

Dependencies Changed?

↓

No

↓

Reuse Value

↓

Yes

↓

Calculate Again
```

---

# Example 1: Simple Calculation

```jsx
import { useMemo } from "react";

function App() {

  const square =
    useMemo(() => {

      return 10 * 10;

    }, []);

  return <h1>{square}</h1>;

}
```

Output

```
100
```

The calculation runs once.

---

# Example 2: Large Array

Imagine:

```jsx
const users = [
  ...
  100000 users
];
```

Finding active users:

```jsx
const activeUsers =
  users.filter(user =>
    user.active
  );
```

Without `useMemo`

```
Render

↓

Filter 100000 Users
```

Again

```
Render

↓

Filter Again
```

Every render repeats the work.

---

# Better

```jsx
const activeUsers =
  useMemo(() => {

    return users.filter(user =>
      user.active
    );

  }, [users]);
```

Now React filters again **only if `users` changes**.

---

# Real-Life Analogy: Library

Imagine reading a large book.

```
Read Book

↓

Write Notes

↓

Store Notes
```

Later,

instead of reading the whole book again,

you simply read your notes.

`useMemo` works the same way.

---

# Example 3: Shopping Cart

Imagine a shopping cart.

```
100 Products

↓

Calculate Total

↓

₹15,450
```

If the user changes the theme from Light to Dark,

should React calculate the total again?

No.

The cart didn't change.

`useMemo` avoids recalculating the total.

```jsx
const total =
  useMemo(() => {

    return calculateTotal(cart);

  }, [cart]);
```

---

# How `useMemo` Works

Suppose:

```jsx
const total =
  useMemo(() => {

    return calculate();

  }, [items]);
```

First render

```
Calculate

↓

Store Result
```

Second render

```
items Changed?

↓

No

↓

Return Stored Result
```

Third render

```
items Changed?

↓

Yes

↓

Calculate Again
```

---

# Internal Flow

```
Component Renders

↓

Check Dependencies

↓

Changed?

↓

Yes

↓

Run Function

↓

Store Result
```

Otherwise

```
Return Previous Value
```

---

# Summary

- `useMemo` remembers the result of a calculation.
- It recalculates only when one of its dependencies changes.
- It is used to optimize performance by avoiding unnecessary work.

---

# 41. When Should `useMemo` Be Used?

## Simple Answer

Use `useMemo` when:

- A calculation is **expensive**.
- The result is used during rendering.
- The calculation doesn't need to run on every render.
- Reusing the previous result improves performance.

---

# Real-Life Analogy: Tax Calculation

Imagine a company calculates employee salaries.

```
5000 Employees

↓

Calculate Salary

↓

Generate Report
```

Now the office wall color changes.

Should salaries be recalculated?

No.

The salary calculation is unrelated.

Similarly,

`useMemo` prevents unnecessary recalculations.

---

# Good Use Cases

## 1. Filtering Large Lists

```jsx
const filteredUsers =
  useMemo(() => {

    return users.filter(user =>
      user.active
    );

  }, [users]);
```

Instead of filtering thousands of users every render,

React reuses the previous result.

---

## 2. Sorting Data

```jsx
const sortedProducts =
  useMemo(() => {

    return [...products].sort(
      compareProducts
    );

  }, [products]);
```

Sorting large arrays can be expensive.

---

## 3. Complex Mathematical Calculations

```jsx
const result =
  useMemo(() => {

    return heavyCalculation();

  }, [numbers]);
```

---

## 4. Dashboard Statistics

Imagine a dashboard.

```
Sales

↓

Revenue

↓

Profit

↓

Growth
```

Each calculation may process thousands of records.

`useMemo` helps avoid recalculating everything on unrelated renders.

---

## 5. Passing Stable Values to Memoized Child Components

Suppose:

```jsx
const settings =
  useMemo(() => {

    return {
      theme,
      language
    };

  }, [theme, language]);
```

This keeps the object reference stable between renders (unless dependencies change), which can help avoid unnecessary re-renders of child components wrapped with `React.memo`.

---

# When NOT to Use `useMemo`

## Simple Calculations

Don't do this:

```jsx
const sum =
  useMemo(() => {

    return a + b;

  }, [a, b]);
```

Adding two numbers is extremely fast.

The overhead of `useMemo` may outweigh any benefit.

Simply write:

```jsx
const sum = a + b;
```

---

## Small Arrays

```jsx
const users =
  data.filter(user =>
    user.active
  );
```

If there are only a few items,

`useMemo` is usually unnecessary.

---

## Every Variable

Some beginners write:

```jsx
const age =
  useMemo(() => 25, []);
```

This provides no benefit.

---

# Good vs Bad Examples

## Good

```jsx
const total =
  useMemo(() => {

    return calculateSalary();

  }, [employees]);
```

Expensive calculation.

Good use.

---

## Bad

```jsx
const name =
  useMemo(() => {

    return "John";

  }, []);
```

Simple value.

No need.

---

# Real-Life Example: Online Store

Imagine Amazon.

Products

```
50,000
```

Filtering

```
Only Laptops
```

Sorting

```
Price Low → High
```

Without `useMemo`

```
Every Button Click

↓

Filter Again

↓

Sort Again
```

Even changing the page theme repeats the work.

With `useMemo`

```
Filter Once

↓

Store Result

↓

Reuse Until Products Change
```

Much more efficient.

---

# `useMemo` vs Normal Variable

Normal Variable

```
Render

↓

Calculate

↓

Forget
```

Next render

```
Calculate Again
```

---

`useMemo`

```
Render

↓

Calculate

↓

Store Result
```

Next render

```
Reuse Stored Result
```

---

# `useMemo` vs `useRef`

| Feature | `useMemo` | `useRef` |
|----------|-----------|----------|
| Stores a value | ✅ Yes | ✅ Yes |
| Recalculates automatically | ✅ Yes (when dependencies change) | ❌ No |
| Causes re-render | ❌ No (it only returns a memoized value during rendering) | ❌ No |
| Used for expensive calculations | ✅ Yes | ❌ No |
| Access DOM elements | ❌ No | ✅ Yes |
| Stores mutable values | ❌ Not its purpose | ✅ Yes |

---

# Common Mistakes

## Using `useMemo` Everywhere

Not every calculation needs optimization.

Measure performance first if possible.

---

## Forgetting Dependencies

Wrong

```jsx
const total =
  useMemo(() => {

    return calculate(cart);

  }, []);
```

If `cart` changes,

the total won't update because React never recalculates it.

Correct

```jsx
const total =
  useMemo(() => {

    return calculate(cart);

  }, [cart]);
```

---

## Thinking `useMemo` Is Guaranteed

`useMemo` is a **performance optimization**, not a semantic guarantee.

React may discard memoized values in some situations (for example, during development or implementation optimizations), so your code should still work correctly without relying on `useMemo` for correctness.

---

# Common Interview Questions

## 1. What is `useMemo`?

**Answer:**

`useMemo` is a React Hook that memoizes the result of a calculation. React recalculates the value only when one of the dependencies changes.

---

## 2. Why Do We Use `useMemo`?

**Answer:**

We use `useMemo` to avoid repeating expensive calculations on every render, improving performance.

---

## 3. Does `useMemo` Improve Every Application?

**Answer:**

No.

It is beneficial only when avoiding expensive recalculations outweighs the overhead of memoization.

---

## 4. Should You Use `useMemo` Everywhere?

**Answer:**

No.

Use it selectively for expensive computations or when maintaining stable object/array references provides a measurable benefit.

---

## 5. What Happens If Dependencies Don't Change?

**Answer:**

React returns the previously memoized value instead of running the calculation again.

---

# Easy Memory Trick

Imagine cooking.

```
Cook Rice

↓

Put in Fridge
```

Tomorrow,

instead of cooking again,

you reheat the rice.

That's `useMemo`.

```
Already Calculated

↓

Reuse Result

↓

Save Time
```

---

# Summary

- `useMemo` memoizes the result of an expensive calculation.
- It recalculates only when one of its dependencies changes.
- It helps optimize rendering performance by avoiding unnecessary work.
- It is useful for filtering, sorting, large computations, dashboard statistics, and providing stable values to memoized child components.
- It should not be used for trivial calculations or simple constant values.
- `useMemo` is a performance optimization, not a requirement for application correctness.