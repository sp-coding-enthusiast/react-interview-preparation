# 61. What are Render and Commit Phases?

## Simple Definition

Whenever React updates the UI, it does **not** immediately change the browser (Real DOM).

Instead, React performs the update in **two phases**:

1. **Render Phase**
2. **Commit Phase**

In simple words,

> **Render Phase = React plans what needs to change.**
>
> **Commit Phase = React actually makes those changes in the browser.**

Think of it like planning and then executing.

---

# Real-Life Analogy: House Renovation

Imagine you're renovating a house.

You don't immediately start breaking walls.

Instead, you first create a plan.

### Step 1

Architect prepares a blueprint.

```
Measure House

↓

Identify Changes

↓

Create Plan
```

Nothing has changed in the actual house.

---

### Step 2

Workers start construction.

```
Break Wall

↓

Paint

↓

Install Windows

↓

Finished House
```

Now the actual house changes.

React works exactly the same way.

---

# Visual Overview

```
State Changes

↓

Render Phase

↓

Commit Phase

↓

Browser Paints Updated UI
```

---

# Why Does React Use Two Phases?

Imagine updating Facebook.

You click:

```
Like Button
```

Without planning,

React might repeatedly update the DOM.

DOM operations are expensive.

Instead,

React first calculates everything.

Then,

it performs all necessary DOM updates together.

This makes rendering much more efficient.

---

# Phase 1: Render Phase

## What Happens?

During the Render Phase,

React:

- Calls your components
- Executes Hooks
- Creates a new Virtual DOM
- Compares it with the previous Virtual DOM
- Decides what needs to change

**No changes are made to the Real DOM yet.**

---

# Visual Representation

```
Component

↓

Execute Function

↓

Create New Virtual DOM

↓

Compare Old Virtual DOM

↓

Prepare Changes
```

Nothing is visible to the user yet.

---

# Example

Initial render

```jsx
<h1>Hello</h1>
```

After state changes

```jsx
<h1>Hello Saurabh</h1>
```

Render Phase compares:

```
Old

↓

Hello
```

```
New

↓

Hello Saurabh
```

React decides:

```
Only Text Changed
```

Still,

the browser hasn't been updated.

---

# Can Render Phase Be Paused?

Yes.

With **Fiber Architecture**,

React can:

- Pause rendering
- Resume rendering
- Restart rendering
- Prioritize rendering

This helps keep the application responsive.

---

# Phase 2: Commit Phase

After React finishes planning,

it enters the Commit Phase.

Now React:

- Updates the Real DOM
- Attaches or removes DOM nodes
- Updates attributes
- Updates text
- Runs layout effects (`useLayoutEffect`)
- Lets the browser paint
- Schedules normal effects (`useEffect`) to run after painting

The user now sees the updated UI.

---

# Visual Representation

```
Prepared Changes

↓

Update Real DOM

↓

Run Layout Effects

↓

Browser Paint

↓

Run useEffect
```

---

# Example

Old UI

```
Hello
```

New UI

```
Hello Saurabh
```

During Commit Phase,

React updates only:

```
Text Node
```

The browser immediately displays:

```
Hello Saurabh
```

---

# Complete Flow

Suppose the user clicks a button.

```
Click Button

↓

setCount()

↓

React Starts Render Phase

↓

Creates New Virtual DOM

↓

Compares Trees

↓

Calculates Differences

↓

Starts Commit Phase

↓

Updates Real DOM

↓

Browser Paints

↓

Runs useEffect
```

---

# Real-Life Analogy: Movie Production

Making a movie has two stages.

### Planning

```
Write Script

↓

Hire Actors

↓

Plan Scenes
```

No movie exists yet.

---

### Shooting

```
Record Scenes

↓

Edit

↓

Release Movie
```

Now the audience sees the final result.

React follows the same pattern.

---

# Which Hooks Run in Each Phase?

## During Render Phase

These Hooks execute while React is rendering the component:

- `useState`
- `useReducer`
- `useMemo` (calculation may run)
- `useCallback` (may create/reuse function)
- `useContext`
- `useRef`

These Hooks help React prepare the next UI.

---

## During Commit Phase

After the DOM is updated:

### `useLayoutEffect`

Runs **immediately after the DOM is updated but before the browser paints**.

```
Commit

↓

useLayoutEffect

↓

Paint
```

---

### `useEffect`

Runs **after the browser paints the updated UI**.

```
Commit

↓

Paint

↓

useEffect
```

---

# Why Is the Commit Phase Fast?

React already knows exactly what changed.

It doesn't need to compare anything again.

It simply applies the prepared updates.

---

# Can Commit Phase Be Interrupted?

No.

Once React starts the Commit Phase,

it completes it synchronously.

Why?

Because partially updating the DOM could leave the UI in an inconsistent state.

---

# Render vs Commit

Imagine editing a document.

### Render

```
Find Mistakes

↓

Decide Corrections

↓

Prepare Changes
```

Nothing printed yet.

---

### Commit

```
Print Final Copy
```

Now everyone sees the updated version.

---

# Side-by-Side Comparison

| Feature | Render Phase | Commit Phase |
|----------|--------------|--------------|
| Purpose | Calculate what should change | Apply those changes |
| Updates Real DOM | ❌ No | ✅ Yes |
| Creates Virtual DOM | ✅ Yes | ❌ No |
| Compares Virtual DOM | ✅ Yes | ❌ No |
| Can be paused (Fiber) | ✅ Yes | ❌ No |
| Can be restarted | ✅ Yes | ❌ No |
| Runs `useLayoutEffect` | ❌ No | ✅ Yes (before paint) |
| Runs `useEffect` | ❌ No | ✅ Yes (after paint) |
| User sees changes | ❌ No | ✅ Yes |

---

# Real-Life Example: Online Shopping

User changes product quantity.

```
+ Button

↓

Render Phase

↓

Calculate New Total

↓

Update Cart

↓

Commit Phase

↓

Browser Shows New Price
```

---

# What Happens If Render Is Interrupted?

Suppose React is rendering a large dashboard.

```
Dashboard

↓

Chart

↓

Graph

↓

User Clicks Search
```

With Fiber,

React can pause the dashboard rendering,

handle the more important search update,

then continue rendering the dashboard later.

The Commit Phase only happens once React has finished preparing a consistent update.

---

# Common Interview Questions

## 1. What Are the Render and Commit Phases?

**Answer:**

React updates the UI in two phases:

- **Render Phase:** React calculates what changes are needed by rendering components and comparing Virtual DOM trees.
- **Commit Phase:** React applies those changes to the Real DOM and runs the appropriate effects.

---

## 2. What Happens During the Render Phase?

**Answer:**

React executes component functions, processes Hooks, creates a new Virtual DOM, compares it with the previous Virtual DOM, and determines what should change. No DOM updates occur during this phase.

---

## 3. What Happens During the Commit Phase?

**Answer:**

React updates the Real DOM, runs `useLayoutEffect`, allows the browser to paint the updated UI, and then runs `useEffect`.

---

## 4. Which Phase Can Be Interrupted?

**Answer:**

The **Render Phase** can be paused, resumed, restarted, or interrupted by Fiber.

The **Commit Phase** cannot be interrupted because DOM updates must be applied consistently.

---

## 5. Why Does React Separate These Phases?

**Answer:**

Separating planning from execution allows React to optimize rendering, minimize expensive DOM operations, and keep applications responsive.

---

## 6. Does React Update the DOM During Rendering?

**Answer:**

No.

During the Render Phase, React only calculates changes.

The Real DOM is updated only during the Commit Phase.

---

# Easy Memory Trick

Imagine preparing dinner.

### Render Phase

```
Read Recipe

↓

Cut Vegetables

↓

Prepare Ingredients
```

No food is served yet.

---

### Commit Phase

```
Cook Food

↓

Serve Dinner
```

Now everyone can eat.

React follows the same sequence.

- **Render = Plan and Prepare**
- **Commit = Apply and Show**

---

# Summary

- React updates the UI in two distinct phases: **Render** and **Commit**.
- The **Render Phase** calculates what needs to change by rendering components and comparing Virtual DOM trees.
- During the Render Phase, the Real DOM is **not** modified.
- The **Commit Phase** applies the calculated updates to the Real DOM.
- `useLayoutEffect` runs during the Commit Phase **before** the browser paints, while `useEffect` runs **after** the browser paints.
- Fiber allows the **Render Phase** to be paused, resumed, and prioritized, but the **Commit Phase** always runs synchronously to keep the UI consistent.
- A simple way to remember:
  - **Render Phase → Think and Plan**
  - **Commit Phase → Apply and Display**