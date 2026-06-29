# 72. What Causes Unnecessary Re-renders?

## Simple Definition

An **unnecessary re-render** happens when React **re-renders a component even though its UI hasn't actually changed.**

In simple words,

> **React executes the component again, but the user sees exactly the same UI.**

This wastes:

- CPU time
- Memory
- Battery
- Performance

Although React is very fast, avoiding unnecessary re-renders becomes important in large applications.

---

# Real-Life Analogy

Imagine a teacher checking attendance.

The teacher asks every student:

```
Are you here?

↓

Yes

↓

Are you here?

↓

Yes
```

Even though nothing changed.

Time is wasted.

React sometimes behaves similarly.

---

# What Actually Happens?

Suppose this component exists.

```jsx
function App() {

  const [count, setCount] =
    useState(0);

  return (

    <>
      <Header />
      <Footer />
    </>

  );

}
```

Click button

```
Count++

↓

App Re-renders

↓

Header Re-renders

↓

Footer Re-renders
```

But

```
Header

Footer
```

didn't change.

Those renders were unnecessary.

---

# Common Causes

---

# 1. Parent Re-render

This is the most common reason.

```jsx
<App>

  <Header />

</App>
```

When

```
App
```

re-renders,

```
Header
```

also re-renders by default.

Even if Header has no changes.

---

Flow

```
Parent Re-render

↓

Child Re-render

↓

Same Output

↓

Unnecessary Work
```

---

# 2. Passing New Object References

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
Old Object !== New Object
```

Child re-renders.

---

Better

```jsx
const user =
  useMemo(() => ({

    name: "John"

  }), []);

<Profile user={user} />
```

Now

```
Same Object Reference
```

No unnecessary re-render (when combined with `React.memo`).

---

# 3. Passing New Functions

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

React sees

```
Old Function !== New Function
```

Child re-renders.

---

Better

```jsx
const handleClick =
  useCallback(() => {

  }, []);

<Button

  onClick={handleClick}

/>
```

---

# 4. Context Updates

Suppose

```jsx
<UserContext.Provider

  value={user}

>
```

When

```
user
```

changes,

every component consuming that context re-renders.

Even if it only uses one small part of the value.

---

# 5. Updating State with a New Reference

Example

```jsx
setUser({

  ...user

});
```

Even though all values are the same,

this creates a new object reference.

React schedules a re-render because the state reference changed.

---

# 6. Large Parent Components

Imagine

```
Dashboard

↓

Header

↓

Sidebar

↓

Products

↓

Footer
```

Updating

```
Products
```

causes

```
Header

Sidebar

Footer
```

to re-render too.

Large component trees make this more expensive.

---

# 7. Missing React.memo()

Without

```jsx
React.memo()
```

every parent re-render causes child re-renders.

---

# Example

```jsx
function Header() {

  return <h1>Header</h1>;

}
```

Every parent render

↓

Header render

---

With

```jsx
const Header =
  React.memo(function Header() {

    return <h1>Header</h1>;

});
```

React can skip rendering when props haven't changed.

---

# 8. Incorrect useEffect Updates

Example

```jsx
useEffect(() => {

  setCount(count + 1);

});
```

No dependency array.

Flow

```
Render

↓

Effect

↓

State Update

↓

Render

↓

Effect

↓

Infinite Loop
```

Always specify dependencies correctly.

---

# How to Reduce Unnecessary Re-renders

Use:

- `React.memo()`
- `useMemo()`
- `useCallback()`
- Smaller components
- Proper state placement
- Avoid unnecessary object/function creation
- Split Context when appropriate

---

# Visual Example

Without Optimization

```
Parent

↓

Header

↓

Sidebar

↓

Products

↓

Footer
```

Update Products

↓

Everything Re-renders

---

With Optimization

```
Parent

↓

Products

↓

Only Products Re-render
```

---

# Summary

Unnecessary re-renders happen because:

- Parent re-renders
- New object references
- New function references
- Context updates
- Poor state organization
- Missing `React.memo`
- Incorrect effects

---

# 73. How React.memo Works?

## Simple Definition

`React.memo()` is a **Higher-Order Component (HOC)** that **prevents unnecessary re-renders by remembering the previous rendered result of a functional component.**

In simple words,

> **If the props haven't changed, React reuses the previous result instead of executing the component again.**

Think of it as a memory cache for components.

---

# Real-Life Analogy

Imagine a photocopy machine.

You ask for the same document.

Without memory

```
Print Again

↓

Print Again

↓

Print Again
```

Wasteful.

---

With memory

```
Already Printed?

↓

Yes

↓

Reuse Existing Copy
```

React.memo behaves similarly.

---

# Without React.memo

```jsx
function Header() {

  console.log("Header");

  return <h1>React</h1>;

}
```

Parent

```jsx
setCount(count + 1);
```

Console

```
App Rendered

Header Rendered
```

Header renders every time.

---

# With React.memo

```jsx
const Header = React.memo(function Header() {

  console.log("Header");

  return <h1>React</h1>;

});
```

Parent

```jsx
setCount(count + 1);
```

Console

```
App Rendered
```

Header doesn't render again because its props didn't change.

---

# How Does React.memo Work Internally?

Suppose

```
Old Props

↓

title="React"
```

New render

```
title="React"
```

React compares

```
Old Props

↓

New Props
```

Same?

```
Yes

↓

Skip Rendering
```

Different?

```
No

↓

Render Component
```

---

# Shallow Comparison

React.memo performs a **shallow comparison** of props.

Example

Primitive values

```jsx
<Header

  title="React"

/>
```

Old

```
React
```

New

```
React
```

Same.

No re-render.

---

Objects

```jsx
<Header

  user={{

    name: "John"

  }}

/>
```

Old

```
Object A
```

New

```
Object B
```

Even if both contain

```
name = John
```

they are different references.

React re-renders.

---

# Why useMemo Helps

```jsx
const user =
  useMemo(() => ({

    name: "John"

  }), []);

<Header user={user} />
```

Now

```
Same Reference
```

React.memo can skip rendering.

---

# Why useCallback Helps

Without

```jsx
<Button

  onClick={() => {}}

/>
```

Every render creates

```
New Function
```

React.memo fails.

---

With

```jsx
const handleClick =
  useCallback(() => {

  }, []);

<Button

  onClick={handleClick}

/>
```

Function reference remains stable.

---

# React.memo Flow

```
Parent Re-render

↓

Props Compared

↓

Same?

↓

Yes

↓

Skip Child

OR

↓

No

↓

Render Child
```

---

# Custom Comparison Function

React.memo also accepts a custom comparison function.

```jsx
const Header = React.memo(

  HeaderComponent,

  (prevProps, nextProps) => {

    return prevProps.id === nextProps.id;

  }

);
```

If the function returns

```
true
```

React skips rendering.

If it returns

```
false
```

React re-renders.

Use this carefully—custom comparisons can become more expensive than simply rendering.

---

# When Should You Use React.memo?

Good candidates:

- Large components
- Expensive rendering
- Frequently re-rendering parents
- Stable props

Poor candidates:

- Tiny components
- Components that always receive new props
- Components that already render very quickly

---

# React.memo vs useMemo

| React.memo | useMemo |
|-------------|----------|
| Memoizes an entire component | Memoizes a calculated value |
| Prevents unnecessary component re-renders | Prevents expensive recalculations |
| Works on props | Works on computations |

---

# React.memo vs useCallback

| React.memo | useCallback |
|-------------|-------------|
| Memoizes a component | Memoizes a function |
| Skips component rendering | Keeps function reference stable |
| Used for child components | Often used together with React.memo |

---

# Common Interview Questions

## 1. What Causes Unnecessary Re-renders?

**Answer:**

Common causes include:

- Parent re-renders
- New object references
- New function references
- Context updates
- Missing `React.memo`
- Incorrect `useEffect`
- Poor state organization

---

## 2. What Is React.memo?

**Answer:**

`React.memo` is a Higher-Order Component that memoizes a functional component and skips re-rendering when its props are shallowly equal to the previous props.

---

## 3. Does React.memo Compare State?

**Answer:**

No.

It compares **props**.

If a component's own **state** changes, it still re-renders.

---

## 4. Does React.memo Compare Objects Deeply?

**Answer:**

No.

It performs a **shallow comparison**.

Different object references are treated as different, even if they contain the same values.

---

## 5. When Does React.memo Not Help?

**Answer:**

When props change every render, such as newly created objects or functions, or when the component is very lightweight and the comparison overhead outweighs any benefit.

---

## 6. Is React.memo Always a Performance Improvement?

**Answer:**

No.

`React.memo` itself performs prop comparisons. For simple components, those comparisons can cost more than just re-rendering. Measure performance before applying it everywhere.

---

# Easy Memory Trick

Imagine a librarian.

You request a book.

The librarian asks:

```
Same Book?

↓

Yes

↓

Give Existing Copy
```

Different book?

```
Fetch New Book
```

React.memo follows the same logic.

- **Same Props → Reuse previous render.**
- **Different Props → Render again.**

---

# Summary

- **Unnecessary re-renders** occur when React executes components even though their visible output hasn't changed.
- Common causes include parent re-renders, new object/function references, context updates, and poor state management.
- **React.memo** memoizes a functional component and skips rendering when its props are shallowly equal.
- `React.memo` compares **props only**, not state.
- `useMemo` and `useCallback` are often used alongside `React.memo` to keep object and function props stable.
- `React.memo` is most effective for expensive components that receive stable props and re-render frequently due to parent updates.