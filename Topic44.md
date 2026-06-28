# 66. What is Concurrent Rendering?

## Simple Definition

**Concurrent Rendering** is a React feature that allows React to **prepare multiple UI updates at the same time and prioritize the most important work without blocking the browser.**

In simple words,

> **Concurrent Rendering allows React to pause, resume, restart, and prioritize rendering work so the application stays responsive.**

**Important:**

Concurrent Rendering **does not mean React renders components in parallel on multiple CPU threads.**

React still runs on JavaScript's **single thread**.

Instead,

React intelligently **schedules rendering work**.

---

# Why Was Concurrent Rendering Needed?

Imagine a dashboard containing:

- Charts
- Tables
- Graphs
- Search Box
- Notifications

Suppose React starts rendering a huge chart.

While it's rendering,

the user types into the search box.

Without Concurrent Rendering,

```
Render Chart

↓

Finish Rendering

↓

Handle Search Input
```

The search feels slow.

---

With Concurrent Rendering,

```
Start Rendering Chart

↓

Pause

↓

Handle Search Input

↓

Continue Rendering Chart
```

The application feels smooth.

---

# Real-Life Analogy: Chef in a Restaurant

Imagine a chef cooking.

Old method

```
Cook One Huge Order

↓

Finish Completely

↓

Take New Orders
```

Customers wait.

---

Concurrent Rendering

```
Cook Main Dish

↓

Urgent Order Arrives

↓

Prepare Urgent Order

↓

Continue Main Dish
```

Everyone gets better service.

React behaves similarly.

---

# Traditional Rendering

Before concurrent features,

React rendered like this.

```
Start

↓

Component 1

↓

Component 2

↓

Component 3

↓

Component 4

↓

Finish
```

Nothing could interrupt the render phase.

---

# Concurrent Rendering

Now React can do this.

```
Component 1

↓

Component 2

↓

Pause

↓

Handle User Input

↓

Resume

↓

Component 3

↓

Finish
```

The browser remains responsive.

---

# What Makes This Possible?

Concurrent Rendering is built on top of **Fiber Architecture**.

Remember Fiber?

Fiber introduced:

- Small units of work
- Scheduling
- Pause
- Resume
- Restart
- Priorities

Concurrent Rendering uses those capabilities.

---

# Render Phase vs Commit Phase

Concurrent Rendering affects only the **Render Phase**.

```
Render Phase

↓

Can Pause

↓

Can Resume

↓

Can Restart
```

But

```
Commit Phase

↓

Cannot Pause

↓

Must Finish
```

Once React starts updating the Real DOM,

it completes the Commit Phase synchronously.

---

# Visual Flow

```
State Update

↓

Concurrent Render

↓

Pause

↓

Resume

↓

Prepare Changes

↓

Commit

↓

Update DOM
```

---

# Priority-Based Rendering

React gives different priorities to different kinds of work.

Imagine these updates happen together.

```
Search Input

Dashboard Chart

Animation

Background Update
```

React may conceptually prioritize them like this.

```
Search Input

↓

Animation

↓

Chart

↓

Background Update
```

This keeps the application responsive.

> **Note:** The exact priorities are determined by React's scheduler and may vary depending on the type of update.

---

# Example

Suppose the user types:

```
R

↓

Re

↓

Rea

↓

React
```

At the same time,

React is rendering:

```
1000 Products
```

Without Concurrent Rendering,

typing may lag.

With Concurrent Rendering,

React can pause rendering the product list,

process the typing,

then continue rendering.

---

# What Happens If New Data Arrives?

Suppose React is rendering:

```
Page Version 1
```

Before it finishes,

new data arrives.

Instead of finishing outdated work,

React can:

```
Stop Old Work

↓

Start New Work
```

This avoids wasting time rendering UI that will immediately become outdated.

---

# Concurrent Rendering Doesn't Mean Immediate DOM Updates

Many developers misunderstand this.

Concurrent Rendering only changes **how React prepares updates**.

The DOM is still updated during the Commit Phase.

```
Render

↓

Prepare

↓

Commit

↓

DOM Update
```

---

# Features That Use Concurrent Rendering

Several modern React features rely on Concurrent Rendering.

Examples include:

- `startTransition()`
- `useTransition()`
- `useDeferredValue()`
- `Suspense`
- Streaming Server Rendering

These features allow React to distinguish between urgent and non-urgent work.

---

# Example: `useTransition`

Suppose a search page filters 10,000 products.

Urgent update:

```
Update Search Box
```

Non-urgent update:

```
Filter Product List
```

With

```jsx
startTransition(() => {

  setProducts(filtered);

});
```

React keeps typing responsive while the expensive list update happens with lower priority.

---

# Example: `useDeferredValue`

Suppose the user types quickly.

```
A

↓

Ap

↓

App

↓

Apple
```

The input updates immediately.

The expensive search results can be slightly delayed.

This creates a smoother experience.

---

# Benefits

Concurrent Rendering provides:

- Better responsiveness
- Smoother typing
- Better scrolling
- Better animations
- Smarter scheduling
- Less UI blocking
- Improved user experience

---

# Does Every React App Use It?

If you're using **React 18 or later** with the modern root API (`createRoot`), React can use concurrent rendering capabilities for supported features.

However,

React does **not** automatically make every update interruptible.

Concurrent behavior is enabled when using features such as transitions or Suspense that take advantage of the concurrent renderer.

---

# Real-Life Example

Imagine an airport.

Old system

```
One Plane Lands

↓

Second Plane Waits

↓

Third Plane Waits
```

---

Concurrent system

```
Emergency Plane

↓

Land First

↓

Passenger Plane

↓

Cargo Plane
```

The airport handles work based on priority.

React does something similar with rendering.

---

# Side-by-Side Comparison

| Feature | Traditional Rendering | Concurrent Rendering |
|----------|----------------------|----------------------|
| Rendering | One uninterrupted task | Can pause and resume during render |
| User Input | May be blocked | Remains responsive |
| Priorities | Limited | Intelligent scheduling |
| Restart Rendering | ❌ No | ✅ Yes |
| Pause Rendering | ❌ No | ✅ Yes |
| Commit Phase | Synchronous | Still synchronous |

---

# Common Interview Questions

## 1. What Is Concurrent Rendering?

**Answer:**

Concurrent Rendering is React's ability to prepare rendering work in an interruptible way, allowing it to pause, resume, restart, and prioritize updates to keep the application responsive.

---

## 2. Does Concurrent Rendering Use Multiple Threads?

**Answer:**

No.

React still runs on JavaScript's single thread.

Concurrent Rendering is about scheduling work, not running it in parallel.

---

## 3. Which Phase Can Be Interrupted?

**Answer:**

Only the **Render Phase**.

The **Commit Phase** always runs synchronously.

---

## 4. What Made Concurrent Rendering Possible?

**Answer:**

Fiber Architecture introduced interruptible rendering and scheduling, making Concurrent Rendering possible.

---

## 5. Which React Features Use Concurrent Rendering?

**Answer:**

Examples include:

- `startTransition()`
- `useTransition()`
- `useDeferredValue()`
- `Suspense`
- Streaming Server Rendering

---

## 6. Does Concurrent Rendering Always Make Apps Faster?

**Answer:**

Not necessarily.

Its primary goal is to make applications **feel more responsive** by prioritizing urgent updates over less important work. It improves the user experience more than raw computation speed.

---

# Easy Memory Trick

Imagine writing a book.

Old way

```
Write Entire Book

↓

Answer Phone
```

You ignore everything else until you're done.

---

Concurrent way

```
Write Chapter

↓

Answer Important Phone Call

↓

Continue Writing
```

You can pause and resume work without losing progress.

React works the same way.

- **Fiber** = The project manager that organizes the work.
- **Concurrent Rendering** = The ability to pause, prioritize, and resume that work.
- **Commit Phase** = Publishing the final version.

---

# Summary

- **Concurrent Rendering** allows React to prepare UI updates in an interruptible and prioritized manner.
- It is built on top of **Fiber Architecture**.
- React can pause, resume, restart, and prioritize work during the **Render Phase**.
- The **Commit Phase** remains synchronous to keep the UI consistent.
- Concurrent Rendering does **not** use multiple threads; it works by intelligently scheduling tasks on JavaScript's single thread.
- Features such as `startTransition`, `useTransition`, `useDeferredValue`, and `Suspense` are built on top of Concurrent Rendering.
- The main benefit is a **more responsive and smoother user experience**, especially during expensive rendering operations.