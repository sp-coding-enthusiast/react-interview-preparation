# 49. What is `useLayoutEffect`?

## Simple Definition

`useLayoutEffect` is a **React Hook** that runs **after React updates the DOM but before the browser paints the screen**.

In simple words,

> **`useLayoutEffect` lets you read or change the DOM before the user sees it.**

This prevents the user from seeing an incorrect or intermediate UI.

---

# Real-Life Analogy: Stage Performance

Imagine a theater.

The actors are getting ready backstage.

```
Actors

↓

Arrange Props

↓

Adjust Lights

↓

Curtain Opens
```

The audience only sees the final, prepared stage.

`useLayoutEffect` works the same way.

It allows React to make final adjustments before the browser displays the page.

---

# Why Do We Need `useLayoutEffect`?

Normally, React renders the page.

Then the browser paints it.

Sometimes, after the page is painted, you need to:

- Measure an element's size
- Measure its position
- Scroll to a location
- Adjust styles
- Reposition tooltips
- Prevent flickering

If you wait until after the browser paints, the user might briefly see the wrong layout.

`useLayoutEffect` avoids that by running **before** the paint.

---

# Visual Timeline

```
React Render

↓

DOM Updated

↓

useLayoutEffect

↓

Browser Paint

↓

User Sees Screen
```

---

# Compare with `useEffect`

`useEffect`

```
React Render

↓

DOM Updated

↓

Browser Paint

↓

User Sees Screen

↓

useEffect Runs
```

Notice:

The user already sees the page before `useEffect` runs.

---

# Basic Syntax

```jsx
useLayoutEffect(() => {

  // Read or update DOM

  return () => {

    // Cleanup

  };

}, []);
```

The syntax is almost identical to `useEffect`.

The main difference is **when it runs**.

---

# Example 1: Measure Width

Suppose you need the width of a `<div>`.

```jsx
import {
  useLayoutEffect,
  useRef
} from "react";

function App() {

  const boxRef =
    useRef(null);

  useLayoutEffect(() => {

    console.log(
      boxRef.current.offsetWidth
    );

  }, []);

  return (
    <div ref={boxRef}>
      Hello
    </div>
  );

}
```

React measures the width before the browser paints the screen.

---

# Example 2: Prevent Flickering

Imagine a tooltip.

Without `useLayoutEffect`

```
Tooltip Appears

↓

Wrong Position

↓

Moves

↓

Correct Position
```

The user briefly sees the incorrect position.

---

With `useLayoutEffect`

```
Tooltip Created

↓

Measure Position

↓

Move Tooltip

↓

Browser Paints

↓

Correct Position Visible
```

The user never sees the incorrect position.

---

# Example 3: Scroll to Bottom

```jsx
useLayoutEffect(() => {

  listRef.current.scrollTop =
    listRef.current.scrollHeight;

}, []);
```

The browser paints the page after the scroll position is already correct.

---

# Real-Life Analogy: Painting a Wall

Imagine painting a wall.

```
Fill Holes

↓

Sand Surface

↓

Paint
```

You prepare the wall before anyone sees it.

`useLayoutEffect` prepares the UI before it is displayed.

---

# When Should You Use `useLayoutEffect`?

Use it when you need to:

- Measure element size
- Measure element position
- Scroll before paint
- Position tooltips
- Position popups
- Synchronize animations
- Avoid visual flickering

---

# When NOT to Use It

Don't use it for:

- Fetching APIs
- Timers
- Logging
- Analytics
- Most side effects

Those belong in `useEffect`.

---

# Why?

`useLayoutEffect` blocks the browser from painting until it finishes.

If you perform slow work inside it,

the screen appears later.

---

# Summary

- `useLayoutEffect` runs after the DOM is updated but before the browser paints.
- It is useful for reading or modifying layout-related information without visual flicker.
- It should be used only when necessary because it can delay painting.

---

# 50. `useLayoutEffect` vs `useEffect`

## Simple Difference

The biggest difference is **timing**.

- **`useEffect` runs after the browser paints the screen.**
- **`useLayoutEffect` runs before the browser paints the screen.**

---

# Easy Memory Trick

Imagine cleaning your room.

### `useLayoutEffect`

```
Clean Room

↓

Guests Arrive
```

Guests never see the mess.

---

### `useEffect`

```
Guests Arrive

↓

Then Clean Room
```

Guests briefly see the mess.

---

# Visual Timeline

## `useEffect`

```
React Render

↓

DOM Updated

↓

Browser Paint

↓

User Sees UI

↓

useEffect Runs
```

---

## `useLayoutEffect`

```
React Render

↓

DOM Updated

↓

useLayoutEffect Runs

↓

Browser Paint

↓

User Sees UI
```

---

# Example: Measuring Height

Suppose you need an element's height.

With `useEffect`

```
Render

↓

Paint

↓

Measure

↓

Update

↓

Paint Again
```

The user may notice the UI shift.

---

With `useLayoutEffect`

```
Render

↓

Measure

↓

Update

↓

Paint Once
```

No visible jump.

---

# Example: Tooltip

Using `useEffect`

```
Wrong Position

↓

User Sees It

↓

Move Tooltip
```

A flicker can occur.

---

Using `useLayoutEffect`

```
Calculate Position

↓

Move Tooltip

↓

Paint
```

The tooltip appears correctly from the start.

---

# Example: API Call

```jsx
useEffect(() => {

  fetch("/users");

}, []);
```

This is the correct choice.

There's no need to block the browser while waiting for network requests.

---

# Example: DOM Measurement

```jsx
useLayoutEffect(() => {

  console.log(
    divRef.current.offsetHeight
  );

}, []);
```

Correct.

---

# Performance Difference

## `useEffect`

```
Paint Screen

↓

Run Effect
```

The browser remains responsive.

---

## `useLayoutEffect`

```
Run Effect

↓

Paint Screen
```

Painting waits until the effect completes.

Slow work here can make the UI feel less responsive.

---

# Real-Life Analogy: Tailor

Imagine buying a suit.

`useLayoutEffect`

```
Measure

↓

Adjust

↓

Wear Suit
```

Perfect fit immediately.

---

`useEffect`

```
Wear Suit

↓

Measure

↓

Adjust Later
```

People first see the poor fit.

---

# Side-by-Side Comparison

| Feature | `useEffect` | `useLayoutEffect` |
|----------|-------------|-------------------|
| Runs after render | ✅ Yes | ✅ Yes |
| Runs before browser paint | ❌ No | ✅ Yes |
| Runs after browser paint | ✅ Yes | ❌ No |
| Blocks painting | ❌ No | ✅ Yes |
| Good for API calls | ✅ Yes | ❌ No |
| Good for timers | ✅ Yes | ❌ No |
| Good for DOM measurement | Possible, but may flicker | ✅ Excellent |
| Good for layout calculations | Possible, but may flicker | ✅ Excellent |
| Good for preventing flicker | ❌ Usually No | ✅ Yes |

---

# When to Use Each

## Use `useEffect`

- API requests
- Logging
- Analytics
- Timers
- WebSocket connections
- Event listeners
- Most side effects

---

## Use `useLayoutEffect`

- Measuring DOM elements
- Reading element position
- Scrolling before paint
- Positioning tooltips or popups
- Synchronizing visual layout changes
- Preventing flickering

---

# Common Mistakes

## Using `useLayoutEffect` Everywhere

Wrong.

It can slow rendering because it blocks painting.

Prefer `useEffect` unless you specifically need pre-paint behavior.

---

## Fetching Data with `useLayoutEffect`

```jsx
useLayoutEffect(() => {

  fetch("/users");

}, []);
```

This provides no benefit and unnecessarily delays painting.

Use `useEffect` instead.

---

## Ignoring Cleanup

Just like `useEffect`, `useLayoutEffect` can return a cleanup function.

```jsx
useLayoutEffect(() => {

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

---

# Common Interview Questions

## 1. What is `useLayoutEffect`?

**Answer:**

`useLayoutEffect` is a React Hook that runs synchronously after the DOM is updated but before the browser paints the screen. It is used for layout measurements and DOM updates that should happen before the user sees the UI.

---

## 2. What Is the Difference Between `useEffect` and `useLayoutEffect`?

**Answer:**

`useEffect` runs after the browser paints the screen.

`useLayoutEffect` runs before the browser paints the screen.

---

## 3. Why Is `useLayoutEffect` Less Common?

**Answer:**

Because it blocks the browser from painting until it finishes. Most side effects don't require this behavior, so `useEffect` is usually the better choice.

---

## 4. When Should You Use `useLayoutEffect`?

**Answer:**

Use it for DOM measurements, positioning elements, synchronizing visual updates, scrolling, or preventing flickering.

---

## 5. Can `useLayoutEffect` Replace `useEffect`?

**Answer:**

Technically, many `useEffect` use cases would still work with `useLayoutEffect`, but it is not recommended. `useLayoutEffect` should be reserved for layout-related work because it can impact performance.

---

# Easy Memory Trick

Imagine hanging a picture frame.

### `useLayoutEffect`

```
Measure Wall

↓

Hang Frame

↓

Guests Arrive
```

Guests see it perfectly placed.

---

### `useEffect`

```
Hang Frame

↓

Guests Arrive

↓

Adjust Frame
```

Guests briefly see it crooked.

---

# Summary

- `useLayoutEffect` runs after React updates the DOM but before the browser paints.
- It is designed for layout-related tasks such as measuring elements, adjusting positions, scrolling, and preventing flickering.
- `useEffect` runs after the browser paints and is the preferred Hook for most side effects like API calls, timers, subscriptions, and logging.
- `useLayoutEffect` should be used sparingly because it blocks painting and can affect performance if overused.
- A simple rule to remember:
  - **If it affects what the user sees before the screen appears → `useLayoutEffect`.**
  - **For almost everything else → `useEffect`.**