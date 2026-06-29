# 91. What is Concurrent Mode?

> **Interview Note (Very Important)**

The term **"Concurrent Mode"** is mostly **historical**.

Starting from **React 18**, React no longer recommends talking about **Concurrent Mode** as a separate mode.

Instead, React introduced the **Concurrent Renderer**, and applications can use **Concurrent Features** such as:

- Automatic Batching
- `startTransition`
- `useTransition`
- `useDeferredValue`
- Suspense
- Streaming SSR
- Selective Hydration

So, in modern React interviews, when someone asks **"What is Concurrent Mode?"**, they usually mean **Concurrent Rendering**.

---

# Simple Definition

**Concurrent Rendering** is React's ability to **prepare multiple versions of the UI, pause rendering, prioritize important updates, and resume work later without blocking the browser.**

In simple words,

> **React becomes smarter about *when* and *how* it renders components instead of doing everything immediately.**

Think of it as React learning **time management**.

---

# Why Was Concurrent Rendering Introduced?

Imagine you're typing into a search box.

Without Concurrent Rendering

```
User Types

↓

React Updates Search Box

↓

React Filters

100,000 Products

↓

UI Freezes

↓

Next Key Press Waits
```

Typing feels slow.

---

With Concurrent Rendering

```
User Types

↓

React Updates Input Immediately

↓

Background Filtering Starts

↓

User Types Again

↓

React Prioritizes Typing

↓

Filtering Continues Later
```

The application feels much smoother.

---

# Real-Life Analogy: Hospital Emergency Room

Imagine a hospital.

Patients arrive.

```
Patient A

↓

Heart Attack

Patient B

↓

Broken Finger

Patient C

↓

Headache
```

Should the doctor treat them in arrival order?

No.

Instead,

```
Heart Attack

↓

Broken Finger

↓

Headache
```

Priority matters.

React works the same way.

---

# Before React 18 (Synchronous Rendering)

Old React worked like this.

```
Start Rendering

↓

Continue Rendering

↓

Continue Rendering

↓

Finish

↓

Browser Responds
```

Once rendering started,

it couldn't stop.

Even if the user clicked something,

React kept working.

---

# Problem

Imagine rendering

```
50,000 Products
```

```
Start

↓

Render 1

↓

Render 2

↓

...

↓

Render 50000

↓

Done
```

The browser waits.

The UI freezes.

---

# Concurrent Rendering

Instead,

React works like this.

```
Start Rendering

↓

Pause

↓

Browser Responds

↓

Resume Rendering

↓

Pause Again

↓

Resume

↓

Finish
```

Now the browser stays responsive.

---

# Visual Example

Without Concurrent Rendering

```
User Clicks

↓

React Busy

↓

Wait...

↓

Finally Responds
```

---

With Concurrent Rendering

```
User Clicks

↓

Pause Background Work

↓

Handle Click

↓

Resume Background Work
```

The application feels instant.

---

# What Does React Actually Do?

React doesn't create multiple browser threads.

Instead,

it **breaks rendering work into smaller units**.

```
Huge Task

↓

Small Task

↓

Small Task

↓

Small Task

↓

Small Task
```

Between these small tasks,

React checks

```
Did User Click?

Did User Type?

Did Something More Important Happen?
```

If yes,

React handles that first.

---

# React Scheduler

Concurrent Rendering relies on the **React Scheduler**.

```
Update

↓

Scheduler

↓

Assign Priority

↓

Render
```

The Scheduler decides

"What should React work on next?"

---

# Priority Levels

Imagine

```
Typing

Animation

Background Search

Analytics
```

React assigns priorities.

```
Typing

⭐⭐⭐⭐⭐

Highest

--------------------

Animation

⭐⭐⭐⭐

--------------------

Button Click

⭐⭐⭐⭐

--------------------

Background Search

⭐⭐

--------------------

Analytics

⭐

Lowest
```

Higher-priority work runs first.

---

# Example

Imagine searching products.

User types

```
Laptop
```

Without Concurrent Rendering

```
Type L

↓

Search

↓

Freeze

↓

Type A

↓

Search

↓

Freeze
```

Typing becomes sluggish.

---

With Concurrent Rendering

```
Type L

↓

Update Input

↓

Background Search

↓

Type A

↓

Update Input

↓

Continue Search
```

Much smoother.

---

# Concurrent Features in React 18

React 18 introduced several APIs built on top of the concurrent renderer.

---

## 1. useTransition

Marks updates as **low priority**.

```jsx
const [isPending, startTransition] =

useTransition();

startTransition(() => {

  setProducts(filtered);

});
```

Typing stays responsive.

Filtering happens in the background.

---

## 2. useDeferredValue

Allows React to defer updating a value.

Example

```
Search Input

↓

Updates Immediately

↓

Filtered List

↓

Updates Slightly Later
```

Useful for expensive filtering.

---

## 3. Suspense

Concurrent Rendering improves how Suspense works.

Instead of blocking the entire page,

React can display

```
Header

↓

Sidebar

↓

Loading...

↓

Content Later
```

---

## 4. Streaming SSR

Server sends

```
Header

↓

Navigation

↓

Content

↓

Footer
```

progressively,

instead of waiting for the whole page.

---

## 5. Selective Hydration

Hydrate

```
Header

↓

First
```

while

```
Footer

↓

Later
```

Important UI becomes interactive sooner.

---

# Does Concurrent Rendering Mean Parallel Rendering?

No.

This is one of the biggest interview misconceptions.

Concurrent Rendering is

```
NOT

Parallel Rendering
```

React still runs on JavaScript's **single thread**.

Instead,

it

```
Pause

↓

Resume

↓

Prioritize
```

work intelligently.

---

# Benefits

Concurrent Rendering provides:

- Smoother UI
- Better responsiveness
- Less UI freezing
- Better user experience
- Smarter scheduling
- Improved handling of expensive updates

---

# Complete Flow

```
User Action

↓

React Scheduler

↓

Assign Priority

↓

Render Small Unit

↓

Pause

↓

Handle User Input

↓

Resume Rendering

↓

Commit UI
```

---

# Concurrent Rendering vs Synchronous Rendering

| Synchronous Rendering | Concurrent Rendering |
|------------------------|----------------------|
| Cannot pause | Can pause work |
| Blocks browser during long renders | Keeps browser responsive |
| No prioritization | Prioritizes important updates |
| Large renders may freeze UI | Smooth user experience |
| Processes work in one continuous run | Breaks work into interruptible chunks |

---

# Real-World Example

Imagine an online shopping website.

User types

```
Gaming Laptop
```

Without Concurrent Rendering

```
Type

↓

Search

↓

Freeze

↓

Results
```

---

With Concurrent Rendering

```
Type

↓

Input Updates

↓

Continue Typing

↓

Search Runs

↓

Results Update
```

Typing remains smooth.

---

# Common Interview Questions

## 1. What Is Concurrent Mode?

**Answer:**

"Concurrent Mode" is an older term. In React 18+, the correct concept is **Concurrent Rendering**, where React can interrupt, prioritize, and resume rendering work to keep the UI responsive.

---

## 2. Does Concurrent Rendering Use Multiple Threads?

**Answer:**

No.

React still runs on JavaScript's single thread.

It improves responsiveness by breaking rendering into smaller units and scheduling them intelligently.

---

## 3. Why Was Concurrent Rendering Introduced?

**Answer:**

To prevent long rendering tasks from blocking user interactions, making applications more responsive and improving the overall user experience.

---

## 4. Which React Features Use Concurrent Rendering?

**Answer:**

Examples include:

- `useTransition`
- `useDeferredValue`
- Suspense
- Streaming SSR
- Selective Hydration
- Automatic Batching

---

## 5. Does Concurrent Rendering Make Applications Faster?

**Answer:**

Not necessarily.

Its primary goal is **better responsiveness**, not shorter execution time. A task may take the same amount of CPU time but feel much smoother because React can pause and prioritize work.

---

## 6. What Is the Difference Between Concurrent Rendering and Parallel Processing?

**Answer:**

- **Concurrent Rendering** manages work on a single thread by interrupting and prioritizing tasks.
- **Parallel Processing** executes work simultaneously on multiple threads or CPU cores.

React's concurrent renderer is **not** parallel processing.

---

# Senior Interview Tip ⭐

If you're asked:

> **"What is Concurrent Mode in React?"**

A strong answer is:

> "Concurrent Mode was the experimental name used before React 18. Today, React uses a concurrent renderer that enables features like `useTransition`, `useDeferredValue`, Suspense, Streaming SSR, and Selective Hydration. It allows React to pause, resume, and prioritize rendering work so the UI remains responsive. It doesn't create multiple threads—it schedules work more intelligently on JavaScript's single thread."

This answer reflects current React terminology and avoids using outdated concepts.

---

# Easy Memory Trick

Imagine a teacher grading exams.

Without Concurrent Rendering

```
Grade

100 Papers

↓

Then

Answer Student Questions
```

Students wait.

---

With Concurrent Rendering

```
Grade

10 Papers

↓

Answer Student

↓

Grade

10 More

↓

Answer Student

↓

Continue
```

The teacher finishes the same work but remains responsive.

React behaves the same way.

- **Concurrent Rendering = Smart Scheduling**
- **Scheduler = Teacher**
- **High Priority = Student Questions**
- **Low Priority = Grading Papers**

---

# Summary

- **Concurrent Mode** is an older term; in **React 18+**, the correct concept is **Concurrent Rendering**.
- Concurrent Rendering allows React to **pause**, **resume**, and **prioritize** rendering work to keep the UI responsive.
- It is powered by the **React Scheduler**, which assigns priorities to updates.
- It **does not use multiple threads** or true parallel processing. Instead, it intelligently schedules work on JavaScript's single thread.
- Features built on the concurrent renderer include:
  - **Automatic Batching**
  - **`useTransition`**
  - **`useDeferredValue`**
  - **Suspense**
  - **Streaming SSR**
  - **Selective Hydration**
- The primary goal is **better user experience and responsiveness**, not necessarily faster execution.