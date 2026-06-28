# 56. What is Reconciliation?

## Simple Definition

**Reconciliation** is the process React uses to **compare the old Virtual DOM with the new Virtual DOM and determine the smallest set of changes needed to update the real DOM.**

In simple words,

> **Reconciliation is React's way of figuring out "What actually changed?" before updating the browser.**

Instead of rebuilding the whole page, React updates **only the parts that changed**.

---

# Real-Life Analogy: Spot the Difference

Imagine you have two pictures.

Picture 1

```
🐶 🐱 🐰 🐼
```

Picture 2

```
🐶 🐱 🦊 🐼
```

Only one animal changed.

Instead of redrawing the entire picture,

you replace only:

```
🐰

↓

🦊
```

That's exactly what React does during reconciliation.

---

# Why Do We Need Reconciliation?

Imagine an online shopping website.

```
Header

Product List

Cart

Footer
```

You add one product to the cart.

Without reconciliation:

```
Rebuild Entire Page
```

Very slow.

With reconciliation:

```
Update Cart Only
```

Much faster.

---

# Visual Representation

Without Reconciliation

```
User Clicks

↓

Rebuild Everything

↓

Browser Updates Entire Page
```

---

With Reconciliation

```
User Clicks

↓

Compare Old UI

↓

Compare New UI

↓

Update Only Changes
```

---

# Step-by-Step Process

Suppose your component is:

```jsx
function App() {

  return <h1>Hello</h1>;

}
```

React creates a Virtual DOM.

```
Virtual DOM

↓

<h1>Hello</h1>
```

---

Later,

state changes.

```jsx
function App() {

  return <h1>Hello Saurabh</h1>;

}
```

React creates another Virtual DOM.

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

Reconciliation compares them.

Only the text changed.

React updates only the text node.

---

# Real-Life Analogy: House Painting

Imagine painting your house.

One wall becomes dirty.

Would you repaint the entire house?

No.

You repaint only that wall.

React works the same way.

---

# What Does React Compare?

React compares:

- Element type
- Props
- Children
- Keys
- Component tree structure

---

# Example

Old

```jsx
<h1>Hello</h1>
```

New

```jsx
<h1>Welcome</h1>
```

React keeps:

```
<h1>
```

Updates only:

```
Text
```

---

Another Example

Old

```jsx
<h1>Hello</h1>
```

New

```jsx
<p>Hello</p>
```

The element type changed.

React removes:

```
<h1>
```

Creates:

```
<p>
```

---

# Reconciliation Flow

```
State Changes

↓

Component Re-renders

↓

New Virtual DOM

↓

Compare with Old Virtual DOM

↓

Find Differences

↓

Update Real DOM
```

---

# Benefits

- Faster UI updates
- Fewer DOM operations
- Better performance
- Smooth user experience

---

# Summary

- Reconciliation is React's process of comparing two Virtual DOM trees.
- It identifies the minimum number of changes needed.
- Only changed parts of the real DOM are updated.

---

# 57. Explain React Diffing Algorithm

## Simple Definition

The **Diffing Algorithm** is the algorithm React uses during reconciliation to compare the **old Virtual DOM** with the **new Virtual DOM**.

In simple words,

> **Diffing is React's "spot the difference" algorithm.**

It determines exactly what changed.

---

# Real-Life Analogy: Newspaper Editor

Imagine yesterday's newspaper.

```
Page 1

Page 2

Page 3
```

Today's newspaper

```
Page 1

Page 2 (Updated)

Page 3
```

The editor doesn't rewrite every page.

Only Page 2 changes.

That's React's diffing process.

---

# How Diffing Works

Suppose

Old

```jsx
<div>

  <h1>Hello</h1>

</div>
```

New

```jsx
<div>

  <h1>Welcome</h1>

</div>
```

React compares

```
div

↓

Same
```

Then

```
h1

↓

Same
```

Then

```
Text

↓

Different
```

Only the text changes.

---

# Rule 1: Different Element Types

Old

```jsx
<div />
```

New

```jsx
<span />
```

Different types.

React removes:

```
div
```

Creates:

```
span
```

---

# Rule 2: Same Element Type

Old

```jsx
<button>

Save

</button>
```

New

```jsx
<button>

Submit

</button>
```

Same element.

React updates only:

```
Text
```

The button itself stays.

---

# Rule 3: Compare Props

Old

```jsx
<button

  className="red"

>
```

New

```jsx
<button

  className="blue"

>
```

React updates only:

```
className
```

Everything else remains unchanged.

---

# Rule 4: Compare Children

Old

```
A

B

C
```

New

```
A

B

D
```

React updates only:

```
C

↓

D
```

---

# What About Lists?

Old

```
Apple

Banana

Orange
```

New

```
Apple

Banana

Mango
```

React updates only:

```
Orange

↓

Mango
```

When items have stable **keys**, React can correctly identify which items changed, moved, were added, or were removed.

---

# Visual Representation

```
Old Tree

↓

Compare

↓

New Tree

↓

Update Differences
```

---

# Summary

- The Diffing Algorithm compares two Virtual DOM trees.
- It checks element types, props, and children.
- It updates only what actually changed.

---

# 58. Why is React Diffing O(n)?

## First, What is O(n)?

**O(n)** means the work grows **roughly in proportion to the number of elements**.

If there are:

```
10 elements

↓

About 10 comparisons
```

```
100 elements

↓

About 100 comparisons
```

```
1000 elements

↓

About 1000 comparisons
```

The work increases linearly.

---

# Why Not Compare Everything?

Imagine two trees with 1000 nodes.

Finding the absolutely smallest set of edits between arbitrary trees is a much harder computer science problem.

React avoids that expensive approach by making two important assumptions.

---

# React's Two Assumptions

## Assumption 1

**Elements of different types produce different trees.**

Example

Old

```jsx
<div />
```

New

```jsx
<button />
```

React doesn't try to deeply compare them.

It simply replaces the old tree.

---

## Assumption 2

**Developers provide stable `key` props for lists.**

Keys tell React which list item is which.

Example

Old

```
1 Apple

2 Banana

3 Orange
```

New

```
1 Apple

2 Banana

3 Mango
```

React immediately knows only item **3** changed.

---

# Real-Life Analogy: Attendance Register

Teacher has a class list.

```
Roll 1

Roll 2

Roll 3

Roll 4
```

If students keep the same roll numbers,

attendance is easy.

Without roll numbers,

the teacher has to guess who is who.

Keys are like roll numbers.

---

# Why Keys Matter

Suppose

Old

```
A

B

C
```

New

```
C

A

B
```

Without keys,

React may think every position changed.

With keys,

React recognizes that the same items have simply moved.

This leads to fewer unnecessary updates and preserves component state where appropriate.

---

# Visual Example

Without Keys

```
Compare Position

↓

Everything Looks Different
```

With Keys

```
Compare Key

↓

Move Existing Elements
```

---

# Why React Doesn't Use a More Complex Algorithm

A mathematically optimal tree comparison algorithm would be much slower for large applications.

React chooses an algorithm that is:

- Fast
- Predictable
- Good enough for almost all real-world user interfaces

This is why React's diffing algorithm is effectively **O(n)** under its assumptions.

---

# Performance Example

Imagine a page with:

```
1000 Components
```

React walks through the tree.

```
Compare Component 1

↓

Compare Component 2

↓

Compare Component 3

↓

...

↓

Compare Component 1000
```

Each element is visited roughly once.

That's why the work is approximately **O(n)**.

---

# Side-by-Side Comparison

| Scenario | React Behavior |
|----------|----------------|
| Same element type | Reuse element and compare props/children |
| Different element type | Replace the old subtree |
| Same props | No DOM update needed |
| Different props | Update only changed props |
| Lists with keys | Match items by key for efficient updates |
| Lists without keys | React falls back to matching by position, which can cause unnecessary updates and state issues |

---

# Real-Life Example

Imagine editing a Word document.

Old

```
Page 1

Page 2

Page 3
```

You change one sentence on Page 2.

The editor doesn't print a brand-new document.

It updates only the modified sentence.

That's how React's diffing algorithm works.

---

# Common Interview Questions

## 1. What is Reconciliation?

**Answer:**

Reconciliation is React's process of comparing the old Virtual DOM with the new Virtual DOM to determine the minimum changes needed to update the real DOM efficiently.

---

## 2. What is the Diffing Algorithm?

**Answer:**

The Diffing Algorithm is the algorithm React uses during reconciliation to compare two Virtual DOM trees and identify what has changed.

---

## 3. Why is React Diffing O(n)?

**Answer:**

React assumes that:
- Elements of different types produce different trees.
- Developers provide stable `key` props for lists.

These assumptions let React compare trees in roughly linear time instead of using a much more expensive general tree-diff algorithm.

---

## 4. What Happens When Element Types Change?

**Answer:**

React treats them as completely different elements, removes the old subtree, and creates a new one.

---

## 5. Why Are Keys Important?

**Answer:**

Keys help React identify which list items are the same across renders, allowing it to preserve component state and efficiently handle additions, removals, and reordering.

---

## 6. Does React Compare the Real DOM During Diffing?

**Answer:**

No.

React compares the **previous Virtual DOM** with the **new Virtual DOM**.

Only after finding the differences does it update the real DOM.

---

# Easy Memory Trick

Imagine a teacher checking homework.

Old notebook

↓

New notebook

↓

Find changed pages

↓

Correct only those pages

The teacher doesn't reread the entire notebook from scratch.

React behaves the same way.

- **Reconciliation** = The overall process of finding and applying updates.
- **Diffing** = The comparison step inside reconciliation.
- **O(n)** = React examines the tree efficiently by following simple rules instead of performing an expensive exhaustive comparison.

---

# Summary

- **Reconciliation** is the overall process React uses to update the UI efficiently.
- **Diffing** is the comparison algorithm used during reconciliation to compare the old and new Virtual DOM trees.
- React compares element types, props, children, and list keys to determine what changed.
- Elements with the same type are reused whenever possible; elements with different types are replaced.
- Stable **keys** allow React to correctly identify list items across renders, making updates more efficient and preserving component state.
- React's diffing algorithm runs in approximately **O(n)** time because it relies on practical assumptions instead of solving the much more expensive general tree comparison problem.