# 46. What is `useContext`?

## Simple Definition

`useContext` is a **React Hook** that allows a component to **access data from a Context without passing it through every intermediate component as props**.

In simple words,

> **`useContext` lets components share data directly, without passing props through every level of the component tree.**

It solves a common problem called **Prop Drilling**.

---

# What is Prop Drilling?

Imagine you have this component structure:

```
App

↓

Header

↓

Navbar

↓

Profile

↓

Avatar
```

Suppose **App** has the logged-in user's name.

```
App

↓

User = "Saurabh"
```

But only **Avatar** needs it.

Without Context, you have to pass the data through every component.

```jsx
<App user={user}>

↓

<Header user={user}>

↓

<Navbar user={user}>

↓

<Profile user={user}>

↓

<Avatar user={user}>
```

Notice that:

- `Header` doesn't need `user`
- `Navbar` doesn't need `user`
- `Profile` doesn't need `user`

They are only forwarding it.

This is called **Prop Drilling**.

---

# Real-Life Analogy: Family Message

Imagine a father wants to tell his grandson:

> "Dinner is ready."

Without Context:

```
Father

↓

Mother

↓

Son

↓

Grandson
```

Everyone passes the same message.

With Context:

```
Father

↓

Family Group

↓

Grandson Reads Message Directly
```

The middle people don't have to relay it.

That's exactly what `useContext` does.

---

# Why Do We Need `useContext`?

Many applications have data that **many components need**.

Examples:

- Logged-in user
- Theme (Light/Dark)
- Language
- Authentication status
- Shopping cart
- Application settings

Instead of passing these values through every component,

we can store them in a Context.

Any component can read them using `useContext`.

---

# Visual Representation

Without Context

```
App

↓

Header

↓

Navbar

↓

Profile

↓

Avatar
```

Every component passes props.

---

With Context

```
Context

↙   ↓   ↘

Header

Navbar

Avatar
```

Every component can access the shared data directly.

---

# How `useContext` Works

React Context has **three parts**.

## Step 1: Create a Context

```jsx
import { createContext } from "react";

const UserContext =
  createContext();
```

This creates a Context object.

---

## Step 2: Provide the Value

```jsx
<UserContext.Provider
  value="Saurabh"
>

  <App />

</UserContext.Provider>
```

The **Provider** makes the value available to all components inside it.

---

## Step 3: Consume the Value

```jsx
import { useContext } from "react";

const user =
  useContext(UserContext);
```

Now the component can access the value directly.

---

# Complete Example

## Create Context

```jsx
import { createContext } from "react";

export const UserContext =
  createContext();
```

---

## Provide Value

```jsx
<UserContext.Provider
  value="Saurabh"
>

  <Profile />

</UserContext.Provider>
```

---

## Read Value

```jsx
import { useContext } from "react";
import { UserContext } from "./UserContext";

function Profile() {

  const user =
    useContext(UserContext);

  return <h1>{user}</h1>;

}
```

Output

```
Saurabh
```

---

# Visual Flow

```
Provider

↓

Stores Value

↓

Any Child Component

↓

useContext()

↓

Gets Value
```

---

# Real-Life Analogy: Wi-Fi

Imagine a Wi-Fi router.

```
Router

↓

Internet

↓

Laptop

↓

Phone

↓

Tablet
```

Every device connects directly to the router.

The laptop doesn't get the internet from the phone.

Similarly,

every component reads the Context directly.

---

# Common Use Cases

## 1. Logged-In User

```jsx
<UserContext.Provider
  value={user}
>
```

Every component can access the current user.

---

## 2. Theme

```jsx
<ThemeContext.Provider
  value="dark"
>
```

Every component knows the current theme.

---

## 3. Language

```jsx
<LanguageContext.Provider
  value="English"
>
```

Every component can display the correct language.

---

## 4. Shopping Cart

```jsx
<CartContext.Provider
  value={cart}
>
```

The cart icon, checkout page, and order summary can all read the same cart data.

---

# Multiple Contexts

An application can have many Contexts.

```jsx
<UserContext.Provider
  value={user}
>

  <ThemeContext.Provider
    value="dark"
  >

    <LanguageContext.Provider
      value="English"
    >

      <App />

    </LanguageContext.Provider>

  </ThemeContext.Provider>

</UserContext.Provider>
```

Components can read one or more contexts as needed.

---

# What Happens When Context Changes?

Suppose:

```
Theme

↓

Light
```

User changes it.

```
Dark
```

React updates the Context value.

Components that read that Context receive the new value and re-render to reflect the change.

---

# `useContext` vs Props

Without Context

```
App

↓

Header

↓

Navbar

↓

Profile

↓

Avatar
```

Many components pass the same prop.

---

With Context

```
Context

↓

Avatar
```

The component that needs the data reads it directly.

---

# Side-by-Side Comparison

| Feature | Props | `useContext` |
|----------|-------|--------------|
| Pass data to child | ✅ Yes | ✅ Yes (through Provider) |
| Avoid Prop Drilling | ❌ No | ✅ Yes |
| Good for local component communication | ✅ Yes | Possible, but often unnecessary |
| Good for app-wide shared data | Limited | ✅ Excellent |
| Easy to understand | ✅ Yes | Requires learning Context |

---

# Should You Use `useContext` Everywhere?

No.

Use **Props** when:

- Data is only needed by a child or two.
- The component hierarchy is shallow.

Use **Context** when:

- Many components need the same data.
- Passing props through multiple levels becomes difficult.

---

# Common Mistakes

## Using Context for Everything

Not every piece of data belongs in Context.

Example:

```jsx
const [count, setCount] =
  useState(0);
```

If only one component uses `count`,

there is no need for Context.

---

## Forgetting the Provider

Wrong

```jsx
const user =
  useContext(UserContext);
```

without wrapping the component tree in:

```jsx
<UserContext.Provider>
```

The component receives the Context's default value (or `undefined` if no default was provided), which may not be what you expect.

---

## Thinking Context Replaces State

Context shares data.

It does **not** replace state management.

Usually,

the state is stored in a component using `useState` or `useReducer`,

and that state is then shared through Context.

---

# `useContext` + `useReducer`

A common pattern in medium and large React applications is:

```
useReducer

↓

Stores State

↓

Context

↓

Shares State

↓

Components Read State
```

This combination avoids prop drilling while keeping state updates organized.

---

# Real-World Example: E-commerce

```
Shopping Cart

↓

Product Page

↓

Header

↓

Checkout

↓

Payment
```

Instead of passing the cart through every component,

store it in `CartContext`.

Any component can read it with:

```jsx
const cart =
  useContext(CartContext);
```

---

# Common Interview Questions

## 1. What is `useContext`?

**Answer:**

`useContext` is a React Hook that allows a component to read values from a Context without passing them through props at every level.

---

## 2. Why Do We Use `useContext`?

**Answer:**

We use `useContext` to avoid prop drilling and to share common data such as themes, logged-in users, languages, or shopping carts across multiple components.

---

## 3. What Problem Does `useContext` Solve?

**Answer:**

It solves **Prop Drilling**, where props have to be passed through intermediate components that don't actually use the data.

---

## 4. Can `useContext` Replace Props?

**Answer:**

No.

Props are still the best choice for communication between closely related components.

`useContext` is mainly for sharing data across many components.

---

## 5. Can `useContext` Replace `useState`?

**Answer:**

No.

`useState` manages state.

`useContext` shares data.

They are often used together.

---

## 6. Can Updating a Context Value Cause Components to Re-render?

**Answer:**

Yes.

When the value provided by a Context changes, React re-renders the components that consume that Context so they receive the updated value.

---

# Easy Memory Trick

Imagine a water tank.

```
Water Tank

↓

Kitchen

↓

Bathroom

↓

Garden
```

Every room gets water directly from the tank.

No room has to pass water to another room.

`useContext` works the same way.

```
Context

↓

Any Component

↓

Reads Shared Data Directly
```

---

# Summary

- `useContext` is a React Hook used to access values stored in a Context.
- It helps avoid **Prop Drilling** by allowing components to read shared data directly.
- A Context consists of three parts:
  1. Create the Context.
  2. Provide the value with a `Provider`.
  3. Read the value using `useContext`.
- It is commonly used for themes, authentication, languages, shopping carts, and application-wide settings.
- `useContext` does not replace `useState`; instead, it is often combined with `useState` or `useReducer` to share state across the component tree.