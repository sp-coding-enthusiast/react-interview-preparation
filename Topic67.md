# 94. What is `useTransition()`?

## Simple Definition

`useTransition()` is a React Hook that lets you **mark some state updates as non-urgent** and also tells you **whether those updates are still in progress.**

In simple words,

> **It helps keep your application responsive while React performs expensive work in the background.**

Unlike `startTransition()`, `useTransition()` also provides an **`isPending`** flag, which is useful for showing loading indicators.

---

# Why Do We Need `useTransition()`?

Imagine an e-commerce website.

The user types:

```
Laptop
```

The application searches through

```
500,000 Products
```

Without `useTransition()`

```
Type

↓

Search Starts

↓

UI Freezes

↓

Next Key Press Delayed
```

Typing feels slow.

---

With `useTransition()`

```
Type

↓

Input Updates Immediately

↓

Search Runs In Background

↓

Loading Indicator Appears

↓

Results Update
```

Typing stays smooth.

---

# Real-Life Analogy

Imagine you're at a restaurant.

You ask the waiter for food.

The waiter says

```
Your Order Is Being Prepared
```

While waiting,

you can still

- Drink water
- Talk to friends
- Read the menu

The restaurant doesn't stop functioning.

Similarly,

`useTransition()` lets React continue handling urgent work while background updates are processed.

---

# Syntax

```jsx
const [isPending, startTransition] =

useTransition();
```

It returns two values.

---

## 1. `isPending`

A boolean.

```
true

↓

Transition Running

-------------------

false

↓

Transition Finished
```

---

## 2. `startTransition`

A function.

Used to mark updates as low priority.

```jsx
startTransition(() => {

  setResults(data);

});
```

---

# Basic Example

```jsx
import {

useState,

useTransition

} from "react";

function Search() {

  const [text, setText] = useState("");

  const [results, setResults] = useState([]);

  const [

    isPending,

    startTransition

  ] = useTransition();

  function handleChange(e) {

    const value = e.target.value;

    // Urgent update
    setText(value);

    // Non-urgent update
    startTransition(() => {

      setResults(filterProducts(value));

    });

  }

  return (

    <>

      <input

        value={text}

        onChange={handleChange}

      />

      {isPending && <p>Searching...</p>}

    </>

  );

}
```

---

# What Happens Internally?

```
User Types

↓

Update Input

⭐⭐⭐⭐⭐

↓

Transition Starts

⭐⭐

↓

isPending = true

↓

React Renders

↓

Done

↓

isPending = false
```

---

# Timeline

```
User Types

↓

Input Updates

↓

Spinner Appears

↓

Background Search

↓

Results Render

↓

Spinner Disappears
```

---

# When Should You Use `useTransition()`?

Good use cases:

- Large search results
- Expensive filtering
- Sorting thousands of rows
- Dashboard updates
- Rendering charts
- Complex UI rendering
- Route navigation

---

# When NOT to Use It

Don't wrap urgent updates.

Examples

```jsx
setEmail(...)
```

```jsx
setPassword(...)
```

```jsx
setChecked(...)
```

These should happen immediately.

---

# Benefits

`useTransition()` provides:

- Smooth typing
- Better responsiveness
- Loading state (`isPending`)
- Lower-priority rendering
- Better user experience

---

# 95. What is `useDeferredValue()`?

## Simple Definition

`useDeferredValue()` is a React Hook that lets you **delay updating a value** until React has finished handling more important work.

In simple words,

> **It lets one value update immediately while another version updates later.**

Unlike `useTransition()`, you **don't wrap state updates**.

Instead,

you create a **deferred copy** of a value.

---

# Why Do We Need `useDeferredValue()`?

Imagine searching

```
500,000 Products
```

Every keystroke causes filtering.

Without `useDeferredValue()`

```
Type

↓

Filter

↓

Render

↓

Type

↓

Filter

↓

Render
```

Everything updates immediately.

The UI may become sluggish.

---

With `useDeferredValue()`

```
Type

↓

Input Updates Immediately

↓

Deferred Value Waits

↓

Filtering Uses Deferred Value

↓

Results Update Later
```

Typing remains responsive.

---

# Real-Life Analogy

Imagine a news reporter.

Breaking news arrives.

The reporter immediately says:

```
Breaking News!

```

A few moments later,

the detailed report is published.

```
Breaking News

↓

Detailed Report Later
```

`useDeferredValue()` behaves similarly.

The important value updates first.

The expensive work follows.

---

# Syntax

```jsx
const deferredValue =

useDeferredValue(value);
```

---

# Example

```jsx
import {

useState,

useDeferredValue

} from "react";

function Search() {

  const [text, setText] =

    useState("");

  const deferredText =

    useDeferredValue(text);

  const results =

    filterProducts(deferredText);

  return (

    <>

      <input

        value={text}

        onChange={(e) =>

          setText(e.target.value)

        }

      />

      <Results

        products={results}

      />

    </>

  );

}
```

---

# What Happens?

Suppose the user types

```
Laptop
```

Current value

```
text

↓

Laptop
```

Deferred value

```
Laptop

↓

Updates Slightly Later
```

Filtering uses

```
Deferred Value
```

instead of the latest value.

---

# Timeline

```
Type

↓

Input Updates

↓

User Keeps Typing

↓

Deferred Value Changes

↓

Results Render
```

---

# What Happens Internally?

```
Current Value

↓

React Keeps It Updated

------------------------

Deferred Value

↓

React Delays Update

↓

Browser Free

↓

Later Render
```

---

# Common Use Cases

Use `useDeferredValue()` when:

- Search boxes
- Large product lists
- Expensive filtering
- Big tables
- Large dashboards
- Complex charts
- Slow rendering

---

# When NOT to Use It

Don't use it for:

- Password fields
- OTP inputs
- Payment forms
- Banking transactions
- Real-time validation requiring instant accuracy

These require immediate updates.

---

# `useTransition()` vs `useDeferredValue()`

| `useTransition()` | `useDeferredValue()` |
|-------------------|----------------------|
| Defers **state updates** | Defers **a value** |
| Gives `isPending` | No loading state |
| Uses `startTransition()` | No wrapper function |
| You decide which updates are low priority | React creates a delayed version of a value |
| Best for actions like filtering or navigation | Best for passing a deferred value to expensive child components |

---

# Real-World Example

Imagine YouTube.

User types

```
React
```

Immediately

```
Input

↓

React
```

Search results

```
Update Slightly Later
```

Typing stays smooth,

results catch up shortly after.

---

# Common Interview Questions

## 1. What Is `useTransition()`?

**Answer:**

`useTransition()` is a React Hook that lets you mark state updates as non-urgent while providing an `isPending` flag to indicate that the transition is still running.

---

## 2. What Is `useDeferredValue()`?

**Answer:**

`useDeferredValue()` returns a deferred version of a value, allowing React to postpone updating that value until more urgent work has completed.

---

## 3. What's the Difference Between `useTransition()` and `useDeferredValue()`?

**Answer:**

- `useTransition()` defers **state updates** and provides an `isPending` loading state.
- `useDeferredValue()` defers **a value** and returns a delayed copy without providing a loading indicator.

---

## 4. Which One Should You Use for Search?

**Answer:**

It depends on the situation.

- Use **`useTransition()`** when you control the state update and want to mark it as low priority or display loading feedback.
- Use **`useDeferredValue()`** when the input should update immediately, but an expensive child component (such as a filtered list) can safely use a slightly delayed version of the value.

---

## 5. Does `useDeferredValue()` Delay User Input?

**Answer:**

No.

The input updates immediately.

Only the deferred value—and the work based on it—is delayed.

---

# Senior Interview Tip ⭐

If you're asked:

> **"When would you choose `useTransition()` over `useDeferredValue()`?"**

A strong answer is:

> "I use `useTransition()` when I want to mark specific state updates as low priority and optionally show progress using `isPending`. I use `useDeferredValue()` when I already have a value that changes frequently—such as a search query—and I want expensive components to render using a delayed version of that value while keeping the input responsive."

This demonstrates that you understand both APIs and know when each is appropriate.

---

# Easy Memory Trick

Imagine writing a letter.

### `useTransition()`

```
Write Letter

↓

Mail Later
```

You choose when the background work begins.

---

### `useDeferredValue()`

```
Original Letter

↓

Photocopy

↓

Photocopy Delivered Later
```

The original stays current,

while the copy catches up.

---

Remember:

- **`useTransition()` = Delay the Update**
- **`useDeferredValue()` = Delay the Value**

---

# Summary

## `useTransition()`

- Marks **state updates** as non-urgent.
- Returns:
  - `startTransition()`
  - `isPending`
- Keeps the UI responsive during expensive updates.
- Ideal for filtering, sorting, dashboards, navigation, and complex rendering.

## `useDeferredValue()`

- Returns a **deferred copy of a value**.
- The original value updates immediately.
- The deferred value updates later when React has time.
- Ideal for search inputs, large lists, expensive child components, and complex rendering.

## Key Difference

- **`useTransition()`** → *Delay the state update.*
- **`useDeferredValue()`** → *Delay the value used by expensive rendering.*