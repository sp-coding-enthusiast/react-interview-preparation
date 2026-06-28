# 68. How Does React Prioritize Updates?

## Simple Definition

React does **not** treat every update equally.

Instead, React **assigns priorities to different updates** and processes the most important ones first.

In simple words,

> **If multiple updates happen at the same time, React decides which ones should happen immediately and which ones can wait.**

This prioritization is handled internally by the **React Scheduler**, which works together with **Fiber Architecture** and **Concurrent Rendering**.

---

# Why Does React Need Priorities?

Imagine a webpage containing:

- Search Box
- Product List
- Charts
- Animations
- Notifications

Now imagine the user:

- Types in the search box
- A chart starts updating
- Notifications arrive
- Analytics are being calculated

Should React process them in the exact order they arrived?

No.

The user expects typing to remain smooth.

---

# Real-Life Analogy: Airport Runway

Imagine four airplanes waiting.

```
Cargo Plane

Passenger Plane

Emergency Plane

Private Jet
```

Should they land in order?

No.

The control tower prioritizes:

```
Emergency Plane

↓

Passenger Plane

↓

Cargo Plane

↓

Private Jet
```

React behaves the same way.

---

# How React Prioritizes Work

When an update occurs,

React asks questions like:

```
Is this user typing?

↓

Is this a button click?

↓

Is this an animation?

↓

Is this background work?
```

Based on the answers,

React decides the priority.

---

# Conceptual Priority Levels

React's internal implementation is complex and may change between versions, but you can think of updates like this:

| Priority | Example |
|----------|----------|
| 🔴 High | Typing, clicking a button, keyboard input |
| 🟡 Medium | Updating visible UI after data arrives |
| 🟢 Low | Large list rendering, transitions |
| ⚪ Idle | Background or non-urgent work |

> **Interview Tip:** You don't need to memorize React's internal lane names. Understanding the concept of urgent vs. non-urgent updates is usually sufficient.

---

# Example 1: Typing While Rendering

Suppose React is rendering:

```
5000 Products
```

At the same time,

the user types:

```
React
```

Without prioritization

```
Render Products

↓

Finish

↓

Handle Typing
```

Typing becomes slow.

---

With prioritization

```
Pause Product Rendering

↓

Update Search Box

↓

Continue Product Rendering
```

Typing stays smooth.

---

# Example 2: Button Click

Suppose

```
Update Chart

↓

Button Click
```

React treats the button click as more urgent.

Flow

```
Button Click

↓

Chart Update
```

The user gets immediate feedback.

---

# Example 3: Notification

Suppose

```
Background Analytics

↓

Notification

↓

Large Table Rendering
```

The Scheduler may process them conceptually like this.

```
Notification

↓

Table

↓

Analytics
```

---

# How Fiber Helps

Fiber breaks rendering into smaller tasks.

Instead of

```
Huge Rendering Task
```

React has

```
Task 1

↓

Task 2

↓

Task 3

↓

Task 4
```

Now the Scheduler can choose

```
Task 1

↓

Pause

↓

Handle Typing

↓

Continue Task 2
```

Without Fiber,

React could not interrupt rendering.

---

# Role of Concurrent Rendering

Concurrent Rendering allows React to:

- Pause rendering
- Resume rendering
- Restart rendering
- Skip outdated work

This makes prioritization possible.

---

# Role of React Scheduler

The Scheduler decides:

```
Which Task?

↓

What Priority?

↓

Run Now?

↓

Delay?

↓

Pause?
```

It acts like a traffic controller.

---

# Example Using `startTransition`

```jsx
function Search() {

  const [text, setText] = useState("");

  const [products, setProducts] = useState([]);

  function handleChange(e) {

    setText(e.target.value);

    startTransition(() => {

      setProducts(filterProducts(e.target.value));

    });

  }

}
```

What happens?

Urgent update

```
Update Text Box
```

Lower-priority update

```
Filter Thousands of Products
```

React keeps typing responsive while delaying the expensive rendering work if necessary.

---

# Example Using `useDeferredValue`

User types

```
R

↓

Re

↓

Rea

↓

React
```

Input updates immediately.

The large product list may update slightly later.

This creates a smoother experience.

---

# Can React Cancel Work?

Yes.

Suppose React is rendering:

```
Old Search Results
```

Before it finishes,

the user types another character.

Instead of finishing outdated work,

React can:

```
Stop Old Rendering

↓

Start New Rendering
```

This avoids wasting time.

---

# Visual Flow

```
State Update

↓

Scheduler

↓

Assign Priority

↓

Fiber Creates Work

↓

Concurrent Rendering

↓

Pause/Resume

↓

Commit

↓

Update DOM
```

---

# Real-Life Analogy: Restaurant

Orders arrive.

```
Coffee

Birthday Cake

Emergency Baby Food

Pizza
```

Chef prepares:

```
Baby Food

↓

Coffee

↓

Pizza

↓

Cake
```

The chef isn't following arrival order.

They're following priority.

React behaves the same way.

---

# Does React Always Reorder Updates?

No.

React preserves correctness.

Prioritization affects **when work is processed**, not the correctness of state updates.

Updates that belong together are still applied in the correct order.

---

# Side-by-Side Comparison

| Without Priorities | With Priorities |
|--------------------|-----------------|
| First come, first served | Important work first |
| Typing may lag | Typing stays responsive |
| Long renders block UI | Long renders can pause |
| Poor user experience | Smooth interactions |
| No interruption | Pause and resume rendering |

---

# Technologies Working Together

```
State Update

↓

Scheduler

↓

Assign Priority

↓

Fiber

↓

Break Into Small Tasks

↓

Concurrent Rendering

↓

Pause / Resume / Restart

↓

Commit Phase

↓

Update DOM
```

---

# Common Interview Questions

## 1. How Does React Prioritize Updates?

**Answer:**

React uses the Scheduler to assign priorities to rendering work. Urgent updates, such as user interactions, are generally processed before less urgent rendering tasks.

---

## 2. Which Updates Get Higher Priority?

**Answer:**

Updates triggered by user interactions like typing, clicking, or keyboard input are generally treated as higher priority than background rendering or transition updates.

---

## 3. Which React Features Help Control Priorities?

**Answer:**

Features such as:

- `startTransition()`
- `useTransition()`
- `useDeferredValue()`

allow developers to mark some updates as less urgent.

---

## 4. Can React Pause Low-Priority Rendering?

**Answer:**

Yes.

With Concurrent Rendering, React can pause lower-priority rendering during the Render Phase, process urgent work, and then continue.

---

## 5. Can React Throw Away Old Rendering Work?

**Answer:**

Yes.

If newer, more relevant updates arrive before rendering finishes, React can abandon outdated rendering work and start rendering the latest state.

---

## 6. Does Prioritization Affect the Commit Phase?

**Answer:**

No.

Prioritization affects the **Render Phase**.

Once React begins the Commit Phase, it updates the Real DOM synchronously.

---

# Easy Memory Trick

Imagine a traffic signal.

Many vehicles arrive.

```
Car

Truck

Ambulance

Bus
```

The traffic police allow:

```
Ambulance

↓

Bus

↓

Car

↓

Truck
```

The goal is to keep the most important traffic moving first.

React's Scheduler does the same for rendering work.

---

# Summary

- React assigns different priorities to different updates.
- The **React Scheduler** determines which rendering work should happen first.
- **Fiber Architecture** breaks rendering into smaller tasks.
- **Concurrent Rendering** allows React to pause, resume, restart, or discard rendering work based on priority.
- Urgent updates (like typing and button clicks) are generally processed before less urgent work (like large list rendering).
- Features such as `startTransition`, `useTransition`, and `useDeferredValue` let developers mark updates as lower priority.
- Prioritization affects the **Render Phase** only; the **Commit Phase** always runs synchronously.