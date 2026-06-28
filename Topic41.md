# 62. What Triggers a Re-render in React?

## Simple Definition

A **re-render** happens when React **executes a component again to calculate what the UI should look like based on the latest data**.

In simple words,

> **A re-render means React runs your component function again to see if the UI needs to change.**

**Important:**

A re-render **does not mean** React updates the Real DOM.

It first creates a new Virtual DOM, compares it with the previous one, and only updates the Real DOM if something actually changed.

---

# Real-Life Analogy: Restaurant Order

Imagine you're a chef.

A customer changes their order.

You don't immediately start cooking.

Instead, you:

```
Read New Order

↓

Check Differences

↓

Cook Only What's Different
```

React works the same way.

A re-render is like reading the updated order.

The Real DOM update happens later during the Commit Phase.

---

# What Happens During a Re-render?

Suppose your component is:

```jsx
function App() {

  return <h1>Hello</h1>;

}
```

React renders it.

Later,

something changes.

React executes the component again.

```
App()

↓

New Virtual DOM

↓

Compare

↓

Update Real DOM (if needed)
```

---

# What Can Trigger a Re-render?

There are several common triggers.

---

# 1. State Changes (`useState`)

This is the most common trigger.

Example

```jsx
const [count, setCount] =
  useState(0);

setCount(1);
```

Flow

```
setCount()

↓

State Changes

↓

React Re-renders

↓

New Virtual DOM
```

Whenever state changes,

React schedules a re-render.

---

# Example

```jsx
function App() {

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

Clicking the button triggers a re-render.

---

# 2. Props Change

When a parent re-renders and passes **different props**, the child also re-renders.

Example

Parent

```jsx
<Child

  age={30}

/>
```

Later

```jsx
<Child

  age={31}

/>
```

Flow

```
Parent

↓

Props Change

↓

Child Re-renders
```

---

# 3. Parent Re-renders

Even if the child's props are unchanged,

the child component is **normally re-rendered** when the parent re-renders.

Example

```
App

↓

Header

↓

Navbar

↓

Footer
```

If `App` re-renders,

its children are rendered again by default.

> **Exception:** Components wrapped with `React.memo` (and whose props haven't changed) can skip this re-render.

---

# 4. Context Value Changes

Suppose

```jsx
<UserContext.Provider

  value={user}

>
```

If `user` changes,

all components consuming that Context re-render.

Flow

```
Context Changes

↓

Consumers Re-render
```

---

# 5. Reducer State Changes (`useReducer`)

Example

```jsx
dispatch({

  type: "increment"

});
```

Flow

```
dispatch()

↓

Reducer

↓

New State

↓

Re-render
```

---

# 6. Force Update (Rare)

Class components have:

```jsx
this.forceUpdate();
```

This forces a re-render even if state hasn't changed.

In modern React,

this is rarely needed.

---

# What Does NOT Trigger a Re-render?

Many developers misunderstand this.

---

# 1. Changing `useRef`

Example

```jsx
const ref =
  useRef(0);

ref.current++;
```

React does **not** re-render.

Why?

Because `useRef` stores mutable values that React does not track for rendering.

---

# 2. Changing Normal Variables

```jsx
let count = 0;

count++;
```

React knows nothing about this variable.

No re-render occurs.

---

# 3. Mutating State Directly

Wrong

```jsx
user.name = "John";
```

React doesn't know anything changed.

Correct

```jsx
setUser({

  ...user,

  name: "John"

});
```

Creating a new state value and updating it through the setter lets React schedule a re-render.

---

# 4. Mutating Arrays

Wrong

```jsx
items.push(newItem);
```

No state update is triggered.

Correct

```jsx
setItems([

  ...items,

  newItem

]);
```

A new array reference is created.

---

# 5. Mutating Objects

Wrong

```jsx
person.age = 30;
```

Correct

```jsx
setPerson({

  ...person,

  age: 30

});
```

Always create a new object instead of mutating the existing one.

---

# Re-render Does NOT Mean DOM Update

This is an important interview point.

Suppose

```jsx
setCount(1);
```

React re-renders.

Old

```
Hello
```

New

```
Hello
```

Nothing changed.

Result

```
No DOM Update
```

React compares the Virtual DOMs.

If they're identical,

the Commit Phase has little or nothing to update.

---

# Visual Flow

```
State Changes

↓

Re-render

↓

New Virtual DOM

↓

Compare

↓

Different?

↓

Yes

↓

Update DOM

OR

↓

No

↓

Skip DOM Update
```

---

# Why Is This Efficient?

Imagine changing one word in a 500-page document.

You don't print the entire document again.

You print only the changed page.

React behaves similarly.

---

# Can React Skip Re-renders?

Yes.

React provides optimization techniques.

Examples:

- `React.memo`
- `useMemo`
- `useCallback`

These help reduce unnecessary work in appropriate situations.

---

# Real-Life Example

Imagine YouTube.

You click:

```
Like
```

Only:

```
Like Count
```

changes.

React doesn't rebuild the entire page.

It re-renders what's needed,

compares the result,

and updates only the changed parts.

---

# Re-render Flow

```
Button Click

↓

setState()

↓

Component Function Executes Again

↓

New Virtual DOM

↓

Diffing

↓

Commit

↓

Browser Updates (if needed)
```

---

# Side-by-Side Comparison

| Action | Re-render? |
|----------|------------|
| `setState()` | ✅ Yes |
| `dispatch()` (`useReducer`) | ✅ Yes |
| Props change | ✅ Yes |
| Parent re-renders | ✅ Usually Yes |
| Context value changes | ✅ Yes (for consuming components) |
| `useRef.current` changes | ❌ No |
| Normal variable changes | ❌ No |
| Direct object mutation | ❌ No (unless state is updated properly) |
| Direct array mutation | ❌ No (unless state is updated properly) |

---

# Common Interview Questions

## 1. What Triggers a Re-render?

**Answer:**

Common triggers include:

- State updates (`useState`)
- Reducer updates (`useReducer`)
- Prop changes
- Parent re-renders
- Context value changes
- Forced updates (rare)

---

## 2. Does `useRef` Trigger a Re-render?

**Answer:**

No.

Changing `ref.current` does not trigger a re-render because React does not use it to determine what should be displayed.

---

## 3. Does Changing a Normal Variable Trigger a Re-render?

**Answer:**

No.

React only tracks state, props, and context. Regular JavaScript variables are not part of React's rendering system.

---

## 4. Does Every Re-render Update the DOM?

**Answer:**

No.

A re-render only creates a new Virtual DOM.

React updates the Real DOM only if the diffing process finds actual changes.

---

## 5. Why Doesn't Direct State Mutation Work?

**Answer:**

React detects state updates through state setter functions (`setState`) or reducer dispatches. Directly mutating an existing object or array doesn't notify React that anything changed.

---

## 6. Can a Child Re-render Even If Its Props Don't Change?

**Answer:**

Yes.

By default, if the parent re-renders, its children are also re-rendered.

Components wrapped with `React.memo` can skip re-rendering if their props are unchanged.

---

# Easy Memory Trick

Imagine a school teacher checking attendance.

A student raises a hand.

The teacher checks the attendance sheet.

If something changed,

the sheet is updated.

If nothing changed,

nothing is written.

Similarly,

React always checks,

but it updates the DOM only when necessary.

---

# Summary

- A **re-render** means React executes a component again to calculate the latest UI.
- A re-render does **not** automatically mean the Real DOM is updated.
- Common re-render triggers include:
  - `useState` updates
  - `useReducer` dispatches
  - Prop changes
  - Parent re-renders
  - Context value changes
- Changing `useRef.current` or normal JavaScript variables does **not** trigger a re-render.
- Directly mutating objects or arrays does not notify React; always create new values when updating state.
- React uses the Virtual DOM and the diffing algorithm to determine whether any actual DOM updates are necessary.