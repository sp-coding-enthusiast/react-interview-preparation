# 80. What is Windowing?

## Simple Definition

**Windowing** is a performance optimization technique where **only the items currently visible inside the browser's viewport (window) are rendered.**

In simple words,

> **React creates only the components that fit inside the visible window and ignores the rest until they are needed.**

**Windowing** and **Virtualization** are often used interchangeably in React.

---

# Why Is It Called "Windowing"?

Imagine your screen is like a small window.

Behind that window exists a huge list.

```
1

2

3

...

100000
```

You cannot see all 100,000 items.

You can only see a small portion.

For example,

```
451

452

453

454

455
```

React renders only that visible "window."

As you scroll,

the window moves.

React updates only the visible portion.

---

# Real-Life Analogy: Looking Through a Train Window

Imagine you're traveling by train.

Outside there are

```
Mountains

Trees

Buildings

Rivers

Cities
```

Can you see everything at once?

No.

You only see what is currently visible through the window.

As the train moves,

the view changes.

The old scenery disappears.

New scenery appears.

React's Windowing works exactly like this.

---

# Why Do We Need Windowing?

Imagine an online shopping website.

```
100,000 Products
```

Without Windowing,

React creates

```
100,000 Product Cards

↓

100,000 DOM Elements

↓

Huge Memory Usage

↓

Slow Rendering
```

But users can only see about

```
20 Products
```

Most of that work is unnecessary.

---

# Without Windowing

```
Item 1

Item 2

Item 3

...

Item 100000
```

Everything exists in the DOM.

Memory usage is very high.

---

# With Windowing

Suppose only 15 items fit on the screen.

React renders

```
Item 520

Item 521

Item 522

...

Item 534
```

Only those items exist in the DOM.

---

# What Happens During Scrolling?

Initially

```
520

521

522

...

534
```

User scrolls.

React removes

```
520

521

522
```

and renders

```
535

536

537
```

The DOM size stays almost constant.

---

# Visual Flow

```
Large List

↓

Viewport

↓

Render Visible Items

↓

User Scrolls

↓

Remove Hidden Items

↓

Render New Visible Items
```

This process repeats continuously.

---

# How Windowing Works Internally

The library calculates:

```
Scroll Position

↓

Visible Area

↓

Start Index

↓

End Index

↓

Render Only Those Items
```

Everything outside the visible range is skipped.

---

# Example

Suppose

```
Item Height = 40px

Viewport Height = 400px
```

Visible items

```
400 / 40

=

10 Items
```

React renders approximately

```
10–15 Items
```

(often with a few extra items above and below the viewport for smoother scrolling).

Even if there are

```
1,000,000 Items
```

the DOM still contains only a small number of elements.

---

# Popular React Libraries

### 1. react-window ✅

Most commonly used.

Features:

- Lightweight
- Fast
- Easy to learn

---

### 2. react-virtualized

Supports:

- Tables
- Grids
- Masonry layouts
- Infinite loading

Larger than `react-window`.

---

### 3. TanStack Virtual

Modern virtualization library.

Supports:

- React
- Vue
- Solid
- Other frameworks

---

# Real-World Examples

Windowing is used in:

- Gmail inbox
- WhatsApp message history
- Amazon product search
- LinkedIn connections
- Facebook feeds
- X (Twitter) timeline
- Admin dashboards
- File explorers
- Data grids
- Log viewers

---

# Windowing vs Virtualization

In React,

these terms usually mean the same thing.

| Windowing | Virtualization |
|------------|----------------|
| Renders only visible items | Renders only visible items |
| Small DOM | Small DOM |
| Improves scrolling | Improves scrolling |
| Better performance | Better performance |

The only difference is terminology.

- **Windowing** emphasizes the visible "window."
- **Virtualization** emphasizes creating a virtual view instead of rendering everything.

---

# Windowing vs Pagination

Pagination

```
Page 1

↓

20 Items

↓

Page 2

↓

20 Items
```

The user changes pages.

---

Windowing

```
Continuous Scroll

↓

Render Visible Items Only
```

The user scrolls naturally.

---

# Windowing vs Infinite Scrolling

Infinite Scrolling

```
Load More Data
```

Windowing

```
Render Only Visible Data
```

These are different concepts.

They are commonly used together.

---

# Example Using react-window

```jsx
import { FixedSizeList } from "react-window";

function Row({ index, style }) {

  return (

    <div style={style}>

      Product {index}

    </div>

  );

}

export default function App() {

  return (

    <FixedSizeList

      height={500}
      width={400}
      itemCount={100000}
      itemSize={50}

    >

      {Row}

    </FixedSizeList>

  );

}
```

Even though there are **100,000 products**, only the rows visible in the viewport are rendered.

---

# Benefits

Windowing provides:

- Fast rendering
- Smooth scrolling
- Low memory usage
- Small DOM size
- Better responsiveness
- Lower CPU usage
- Better battery life

---

# Without vs With Windowing

| Without Windowing | With Windowing |
|-------------------|----------------|
| Render all items | Render only visible items |
| Large DOM | Small DOM |
| High memory usage | Low memory usage |
| Slow scrolling | Smooth scrolling |
| Slow initial load | Fast initial load |
| Poor performance on huge lists | Excellent performance |

---

# Common Interview Questions

## 1. What Is Windowing?

**Answer:**

Windowing is a rendering technique where only the items visible in the viewport are rendered, while off-screen items are removed from the DOM.

---

## 2. Is Windowing the Same as Virtualization?

**Answer:**

Yes.

In React, **Windowing** and **Virtualization** are generally the same concept. The terms are often used interchangeably.

---

## 3. Why Is Windowing Important?

**Answer:**

It improves performance by reducing the number of rendered DOM elements, lowering memory usage, and providing smooth scrolling for very large lists.

---

## 4. Which Libraries Support Windowing?

**Answer:**

Popular libraries include:

- `react-window`
- `react-virtualized`
- `TanStack Virtual`

---

## 5. Does Windowing Reduce Network Requests?

**Answer:**

No.

Windowing only limits the number of **rendered elements**.

Reducing network requests is handled by techniques such as pagination, infinite scrolling, or lazy data fetching.

---

## 6. Can Windowing and Infinite Scrolling Be Used Together?

**Answer:**

Yes.

Infinite scrolling controls **how much data is loaded**, while windowing controls **how much of that loaded data is rendered**.

---

# Senior Interview Tip ⭐

If an interviewer asks:

> **"What's the difference between Windowing and Virtualization?"**

A strong answer is:

> **In React, there is essentially no practical difference. Both terms describe rendering only the visible portion of a large list to improve performance. "Windowing" focuses on the visible viewport (window), while "Virtualization" is the broader term used by most libraries and documentation.**

This shows both conceptual understanding and awareness of industry terminology.

---

# Easy Memory Trick

Imagine standing at a window in your house.

Outside there are

```
100,000 Cars
```

Can you see them all?

No.

You only see the cars currently passing your window.

As time passes,

new cars appear,

old cars disappear.

React behaves the same way.

- **Entire List = Road**
- **Viewport = Window**
- **Visible Items = Cars Passing By**
- **Scrolling = Looking at a Different Part of the Road**

---

# Summary

- **Windowing** is a technique that renders only the items visible in the browser's viewport.
- In React, **Windowing** and **Virtualization** refer to the same optimization strategy.
- As users scroll, off-screen items are removed and newly visible items are rendered.
- Windowing significantly reduces DOM size, memory usage, and rendering work.
- Popular libraries include **`react-window`**, **`react-virtualized`**, and **TanStack Virtual**.
- Windowing is often combined with **pagination**, **infinite scrolling**, and **lazy loading** to build highly scalable React applications.