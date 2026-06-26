# 27. What is componentDidMount()?

## Simple Definition

`componentDidMount()` is a **lifecycle method** in **Class Components** that runs **once after the component has been created and displayed on the screen**.

In simple words,

> It is called **immediately after the component is mounted (added to the DOM).**

Think of it as:

```
Component Created

↓

Displayed

↓

componentDidMount() Runs
```

---

# Real-Life Analogy: Opening a Shop

Imagine you own a shop.

Morning routine:

```
Open Shop

↓

Turn On Lights

↓

Start Computer

↓

Open Cash Counter

↓

Ready for Customers
```

These tasks happen **only once** when the shop opens.

Similarly,

`componentDidMount()` runs only once after a component appears on the screen.

---

# Why Do We Use componentDidMount()?

This is the ideal place to:

- Call APIs
- Load data
- Start timers
- Subscribe to events
- Open WebSocket connections
- Initialize third-party libraries

These actions should happen only once when the component is first displayed.

---

# Example

```jsx
class App extends React.Component {

  componentDidMount() {
    console.log("Component Mounted");
  }

  render() {
    return <h1>Hello React</h1>;
  }
}
```

Output

```
Hello React

Component Mounted
```

The message appears only once.

---

# API Call Example

```jsx
class Users extends React.Component {

  componentDidMount() {
    fetch("/api/users")
      .then(res => res.json())
      .then(data =>
        console.log(data)
      );
  }

  render() {
    return <h1>Users</h1>;
  }
}
```

Flow

```
Component Appears

↓

API Called

↓

Data Received

↓

Display Data
```

---

# Modern React Equivalent

Functional Components use `useEffect`.

```jsx
useEffect(() => {
  console.log("Mounted");
}, []);
```

The empty dependency array (`[]`) means the effect runs only once after the component mounts.

---

# Summary

- Runs only once.
- Executes after the component is added to the DOM.
- Used for API calls, timers, subscriptions, and initialization.
- Equivalent Hook:

```jsx
useEffect(() => {}, []);
```

---

# 28. What is componentDidUpdate()?

## Simple Definition

`componentDidUpdate()` is a **lifecycle method** that runs **after a component updates**.

A component updates when:

- State changes.
- Props change.

Think of it as:

```
State Changes

↓

Component Updates

↓

componentDidUpdate() Runs
```

---

# Real-Life Analogy: Scoreboard

Imagine a cricket scoreboard.

Initially

```
Score

100
```

A batsman hits a four.

```
104
```

The scoreboard updates.

Every time the score changes,

the display refreshes.

`componentDidUpdate()` behaves similarly.

---

# Example

```jsx
class Counter extends React.Component {

  state = {
    count: 0
  };

  componentDidUpdate() {
    console.log("Updated");
  }

  render() {
    return (
      <>
        <h1>{this.state.count}</h1>

        <button
          onClick={() =>
            this.setState({
              count:
                this.state.count + 1
            })
          }
        >
          +
        </button>
      </>
    );
  }
}
```

Output

```
0

↓

Click

↓

1

↓

Updated
```

Every click triggers an update.

---

# Accessing Previous Values

`componentDidUpdate()` receives the previous Props and State.

```jsx
componentDidUpdate(prevProps, prevState) {

  if (
    prevState.count !==
    this.state.count
  ) {
    console.log("Count Changed");
  }

}
```

This allows you to compare old and new values before taking action.

---

# Real-Life Example

Shopping Cart

```
Items

↓

1

↓

2

↓

3
```

Whenever the cart changes,

you might save the new cart to a server.

That logic belongs in `componentDidUpdate()`.

---

# Modern React Equivalent

```jsx
useEffect(() => {
  console.log("Count Changed");
}, [count]);
```

The effect runs whenever `count` changes.

---

# Important Note

Be careful not to call `setState()` unconditionally inside `componentDidUpdate()`.

Wrong

```jsx
componentDidUpdate() {
  this.setState({
    count: 100
  });
}
```

This causes an infinite update loop.

Correct

```jsx
componentDidUpdate(
  prevProps,
  prevState
) {

  if (
    prevState.count !==
    this.state.count
  ) {
    // Safe update logic
  }

}
```

Always compare previous and current values before updating State.

---

# Summary

- Runs after every update.
- Triggered by changes to State or Props.
- Useful for responding to changes, making follow-up API calls, or syncing data.
- Equivalent Hook:

```jsx
useEffect(() => {}, [dependency]);
```

---

# 29. What is componentWillUnmount()?

## Simple Definition

`componentWillUnmount()` is a **lifecycle method** that runs **just before a component is removed from the screen**.

Think of it as:

```
Component Visible

↓

User Leaves Page

↓

componentWillUnmount()

↓

Component Removed
```

---

# Real-Life Analogy: Leaving a Hotel Room

Before leaving a hotel room,

you:

- Turn off the lights.
- Switch off the TV.
- Pack your luggage.
- Lock the door.

These cleanup tasks happen before you leave.

`componentWillUnmount()` is React's cleanup stage.

---

# Why Is Cleanup Important?

Some components create resources that continue running:

- Timers
- Event listeners
- WebSocket connections
- API subscriptions

If they aren't cleaned up,

they continue using memory even after the component disappears.

This can cause **memory leaks** or unexpected behavior.

---

# Timer Example

```jsx
class Timer extends React.Component {

  componentDidMount() {
    this.timer =
      setInterval(() => {
        console.log("Running");
      }, 1000);
  }

  componentWillUnmount() {
    clearInterval(this.timer);
  }

  render() {
    return <h1>Timer</h1>;
  }
}
```

Flow

```
Component Appears

↓

Timer Starts

↓

User Leaves

↓

Timer Stops
```

---

# Event Listener Example

```jsx
componentDidMount() {
  window.addEventListener(
    "resize",
    this.handleResize
  );
}

componentWillUnmount() {
  window.removeEventListener(
    "resize",
    this.handleResize
  );
}
```

This prevents the event listener from continuing after the component is gone.

---

# Modern React Equivalent

```jsx
useEffect(() => {

  const timer =
    setInterval(() => {}, 1000);

  return () => {
    clearInterval(timer);
  };

}, []);
```

The function returned from `useEffect` is the cleanup function.

It runs when the component unmounts.

---

# Real-Life Example

Imagine listening to music.

```
Open Music App

↓

Song Playing
```

You close the app.

The music should stop.

If it keeps playing,

something is wrong.

`componentWillUnmount()` ensures resources are properly cleaned up.

---

# Summary

- Runs just before the component is removed.
- Used for cleanup tasks.
- Prevents memory leaks.
- Stops timers, subscriptions, WebSockets, and event listeners.
- Equivalent Hook:

```jsx
useEffect(() => {

  return () => {
    // Cleanup
  };

}, []);
```

---

# Complete Lifecycle Flow

```
Component Created

↓

componentDidMount()

↓

User Interacts

↓

State Changes

↓

componentDidUpdate()

↓

User Leaves Page

↓

componentWillUnmount()

↓

Component Removed
```

---

# Class Lifecycle vs Functional Components

| Class Component | Functional Component |
|-----------------|----------------------|
| `componentDidMount()` | `useEffect(() => {}, [])` |
| `componentDidUpdate()` | `useEffect(() => {}, [dependencies])` |
| `componentWillUnmount()` | Cleanup function returned from `useEffect()` |

---

# Real-World Examples

## YouTube

```
Open Video

↓

componentDidMount()

↓

Play Video

↓

Change Quality

↓

componentDidUpdate()

↓

Close Video

↓

componentWillUnmount()
```

---

## WhatsApp

```
Open Chat

↓

Load Messages

↓

Receive New Message

↓

Leave Chat

↓

Disconnect Listener
```

---

## Amazon

```
Open Product

↓

Load Product

↓

Change Quantity

↓

Leave Page

↓

Stop Active Processes
```

---

# Common Interview Questions

## 1. What is componentDidMount()?

**Answer:**

It is a lifecycle method that runs once after a class component is mounted. It is commonly used for API calls, timers, subscriptions, and initialization tasks.

---

## 2. What is componentDidUpdate()?

**Answer:**

It runs after a component updates due to changes in State or Props. It is useful for responding to updates or comparing previous and current values.

---

## 3. What is componentWillUnmount()?

**Answer:**

It runs just before a component is removed from the DOM. It is used to clean up timers, event listeners, subscriptions, and other resources to prevent memory leaks.

---

## 4. Which Hook Replaces These Lifecycle Methods?

**Answer:**

`useEffect()`

- Mounting → `useEffect(() => {}, [])`
- Updating → `useEffect(() => {}, [dependencies])`
- Unmounting → Cleanup function returned from `useEffect()`

---

# Easy Memory Trick

Think of moving into and out of a rented house.

```
Move In

↓

Arrange Furniture

(componentDidMount)

↓

Live in the House

(Change Things)

(componentDidUpdate)

↓

Move Out

↓

Clean the House

(componentWillUnmount)
```

The same idea applies to React components.

---

# Summary

- `componentDidMount()` runs once after a component is added to the DOM and is used for initialization tasks.
- `componentDidUpdate()` runs after changes to State or Props and is used to respond to updates.
- `componentWillUnmount()` runs just before a component is removed and is used for cleanup.
- In modern React, these lifecycle methods are replaced by the `useEffect` Hook and its cleanup function.
- Understanding these lifecycle methods is essential because many existing enterprise applications still use Class Components, while new projects typically use Functional Components with Hooks.