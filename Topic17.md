# 25. Controlled vs Uncontrolled Components

## Introduction

Whenever users type into a form, React needs a way to handle that data.

For example:

- Login Form
- Registration Form
- Search Box
- Contact Form
- Feedback Form

There are **two ways** to handle form data in React:

1. **Controlled Components**
2. **Uncontrolled Components**

The biggest difference is:

> **Who controls the form data?**

- **Controlled Component** → React controls the data.
- **Uncontrolled Component** → The browser (DOM) controls the data.

---

# Real-Life Analogy: Driving a Car

Imagine two cars.

## Car 1: Remote Controlled Car

A person controls every movement.

```
Remote

↓

Car
```

The car only moves when the remote tells it to.

This is like a **Controlled Component**.

React controls the input value.

---

## Car 2: Self-Driven Car

The car manages itself.

```
Car

↓

Moves on its own
```

The driver checks it only when needed.

This is like an **Uncontrolled Component**.

The browser stores the value until React asks for it.

---

# What is a Controlled Component?

A **Controlled Component** is a form element whose value is **controlled by React State**.

The value displayed in the input always comes from React.

```
State

↓

Input Box

↓

User Types

↓

State Updates

↓

React Re-renders

↓

Input Updates
```

React is the **single source of truth**.

---

# Example of Controlled Component

```jsx
import { useState } from "react";

function App() {
  const [name, setName] = useState("");

  return (
    <input
      type="text"
      value={name}
      onChange={(e) =>
        setName(e.target.value)
      }
    />
  );
}
```

---

# How It Works

Initially

```
State = ""
```

User types

```
J
```

Flow

```
User Types

↓

onChange Event

↓

setName("J")

↓

State Updates

↓

React Re-renders

↓

Input Shows "J"
```

Every keystroke updates React State.

---

# Visual Representation

```
User

↓

Types

↓

onChange

↓

State

↓

React

↓

UI
```

Everything passes through React.

---

# Benefits of Controlled Components

- Easy validation
- Easy error handling
- Easy form submission
- Easy to reset forms
- Keeps UI and data synchronized
- Single source of truth

This is why **Controlled Components are the recommended approach in React**.

---

# Example: Live Character Counter

```jsx
import { useState } from "react";

function App() {
  const [text, setText] =
    useState("");

  return (
    <>
      <input
        value={text}
        onChange={(e) =>
          setText(e.target.value)
        }
      />

      <p>
        Characters:
        {text.length}
      </p>
    </>
  );
}
```

As the user types,

the character count updates instantly.

This is possible because React always knows the current value.

---

# What is an Uncontrolled Component?

An **Uncontrolled Component** lets the **browser (DOM)** manage the input value.

React does **not** store every keystroke in State.

Instead, React reads the value **only when needed**, usually using a **Ref**.

---

# Example of Uncontrolled Component

```jsx
import { useRef } from "react";

function App() {
  const inputRef = useRef();

  function showValue() {
    alert(inputRef.current.value);
  }

  return (
    <>
      <input
        type="text"
        ref={inputRef}
      />

      <button onClick={showValue}>
        Show Value
      </button>
    </>
  );
}
```

---

# How It Works

Initially

```
Input

↓

Empty
```

User types

```
Saurabh
```

React State doesn't change.

Only when the button is clicked,

React reads

```jsx
inputRef.current.value
```

---

# Visual Representation

```
User

↓

Input Box

↓

Browser Stores Value

↓

React Reads It Later
```

---

# Controlled vs Uncontrolled Flow

## Controlled

```
User

↓

Input

↓

State

↓

React

↓

Updated UI
```

---

## Uncontrolled

```
User

↓

Input

↓

Browser Stores Value

↓

React Reads Value Only When Needed
```

---

# Example: Login Form

## Controlled

```jsx
const [email, setEmail] =
  useState("");

<input
  value={email}
  onChange={(e) =>
    setEmail(e.target.value)
  }
/>
```

React always knows the email.

---

## Uncontrolled

```jsx
const emailRef = useRef();

<input ref={emailRef} />
```

React only knows the email when you read:

```jsx
emailRef.current.value
```

---

# Real-Life Analogy: Classroom Attendance

## Controlled

Teacher records attendance immediately.

```
Student Enters

↓

Teacher Updates Register
```

The register is always up to date.

---

## Uncontrolled

Teacher waits until the end of the day.

```
Students Enter

↓

Teacher Checks Classroom Later
```

The information isn't tracked continuously.

---

# When Should You Use Controlled Components?

Use Controlled Components when:

- Building login forms
- Registration forms
- Search boxes
- Form validation
- Real-time calculations
- Live search
- Character counters
- Dynamic UI updates

These are the most common scenarios in React applications.

---

# When Should You Use Uncontrolled Components?

Use Uncontrolled Components when:

- Integrating with non-React libraries
- Working with legacy HTML forms
- Reading values only on form submission
- Handling file inputs (`<input type="file">`), which are typically uncontrolled

They are less common in modern React applications.

---

# Comparison Table

| Feature | Controlled Component | Uncontrolled Component |
|----------|----------------------|------------------------|
| Data Owner | React State | Browser (DOM) |
| Uses `useState` | ✅ Yes | ❌ No |
| Uses `useRef` | Optional | ✅ Usually Yes |
| Value Updates | Every keystroke updates State | DOM stores value until read |
| Validation | Easy | More manual |
| Live UI Updates | Easy | Difficult |
| Recommended | ✅ Yes | Only for specific use cases |

---

# Controlled Example: Search Box

```jsx
import { useState } from "react";

function Search() {
  const [query, setQuery] =
    useState("");

  return (
    <>
      <input
        value={query}
        onChange={(e) =>
          setQuery(e.target.value)
        }
      />

      <h2>
        Searching for:
        {query}
      </h2>
    </>
  );
}
```

Every character typed immediately updates the UI.

---

# Uncontrolled Example: Search Box

```jsx
import { useRef } from "react";

function Search() {
  const inputRef = useRef();

  function search() {
    console.log(
      inputRef.current.value
    );
  }

  return (
    <>
      <input ref={inputRef} />

      <button onClick={search}>
        Search
      </button>
    </>
  );
}
```

The UI doesn't automatically reflect the input value because React isn't tracking it.

---

# Common Interview Questions

## 1. What is a Controlled Component?

**Answer:**

A Controlled Component is a form element whose value is managed by React State. The input's displayed value comes from State, and every change updates that State using an `onChange` event.

---

## 2. What is an Uncontrolled Component?

**Answer:**

An Uncontrolled Component stores its value in the browser's DOM. React typically accesses the value using a `ref` when needed instead of tracking every change.

---

## 3. Which Approach Does React Recommend?

**Answer:**

React generally recommends **Controlled Components** because they provide a single source of truth, simplify validation, and make form behavior more predictable.

---

## 4. Which Hook Is Commonly Used in Controlled Components?

**Answer:**

`useState`

---

## 5. Which Hook Is Commonly Used in Uncontrolled Components?

**Answer:**

`useRef`

---

## 6. Is `<input type="file">` Usually Controlled?

**Answer:**

No.

File inputs are typically **uncontrolled** because, for security reasons, browsers do not allow JavaScript to set the selected file programmatically.

---

# Easy Memory Trick

Imagine a bank.

## Controlled

Every transaction is immediately recorded in the bank's system.

```
Deposit

↓

Bank Updates Database
```

The system always has the latest balance.

---

## Uncontrolled

You keep track of your money in your wallet.

The bank only knows the amount when you visit the branch and show it.

React behaves similarly.

- **Controlled** → React always knows the latest value.
- **Uncontrolled** → The browser keeps the value until React asks for it.

---

# Summary

- Controlled Components store form data in React State.
- Uncontrolled Components let the browser manage form data and typically use `useRef` to access it.
- Controlled Components are the preferred approach for most React applications because they support validation, live updates, and predictable state management.
- Uncontrolled Components are useful in specific scenarios, such as integrating with non-React code or working with file inputs.
- Understanding both approaches helps you choose the right solution based on the application's requirements.