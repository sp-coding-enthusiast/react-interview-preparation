# 36. What Causes `useEffect` to Execute?

## Simple Definition

`useEffect` executes when **React decides that the effect needs to run**.

Exactly **when** it runs depends on the **dependency array**.

In simple words,

> React checks the dependency array after every render. If the required conditions are met, it executes the `useEffect` function.

---

# The Basic Rule

React follows this sequence:

```
Component Renders

↓

React Updates the DOM

↓

React Checks Dependencies

↓

If Needed

↓

useEffect Executes
```

**Important:**

`useEffect` always runs **after** React has finished rendering and updating the DOM.

---

# Real-Life Analogy: Fire Alarm

Imagine a fire alarm system.

```
Smoke Detector

↓

Checks for Smoke

↓

Smoke Found?

↓

Yes

↓

Alarm Rings
```

No smoke?

```
No Alarm
```

Similarly,

React checks the dependencies.

```
Dependencies Changed?

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

# Four Situations That Cause `useEffect` to Execute

## 1. Component Mounts (First Render)

When the component appears for the first time,

React executes effects with an empty dependency array.

```jsx
useEffect(() => {

  console.log("Mounted");

}, []);
```

Flow

```
Component Created

↓

Rendered

↓

useEffect Runs
```

This happens **only once**.

---

# Example

```jsx
function App() {

  useEffect(() => {

    console.log("Hello");

  }, []);

  return <h1>React</h1>;

}
```

Output

```
React

↓

Hello
```

---

## 2. A Dependency Changes

Suppose:

```jsx
const [count, setCount] =
  useState(0);
```

Effect

```jsx
useEffect(() => {

  console.log("Count Changed");

}, [count]);
```

Flow

```
count Changes

↓

React Re-renders

↓

useEffect Runs
```

Every time `count` changes,

the effect executes again.

---

# Example

```jsx
import { useState, useEffect } from "react";

function App() {

  const [count, setCount] =
    useState(0);

  useEffect(() => {

    console.log("Updated");

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

Updated

↓

Click

↓

Updated

↓

Click

↓

Updated
```

---

## 3. Every Render (No Dependency Array)

```jsx
useEffect(() => {

  console.log("Runs");

});
```

No dependency array means:

```
Run after every render.
```

Flow

```
Render

↓

Effect

↓

Render

↓

Effect

↓

Render

↓

Effect
```

This includes:

- Initial render
- State updates
- Parent-triggered re-renders

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

Every render executes the effect.

---

## 4. Before Cleanup (When Dependencies Change or Component Unmounts)

Consider:

```jsx
useEffect(() => {

  console.log("Start");

  return () => {

    console.log("Cleanup");

  };

}, [count]);
```

When `count` changes,

React does this:

```
Old Effect Cleanup

↓

Run New Effect
```

When the component is removed,

React performs the cleanup one final time.

---

# Visual Flow

```
count = 1

↓

Cleanup Old Effect

↓

Run New Effect
```

Later,

```
Component Removed

↓

Cleanup Runs
```

---

# Timeline Example

```jsx
useEffect(() => {

  console.log("Effect Started");

  return () => {

    console.log("Cleanup");

  };

}, [count]);
```

Imagine:

```
count = 0
```

Output

```
Effect Started
```

User clicks

```
count = 1
```

Output

```
Cleanup

↓

Effect Started
```

User clicks again

```
count = 2
```

Output

```
Cleanup

↓

Effect Started
```

Component removed

Output

```
Cleanup
```

---

# React's Decision Process

Think of React asking:

```
Did Component Mount?

↓

Yes

↓

Run Effect
```

Otherwise

```
Did Dependency Change?

↓

Yes

↓

Cleanup Previous Effect

↓

Run New Effect
```

Otherwise

```
No Changes

↓

Skip Effect
```

---

# What Does **Not** Cause `useEffect` to Execute?

The following do **not** automatically execute an effect unless they cause a render or match its dependencies:

### Changing a Normal Variable

```jsx
let count = 0;

count++;
```

React doesn't track normal variables.

No re-render.

No effect.

---

### Mutating an Object Without Updating State

```jsx
user.name = "Rahul";
```

React doesn't know anything changed.

No re-render.

No effect.

Instead,

update State correctly.

```jsx
setUser({
  ...user,
  name: "Rahul"
});
```

---

# Parent Re-render

Suppose:

```
Parent

↓

Child
```

If the parent re-renders,

the child usually re-renders as well.

If the child has:

```jsx
useEffect(() => {

});
```

(with no dependency array)

the effect runs again because the child re-rendered.

If the child has:

```jsx
useEffect(() => {

}, []);
```

it **does not** run again after that initial mount, even though the child re-rendered.

---

# Common Triggers

| Situation | Does `useEffect` Execute? |
|-----------|---------------------------|
| First render with `[]` | ✅ Yes |
| State changes (listed as dependency) | ✅ Yes |
| Props change (listed as dependency) | ✅ Yes |
| Parent causes a re-render (no dependency array) | ✅ Yes |
| Every render (no dependency array) | ✅ Yes |
| Dependency didn't change | ❌ No |
| Normal variable changes | ❌ No |
| Object mutated without State update | ❌ No |

---

# Real-World Examples

## Weather App

```jsx
useEffect(() => {

  fetchWeather(city);

}, [city]);
```

Trigger:

```
City Changes

↓

Fetch Weather
```

---

## Search Box

```jsx
useEffect(() => {

  searchProducts(query);

}, [query]);
```

Trigger:

```
Search Text Changes

↓

Search Again
```

---

## Login Page

```jsx
useEffect(() => {

  checkLogin();

}, []);
```

Trigger:

```
Page Opens

↓

Check Login
```

Runs only once after mounting.

---

# Common Interview Questions

## 1. What Causes `useEffect` to Execute?

**Answer:**

`useEffect` executes after React renders the component. Whether it runs again depends on the dependency array:

- No dependency array → After every render.
- Empty array (`[]`) → Once after mounting.
- Dependencies (e.g., `[count]`) → After mounting and whenever those dependencies change.

---

## 2. Does `useEffect` Run Before Rendering?

**Answer:**

No.

It runs **after** React has rendered the component and updated the DOM.

---

## 3. Does `setState()` Cause `useEffect` to Run?

**Answer:**

Calling a state setter causes a re-render. If the updated State is included in the dependency array (or there is no dependency array), the effect will execute after the render.

---

## 4. Does Changing a Normal Variable Trigger `useEffect`?

**Answer:**

No.

React only tracks State, Props, and re-renders—not ordinary JavaScript variables.

---

## 5. When Does the Cleanup Function Run?

**Answer:**

The cleanup function runs:

- Before the effect runs again because one of its dependencies changed.
- Before the component unmounts.

---

# Easy Memory Trick

Imagine a motion-sensor light.

```
Person Walks By

↓

Motion Detected

↓

Light Turns On
```

No movement?

```
Light Stays Off
```

`useEffect` behaves similarly.

```
Dependency Changed?

↓

Yes

↓

Run Effect

↓

No

↓

Skip
```

---

# Summary

- `useEffect` always executes **after** React renders and updates the DOM.
- It runs once after mounting when the dependency array is empty (`[]`).
- It runs again when any listed dependency changes.
- Without a dependency array, it runs after every render.
- Before re-running an effect because of dependency changes, React executes the previous cleanup function.
- Cleanup also runs when the component unmounts.
- Changes to ordinary JavaScript variables do not trigger `useEffect`; only re-renders and dependency changes do.