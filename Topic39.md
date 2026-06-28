# 59. What is Fiber Architecture?

## Simple Definition

**Fiber Architecture** is React's **new rendering engine** introduced in **React 16**.

It is responsible for:

- Rendering components
- Managing updates
- Prioritizing work
- Pausing and resuming rendering
- Making React applications smoother and more responsive

In simple words,

> **Fiber is React's internal "work manager" that decides what should be rendered, when it should be rendered, and in what order.**

Think of Fiber as the **brain** behind React rendering.

---

# Before Fiber

Before React 16,

React used a rendering algorithm called **Stack Reconciler**.

When something changed,

React would render everything in one go.

```
Start Rendering

↓

Continue

↓

Continue

↓

Finish Everything

↓

Browser Gets Control
```

There was **no pause**.

If rendering took a long time,

the browser had to wait.

This caused:

- Slow UI
- Frozen animations
- Delayed button clicks
- Poor user experience

---

# Real-Life Analogy: Reading a Book

Imagine reading a 1000-page book.

### Old React

```
Start Reading

↓

Don't Stop

↓

Read 1000 Pages

↓

Take Break
```

Very tiring.

---

### Fiber

```
Read 20 Pages

↓

Take Break

↓

Continue

↓

Take Break

↓

Continue
```

Much better.

Fiber divides work into small pieces.

---

# What Does Fiber Do?

Fiber helps React:

- Break rendering into smaller tasks
- Pause work
- Resume work later
- Prioritize important updates
- Skip unnecessary work
- Keep the UI responsive

---

# Visual Representation

Without Fiber

```
Huge Rendering Task

↓

Execute Everything

↓

Browser Waits
```

---

With Fiber

```
Task

↓

Small Piece

↓

Pause

↓

Continue

↓

Pause

↓

Finish
```

The browser gets opportunities to respond to user interactions between chunks of work.

---

# Why Is This Useful?

Imagine a page containing:

```
1000 Products

Images

Charts

Buttons

Forms

Animations
```

The user clicks:

```
Buy Now
```

Without Fiber,

the click might wait until all rendering finishes.

With Fiber,

React can prioritize the click so the application feels responsive.

---

# What Is a Fiber?

A **Fiber** is an internal JavaScript object representing a single unit of work for a React element or component.

Each component has its own Fiber object.

Conceptually, a Fiber stores information such as:

- Component type
- Props
- State
- Pending updates
- Child and sibling relationships
- Effect information

---

# Visual Example

```
App

↓

Header

↓

Sidebar

↓

Content

↓

Footer
```

Each component has its own Fiber.

```
Fiber

↓

Header
```

```
Fiber

↓

Sidebar
```

```
Fiber

↓

Content
```

React processes these Fibers to perform rendering work.

---

# Fiber Tree

Your component tree

```
App

├── Header

├── Sidebar

├── Main

│     ├── Card

│     └── Card

└── Footer
```

Internally,

React builds a corresponding Fiber tree.

Each node is a Fiber.

---

# How Fiber Processes Work

Suppose React needs to update:

```
Dashboard
```

Fiber can process:

```
Task 1

↓

Pause

↓

Task 2

↓

Pause

↓

Task 3
```

Instead of blocking the browser for one long operation.

---

# Two Main Phases of Fiber

## 1. Render Phase

React:

- Creates or updates the Fiber tree
- Calculates what should change
- Can pause, resume, or restart this work if needed

No DOM changes happen yet.

---

## 2. Commit Phase

React applies the calculated changes.

Examples:

- Update the DOM
- Run layout effects (`useLayoutEffect`)
- Paint updates
- Schedule normal effects (`useEffect`)

The commit phase is synchronous because the UI must stay consistent.

---

# Visual Flow

```
State Changes

↓

Render Phase

↓

Prepare Updates

↓

Commit Phase

↓

Update DOM

↓

Browser Paint
```

---

# Benefits of Fiber

- Better responsiveness
- Interruptible rendering
- Prioritized updates
- Better animations
- Better user experience
- Foundation for modern React features

---

# Summary

- Fiber is React's rendering engine introduced in React 16.
- It breaks rendering work into small units.
- It allows React to pause, resume, and prioritize rendering work.

---

# 60. Why Was Fiber Introduced?

## Simple Answer

Fiber was introduced to solve the performance limitations of the old rendering engine.

Before Fiber,

React processed rendering as one large, uninterrupted task.

Large updates could block the browser and make applications feel slow.

---

# Real-Life Analogy: Restaurant Kitchen

Imagine a restaurant.

Old kitchen

```
Cook Order 1

↓

Finish Completely

↓

Cook Order 2

↓

Finish Completely

↓

Cook Order 3
```

If an urgent order arrives,

it must wait.

---

Fiber Kitchen

```
Cook Order 1

↓

Urgent Order Arrives

↓

Cook Urgent Order

↓

Continue Order 1
```

Much better.

---

# Problems Before Fiber

## Problem 1: Blocking Rendering

Large applications

```
Dashboard

↓

Charts

↓

Tables

↓

Graphs

↓

Forms
```

Everything rendered in one long task.

The browser couldn't respond until React finished.

---

## Problem 2: Poor User Experience

Imagine clicking:

```
Submit
```

Nothing happens for a moment because React is busy rendering.

Users feel the app is frozen.

---

## Problem 3: No Priorities

All updates were treated the same.

Changing:

```
Background Color
```

had the same priority as:

```
Typing in a Search Box
```

Ideally, user input should be handled first.

---

# What Fiber Changed

Fiber introduced the ability to:

- Pause work
- Resume work
- Restart work
- Prioritize work
- Spread rendering across multiple chunks

---

# Priority Example

Suppose these updates happen together.

```
Animation

Search Input

Background Update

Chart Refresh
```

Fiber can conceptually prioritize them like this:

```
Search Input

↓

Animation

↓

Chart

↓

Background
```

This keeps the application feeling responsive.

> **Note:** React's scheduler determines priorities internally. The exact ordering depends on the type of update.

---

# Time Slicing

Fiber enables React to split rendering work into smaller chunks.

Instead of:

```
100 ms Work
```

React can conceptually do:

```
10 ms

↓

Pause

↓

10 ms

↓

Pause

↓

10 ms
```

This gives the browser opportunities to handle input, painting, and other work between chunks.

---

# Better Animations

Without Fiber

```
Animation

↓

Rendering Blocks

↓

Animation Stutters
```

---

With Fiber

```
Animation

↓

Pause Rendering

↓

Continue Rendering

↓

Smooth Animation
```

---

# Foundation for Concurrent Features

Fiber made many modern React capabilities possible, including:

- Concurrent rendering
- Transitions
- Suspense improvements
- Better scheduling
- Improved responsiveness

---

# Visual Comparison

Old React

```
One Huge Task

↓

Finish

↓

Browser Responds
```

---

Fiber

```
Small Task

↓

Pause

↓

Browser Responds

↓

Continue
```

---

# Side-by-Side Comparison

| Feature | Before Fiber | Fiber |
|----------|--------------|--------|
| Rendering | One large synchronous task | Split into smaller units |
| Pause work | ❌ No | ✅ Yes (during render phase) |
| Resume work | ❌ No | ✅ Yes |
| Prioritize updates | ❌ No | ✅ Yes |
| Interrupt rendering | ❌ No | ✅ Yes (render phase) |
| Better responsiveness | Limited | ✅ Yes |
| Foundation for concurrent features | ❌ No | ✅ Yes |

---

# Real-Life Example

Imagine writing a report.

Old method

```
Write 100 Pages

↓

Submit
```

No breaks.

---

Fiber method

```
Write 10 Pages

↓

Save

↓

Continue

↓

Save

↓

Continue
```

You can pause, make corrections, or respond to something urgent before continuing.

---

# Common Interview Questions

## 1. What is Fiber Architecture?

**Answer:**

Fiber is React's rendering engine introduced in React 16. It represents each component as a unit of work and enables React to pause, resume, prioritize, and schedule rendering more efficiently.

---

## 2. Why Was Fiber Introduced?

**Answer:**

Fiber was introduced to overcome the limitations of the old synchronous rendering engine by enabling interruptible rendering, better scheduling, and improved application responsiveness.

---

## 3. What Is a Fiber?

**Answer:**

A Fiber is an internal JavaScript object representing a React component or element. It stores information needed for rendering, updates, and scheduling.

---

## 4. What Are the Two Main Phases of Fiber?

**Answer:**

- **Render Phase:** React calculates what changes are needed. This phase can be paused, resumed, or restarted.
- **Commit Phase:** React applies those changes to the DOM and runs the appropriate effects. This phase is synchronous.

---

## 5. Can Fiber Pause DOM Updates?

**Answer:**

No.

Fiber can pause and resume the **render phase**, where React calculates updates.

Once React enters the **commit phase**, applying changes to the DOM happens synchronously to keep the UI consistent.

---

## 6. Does Fiber Improve Performance?

**Answer:**

Yes.

Fiber improves perceived performance by scheduling work intelligently, prioritizing important updates, reducing UI blocking, and enabling modern concurrent rendering capabilities.

---

# Easy Memory Trick

Imagine a project manager.

Without Fiber

```
Complete Entire Project

↓

Then Check Email
```

Everything else waits.

---

With Fiber

```
Work

↓

Pause

↓

Reply to Important Email

↓

Continue Work
```

The project manager decides what is most important at any moment.

That's exactly what Fiber does for React.

---

# Summary

- **Fiber Architecture** is React's rendering engine introduced in React 16.
- A **Fiber** is an internal data structure that represents a component and its rendering work.
- Fiber breaks rendering into smaller units that can be paused, resumed, restarted, and prioritized during the render phase.
- The **render phase** calculates updates, while the **commit phase** applies them to the DOM.
- Fiber was introduced to solve the limitations of the old synchronous rendering engine and to improve responsiveness.
- It also serves as the foundation for modern React capabilities such as concurrent rendering, transitions, and improved scheduling.