# 18. What is Conditional Rendering?

## Simple Definition

**Conditional Rendering** means **displaying different UI (User Interface) based on a condition**.

In simple words,

> React decides **what to show** depending on whether a condition is **true** or **false**.

It works just like JavaScript's `if`, `else`, and ternary (`? :`) operators.

---

# Real-Life Analogy: Traffic Signal

Imagine you're driving.

If the traffic light is **Green**, you go.

```
Green

↓

Drive
```

If the traffic light is **Red**, you stop.

```
Red

↓

Stop
```

The action changes based on a condition.

React works the same way.

---

# Another Analogy: Umbrella

Imagine you're leaving your house.

```
Is it raining?

↓

Yes

↓

Take Umbrella
```

```
Is it raining?

↓

No

↓

Don't Take Umbrella
```

The decision depends on a condition.

This is exactly what Conditional Rendering does.

---

# Why Do We Need Conditional Rendering?

Not everything on a webpage should always be visible.

Examples:

- Show **Login** button if the user is logged out.
- Show **Logout** button if the user is logged in.
- Show **Loading...** while data is being fetched.
- Show **No Products Found** when the list is empty.
- Show **Admin Panel** only for administrators.

Instead of creating multiple pages, React shows or hides UI based on conditions.

---

# Basic Example Using if

```jsx
function App() {
  const isLoggedIn = true;

  if (isLoggedIn) {
    return <h1>Welcome Back!</h1>;
  }

  return <h1>Please Login</h1>;
}
```

Output

```
Welcome Back!
```

If `isLoggedIn` becomes `false`:

```
Please Login
```

---

# Visual Representation

```
isLoggedIn?

↓

Yes

↓

Welcome Back

```

```
isLoggedIn?

↓

No

↓

Please Login
```

---

# Example: Login Button

```jsx
function App() {
  const isLoggedIn = false;

  return (
    <>
      {isLoggedIn
        ? <button>Logout</button>
        : <button>Login</button>}
    </>
  );
}
```

Output

If `true`

```
Logout
```

If `false`

```
Login
```

---

# Understanding the Ternary Operator

The ternary operator is the most commonly used way to perform Conditional Rendering.

Syntax

```jsx
condition
  ? "True Part"
  : "False Part"
```

Example

```jsx
const age = 20;

age >= 18
  ? "Adult"
  : "Minor";
```

Output

```
Adult
```

React Example

```jsx
{
  age >= 18
    ? <h1>Adult</h1>
    : <h1>Minor</h1>
}
```

---

# Example: Movie Ticket

```
Age >= 18

↓

Adult Ticket
```

```
Age < 18

↓

Child Ticket
```

React

```jsx
function Ticket({ age }) {
  return (
    <>
      {age >= 18
        ? <h2>Adult Ticket</h2>
        : <h2>Child Ticket</h2>}
    </>
  );
}
```

---

# Conditional Rendering Using &&

Sometimes you only want to display something when a condition is **true**.

Example

```jsx
function App() {
  const isAdmin = true;

  return (
    <>
      {isAdmin &&
        <button>Delete User</button>}
    </>
  );
}
```

Output

```
Delete User
```

If `isAdmin` is `false`

Nothing is displayed.

---

# When to Use &&

Show something only if a condition is true.

Examples:

- Admin Panel
- Premium Badge
- New Notification
- Discount Label

Example

```jsx
{hasDiscount &&
    <p>50% OFF</p>}
```

---

# Example: Shopping Cart

Suppose a cart has items.

```jsx
const items = 5;
```

Show the cart count.

```jsx
{
  items > 0 &&
  <h2>Cart ({items})</h2>
}
```

Output

```
Cart (5)
```

If

```jsx
items = 0;
```

Nothing is shown.

---

# Example: Loading Screen

Initially

```
Loading...
```

Once data is loaded

```
Product List
```

```jsx
function App() {
  const loading = true;

  return (
    <>
      {loading
        ? <h2>Loading...</h2>
        : <h2>Products</h2>}
    </>
  );
}
```

Output

Initially

```
Loading...
```

Later

```
Products
```

---

# Example: Weather App

```
Temperature > 30

↓

Hot Weather
```

Otherwise

```
Pleasant Weather
```

React

```jsx
function Weather({ temp }) {
  return (
    <>
      {temp > 30
        ? <h2>Hot Weather</h2>
        : <h2>Pleasant Weather</h2>}
    </>
  );
}
```

---

# Example: Student Result

```jsx
function Result({ marks }) {
  return (
    <>
      {marks >= 40
        ? <h2>Pass</h2>
        : <h2>Fail</h2>}
    </>
  );
}
```

Output

Marks = 70

```
Pass
```

Marks = 25

```
Fail
```

---

# Using Variables

Instead of writing logic inside JSX.

```jsx
function App() {

  const isLoggedIn = true;

  let message;

  if (isLoggedIn)
    message = "Welcome";

  else
    message = "Login";

  return <h1>{message}</h1>;
}
```

This approach is useful when the logic becomes more complex.

---

# Returning Nothing

Sometimes you don't want to render anything.

```jsx
function Message({ show }) {

  if (!show)
    return null;

  return <h2>Hello</h2>;
}
```

If

```jsx
show = false;
```

Nothing appears on the screen.

---

# Which Method Should You Use?

| Method | Best For |
|----------|----------|
| `if...else` | Large or complex conditions |
| Ternary (`? :`) | Choosing between two UI elements |
| `&&` | Showing something only when the condition is true |
| `return null` | Rendering nothing |

---

# Real-Life Examples

## Netflix

```
Subscribed?

↓

Yes

↓

Watch Now
```

```
No

↓

Subscribe
```

---

## Amazon

```
Items Available?

↓

Yes

↓

Show Products
```

```
No

↓

Out of Stock
```

---

## Gmail

```
Unread Emails?

↓

Yes

↓

Show Notification
```

```
No

↓

Hide Notification
```

---

## Banking App

```
Logged In?

↓

Yes

↓

Show Account Balance
```

```
No

↓

Show Login Screen
```

---

# Common Interview Questions

## 1. What is Conditional Rendering?

**Answer:**

Conditional Rendering is the process of displaying different UI elements based on a condition. React uses JavaScript conditions such as `if`, `else`, the ternary operator, and `&&` to decide what should be rendered.

---

## 2. Which Operators Are Commonly Used?

- `if...else`
- Ternary (`? :`)
- Logical AND (`&&`)
- `return null`

---

## 3. When Should We Use `&&`?

Use `&&` when you want to render something **only if a condition is true**, without providing an alternative UI.

Example:

```jsx
{isAdmin && <button>Delete</button>}
```

---

## 4. What Does `return null` Mean?

It tells React to render nothing.

The component still exists, but no UI is displayed.

---

# Easy Memory Trick

Imagine entering a shopping mall.

```
Are you a Member?

↓

Yes

↓

VIP Lounge
```

```
No

↓

Regular Entrance
```

The mall changes what you see based on a condition.

React does exactly the same thing with components.

---

# Summary

- Conditional Rendering means showing different UI based on a condition.
- React uses normal JavaScript conditions for rendering.
- `if...else` is suitable for complex logic.
- The ternary operator (`? :`) is ideal for choosing between two UI elements.
- The `&&` operator is useful when rendering something only if a condition is true.
- Returning `null` tells React to render nothing.
- Conditional Rendering helps build dynamic and interactive applications such as login pages, shopping carts, dashboards, loading screens, and admin panels.