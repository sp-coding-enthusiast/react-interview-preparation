# 75. What is Memoization?

## Simple Definition

**Memoization** is an optimization technique where **the result of an expensive operation is stored (cached) so it can be reused instead of being calculated again.**

In simple words,

> **If you've already solved a problem once, remember the answer instead of solving it again.**

React uses memoization to improve performance by avoiding unnecessary work.

---

# Real-Life Analogy: Mathematics Exam

Imagine your teacher asks:

```
256 × 512
```

You calculate it.

```
131072
```

Five minutes later, the teacher asks the same question again.

Without memoization

```
Calculate Again

↓

131072
```

With memoization

```
Already Know Answer

↓

Use Previous Result
```

No extra work.

React follows the same idea.

---

# Why Do We Need Memoization?

Imagine a dashboard containing:

- 10,000 Products
- Charts
- Search
- Filters

Every time the user types:

```
R

↓

Re

↓

Rea
```

React re-renders.

Without memoization,

the expensive calculations happen every time.

```
Render

↓

Calculate

↓

Render

↓

Calculate

↓

Render

↓

Calculate
```

Very inefficient.

---

# With Memoization

```
Render

↓

Calculate Once

↓

Store Result

↓

Reuse Result
```

Much faster.

---

# Memoization in React

React provides three common memoization tools.

| Hook/API | What It Memoizes |
|-----------|------------------|
| `React.memo()` | Component |
| `useMemo()` | Value / Calculation |
| `useCallback()` | Function |

Think of them like this:

```
React.memo

↓

Remember Component

------------------

useMemo

↓

Remember Value

------------------

useCallback

↓

Remember Function
```

---

# Example

Without memoization

```jsx
const filtered =
products.filter(...);
```

Every render

```
Filter 10,000 Products
```

Again.

---

With

```jsx
const filtered =

useMemo(() => {

  return products.filter(...);

}, [products]);
```

Flow

```
Products Changed?

↓

No

↓

Reuse Previous List

↓

Skip Filtering
```

---

# Benefits

Memoization provides:

- Faster rendering
- Less CPU usage
- Better responsiveness
- Better battery life
- Smoother applications

---

# Summary

Memoization means

```
Calculate Once

↓

Remember

↓

Reuse
```

---

# 76. useMemo Real-World Example

## What is useMemo?

`useMemo()` remembers the result of an **expensive calculation**.

> **If the dependencies haven't changed, React returns the cached value instead of recalculating it.**

---

# Real-Life Analogy: Restaurant Menu

Imagine a restaurant.

A customer asks:

```
Vegetarian Menu
```

The waiter prepares it.

Another customer asks for the same menu.

Without memory

```
Create Menu Again
```

With memory

```
Already Prepared

↓

Reuse Menu
```

---

# Real-World Scenario: E-Commerce Search

Suppose Amazon has

```
100,000 Products
```

User types

```
L

↓

La

↓

Lap

↓

Lapt
```

Every keystroke filters all products.

Without `useMemo`

```jsx
const filteredProducts =

products.filter(product =>

product.name.includes(search)

);
```

Every render

```
Filter 100,000 Products
```

Again.

Even if

```
Theme Changed
```

the filtering runs again.

---

# With useMemo

```jsx
const filteredProducts = useMemo(() => {

  return products.filter(product =>

    product.name.includes(search)

  );

}, [products, search]);
```

Now React asks:

```
Products Changed?

OR

Search Changed?
```

No?

```
Reuse Previous Result
```

Yes?

```
Calculate Again
```

---

# Visual Flow

Without useMemo

```
Render

↓

Filter Products

↓

Render

↓

Filter Products

↓

Render

↓

Filter Products
```

---

With useMemo

```
Render

↓

Filter Once

↓

Cache Result

↓

Next Render

↓

Reuse Cached Result
```

---

# Other Real-World Examples

### Analytics Dashboard

```
Calculate Revenue

↓

Very Expensive
```

Use `useMemo()`.

---

### Sorting

```
Sort 50,000 Records
```

Use `useMemo()`.

---

### Data Visualization

```
Generate Chart Data
```

Use `useMemo()`.

---

### Financial Reports

```
Calculate Tax

Calculate Profit

Calculate Growth
```

Memoize the results.

---

# When NOT to Use useMemo

Don't memoize everything.

Bad

```jsx
const total =

useMemo(() =>

a + b

);
```

Simple calculations are faster than memoization.

Use `useMemo` only when the calculation is expensive or when you need a stable reference for optimization.

---

# Summary

`useMemo`

```
Expensive Calculation

↓

Cache Value

↓

Reuse Value
```

---

# 77. useCallback Real-World Example

## What is useCallback?

`useCallback()` remembers a **function**.

> **If the dependencies haven't changed, React returns the same function reference instead of creating a new one.**

---

# Real-Life Analogy: Company Phone Number

Imagine a company.

Without `useCallback`

Every day

```
New Phone Number
```

Customers become confused.

---

With `useCallback`

```
Same Phone Number

Every Day
```

Easy to contact.

React works similarly.

---

# Real-World Scenario: Product List

Suppose

```
Dashboard

↓

Product List

↓

1000 Product Cards
```

Parent component

```jsx
<Product

onDelete={() =>

deleteProduct(id)

}

/>
```

Every render creates

```
New Function
```

Each product card receives a different function reference.

Even with

```jsx
React.memo()
```

the child still re-renders.

---

# Better

```jsx
const handleDelete =

useCallback((id) => {

  deleteProduct(id);

}, []);
```

Now

```
Same Function Reference
```

Children can skip unnecessary re-renders when used with `React.memo`.

---

# Another Real-World Example

Imagine

```
Dashboard

↓

Sidebar

↓

Chart

↓

Profile
```

Updating

```
Profile Name
```

shouldn't force

```
Chart
```

to receive a new callback every time.

`useCallback()` keeps the callback stable.

---

# Visual Flow

Without useCallback

```
Render

↓

New Function

↓

Child Re-render

↓

Render

↓

New Function

↓

Child Re-render
```

---

With useCallback

```
Render

↓

Function Cached

↓

Same Function

↓

Child Skips Render
```

---

# Common Use Cases

Use `useCallback` for:

- Event handlers
- Passing callbacks to memoized child components
- Custom hooks returning functions
- Large lists
- Expensive child components

---

# When NOT to Use useCallback

Don't do this everywhere.

Bad

```jsx
const add =

useCallback(() => {

  return a + b;

}, []);
```

If the function is small and isn't passed to memoized children (or stored where a stable reference matters), `useCallback` adds unnecessary complexity.

---

# useMemo vs useCallback

| useMemo | useCallback |
|----------|-------------|
| Caches a value | Caches a function |
| Returns the calculated result | Returns the same function reference |
| Used for expensive calculations | Used for stable callback references |
| Example: Filter products | Example: `onClick` handler |

---

# React.memo + useMemo + useCallback

These three are often used together.

```
React.memo

↓

Skip Component Rendering

------------------

useMemo

↓

Skip Expensive Calculations

------------------

useCallback

↓

Keep Function Stable
```

Together they help optimize rendering in React applications.

---

# Common Interview Questions

## 1. What Is Memoization?

**Answer:**

Memoization is an optimization technique that stores the result of an expensive operation so it can be reused instead of being recalculated.

---

## 2. What Does useMemo Cache?

**Answer:**

`useMemo` caches the result of a calculation and recomputes it only when one of its dependencies changes.

---

## 3. What Does useCallback Cache?

**Answer:**

`useCallback` caches a function reference and returns the same function until one of its dependencies changes.

---

## 4. When Should You Use useMemo?

**Answer:**

Use it for expensive calculations such as filtering, sorting, transforming large datasets, or generating complex derived values.

---

## 5. When Should You Use useCallback?

**Answer:**

Use it when passing callbacks to memoized child components, returning functions from custom hooks, or when a stable function reference is important.

---

## 6. Should You Use useMemo and useCallback Everywhere?

**Answer:**

No.

Both hooks have their own overhead. They should be used only when they provide a measurable performance benefit or solve a reference stability problem.

---

# Easy Memory Trick

Imagine a school.

```
Student

↓

Homework

↓

Teacher
```

Three ways to save work.

### React.memo

```
Remember Student
```

Don't ask the student again if nothing changed.

---

### useMemo

```
Remember Homework Answer
```

Don't solve the same problem again.

---

### useCallback

```
Remember Teacher's Phone Number
```

Don't change the contact number every day.

---

# Summary

- **Memoization** means storing previously computed results so they can be reused instead of recalculated.
- React provides three primary memoization tools:
  - **`React.memo`** → Memoizes components.
  - **`useMemo`** → Memoizes expensive calculated values.
  - **`useCallback`** → Memoizes function references.
- Use **`useMemo`** for expensive operations like filtering, sorting, aggregating, or transforming large datasets.
- Use **`useCallback`** when passing callbacks to memoized child components or when a stable function reference is required.
- Avoid overusing these optimizations—measure performance first and optimize only where they provide real value.