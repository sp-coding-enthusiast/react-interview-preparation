# 67. What is React Scheduler?

## Simple Definition

**React Scheduler** is React's internal system that **decides when different rendering tasks should run based on their priority.**

In simple words,

> **React Scheduler is like a smart manager that decides which work should be done first and which work can wait.**

It works together with:

- Fiber Architecture
- Concurrent Rendering

Without the Scheduler, React wouldn't know which updates are urgent and which can be delayed.

---

# Real-Life Analogy: Hospital Emergency Room

Imagine a hospital.

Patients arrive at the same time.

```
Person with Fever

Person with Broken Arm

Heart Attack Patient

Regular Check-up
```

Does the doctor treat them in the order they arrived?

No.

The doctor prioritizes based on urgency.

```
Heart Attack

↓

Broken Arm

↓

Fever

↓

Regular Check-up
```

The React Scheduler works exactly the same way.

---

# Why Was React Scheduler Introduced?

Before React 16 (Fiber),

React processed updates in one large task.

```
Update 1

↓

Update 2

↓

Update 3
```

Everything waited.

There was no concept of priority.

This caused:

- Slow typing
- Frozen UI
- Delayed animations
- Poor user experience

---

After Fiber,

React could split work into small pieces.

Now it needed something to decide:

- Which task runs first?
- Which task can wait?
- Which task should pause?
- Which task should restart?

That's why the Scheduler was introduced.

---

# What Does the Scheduler Do?

The Scheduler decides:

- Which update is most important
- Which work should run now
- Which work should wait
- When rendering should pause
- When rendering should continue

Think of it as a traffic controller for React updates.

---

# Visual Representation

Without Scheduler

```
Task A

↓

Task B

↓

Task C

↓

Task D
```

Everything runs in order.

---

With Scheduler

```
Urgent Task

↓

Medium Task

↓

Low Priority Task

↓

Background Task
```

More important work is handled first.

---

# Example

Imagine a page with:

- Search box
- Product list
- Dashboard chart
- Notifications

The user types:

```
React
```

At the same time,

a huge chart is rendering.

Without Scheduler

```
Render Chart

↓

Finish

↓

Handle Typing
```

Typing feels slow.

---

With Scheduler

```
Pause Chart

↓

Handle Typing

↓

Resume Chart
```

The UI stays responsive.

---

# Scheduler and Fiber

Fiber breaks rendering into small pieces.

Imagine this.

```
Huge Work

↓

Piece 1

↓

Piece 2

↓

Piece 3

↓

Piece 4
```

The Scheduler decides:

```
Run Piece 1

↓

Pause

↓

Handle User Input

↓

Continue Piece 2
```

Without Fiber,

the Scheduler wouldn't be able to pause rendering.

Without the Scheduler,

Fiber wouldn't know what to prioritize.

They work together.

---

# Priority Levels (Concept)

React internally assigns different priorities to different kinds of work.

Conceptually, you can think of them like this:

```
High Priority

↓

Normal Priority

↓

Low Priority

↓

Idle Work
```

Examples

High Priority

```
Typing

Button Click

Keyboard Input
```

Lower Priority

```
Loading Large List

Background Updates

Analytics
```

> **Note:** React's actual internal priority model is more detailed and may change between versions. The important idea is that urgent user interactions are generally prioritized over less urgent rendering work.

---

# How Scheduler Works

Suppose React receives:

```
Search Input

↓

Chart Update

↓

Theme Change

↓

Notification
```

The Scheduler might process them conceptually like this.

```
Search Input

↓

Notification

↓

Chart

↓

Theme
```

This helps keep the application responsive.

---

# Does Scheduler Update the DOM?

No.

The Scheduler **never updates the Real DOM**.

Its job is only to schedule rendering work.

```
Scheduler

↓

Render Phase

↓

Commit Phase

↓

DOM Update
```

---

# Scheduler During Concurrent Rendering

Suppose React is rendering:

```
1000 Products
```

Suddenly,

the user clicks:

```
Checkout
```

The Scheduler can decide:

```
Pause Products

↓

Handle Checkout

↓

Continue Products
```

This is one of the biggest benefits of Concurrent Rendering.

---

# Real-Life Analogy: Airport Control Tower

Planes arrive.

```
Cargo Plane

Passenger Plane

Emergency Plane

Private Jet
```

Control Tower decides:

```
Emergency

↓

Passenger

↓

Cargo

↓

Private Jet
```

The Scheduler plays the same role inside React.

---

# Relationship Between React Technologies

```
Fiber

↓

Breaks Work Into Small Pieces

↓

Scheduler

↓

Chooses Which Piece Runs Next

↓

Concurrent Rendering

↓

Allows Pause and Resume

↓

Commit Phase

↓

Updates DOM
```

Think of it like:

- **Fiber** → Creates manageable tasks.
- **Scheduler** → Chooses the order.
- **Concurrent Rendering** → Lets React interrupt and continue work.
- **Commit Phase** → Applies the final changes to the DOM.

---

# Features That Use Scheduler

Several modern React features rely on the Scheduler.

Examples:

- `startTransition()`
- `useTransition()`
- `useDeferredValue()`
- `Suspense`
- Concurrent Rendering

These features help React decide which updates are urgent and which can be delayed.

---

# Example with `startTransition`

```jsx
startTransition(() => {

  setProducts(filteredProducts);

});
```

The Scheduler understands this update is less urgent than typing into an input field.

It can delay rendering the product list while keeping typing responsive.

---

# Benefits

The Scheduler provides:

- Better responsiveness
- Better user experience
- Intelligent prioritization
- Less UI blocking
- Smoother typing
- Better animations
- More efficient rendering

---

# Side-by-Side Comparison

| Feature | Without Scheduler | With Scheduler |
|----------|-------------------|----------------|
| Task Order | First come, first served | Priority-based |
| User Input | Can be delayed | Higher priority |
| Large Rendering | Blocks UI | Can be paused |
| Scheduling | Simple | Intelligent |
| Works with Fiber | ❌ No | ✅ Yes |
| Supports Concurrent Rendering | ❌ No | ✅ Yes |

---

# Common Interview Questions

## 1. What Is React Scheduler?

**Answer:**

React Scheduler is React's internal scheduling system that determines when rendering tasks should run and which updates should be given higher priority.

---

## 2. Why Was Scheduler Introduced?

**Answer:**

It was introduced to prioritize rendering work, improve responsiveness, and prevent long rendering tasks from blocking user interactions.

---

## 3. Does Scheduler Update the DOM?

**Answer:**

No.

The Scheduler only decides when rendering work should happen.

The Real DOM is updated later during the Commit Phase.

---

## 4. How Does Scheduler Work with Fiber?

**Answer:**

Fiber breaks rendering into small units of work.

The Scheduler decides the order in which those units should be processed based on their priority.

---

## 5. What Is the Difference Between Scheduler and Concurrent Rendering?

**Answer:**

- **Scheduler** decides **when** rendering work should run and what priority it should have.
- **Concurrent Rendering** is React's ability to pause, resume, restart, and prioritize rendering based on those scheduling decisions.

---

## 6. Does Scheduler Use Multiple Threads?

**Answer:**

No.

The Scheduler works on JavaScript's single thread.

It schedules work intelligently rather than executing it in parallel.

---

# Easy Memory Trick

Imagine a school principal.

Teachers submit many requests.

```
Exam Paper

Sports Event

Holiday Request

Maintenance Work
```

The principal decides:

```
Exam Paper

↓

Holiday

↓

Sports

↓

Maintenance
```

The principal doesn't do the work.

They decide **what should happen first**.

Similarly,

- **Fiber** creates the work.
- **Scheduler** prioritizes the work.
- **Concurrent Rendering** allows React to pause and resume the work.
- **Commit Phase** applies the work to the browser.

---

# Summary

- **React Scheduler** is React's internal task scheduler that determines when rendering work should run.
- It works closely with **Fiber Architecture** and **Concurrent Rendering**.
- The Scheduler assigns priorities so urgent updates, like user input, are processed before less important work.
- It can pause lower-priority rendering and resume it later, helping keep applications responsive.
- The Scheduler **does not update the DOM**; it only schedules rendering work.
- Modern React features such as `startTransition`, `useTransition`, `useDeferredValue`, and `Suspense` rely on the Scheduler.
- A simple way to remember:
  - **Fiber creates the work.**
  - **Scheduler prioritizes the work.**
  - **Concurrent Rendering manages interruptible work.**
  - **Commit Phase updates the DOM.**