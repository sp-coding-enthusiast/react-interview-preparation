# Parent Re-render Impact on Children

One of the most misunderstood topics in React interviews is:

> **"If a parent component re-renders, will all child components also re-render?"**

The short answer is:

> **Yes, by default React re-renders all child components when the parent re-renders.**

However,

> **Re-render does NOT necessarily mean the browser (Real DOM) is updated.**

Let's understand this step by step.

---

# What Happens During a Parent Re-render?

Consider this component tree.

```
App

├── Header
├── Sidebar
├── Main
└── Footer
```

Suppose `App` has a state.

```jsx
function App() {

  const [count, setCount] = useState(0);

  return (
    <>
      <Header />
      <Sidebar />
      <Main />
      <Footer />
    </>
  );

}
```

When

```jsx
setCount(count + 1);
```

is called,

React executes:

```
App()
```

again.

Since `App()` executes again,

it returns

```jsx
<>
  <Header />
  <Sidebar />
  <Main />
  <Footer />
</>
```

React now also executes:

```
Header()

Sidebar()

Main()

Footer()
```

Again.

---

# Visual Flow

```
State Changes

↓

App Re-renders

↓

Header Re-renders

↓

Sidebar Re-renders

↓

Main Re-renders

↓

Footer Re-renders
```

This is the default React behavior.

---

# Does This Mean Everything Updates on Screen?

No.

React first creates a new Virtual DOM.

```
Old Virtual DOM

↓

New Virtual DOM

↓

Diffing

↓

Only Actual Changes

↓

Real DOM Update
```

If `Header` renders the same output,

its DOM is **not** updated.

---

# Example

Parent

```jsx
function App() {

  const [count, setCount] =
    useState(0);

  return (

    <>
      <h1>{count}</h1>

      <Header />

    </>

  );

}
```

Header

```jsx
function Header() {

  console.log("Header Rendered");

  return <h2>React Course</h2>;

}
```

Click button

```
Count = 1
```

Console

```
Header Rendered
```

Even though Header's UI never changed.

Why?

Because the parent rendered again.

---

# Why Does React Do This?

React follows a simple rule.

```
Parent Executes

↓

Children Execute

↓

Compare Results

↓

Update Only Differences
```

This keeps React predictable.

---

# Real-Life Analogy

Imagine a manager checking reports.

Departments:

```
HR

Sales

Finance
```

Every morning,

the manager asks each department for its report.

Even if Finance has nothing new,

the manager still asks.

Later,

only updated reports are filed.

That's how React works.

---

# When Do Children NOT Re-render?

React provides optimization techniques.

---

# 1. React.memo()

```jsx
const Header = React.memo(function Header() {

  return <h1>Header</h1>;

});
```

If the parent re-renders,

but Header's props stay the same,

React skips executing `Header`.

---

Visual

```
Parent Re-render

↓

Props Same?

↓

Yes

↓

Skip Header
```

---

# Example

```jsx
const Header = React.memo(() => {

  console.log("Header");

  return <h1>Header</h1>;

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

No

```
Header Rendered
```

because React.memo prevented it.

---

# 2. Stable Props Matter

Consider

```jsx
<Header

  title="React"

/>
```

String

```
"React"
```

doesn't change.

React.memo can skip rendering.

---

But

```jsx
<Header

  user={{ name: "John" }}

/>
```

Every render creates

```
New Object
```

React sees

```
Old Object !== New Object
```

Header re-renders.

---

Better

```jsx
const user =
  useMemo(() => ({

    name: "John"

  }), []);

<Header user={user} />
```

Now the object reference remains stable.

---

# 3. useCallback()

Functions are recreated every render.

Without

```jsx
useCallback()
```

```jsx
<Header

  onClick={() => {}}

/>
```

Every render

↓

New function

↓

React.memo fails.

---

With

```jsx
const handleClick =
  useCallback(() => {

  }, []);

<Header

  onClick={handleClick}

/>
```

Now

```
Same Function Reference
```

React.memo works.

---

# Component Tree Example

```
App

├── Header

├── Sidebar

├── Dashboard

│     ├── Card

│     ├── Card

│     └── Card

└── Footer
```

Suppose only:

```
Dashboard Count
```

changes.

Without optimization,

React executes:

```
App

Header

Sidebar

Dashboard

Card

Card

Card

Footer
```

Again.

DOM updates only where needed.

---

With React.memo,

```
Header

↓

Skipped
```

```
Sidebar

↓

Skipped
```

```
Footer

↓

Skipped
```

Only Dashboard re-renders.

---

# Does React.memo Stop DOM Updates or Component Execution?

It prevents **unnecessary component execution** when props are unchanged.

If the component isn't executed,

React also doesn't need to compare its output for that render.

---

# Parent Re-render vs Child Re-render

| Scenario | Child Re-renders? |
|-----------|-------------------|
| Parent re-renders | ✅ Yes (default) |
| Parent re-renders + `React.memo` + same props | ❌ No |
| Props change | ✅ Yes |
| Context consumed by child changes | ✅ Yes |
| Child state changes | ✅ Yes (the child itself re-renders) |
| `useRef.current` changes | ❌ No |

---

# Interview Questions

## 1. If a Parent Re-renders, Will All Children Re-render?

**Answer:**

Yes.

By default, React executes all child components whenever the parent re-renders.

---

## 2. Does Child Re-render Mean DOM Update?

**Answer:**

No.

A child re-render only means React executes the component again.

The Real DOM is updated only if the rendered output changes.

---

## 3. How Can You Prevent Unnecessary Child Re-renders?

**Answer:**

By using:

- `React.memo()`
- `useMemo()`
- `useCallback()`

and by keeping props stable.

---

## 4. Why Does React Re-render Children?

**Answer:**

Because React builds the UI by executing the parent component, which naturally recreates its child elements. React then compares the new results with the previous render to determine whether any DOM updates are needed.

---

# Easy Memory Trick

Imagine a teacher taking attendance.

```
Teacher

↓

Calls Every Student's Name
```

Even if every student is present,

the teacher still checks everyone.

Only the attendance register is updated if something actually changed.

Similarly,

```
Parent Re-render

↓

Children Execute

↓

React Checks Output

↓

DOM Updates Only If Needed
```

---

# Summary

- By default, **when a parent re-renders, all of its child components also re-render**.
- A **re-render** means React executes the component function again.
- A re-render **does not automatically update the Real DOM**.
- React compares the new Virtual DOM with the previous one and updates only the parts that changed.
- `React.memo()` can prevent unnecessary child re-renders when props are unchanged.
- `useMemo()` and `useCallback()` help keep object and function props stable so that `React.memo()` can be effective.
- Understanding the difference between **component re-rendering** and **DOM updates** is a very common React interview topic.