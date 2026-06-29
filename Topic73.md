# 108. Why Redux?

> **Interview Note ⭐**
>
> This is one of the most frequently asked React interview questions.
>
> Many developers answer:
>
> > "Redux is used for state management."
>
> While this is true, it is **not a complete answer**.
>
> A better answer is:
>
> > "Redux provides a centralized, predictable, and scalable way to manage shared application state across multiple components."
>
> The interviewer wants to know **what problems Redux solves**, not just what it is.

---

# Simple Definition

**Redux** is a **state management library** that stores your application's shared data in a **single centralized store**.

Instead of each component maintaining its own copy of shared data, Redux keeps one source of truth.

In simple words,

> **Redux is like a central warehouse where all important application data is stored, and every component gets data from the same place.**

---

# Why Do We Need Redux?

Imagine an e-commerce website.

You have these components:

```
Navbar

Product List

Shopping Cart

Checkout

Order Summary

Wishlist

User Profile
```

All of them need access to:

- Logged-in user
- Shopping cart
- Theme
- Language
- Notifications

Without Redux,

each component may manage or pass this data separately.

---

# Problem Without Redux

Suppose a user adds a product to the cart.

```
Product Page

↓

Cart Updated
```

Now these components also need updating:

```
Navbar

↓

Cart Count

----------------

Checkout

↓

Updated Total

----------------

Order Summary

↓

Updated Items

----------------

Mini Cart

↓

Updated Products
```

Passing the cart through many components becomes difficult.

This is called **Prop Drilling**.

---

# What is Prop Drilling?

Imagine this component hierarchy:

```
App

↓

Home

↓

Products

↓

Product

↓

AddToCartButton
```

The cart exists in `App`.

But `AddToCartButton` needs it.

Without Redux,

you pass props through every level.

```
App

↓

Home

↓

Products

↓

Product

↓

Button
```

Even if intermediate components don't use the data.

---

# Real-Life Analogy

Imagine a school.

Without Redux

A teacher wants student information.

```
Teacher

↓

Vice Principal

↓

Principal

↓

Office

↓

Records
```

Lots of unnecessary communication.

---

With Redux

Everyone accesses the central database.

```
Teacher

↓

School Database
```

Much simpler.

---

# Redux Solution

Redux creates a **single store**.

```
Store

↓

User

↓

Cart

↓

Theme

↓

Language

↓

Products

↓

Orders
```

Every component reads from the same store.

---

# Visual Architecture

```
            Redux Store

        ┌───────────────┐

User --->│               │<--- Navbar

Cart --->│               │<--- Checkout

Theme -->│               │<--- Product Page

Orders ->│               │<--- Wishlist

        └───────────────┘
```

One source of truth.

---

# Why Not Just React State?

React's `useState()` is perfect for **local state**.

Example

```jsx
const [isOpen, setIsOpen] =

useState(false);
```

This state belongs only to one component.

---

But imagine

```
Cart

↓

Needed By

10 Components
```

Managing it with only `useState()` becomes difficult.

---

# Example Without Redux

```
App

↓

Cart

↓

Navbar

↓

Products

↓

Product

↓

Button
```

Props must travel through multiple levels.

---

# Example With Redux

```
Navbar

↓

Redux Store

----------------

Product

↓

Redux Store

----------------

Checkout

↓

Redux Store
```

No prop drilling.

---

# Core Redux Concepts

Redux revolves around three main ideas:

```
Store

↓

Actions

↓

Reducers
```

We'll learn each in detail later.

For now, understand:

### Store

Holds application state.

---

### Action

Describes **what happened**.

Example

```
ADD_TO_CART
```

---

### Reducer

Updates the state based on the action.

---

# How Redux Works

Imagine clicking:

```
Add To Cart
```

Flow

```
Button Click

↓

Dispatch Action

↓

Redux Store

↓

Reducer

↓

New State

↓

React Updates UI
```

---

# Step-by-Step Flow

```
User Clicks

↓

Action Created

↓

Reducer Executes

↓

Store Updated

↓

React Re-renders

↓

UI Updated
```

---

# Example

User clicks

```
Add Laptop
```

Action

```jsx
dispatch({

type: "ADD_TO_CART",

payload: laptop

});
```

Reducer

```jsx
cart.push(laptop);
```

Store

```
Cart

↓

1 Item
```

UI

```
Navbar

↓

Cart (1)
```

Automatically updated.

---

# Problems Redux Solves

---

## 1. Prop Drilling

Without Redux

```
App

↓

A

↓

B

↓

C

↓

D
```

Pass props through every level.

---

With Redux

```
A

↓

Store

----------------

D

↓

Store
```

---

## 2. Shared State

Many components can use the same state.

Example

```
Theme

↓

Navbar

↓

Sidebar

↓

Footer

↓

Settings
```

One source of truth.

---

## 3. Predictable Updates

State changes happen through actions.

```
Action

↓

Reducer

↓

Store
```

Easy to understand.

---

## 4. Easier Debugging

Every change follows the same flow.

```
Action

↓

Reducer

↓

State
```

No hidden updates.

---

## 5. Time Travel Debugging

With **Redux DevTools**, developers can:

- View dispatched actions
- Inspect state changes
- Replay actions
- Jump back to previous states

This makes debugging much easier.

---

## 6. Better Scalability

Large applications often contain:

- Authentication
- Shopping carts
- Dashboards
- Notifications
- User settings

Redux keeps these organized in one place.

---

# Real-World Example

Imagine Amazon.

Shared state includes:

```
User

Cart

Orders

Theme

Wishlist

Address

Coupons
```

Every page needs some of this data.

Instead of duplicating it,

Redux stores it once.

---

# When Should You Use Redux?

Good candidates include:

- Large applications
- Shared state across many components
- Complex workflows
- Enterprise dashboards
- E-commerce sites
- Banking apps
- Admin portals

---

# When Should You NOT Use Redux?

Avoid Redux for:

- Small projects
- Local component state
- Simple forms
- Temporary UI state
- Modal open/close state
- Single-page demos

React's built-in Hooks are often enough.

---

# React Context vs Redux

| React Context | Redux |
|---------------|--------|
| Shares data | Manages complex shared state |
| Good for small/medium apps | Better for large applications |
| Basic state management | Predictable state updates with actions and reducers |
| No built-in DevTools like Redux | Excellent DevTools support |
| Simpler setup | More structure and scalability |

> **Note:** Context itself doesn't inherently cause excessive re-renders. Performance depends on how providers are structured and how often the provided value changes. Redux offers fine-grained subscriptions that can make large applications easier to optimize.

---

# Modern Redux

Today, developers typically use **Redux Toolkit (RTK)** instead of writing classic Redux boilerplate.

Redux Toolkit:

- Reduces boilerplate
- Simplifies store setup
- Uses Immer for immutable updates
- Includes recommended best practices
- Is the official approach recommended by the Redux team

---

# Common Interview Questions

## 1. Why Do We Need Redux?

**Answer:**

Redux provides a centralized and predictable way to manage shared application state. It helps avoid prop drilling, simplifies state sharing across components, and scales well for large applications.

---

## 2. What Problems Does Redux Solve?

**Answer:**

Redux solves:

- Prop drilling
- Shared state management
- Predictable state updates
- Easier debugging
- Application scalability
- State consistency

---

## 3. Can Redux Replace `useState()`?

**Answer:**

No.

`useState()` is ideal for local component state.

Redux is designed for shared application state that multiple components need to access or update.

---

## 4. Is Redux Always Needed?

**Answer:**

No.

Small applications often work perfectly with React's built-in Hooks (`useState`, `useReducer`, and `useContext`).

Redux becomes valuable as shared state grows more complex.

---

## 5. Why Is Redux Predictable?

**Answer:**

Because state changes happen through a well-defined flow:

```
Action

↓

Reducer

↓

Store
```

This makes updates consistent and easier to debug.

---

## 6. Why Is Redux Toolkit Preferred?

**Answer:**

Redux Toolkit is the official recommended way to use Redux because it simplifies configuration, reduces boilerplate, encourages best practices, and provides utilities for writing immutable update logic more easily.

---

# Senior Interview Tip ⭐

If you're asked:

> **"Why would you choose Redux over React Context?"**

A strong answer is:

> "I use React Context for relatively stable global values such as themes or authentication status. For large applications with complex shared state, frequent updates, and advanced debugging requirements, I prefer Redux Toolkit because it provides predictable state management, better tooling, scalable architecture, and efficient subscriptions."

This demonstrates that you understand the strengths and trade-offs of each approach.

---

# Easy Memory Trick

Imagine a city.

Without Redux

```
Every House

Stores

Its Own Water
```

Difficult to manage.

---

With Redux

```
Central Water Tank

↓

Every House

Uses It
```

One reliable source.

Think:

- **Redux Store = Water Tank**
- **Components = Houses**
- **Actions = Water Requests**
- **Reducers = Water Distribution Rules**

---

# Summary

## Why Redux?

Redux provides a **centralized, predictable, and scalable** way to manage shared application state.

It solves problems such as:

- Prop drilling
- Shared state across many components
- Predictable state updates
- Easier debugging
- Better scalability
- State consistency

## When to Use Redux

Use Redux for:

- Large applications
- Enterprise dashboards
- E-commerce
- Banking systems
- Complex shared state

Avoid Redux for:

- Small projects
- Local UI state
- Simple forms
- Temporary component state

## Modern Recommendation

Use **Redux Toolkit (RTK)** instead of classic Redux.

It is simpler, reduces boilerplate, and follows the official best practices recommended by the Redux team.