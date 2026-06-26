# 24. What is Lifting State Up?

## Simple Definition

**Lifting State Up** means **moving the State from a child component to its nearest common parent component so that multiple components can share the same data.**

In simple words,

> If two or more components need the same data, don't keep that data inside one child component. Move it to their common parent and pass it down using **Props**.

---

# Real-Life Analogy: Family TV Remote

Imagine two siblings watching TV.

```
👦 Brother

👧 Sister
```

The TV remote is with the brother.

```
Brother

↓

Remote
```

The sister wants to change the channel.

She can't because only the brother has the remote.

This causes a problem.

Instead, the parents keep the remote.

```
Parents

↓

Remote

↓

Brother

↓

Sister
```

Now both children can use the TV through the parents.

This is exactly what **Lifting State Up** does.

---

# Why Do We Need Lifting State Up?

Imagine two components.

```
Temperature Input A

Temperature Input B
```

If each component has its own State,

```
Input A = 25°C

Input B = 0°C
```

Changing one input doesn't update the other.

The data becomes inconsistent.

---

# Without Lifting State

```
App

│

├── Celsius Input
│      State = 25
│
└── Fahrenheit Input
       State = 77
```

Each component has its own State.

They don't know about each other.

---

# The Problem

Suppose the user changes Celsius.

```
30°C
```

The Fahrenheit component still shows

```
77°F
```

Wrong!

Both values should stay synchronized.

---

# Solution: Lift the State Up

Move the State to the parent.

```
App

│

├── State
│      Temperature = 30
│
├── Celsius Input
│
└── Fahrenheit Input
```

Now both components receive the same data from the parent.

Everything stays synchronized.

---

# Visual Representation

Without Lifting State

```
App

│

├── Child A
│      State
│
└── Child B
       State
```

Two separate States.

---

With Lifting State

```
App

│

│ State

│

├── Child A

└── Child B
```

One shared State.

---

# Example Without Lifting State

```jsx
function ChildA() {
  const [count, setCount] =
    useState(0);

  return <h2>{count}</h2>;
}

function ChildB() {
  const [count, setCount] =
    useState(0);

  return <h2>{count}</h2>;
}
```

Output

```
Child A

0

Child B

0
```

Click Child A

```
Child A

1

Child B

0
```

The values are different because each component owns its own State.

---

# Example With Lifting State

Parent Component

```jsx
import { useState } from "react";

function App() {
  const [count, setCount] =
    useState(0);

  return (
    <>
      <ChildA
        count={count}
        setCount={setCount}
      />

      <ChildB
        count={count}
      />
    </>
  );
}
```

Child A

```jsx
function ChildA({
  count,
  setCount
}) {
  return (
    <>
      <h2>{count}</h2>

      <button
        onClick={() =>
          setCount(count + 1)
        }
      >
        Increment
      </button>
    </>
  );
}
```

Child B

```jsx
function ChildB({ count }) {
  return <h2>{count}</h2>;
}
```

Output

Initially

```
Child A

0

Child B

0
```

Click Increment

```
Child A

1

Child B

1
```

Both components update together because they share the same State.

---

# Data Flow

```
Parent

↓

State

↓

Props

↓

Child A

↓

Child B
```

State lives in the parent.

Children receive it as Props.

---

# Another Example: Shopping Cart

Imagine this application.

```
Navbar

Product List
```

Navbar displays

```
Cart (2)
```

Product List contains

```
Add to Cart
```

If the Product List keeps the cart count as its own State,

the Navbar won't know about it.

Wrong Design

```
App

│

├── Navbar

└── Product List
       Cart State
```

Correct Design

```
App

│

│ Cart State

│

├── Navbar

└── Product List
```

Now:

- Navbar displays the cart count.
- Product List updates the cart.
- Both stay synchronized.

---

# Another Example: Login Status

```
Header

↓

Welcome Saurabh
```

```
Sidebar

↓

Logout Button
```

Both need to know whether the user is logged in.

Instead of storing login State in both components,

store it in the parent.

```
App

↓

LoggedIn State

↓

Header

↓

Sidebar
```

---

# Another Example: Theme

Suppose your app supports Dark Mode.

Many components need to know the current theme.

```
Navbar

Sidebar

Footer

Dashboard
```

Instead of giving each component its own theme State,

store it once.

```
App

↓

Dark Mode State

↓

Navbar

↓

Sidebar

↓

Dashboard

↓

Footer
```

Now changing the theme updates the whole application.

---

# When Should You Lift State Up?

Lift State Up when:

- Multiple components need the same data.
- Two components must stay synchronized.
- One component updates data and another displays it.
- You want a single source of truth.

---

# Single Source of Truth

One of React's most important principles is the **Single Source of Truth**.

Instead of having multiple copies of the same data,

store it in one place.

```
App

↓

One State

↓

Many Components
```

This avoids inconsistencies and bugs.

---

# Benefits of Lifting State Up

## 1. Shared Data

Multiple components can use the same data.

---

## 2. Keeps UI in Sync

Changing the State updates every component using it.

---

## 3. Easier Debugging

Only one place manages the data.

---

## 4. Avoids Duplicate State

Don't keep multiple copies of the same information.

---

## 5. Follows React's One-Way Data Flow

```
Parent

↓

Props

↓

Children
```

Everything follows a predictable pattern.

---

# Common Mistake

Wrong

```
Navbar

↓

Cart Count State
```

```
Products

↓

Cart Count State
```

Now there are two separate cart counts.

Keeping them synchronized becomes difficult.

---

Correct

```
App

↓

Cart Count State

↓

Navbar

↓

Products
```

One State.

Everyone shares it.

---

# Interview Questions

## 1. What is Lifting State Up?

**Answer:**

Lifting State Up is the process of moving State from child components to their nearest common parent so that multiple components can share and stay synchronized with the same data.

---

## 2. Why Do We Lift State Up?

**Answer:**

We lift State Up when multiple components need access to the same data or when they must remain synchronized. It creates a single source of truth and avoids duplicate State.

---

## 3. What Is the Single Source of Truth?

**Answer:**

The Single Source of Truth means storing a piece of shared data in one place (usually the nearest common parent) and passing it down to child components through Props.

---

## 4. Does Lifting State Up Mean Every State Should Be Moved to the Parent?

**Answer:**

No.

Keep State as close as possible to where it is used.

Only lift it when multiple components need to share or coordinate the same data.

---

# Easy Memory Trick

Imagine a family calendar.

Wrong

```
Dad has one calendar.

Mom has another.

Kids have another.
```

Birthdays get out of sync.

Correct

```
One Family Calendar

↓

Dad

↓

Mom

↓

Kids
```

Everyone reads from the same calendar.

React State works the same way.

Shared data should have **one owner**.

---

# Summary

- Lifting State Up means moving shared State to the nearest common parent component.
- It allows multiple child components to access the same data through Props.
- It helps keep the UI synchronized and consistent.
- It follows React's One-Way Data Flow by keeping State in the parent and passing it down.
- It supports the principle of a **Single Source of Truth**, reducing bugs caused by duplicate State.
- Lift State only when multiple components need the same data; otherwise, keep State local to the component that owns it.