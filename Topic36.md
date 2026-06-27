# 51. What is `useImperativeHandle`?

## Simple Definition

`useImperativeHandle` is a **React Hook** that allows a **parent component to control what methods or values are exposed when using a `ref`**.

In simple words,

> **Instead of giving the parent access to the entire child component or DOM element, `useImperativeHandle` lets the child decide what the parent is allowed to access.**

Think of it as creating a **public API** for your component.

---

# Why Do We Need `useImperativeHandle`?

Normally, a parent can get a reference to a DOM element using `useRef`.

Example:

```jsx
const inputRef = useRef(null);

<input ref={inputRef} />
```

Now the parent has access to the actual DOM element.

Sometimes, this is **too much access**.

Instead of exposing everything,

you may want to expose only a few safe operations like:

- Focus the input
- Clear the input
- Open a modal
- Close a modal

This is where `useImperativeHandle` helps.

---

# Real-Life Analogy: TV Remote

Imagine you own a TV.

The TV has hundreds of internal components.

But the remote only gives you a few buttons.

```
Power

Volume

Channel

Mute
```

You **cannot** access the TV's internal wiring.

The TV decides what controls are available.

`useImperativeHandle` works exactly like this.

The child component exposes only the methods it wants the parent to use.

---

# Visual Representation

Without `useImperativeHandle`

```
Parent

↓

Entire DOM Element
```

Parent can access everything.

---

With `useImperativeHandle`

```
Parent

↓

focus()

clear()

reset()
```

Only selected methods are available.

---

# Basic Syntax

```jsx
useImperativeHandle(

  ref,

  () => ({

    method1() {},

    method2() {}

  })

);
```

Parameters:

- **ref** → The forwarded ref from the parent.
- **Function** → Returns the object that should be exposed.
- **Optional dependency array** → Recreates the exposed object only when dependencies change.

---

# It Works with `forwardRef`

`useImperativeHandle` is commonly used together with `forwardRef`.

Child Component

```jsx
import {

  forwardRef,

  useImperativeHandle,

  useRef

} from "react";

const Input = forwardRef((props, ref) => {

  const inputRef = useRef();

  useImperativeHandle(ref, () => ({

    focus() {

      inputRef.current.focus();

    }

  }));

  return <input ref={inputRef} />;

});
```

Parent

```jsx
const inputRef = useRef();

<Input ref={inputRef} />

<button

  onClick={() =>

    inputRef.current.focus()

  }

>

  Focus

</button>
```

The parent can call only the exposed `focus()` method.

---

# Flow Diagram

```
Parent

↓

Calls focus()

↓

Child Receives Call

↓

Child Focuses Input
```

---

# Example: Modal

Suppose you create a reusable modal.

Instead of exposing the entire component,

you expose only:

```jsx
{

  open(),

  close()

}
```

Parent

```jsx
modalRef.current.open();
```

or

```jsx
modalRef.current.close();
```

The parent doesn't know how the modal works internally.

It simply uses the public methods.

---

# Real-Life Analogy: ATM

When using an ATM,

you can:

```
Withdraw Money

Deposit Money

Check Balance
```

You cannot directly edit the bank database.

The ATM exposes only safe operations.

`useImperativeHandle` works the same way.

---

# When Should You Use `useImperativeHandle`?

Good use cases:

- Focus an input
- Open/close modal
- Reset a form
- Scroll a list
- Play/pause video
- Trigger animations
- Expose a limited API from reusable components

---

# When NOT to Use It

Don't use it for normal communication between parent and child.

Usually:

- Parent passes **props**
- Child notifies parent through **callbacks**

Only use `useImperativeHandle` when the parent genuinely needs to **imperatively control** the child.

---

# Benefits

- Encapsulation
- Better reusable components
- Hide internal implementation
- Expose only necessary methods
- Safer API for component libraries

---

# Summary

- `useImperativeHandle` customizes what a parent can access through a `ref`.
- It is typically used with `forwardRef`.
- It is ideal for exposing a small, controlled API such as `focus()`, `open()`, or `reset()`.

---

# 52. What is `useDebugValue`?

## Simple Definition

`useDebugValue` is a **React Hook** used to display **custom labels or information for Custom Hooks inside React Developer Tools**.

In simple words,

> **`useDebugValue` helps developers understand what a Custom Hook is doing while debugging.**

It is meant for **developers**, not end users.

---

# Why Do We Need `useDebugValue`?

Imagine you create a Custom Hook.

```jsx
function useUser() {

  ...

}
```

When inspecting your app in **React Developer Tools**,

you may only see:

```
useUser
```

That doesn't reveal much.

With `useDebugValue`,

you can display something more meaningful.

```
useUser

↓

Logged In
```

or

```
useUser

↓

Guest User
```

This makes debugging easier.

---

# Real-Life Analogy: Luggage Tag

Imagine you have many suitcases.

Without labels,

they all look similar.

```
🧳

🧳

🧳
```

With labels,

```
🧳 Clothes

🧳 Documents

🧳 Electronics
```

You immediately know what's inside.

`useDebugValue` works the same way.

It labels your Custom Hooks in development tools.

---

# Basic Syntax

```jsx
useDebugValue(value);
```

Example

```jsx
useDebugValue("Online");
```

---

# Example 1

```jsx
function useUser() {

  const [user] =
    useState("Saurabh");

  useDebugValue(user);

  return user;

}
```

React Developer Tools may display:

```
useUser

↓

Saurabh
```

---

# Example 2

```jsx
function useTheme() {

  const theme =
    "Dark";

  useDebugValue(theme);

  return theme;

}
```

Developer Tools

```
useTheme

↓

Dark
```

---

# Example 3: Connection Status

```jsx
function useConnection() {

  const connected = true;

  useDebugValue(

    connected

      ? "Connected"

      : "Disconnected"

  );

}
```

Developer Tools

```
useConnection

↓

Connected
```

---

# Formatting Debug Values

Sometimes creating the debug text is expensive.

You can provide a formatter function.

```jsx
useDebugValue(

  date,

  value =>

    value.toLocaleString()

);
```

The formatter is only called by React Developer Tools when needed, helping avoid unnecessary work during normal rendering.

---

# Visual Representation

Without `useDebugValue`

```
React DevTools

↓

useTheme
```

---

With `useDebugValue`

```
React DevTools

↓

useTheme

↓

Dark
```

---

# Does It Affect Users?

No.

Users never see it.

It appears only inside React Developer Tools.

---

# Should You Use It Everywhere?

No.

It is most useful when creating reusable Custom Hooks that other developers will inspect.

For small applications,

it is often unnecessary.

---

# Benefits

- Easier debugging
- Better Developer Tools output
- Helpful for reusable libraries
- Better developer experience

---

# Common Mistakes

## Using It in Components

Wrong

```jsx
function App() {

  useDebugValue("Hello");

}
```

It provides little value here.

It is mainly intended for **Custom Hooks**.

---

## Thinking It Changes the UI

It doesn't.

It only helps while debugging.

---

# Side-by-Side Comparison

| Feature | `useImperativeHandle` | `useDebugValue` |
|----------|-----------------------|-----------------|
| Purpose | Expose selected methods through a `ref` | Display labels in React Developer Tools |
| Used by | Parent and child components | Developers |
| Affects UI | ❌ No | ❌ No |
| Commonly used with | `forwardRef` | Custom Hooks |
| Main goal | Controlled imperative API | Easier debugging |

---

# Real-World Examples

## `useImperativeHandle`

Reusable component library.

```
Date Picker

↓

open()

close()

clear()
```

The parent uses only those public methods.

---

## `useDebugValue`

Reusable authentication Hook.

```
useAuth

↓

Authenticated
```

React Developer Tools immediately show the authentication status.

---

# Common Interview Questions

## 1. What is `useImperativeHandle`?

**Answer:**

`useImperativeHandle` is a React Hook that customizes the value exposed through a `ref`. It is commonly used with `forwardRef` to expose a limited set of methods or properties to a parent component.

---

## 2. Why Do We Use `useImperativeHandle`?

**Answer:**

We use it to expose only the methods that a parent needs, improving encapsulation and preventing direct access to the child component's internal implementation.

---

## 3. What is `useDebugValue`?

**Answer:**

`useDebugValue` is a React Hook that displays custom debugging information for Custom Hooks inside React Developer Tools.

---

## 4. Does `useDebugValue` Affect the UI?

**Answer:**

No.

It is only visible in React Developer Tools and has no effect on what users see.

---

## 5. Can `useDebugValue` Be Used Without a Custom Hook?

**Answer:**

Yes, technically.

However, it is intended for Custom Hooks, where it provides meaningful debugging information.

---

# Easy Memory Trick

Imagine building a car.

### `useImperativeHandle`

```
Driver

↓

Steering Wheel

↓

Car
```

The driver gets only the controls they need—not the engine internals.

---

### `useDebugValue`

```
Mechanic

↓

Dashboard Diagnostics

↓

Engine Status
```

It helps diagnose the system but doesn't change how the car drives.

---

# Summary

- `useImperativeHandle` lets a child component expose a controlled set of methods or values through a `ref`.
- It is commonly paired with `forwardRef` to build reusable components with a clear public API.
- `useDebugValue` is used to display helpful information about Custom Hooks in React Developer Tools.
- `useDebugValue` has no impact on application behavior or the user interface.
- A simple way to remember them is:
  - **`useImperativeHandle` → Control what the parent can do.**
  - **`useDebugValue` → Help developers understand what a Custom Hook is doing.**