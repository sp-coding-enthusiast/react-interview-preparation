# 83. What is Code Splitting?

## Simple Definition

**Code Splitting** is a technique where **a large JavaScript bundle is divided into smaller bundles that can be downloaded separately.**

In simple words,

> **Instead of downloading the entire application at once, the browser downloads only the code it needs right now and loads the remaining code later.**

Code Splitting is one of the most important performance optimization techniques in modern React applications.

---

# Why Do We Need Code Splitting?

Imagine a React application with:

```
Home

Products

Orders

Dashboard

Reports

Admin Panel

Analytics

Settings
```

When the user visits

```
Home
```

should the browser also download

```
Reports

Analytics

Admin Panel
```

No.

The user may never visit those pages.

Downloading everything wastes:

- Time
- Bandwidth
- Memory

---

# Real-Life Analogy: School Books

Imagine a student has

```
Math

Science

History

English

Computer Science

Geography
```

Should the student carry every book every day?

```
Carry All Books

↓

Heavy Bag

↓

Difficult Journey
```

Instead,

carry only today's books.

```
Today's Subjects

↓

Carry Only Required Books
```

Code Splitting follows the same principle.

---

# Without Code Splitting

Suppose your application size is

```
10 MB
```

The browser downloads

```
Entire 10 MB

↓

Wait

↓

Render Application
```

Even if the user only visits the Home page.

---

# With Code Splitting

The application is divided into:

```
Home.js

Products.js

Dashboard.js

Reports.js

Settings.js
```

Now the browser downloads

```
Home.js

↓

Render Home Page
```

Later,

when the user opens

```
Reports
```

React downloads

```
Reports.js
```

only at that time.

---

# Visual Example

Without Code Splitting

```
App Bundle

↓

10 MB Download

↓

Show Home
```

---

With Code Splitting

```
Home Bundle

↓

500 KB Download

↓

Show Home

↓

User Opens Reports

↓

Download Reports Bundle
```

Much faster initial loading.

---

# How Code Splitting Works

```
Large Bundle

↓

Split into Small Chunks

↓

Download First Chunk

↓

User Navigates

↓

Download Next Chunk

↓

Repeat
```

Each chunk is loaded only when required.

---

# What Creates Code Splits?

Modern bundlers automatically create separate chunks.

Popular bundlers include:

- Webpack
- Vite (uses Rollup for production builds)
- Rollup
- Parcel

When React sees a **dynamic import**, the bundler creates a separate JavaScript file.

---

# Dynamic Import

Normal import

```jsx
import Dashboard from "./Dashboard";
```

This is **eager loading**.

The Dashboard code is included in the initial bundle.

---

Dynamic import

```jsx
import("./Dashboard");
```

This tells the bundler:

```
Create Separate Chunk
```

Download it later.

---

# React.lazy()

React provides built-in support.

```jsx
import { lazy } from "react";

const Dashboard = lazy(() =>

  import("./Dashboard")

);
```

React downloads

```
Dashboard.js
```

only when it is rendered.

---

# Why Suspense Is Needed

While the chunk is downloading,

React displays fallback content.

```jsx
<Suspense

fallback={<Loading />}

>

  <Dashboard />

</Suspense>
```

Flow

```
Navigate

↓

Downloading Bundle

↓

Loading Spinner

↓

Bundle Ready

↓

Render Dashboard
```

---

# Route-Based Code Splitting

This is the most common approach.

Routes

```
/

↓

/products

↓

/orders

↓

/reports

↓

/settings
```

Each route gets its own bundle.

Example

```
Home Bundle

Products Bundle

Orders Bundle

Reports Bundle
```

The browser downloads only the route the user visits.

---

# Component-Based Code Splitting

Large components can also be split.

Imagine

```
Dashboard

↓

Chart

↓

Map

↓

Analytics

↓

Reports
```

Instead of downloading everything,

download

```
Chart

↓

Only When Opened
```

---

# Real-World Example

Imagine Netflix.

When you open Netflix,

does it download every movie page?

No.

```
Home Page

↓

Download Home

↓

User Opens Movie

↓

Download Movie Details

↓

User Plays Trailer

↓

Download Player
```

Exactly how Code Splitting works.

---

# Benefits

Code Splitting provides:

- Faster initial page load
- Smaller JavaScript bundles
- Lower memory usage
- Reduced bandwidth
- Better performance
- Improved user experience

---

# Code Splitting vs Lazy Loading

Many developers confuse these.

### Code Splitting

```
Split Code

↓

Multiple Bundles
```

---

### Lazy Loading

```
Download Bundle

Only When Needed
```

Example

```
10 MB App

↓

Split

↓

1 MB

2 MB

3 MB

4 MB
```

This is Code Splitting.

---

Later

```
Download

Only 1 MB
```

This is Lazy Loading.

---

# Together

```
Large Bundle

↓

Code Splitting

↓

Small Bundles

↓

Lazy Loading

↓

Download Required Bundle
```

They usually work together.

---

# Code Splitting vs Tree Shaking

Tree Shaking

```
Remove

Unused Code
```

---

Code Splitting

```
Split

Remaining Code

Into Chunks
```

They solve different problems.

---

# Real Build Example

Suppose your project contains:

```
Home

Products

Orders

Reports

Charts

Admin
```

After building,

the output may look like:

```
main.js

home.chunk.js

products.chunk.js

reports.chunk.js

admin.chunk.js
```

Each chunk is downloaded independently.

---

# Complete Flow

```
Application

↓

Split Into Bundles

↓

User Opens Home

↓

Download Home Bundle

↓

User Opens Reports

↓

Download Reports Bundle

↓

User Opens Admin

↓

Download Admin Bundle
```

---

# Common Interview Questions

## 1. What Is Code Splitting?

**Answer:**

Code Splitting is the process of dividing a large JavaScript bundle into smaller chunks so that only the required code is downloaded initially.

---

## 2. Why Is Code Splitting Important?

**Answer:**

It reduces the initial bundle size, improves page load performance, lowers bandwidth usage, and provides a better user experience.

---

## 3. How Do You Implement Code Splitting in React?

**Answer:**

By using dynamic imports (`import()`) together with `React.lazy()` and `Suspense`.

Example:

```jsx
const Reports = lazy(() => import("./Reports"));
```

---

## 4. What Is the Difference Between Code Splitting and Lazy Loading?

**Answer:**

- **Code Splitting** divides the application into multiple bundles.
- **Lazy Loading** loads those bundles only when they are required.

---

## 5. What Is Route-Based Code Splitting?

**Answer:**

Each application route (such as `/home`, `/products`, `/reports`) is built into its own bundle, which is downloaded only when the user navigates to that route.

---

## 6. Does React Perform Code Splitting Automatically?

**Answer:**

React itself does not split code automatically.

Bundlers like **Webpack**, **Vite (Rollup)**, or **Parcel** generate separate chunks when they encounter **dynamic imports (`import()`)**. React uses those chunks through APIs like `React.lazy()`.

---

# Senior Interview Tip ⭐

If you're asked:

> **"How would you improve the initial load time of a React application?"**

A strong answer would be:

1. Split the application into smaller bundles using **Code Splitting**.
2. Lazy load routes and heavy components with `React.lazy()` and `Suspense`.
3. Optimize images and other static assets.
4. Use **Tree Shaking** to remove unused code.
5. Enable compression (Gzip/Brotli) and serve assets through a CDN.
6. Use **Virtualization** for large lists.
7. Measure improvements using **Lighthouse**, browser performance tools, and the **React DevTools Profiler**.

---

# Easy Memory Trick

Imagine moving to a new apartment.

Without Code Splitting

```
Move

Everything

In One Trip
```

Heavy and slow.

---

With Code Splitting

```
Move

Kitchen

Today

↓

Bedroom

Tomorrow

↓

Books

Later
```

Smaller trips are much easier.

React follows the same idea.

- **Code Splitting = Divide the Application**
- **Lazy Loading = Bring Each Part Only When Needed**
- **Suspense = Keep the User Informed While Waiting**

---

# Summary

- **Code Splitting** divides a large JavaScript bundle into smaller bundles (chunks).
- These chunks can be downloaded independently, reducing the initial page load time.
- React implements Code Splitting using **dynamic imports (`import()`)**, **`React.lazy()`**, and **`Suspense`**.
- **Route-based** and **component-based** code splitting are the two most common approaches.
- **Code Splitting** creates separate bundles, while **Lazy Loading** determines when those bundles are downloaded.
- Together, Code Splitting and Lazy Loading are essential techniques for building fast, scalable React applications.