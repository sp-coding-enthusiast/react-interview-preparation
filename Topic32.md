# 44. What is `useReducer`?

## Simple Definition

`useReducer` is a **React Hook** used to manage **complex state**.

Instead of updating the state directly like `useState`, it updates the state by **dispatching actions** to a **reducer function**.

In simple words,

> **`useReducer` is like a manager who receives requests (actions), decides what should happen, and updates the state accordingly.**

---

# Real-Life Analogy: Bank Counter

Imagine you go to a bank.

You don't directly change your account balance.

Instead, you submit a request.

```
Deposit ₹1000

or

Withdraw ₹500
```

The bank manager checks the request.

```
Request

↓

Verify

↓

Update Balance

↓

Return New Balance
```

You never modify the balance yourself.

`useReducer` works exactly like this.

---

# Why Do We Need `useReducer`?

For small state values like:

```jsx
const [count, setCount] = useState(0);
```

`useState` is perfect.

But imagine managing:

- User profile
- Shopping cart
- Login form
- Employee details
- Order management
- Dashboard with multiple values

The component can end up with many `useState` calls.

Example:

```jsx
const [name, setName] = useState("");
const [age, setAge] = useState(25);
const [city, setCity] = useState("");
const [salary, setSalary] = useState(50000);
const [department, setDepartment] = useState("");
```

As the application grows,

this becomes harder to maintain.

`useReducer` helps organize all related state logic in one place.

---

# Basic Syntax

```jsx
const [state, dispatch] =
  useReducer(
    reducer,
    initialState
  );
```

It returns:

- Current state
- `dispatch()` function

---

# Visual Representation

```
User Action

↓

dispatch()

↓

Reducer

↓

New State

↓

UI Updates
```

---

# The Three Parts of `useReducer`

## 1. Initial State

```jsx
const initialState = {

  count: 0

};
```

---

## 2. Reducer Function

```jsx
function reducer(
  state,
  action
) {

  switch (action.type) {

    case "increment":

      return {
        count: state.count + 1
      };

    case "decrement":

      return {
        count: state.count - 1
      };

    default:

      return state;

  }

}
```

The reducer decides how the state should change.

---

## 3. `useReducer`

```jsx
const [state, dispatch] =
  useReducer(
    reducer,
    initialState
  );
```

---

# Complete Example

```jsx
import { useReducer } from "react";

const initialState = {

  count: 0

};

function reducer(
  state,
  action
) {

  switch (action.type) {

    case "increment":

      return {
        count: state.count + 1
      };

    case "decrement":

      return {
        count: state.count - 1
      };

    default:

      return state;

  }

}

function App() {

  const [state, dispatch] =
    useReducer(
      reducer,
      initialState
    );

  return (

    <>
      <h1>{state.count}</h1>

      <button
        onClick={() =>
          dispatch({
            type: "increment"
          })
        }
      >
        +
      </button>

      <button
        onClick={() =>
          dispatch({
            type: "decrement"
          })
        }
      >
        -
      </button>
    </>

  );

}
```

---

# Flow Diagram

```
Click +

↓

dispatch({
  type: "increment"
})

↓

Reducer Executes

↓

New State

↓

React Re-renders

↓

Updated UI
```

---

# What is an Action?

An **action** is simply an object that describes **what happened**.

Example:

```jsx
{
  type: "increment"
}
```

Another example:

```jsx
{
  type: "login"
}
```

Or with extra data:

```jsx
{
  type: "addProduct",
  payload: product
}
```

The reducer reads the action and decides how to update the state.

---

# What is a Reducer?

A reducer is a **pure function**.

It receives:

```jsx
(state, action)
```

and returns

```jsx
newState
```

It should not:

- Call APIs
- Modify the existing state directly
- Perform side effects

It should only calculate and return the next state.

---

# Real-Life Analogy: Restaurant

Imagine a restaurant.

Customer

```
"I want Pizza"
```

↓

Waiter

```
Takes Order
```

↓

Kitchen

```
Prepares Pizza
```

↓

Customer Receives Food

In React:

```
dispatch()

↓

Reducer

↓

Updated State

↓

UI Updates
```

---

# Example: Shopping Cart

Initial state

```jsx
{

  items: [],

  total: 0

}
```

Action

```jsx
dispatch({

  type: "ADD_ITEM",

  payload: product

});
```

Reducer

```jsx
case "ADD_ITEM":

  return {

    ...state,

    items: [
      ...state.items,
      action.payload
    ]

  };
```

---

# Benefits of `useReducer`

- Keeps related state logic together.
- Makes state updates predictable.
- Easier to maintain large components.
- Easy to test because reducers are pure functions.
- Works well with Context API for global state management.

---

# Summary

- `useReducer` is a Hook for managing complex state.
- It updates state through actions and a reducer function.
- It is useful when many state values are related or when state transitions are complex.

---

# 45. When Should You Use `useReducer` Over `useState`?

## Simple Answer

Use:

- **`useState`** for **simple state**.
- **`useReducer`** for **complex state**.

---

# Real-Life Analogy: Bicycle vs Car

Imagine traveling.

Going to a nearby shop?

```
Use Bicycle
```

Traveling with five people and luggage?

```
Use Car
```

Both help you travel,

but one is better for more complex situations.

Similarly,

`useState` is simple.

`useReducer` is better for complicated state logic.

---

# Use `useState` For

- Counter
- Theme
- Toggle button
- Search input
- Simple forms
- Boolean values

Example

```jsx
const [count, setCount] =
  useState(0);
```

Simple and easy.

---

# Use `useReducer` For

- Shopping carts
- Login flows
- Dashboards
- Multi-step forms
- Complex objects
- State with many related updates

---

# Example: Simple Counter

```jsx
const [count, setCount] =
  useState(0);
```

Easy.

No need for `useReducer`.

---

# Example: Employee Form

Imagine storing:

```jsx
{

  name,

  email,

  age,

  salary,

  department,

  address,

  manager,

  projects

}
```

Updating different fields with multiple `setState` calls can become difficult to manage.

A reducer keeps all update logic in one place.

---

# Example: Shopping Cart

Actions

```
Add Item

Remove Item

Increase Quantity

Decrease Quantity

Clear Cart

Apply Coupon
```

Instead of many setters,

one reducer handles everything.

---

# Example: Login State

Instead of

```jsx
const [loading, setLoading] =
  useState(false);

const [user, setUser] =
  useState(null);

const [error, setError] =
  useState("");
```

A reducer can manage all related state transitions together.

---

# Visual Comparison

## `useState`

```
Button Click

↓

setState()

↓

React Updates
```

Simple.

---

## `useReducer`

```
Button Click

↓

dispatch()

↓

Reducer

↓

New State

↓

React Updates
```

More organized for complex scenarios.

---

# Side-by-Side Comparison

| Feature | `useState` | `useReducer` |
|----------|------------|--------------|
| Easy to learn | ✅ Yes | Slightly more advanced |
| Simple state | ✅ Excellent | Possible, but often unnecessary |
| Complex state | Limited | ✅ Excellent |
| Many related state updates | Can become harder to manage | ✅ Centralized in reducer |
| Predictable state transitions | Good | Excellent |
| Works with Context API | Yes | ✅ Very common |

---

# Real-World Examples

## Counter

```jsx
const [count, setCount] =
  useState(0);
```

Best choice.

---

## Shopping Cart

```jsx
dispatch({

  type: "ADD_ITEM",

  payload: product

});
```

Better choice.

---

## Employee Management

```
Add Employee

Update Employee

Delete Employee

Transfer Department
```

Many actions.

`useReducer` keeps the logic organized.

---

## Multi-Step Form

```
Step 1

↓

Step 2

↓

Step 3

↓

Submit
```

A reducer can manage the current step, entered data, validation status, and submission state in one place.

---

# Common Mistakes

## Using `useReducer` for Everything

Not every component needs it.

This is unnecessary:

```jsx
const [state, dispatch] =
  useReducer(
    reducer,
    { count: 0 }
  );
```

for a simple counter in many cases.

`useState` is simpler.

---

## Mutating State Inside the Reducer

Wrong

```jsx
state.count++;

return state;
```

This mutates the existing state.

Correct

```jsx
return {

  ...state,

  count: state.count + 1

};
```

Always return a **new state object**.

---

## Performing Side Effects in a Reducer

Reducers should stay pure.

Don't:

- Call APIs
- Start timers
- Modify the DOM

Do those tasks in event handlers or `useEffect`.

---

# Common Interview Questions

## 1. What is `useReducer`?

**Answer:**

`useReducer` is a React Hook for managing complex state. It updates state by dispatching actions to a reducer function, which returns the next state.

---

## 2. Why Use `useReducer` Instead of `useState`?

**Answer:**

Use `useReducer` when state logic is complex, involves multiple related values, or has many different update actions.

---

## 3. What Does `dispatch()` Do?

**Answer:**

`dispatch()` sends an action object to the reducer. The reducer processes the action and returns the updated state.

---

## 4. What Is an Action?

**Answer:**

An action is an object describing what happened. It usually contains a `type` property and may include additional data in a `payload`.

Example:

```jsx
{
  type: "ADD_ITEM",
  payload: product
}
```

---

## 5. What Is a Reducer Function?

**Answer:**

A reducer is a pure function that receives the current state and an action, then returns the next state without mutating the existing state.

---

## 6. Can `useReducer` Replace `useState`?

**Answer:**

Yes, technically.

However, `useState` is usually the better choice for simple state because it is easier to read and write.

---

# Easy Memory Trick

Imagine a company.

### `useState`

You directly switch a light on.

```
Press Switch

↓

Light On
```

Simple.

---

### `useReducer`

You submit a request.

```
Employee

↓

Manager

↓

Approve Request

↓

Work Completed
```

The manager (reducer) decides how the state changes.

---

# Summary

- `useReducer` is designed for managing complex state logic.
- It uses **state**, **actions**, **dispatch**, and a **reducer function**.
- The reducer is a pure function that calculates and returns the next state.
- `useState` is ideal for simple values like counters, toggles, and text inputs.
- `useReducer` is better for complex forms, shopping carts, dashboards, login flows, and applications with many related state transitions.
- A simple rule to remember:
  - **Simple state → `useState`**
  - **Complex, related state with multiple actions → `useReducer`**