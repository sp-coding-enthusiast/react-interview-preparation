# 36. What is the Dependency Array in `useEffect`?

## Simple Definition

The **dependency array** is the **second argument** passed to the `useEffect` Hook.

It tells React:

> **"When should this effect run again?"**

Think of it as a **rule book** that React follows.

---

# Basic Syntax

```jsx
useEffect(() => {

  // Side effect

}, [dependencies]);
```

The second parameter:

```jsx
[dependencies]
```

is called the **dependency array**.

---

# Real-Life Analogy: Security Guard

Imagine a security guard at an office.

The guard has one rule:

```
Only allow people with an ID card.
```

If a person has an ID card,

the gate opens.

If not,

the gate stays closed.

Similarly,

React checks the dependency array.

```
Dependency Changed?

↓

Yes

↓

Run useEffect()

↓

No

↓

Do Nothing
```

---

# Why Do We Need a Dependency Array?

Imagine a Weather App.

```
City

↓

Bangalore
```

When the city changes,

you want to fetch new weather.

But if nothing changes,

there's no need to call the API again.

The dependency array tells React exactly **when** to run the effect.

---

# Without a Dependency Array

```jsx
useEffect(() => {

  console.log("Running");

});
```

This runs **after every render**.

Flow

```
Render

↓

useEffect()

↓

Render

↓

useEffect()

↓

Render

↓

useEffect()
```

Every render triggers the effect.

---

# Example

```jsx
function App() {

  const [count, setCount] =
    useState(0);

  useEffect(() => {
    console.log("Effect");
  });

  return (
    <button
      onClick={() =>
        setCount(count + 1)
      }
    >
      {count}
    </button>
  );
}
```

Output

```
Initial Render

↓

Effect

↓

Click

↓

Effect

↓

Click

↓

Effect
```

The effect runs after every render.

---

# Empty Dependency Array

```jsx
useEffect(() => {

  console.log("Mounted");

}, []);
```

The empty array means:

> "This effect doesn't depend on any changing values."

So React runs it only once after the component mounts.

Flow

```
First Render

↓

useEffect()

↓

Never Again
```

---

# Real-Life Analogy

Imagine opening your laptop.

```
Turn On Laptop

↓

Connect to Wi-Fi

↓

Done
```

You don't reconnect to Wi-Fi every second.

You do it once when the laptop starts.

An empty dependency array works the same way.

---

# One Dependency

```jsx
useEffect(() => {

  console.log("Count Changed");

}, [count]);
```

Now React watches only one value:

```
count
```

If `count` changes,

the effect runs.

Otherwise,

it doesn't.

---

# Visual Flow

```
count Changes

↓

Run useEffect()

↓

Update Complete
```

---

# Example

```jsx
import { useState, useEffect } from "react";

function App() {

  const [count, setCount] =
    useState(0);

  useEffect(() => {

    console.log("Count Updated");

  }, [count]);

  return (
    <button
      onClick={() =>
        setCount(count + 1)
      }
    >
      {count}
    </button>
  );
}
```

Output

```
0

↓

Count Updated

↓

Click

↓

Count Updated

↓

Click

↓

Count Updated
```

---

# Multiple Dependencies

Suppose you have:

```jsx
const [count, setCount] =
  useState(0);

const [user, setUser] =
  useState("Saurabh");
```

Use:

```jsx
useEffect(() => {

  console.log("Changed");

}, [count, user]);
```

The effect runs whenever:

- `count` changes
- `user` changes

---

# Visual Representation

```
count Changes

↓

Run Effect
```

OR

```
user Changes

↓

Run Effect
```

---

# Real-Life Example: Shopping Cart

Imagine an online shopping website.

The total price depends on:

- Quantity
- Discount

Whenever either changes,

the total should be recalculated.

```
Quantity

↓

OR

↓

Discount

↓

Calculate Total
```

Dependency array:

```jsx
[quantity, discount]
```

---

# Example

```jsx
useEffect(() => {

  calculateTotal();

}, [quantity, discount]);
```

React only recalculates when one of those values changes.

---

# How React Checks Dependencies

React compares the **previous value** with the **new value** using **reference equality** (similar to `Object.is`).

If the value changed,

the effect runs.

Example

```
Old Count

5
```

New Count

```
6
```

Changed?

```
Yes
```

Run the effect.

---

# Objects and Arrays

Consider:

```jsx
const user = {
  name: "Saurabh"
};
```

Every render creates a new object.

Even if the contents are the same,

the reference is different.

```
Old Object

↓

Memory A
```

```
New Object

↓

Memory B
```

React treats them as different.

The effect runs again.

---

# Example

```jsx
const user = {
  name: "John"
};

useEffect(() => {

  console.log("Effect");

}, [user]);
```

The effect may run on every render because `user` is a new object each time.

To avoid unnecessary reruns, developers often use `useMemo`, move the object outside the component (if appropriate), or depend only on the specific primitive values that matter.

---

# Missing Dependencies

Suppose you write:

```jsx
useEffect(() => {

  console.log(count);

}, []);
```

But inside the effect,

you use:

```
count
```

without including it in the dependency array.

This can cause the effect to use an **old (stale)** value.

Modern React projects typically use the **ESLint React Hooks plugin**, which warns when required dependencies are missing.

---

# Infinite Loop Example

Wrong

```jsx
useEffect(() => {

  setCount(count + 1);

}, [count]);
```

Flow

```
count Changes

↓

Effect Runs

↓

setCount()

↓

count Changes

↓

Effect Runs Again
```

This creates an infinite loop.

Always think carefully before updating a dependency inside an effect that depends on that same value.

---

# Common Dependency Patterns

## Run Once

```jsx
useEffect(() => {

}, []);
```

Use for:

- Initial API calls
- Initial setup
- Starting subscriptions

---

## Run When One Value Changes

```jsx
useEffect(() => {

}, [count]);
```

Use for:

- Updating the document title
- Fetching data for a selected item
- Reacting to a search term

---

## Run When Multiple Values Change

```jsx
useEffect(() => {

}, [count, user]);
```

Use when the effect depends on several values.

---

## Run After Every Render

```jsx
useEffect(() => {

});
```

Use sparingly.

Many effects don't need to run after every render.

---

# Dependency Array Cheat Sheet

| Dependency Array | When It Runs |
|------------------|--------------|
| No dependency array | After every render |
| `[]` | Once after the component mounts |
| `[count]` | After mounting and whenever `count` changes |
| `[count, user]` | After mounting and whenever `count` or `user` changes |

---

# Real-World Examples

## Weather App

```jsx
useEffect(() => {

  fetchWeather(city);

}, [city]);
```

New city,

new weather.

---

## Search Box

```jsx
useEffect(() => {

  searchProducts(query);

}, [query]);
```

Every time the search query changes,

search again.

---

## Shopping Cart

```jsx
useEffect(() => {

  calculateTotal();

}, [items, discount]);
```

Recalculate only when needed.

---

# Common Interview Questions

## 1. What Is the Dependency Array?

**Answer:**

The dependency array is the second argument to `useEffect`. It tells React when to re-run the effect by listing the values the effect depends on.

---

## 2. What Does an Empty Dependency Array Mean?

**Answer:**

`[]` tells React to run the effect only once after the component mounts because the effect has no changing dependencies.

---

## 3. What Happens If You Don't Provide a Dependency Array?

**Answer:**

The effect runs after every render.

---

## 4. Can We Have Multiple Dependencies?

**Answer:**

Yes.

Example:

```jsx
useEffect(() => {

}, [count, user]);
```

The effect runs whenever either dependency changes.

---

## 5. Why Should We Include All Used Values in the Dependency Array?

**Answer:**

Including all values used inside the effect helps ensure the effect always works with the latest data and avoids stale values or unexpected behavior. The React Hooks ESLint plugin helps identify missing dependencies.

---

## 6. Why Can Objects and Arrays Cause Extra Effect Runs?

**Answer:**

React compares dependencies by reference. A newly created object or array has a different reference, even if its contents are identical, so React considers it changed.

---

# Easy Memory Trick

Imagine a smoke detector.

```
Smoke?

↓

Yes

↓

Alarm
```

```
No Smoke

↓

No Alarm
```

The dependency array works the same way.

```
Dependency Changed?

↓

Yes

↓

Run useEffect()

↓

No

↓

Skip
```

---

# Summary

- The dependency array is the second parameter of `useEffect`.
- It tells React when an effect should run again.
- No dependency array means the effect runs after every render.
- An empty dependency array (`[]`) means the effect runs only once after mounting.
- Listing dependencies (e.g., `[count]`) causes the effect to run whenever those values change.
- React compares dependencies by reference, so objects and arrays may trigger additional effect executions if recreated on each render.
- Including all values used inside the effect helps keep it synchronized with the latest State and Props.