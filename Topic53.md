# 79. What is Virtualization?

## Simple Definition

**Virtualization** (also called **Windowing**) is a technique where React **renders only the items that are currently visible on the screen instead of rendering the entire list.**

In simple words,

> **Instead of creating thousands of components, React creates only the few that the user can actually see.**

As the user scrolls, React removes items that go out of view and renders the new ones that come into view.

---

# Why Do We Need Virtualization?

Imagine an e-commerce website.

```
Products

↓

100,000 Products
```

Without virtualization,

React tries to render

```
100,000 Components

↓

100,000 DOM Elements

↓

Huge Memory Usage

↓

Slow Initial Load

↓

Laggy Scrolling
```

Even though the user can only see about **20 products** at a time.

Most of that work is unnecessary.

---

# Real-Life Analogy: Library

Imagine a library with **100,000 books**.

When someone asks for one book,

do you place all 100,000 books on the table?

No.

You bring only the book the reader needs.

When they ask for another,

you replace it with the new one.

Virtualization works exactly like that.

---

# Without Virtualization

Suppose we have

```jsx
products.map(product => (

  <ProductCard
    key={product.id}
    product={product}
  />

));
```

If

```
products = 100,000
```

React creates

```
Product 1

Product 2

Product 3

...

Product 100000
```

Everything exists in memory and the DOM.

---

# With Virtualization

Suppose only **20 products** fit on the screen.

React renders

```
Product 451

Product 452

Product 453

...

Product 470
```

Only these items exist in the DOM.

---

# What Happens While Scrolling?

Initially

```
Visible

451

452

453

...

470
```

User scrolls.

Now React removes

```
451

452

453
```

and creates

```
471

472

473
```

The total number of rendered components stays almost the same.

---

# Visual Flow

Without Virtualization

```
100,000 Items

↓

Render All

↓

Huge DOM

↓

Slow
```

---

With Virtualization

```
100,000 Items

↓

Render 20

↓

Scroll

↓

Replace Old Items

↓

Render Next 20
```

Much faster.

---

# How Virtualization Works Internally

React (or a virtualization library) calculates:

```
Current Scroll Position

↓

Which Items Are Visible?

↓

Render Only Those Items

↓

Recycle or Remove Hidden Items

↓

Continue Scrolling
```

The browser only manages a small number of DOM elements.

---

# Example

Imagine

```
Height of each row = 50px

Screen height = 500px
```

Visible rows

```
500 / 50

=

10 Rows
```

Even if there are

```
10,000 Rows
```

React renders only about

```
10–15 Rows
```

(often a few extra rows are rendered as a buffer for smoother scrolling).

---

# Popular Libraries

The most widely used libraries are:

### 1. react-window ✅

- Lightweight
- Fast
- Easy to use
- Recommended for most projects

---

### 2. react-virtualized

- More features
- Supports tables, grids, and complex layouts
- Larger bundle size

---

### 3. TanStack Virtual

- Modern virtualization library
- Works well with React and other frameworks
- Flexible and actively maintained

---

# Real-World Applications

Virtualization is commonly used in:

- Amazon product lists
- LinkedIn connections
- Gmail inbox
- WhatsApp chat history
- Facebook feeds
- X (Twitter) timeline
- File explorers
- Admin dashboards
- Analytics tables

---

# Virtualization + Infinite Scrolling

These concepts are different but often used together.

Infinite Scrolling

```
Load More Data
```

Virtualization

```
Render Only Visible Data
```

Together

```
Load 20 More

↓

Render Only Visible Items
```

Excellent performance.

---

# Virtualization + Pagination

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

Virtualization

```
Current Page

↓

Render Only Visible Items
```

Both techniques can complement each other.

---

# Benefits

Virtualization provides:

- Faster rendering
- Smooth scrolling
- Lower memory usage
- Smaller DOM
- Better CPU utilization
- Better battery life
- Improved user experience

---

# Without vs With Virtualization

| Without Virtualization | With Virtualization |
|-------------------------|---------------------|
| Render every item | Render only visible items |
| Large DOM | Small DOM |
| High memory usage | Low memory usage |
| Slow scrolling | Smooth scrolling |
| Long initial render | Fast initial render |
| Poor performance with huge lists | Excellent performance with huge lists |

---

# Virtualization vs Pagination

| Virtualization | Pagination |
|----------------|------------|
| Renders only visible items | Loads a fixed number of items per page |
| User scrolls continuously | User changes pages |
| Great for long continuous lists | Great for page-based navigation |
| Can work with all loaded data | Often loads only one page of data |

---

# Virtualization vs Infinite Scrolling

| Virtualization | Infinite Scrolling |
|----------------|--------------------|
| Controls **how much is rendered** | Controls **how much data is loaded** |
| Reduces DOM size | Reduces initial network requests |
| Works with loaded data | Fetches additional data as needed |
| Often combined with infinite scrolling | Often combined with virtualization |

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

Even though there are **100,000 products**, only the rows needed for the current viewport are rendered.

---

# Common Interview Questions

## 1. What Is Virtualization?

**Answer:**

Virtualization is a rendering technique where only the items visible in the viewport are rendered, while off-screen items are not kept in the DOM.

---

## 2. Why Is Virtualization Important?

**Answer:**

It significantly improves performance by reducing the number of rendered DOM elements, lowering memory usage, and enabling smooth scrolling for very large lists.

---

## 3. Which React Libraries Support Virtualization?

**Answer:**

Popular libraries include:

- `react-window`
- `react-virtualized`
- `TanStack Virtual`

---

## 4. Does Virtualization Reduce Data Size?

**Answer:**

No.

Virtualization reduces the number of **rendered DOM elements**, not necessarily the amount of data loaded into memory. Data loading is handled separately through techniques like pagination or infinite scrolling.

---

## 5. Can Virtualization Be Used with Infinite Scrolling?

**Answer:**

Yes.

They solve different problems and are commonly used together. Infinite scrolling loads more data, while virtualization limits how much of that data is rendered.

---

## 6. What Is Another Name for Virtualization?

**Answer:**

It is also commonly called **Windowing**, because only the "window" (visible portion) of the list is rendered.

---

# Senior Interview Tip ⭐

If you're asked:

> **"How would you display one million records in React?"**

A strong answer would be:

1. Fetch data incrementally using **pagination** or **infinite scrolling**.
2. Render visible rows only using **virtualization** (`react-window`, `TanStack Virtual`, or `react-virtualized`).
3. Memoize row components with **`React.memo`**.
4. Memoize expensive calculations with **`useMemo`**.
5. Use stable keys and profile the application with **React DevTools Profiler** to verify improvements.

This shows you understand both **data loading** and **rendering optimization**.

---

# Easy Memory Trick

Imagine a train.

The train has **1,000 seats**, but only **50 passengers** are traveling right now.

You don't build a train with every possible passenger standing inside.

Instead,

```
Passengers Enter

↓

Travel

↓

Passengers Exit

↓

New Passengers Enter
```

Only the people currently traveling occupy the train.

React virtualization works the same way.

- **Large List = Entire City**
- **Visible Items = People on the Train**
- **Scroll = Passengers Getting On and Off**

---

# Summary

- **Virtualization (Windowing)** is a technique that renders only the items currently visible in the viewport.
- It dramatically reduces the number of DOM elements, improving rendering speed, memory usage, and scrolling performance.
- As the user scrolls, off-screen items are removed and newly visible items are rendered.
- Popular React libraries include **`react-window`**, **`react-virtualized`**, and **TanStack Virtual**.
- Virtualization is different from **pagination** and **infinite scrolling**, but it is often used together with them for maximum performance.
- For large datasets, virtualization is one of the most effective optimization techniques available in React.