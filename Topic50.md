# 74. When Should React.memo NOT Be Used?

## Simple Definition

`React.memo()` is a great optimization tool, but **it should not be used everywhere.**

In simple words,

> **If the cost of comparing props is greater than the cost of simply re-rendering the component, then using `React.memo` actually hurts performance instead of improving it.**

A common mistake among beginners is:

> "If `React.memo` improves performance, let's wrap every component with it."

This is **not** a good practice.

---

# Real-Life Analogy

Imagine you have a security guard at every room in your house.

Before anyone enters a room, the guard spends **10 seconds checking their identity**.

But entering the room itself takes only **1 second**.

```
Security Check

↓

10 Seconds

↓

Enter Room

↓

1 Second
```

The checking process is slower than simply entering.

React.memo works similarly.

Before skipping a render, React must compare props.

Sometimes that comparison costs more than re-rendering.

---

# How React.memo Works

Whenever a parent re-renders,

React does this:

```
Parent Re-render

↓

Compare Old Props

↓

Compare New Props

↓

Same?

↓

Skip Render

OR

↓

Render Component
```

Notice something?

Even when rendering is skipped,

React still performs the comparison.

That comparison itself has a cost.

---

# Case 1: Small Components

Consider

```jsx
function Title() {

  return <h1>React</h1>;

}
```

Rendering this component takes almost no time.

If you write

```jsx
const Title = React.memo(function Title() {

  return <h1>React</h1>;

});
```

Now React performs

```
Prop Comparison

↓

Skip Render
```

The comparison may cost more than simply rendering:

```
<h1>React</h1>
```

---

**Rule**

Tiny components usually **don't need React.memo**.

---

# Case 2: Component Rarely Re-renders

Imagine

```
Header
```

renders only once.

```
Application Starts

↓

Header Render

↓

Never Changes
```

Wrapping it with

```jsx
React.memo()
```

provides almost no benefit.

---

# Case 3: Props Change Every Time

Example

```jsx
<Profile

  user={{

    name: "John"

  }}

/>
```

Every render creates

```
New Object
```

React compares

```
Old Object

↓

New Object
```

Different reference.

React renders again.

```
React.memo

↓

Cannot Help
```

---

# Case 4: Inline Functions

Example

```jsx
<Button

  onClick={() => {}}

/>
```

Every render creates

```
New Function
```

React compares

```
Old Function

↓

New Function
```

Different.

Child renders again.

React.memo becomes ineffective.

---

Better

```jsx
const handleClick = useCallback(() => {

}, []);

<Button

  onClick={handleClick}

/>
```

Now the function reference stays stable.

---

# Case 5: Component Has Its Own State

Suppose

```jsx
const Counter = React.memo(function Counter() {

  const [count, setCount] =
    useState(0);

});
```

Click button

```
setCount()

↓

Counter State Changes

↓

Counter Re-renders
```

React.memo **does not stop re-renders caused by the component's own state changes.**

It only checks **props**.

---

# Case 6: Context Changes Frequently

Suppose

```jsx
const theme =
useContext(ThemeContext);
```

Even if

```jsx
React.memo()
```

is used,

changing the context value causes the component to re-render.

```
Context Update

↓

Consumer Re-render
```

React.memo cannot prevent this.

---

# Case 7: Expensive Custom Comparison

React.memo supports

```jsx
React.memo(Component,

(prev, next) => {

});
```

Imagine

```jsx
(prev, next) => {

  // Compare hundreds
  // of properties

}
```

Now every parent render performs an expensive comparison.

Sometimes

```
Comparison Cost

>

Rendering Cost
```

This makes performance worse.

---

# Case 8: Premature Optimization

Many developers do this.

```
React.memo()

useMemo()

useCallback()

Everywhere
```

The code becomes

- Harder to read
- Harder to debug
- Harder to maintain

without any measurable performance improvement.

React's default rendering is already highly optimized.

---

# When SHOULD You Use React.memo?

Good candidates

✅ Expensive components

```
Large Tables

Charts

Maps

Thousands of Rows

Complex Dashboards
```

---

Frequently re-rendering parents

```
Dashboard

↓

Small Updates

↓

Children
```

If children receive stable props,

React.memo can prevent unnecessary work.

---

# Visual Example

Without React.memo

```
Parent

↓

Header

↓

Sidebar

↓

Chart

↓

Footer
```

Parent changes

↓

Everything renders

---

With React.memo

```
Parent

↓

Header

↓

Sidebar

↓

Chart

↓

Footer
```

Only

```
Chart Props Changed
```

↓

Chart renders.

Others are skipped.

---

# Decision Flow

```
Is Component Expensive?

↓

No

↓

Don't Use React.memo

------------

Yes

↓

Does Parent Re-render Often?

↓

No

↓

Don't Use React.memo

------------

Yes

↓

Are Props Stable?

↓

No

↓

Fix Props First

------------

Yes

↓

Use React.memo
```

---

# Performance Rule

React.memo helps when:

```
Rendering Cost

>

Prop Comparison Cost
```

React.memo hurts when:

```
Prop Comparison Cost

>

Rendering Cost
```

---

# React.memo vs Normal Rendering

| Normal Component | React.memo Component |
|------------------|----------------------|
| Always re-renders when parent renders | Compares props first |
| No comparison cost | Small comparison cost |
| Simple | Optimized for stable props |
| Best for small components | Best for expensive components |

---

# Common Interview Questions

## 1. Should Every Component Use React.memo?

**Answer:**

No.

React.memo should only be used when it provides a measurable performance benefit. Wrapping every component can actually reduce performance and make the code harder to maintain.

---

## 2. Why Can React.memo Hurt Performance?

**Answer:**

Because React must compare previous and current props on every parent re-render. If the component is very small, that comparison can cost more than simply rendering it again.

---

## 3. Does React.memo Prevent State Updates?

**Answer:**

No.

If a component's own state changes, it re-renders regardless of React.memo.

---

## 4. Does React.memo Prevent Context Updates?

**Answer:**

No.

When a context value changes, components consuming that context re-render even if they are wrapped with React.memo.

---

## 5. Why Doesn't React.memo Work with Inline Objects or Functions?

**Answer:**

Because every render creates a new object or function reference.

React.memo performs a shallow comparison, so new references are considered different.

---

## 6. When Is React.memo Most Useful?

**Answer:**

When:

- The component is expensive to render.
- The parent re-renders frequently.
- The component receives stable props.
- Rendering is more expensive than comparing props.

---

# Senior Interview Tip ⭐

Many candidates say:

> **"React.memo improves performance."**

A better answer is:

> **"React.memo can improve performance by avoiding unnecessary re-renders, but only when the component is expensive to render and receives stable props. Since it performs a shallow prop comparison on every parent render, using it indiscriminately may actually reduce performance."**

This demonstrates a deeper understanding of how React optimizations work.

---

# Easy Memory Trick

Imagine a teacher checking homework.

Scenario 1

```
Homework = 1 Page

Checking = 5 Minutes

Reading = 30 Seconds
```

Checking takes longer than reading.

Not worth it.

---

Scenario 2

```
Homework = 500 Pages

Checking = 5 Minutes

Reading = 5 Hours
```

Now checking first saves a huge amount of time.

React.memo follows the same idea.

- **Small Component → Just render it.**
- **Large Expensive Component → Compare first.**

---

# Summary

- **React.memo is not a universal optimization.**
- Avoid using it for:
  - Small, lightweight components
  - Components that rarely re-render
  - Components receiving new object or function props every render
  - Components whose own state changes frequently
  - Components that depend heavily on frequently changing context
  - Cases with expensive custom comparison functions
  - Premature optimization without performance evidence
- Use `React.memo` when:
  - The component is expensive to render.
  - The parent re-renders frequently.
  - Props remain stable across renders.
  - The cost of rendering is greater than the cost of comparing props.
- **Rule of Thumb:**
  - **Don't optimize first.**
  - **Measure first, then optimize where it matters.**