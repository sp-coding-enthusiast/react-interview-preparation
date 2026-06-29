# 88. How to Reduce Time To Interactive (TTI)?

## Simple Definition

**Time To Interactive (TTI)** is the time it takes for a webpage to become **fully usable** after a user opens it.

In simple words,

> **TTI is the time between opening a webpage and being able to interact with it without delays.**

A page may **look loaded**, but if buttons, inputs, or menus don't respond yet, it is **not interactive**.

---

# Real-Life Analogy: Opening a Restaurant

Imagine you arrive at a restaurant.

### Scenario 1

```
Restaurant Open

↓

Lights On

↓

Chef Ready

↓

Waiters Ready

↓

You Can Order
```

Everything works immediately.

Low TTI.

---

### Scenario 2

```
Restaurant Open

↓

Lights On

↓

Kitchen Not Ready

↓

Wait 5 Minutes

↓

Now Order
```

The restaurant looks open but isn't actually usable.

High TTI.

A React application behaves the same way.

---

# Why Is TTI Important?

Imagine a banking application.

```
Page Opens

↓

Transfer Button Visible

↓

Button Doesn't Work

↓

User Clicks Again

↓

Nothing Happens
```

Poor user experience.

Users often assume the application is broken.

---

# Visual Example

```
User Opens Website

↓

HTML Loaded

↓

CSS Loaded

↓

JavaScript Downloading

↓

JavaScript Executing

↓

React Hydrates

↓

Application Interactive
```

Everything before the last step contributes to TTI.

---

# What Increases TTI?

Large JavaScript bundles

```
15 MB Bundle

↓

Long Download

↓

Slow TTI
```

---

Heavy JavaScript execution

```
Download Complete

↓

Execute Huge Script

↓

Main Thread Blocked
```

---

Large images

```
Download Large Images

↓

Slower Initial Loading
```

---

Too many API requests

```
10 API Calls

↓

Wait

↓

Render
```

---

Rendering thousands of components

```
50,000 DOM Elements

↓

Slow Rendering
```

---

# Techniques to Reduce TTI

---

# 1. Code Splitting ⭐⭐⭐⭐⭐

Instead of

```
Download

Entire App

↓

15 MB
```

Split into

```
Home

Dashboard

Reports
```

Download only the current page.

Example

```jsx
const Dashboard = lazy(() =>

  import("./Dashboard")

);
```

---

# 2. Lazy Loading ⭐⭐⭐⭐⭐

Load components only when needed.

Instead of

```
Download Everything
```

Use

```
Home

↓

Later

↓

Reports
```

This reduces the amount of JavaScript needed before the app becomes interactive.

---

# 3. Tree Shaking ⭐⭐⭐⭐⭐

Remove unused code.

```
100 Functions

↓

Use 10

↓

Remove 90
```

Smaller bundle.

---

# 4. Minification

Before

```javascript
function calculateTotal(price, tax) {

  return price + tax;

}
```

After

```javascript
function a(b,c){return b+c}
```

Less JavaScript.

Faster download and parsing.

---

# 5. Compression (Gzip/Brotli)

Example

```
2 MB

↓

Brotli

↓

350 KB
```

Browser downloads much less data.

---

# 6. Optimize Images

Bad

```
12 MB Banner
```

Better

```
250 KB WebP
```

Techniques:

- Resize images
- Compress images
- Use WebP or AVIF
- Lazy load off-screen images

---

# 7. Lazy Load Images

Instead of

```
100 Images

↓

Download All
```

Download only

```
Visible Images
```

Example

```html
<img

loading="lazy"

src="product.jpg"

alt="Product"

/>
```

---

# 8. Reduce Initial API Calls

Bad

```
Dashboard

↓

10 APIs

↓

Wait

↓

Render
```

Better

```
Dashboard

↓

Load Essential Data

↓

Render

↓

Fetch Remaining Data
```

Prioritize only what's needed for the first screen.

---

# 9. Optimize React Rendering

Avoid unnecessary re-renders.

Use

- `React.memo`
- `useMemo`
- `useCallback`

when profiling shows they are beneficial.

---

# 10. Virtualization

Instead of

```
Render

50,000 Rows
```

Render

```
20 Visible Rows
```

Huge performance improvement for large lists.

---

# 11. Use a CDN

Serve assets from servers closer to users.

Benefits:

- Faster downloads
- Lower latency
- Better caching

---

# 12. Reduce Third-Party Libraries

Instead of

```
Moment.js

+

Huge Chart Library

+

Unused Utilities
```

Use lighter alternatives where appropriate and remove unused dependencies.

---

# 13. Efficient State Management

Bad

```
Global State

↓

Entire App Re-renders
```

Better

```
Local State

↓

Only Necessary Components Re-render
```

---

# 14. Avoid Blocking the Main Thread

Bad

```javascript
for (

let i = 0;

i < 1000000000;

i++

) {}
```

The browser freezes.

Break heavy work into smaller chunks or move CPU-intensive work to a **Web Worker** so the UI thread stays responsive.

---

# 15. Server-Side Rendering (SSR)

Instead of

```
Download JS

↓

Render Page
```

Server sends pre-rendered HTML.

```
Server

↓

HTML

↓

Browser Displays Quickly
```

Users see meaningful content sooner.

---

# 16. Streaming SSR

Instead of waiting for the entire page,

send HTML progressively.

```
Header

↓

Content

↓

Footer
```

Users see content earlier while the rest continues loading.

---

# 17. Selective Hydration

Hydrate only the parts users interact with first.

```
Header

↓

Interactive

↓

Footer Later
```

This reduces work during initial load.

---

# 18. Prefetch & Preload (When Appropriate)

Modern browsers can fetch resources before they are needed.

Examples:

- Prefetch likely future routes
- Preload critical fonts or scripts

Use these carefully to avoid downloading unnecessary resources.

---

# Performance Optimization Flow

```
Large Bundle

↓

Tree Shaking

↓

Code Splitting

↓

Lazy Loading

↓

Minification

↓

Compression

↓

Optimized Images

↓

Fast Download

↓

Fast JavaScript Execution

↓

Low TTI
```

---

# Measuring TTI

Use tools like:

- Chrome Lighthouse
- Chrome DevTools Performance Panel
- Web Vitals
- React DevTools Profiler (for React rendering performance)

These tools help identify what is delaying interactivity.

---

# Real-World Example

Suppose an admin dashboard.

Without optimization

```
12 MB Bundle

↓

40 Charts

↓

100 APIs

↓

6 Second TTI
```

Users wait before interacting.

---

Optimized dashboard

```
Tree Shaking

↓

Code Splitting

↓

Lazy Loading

↓

Virtualization

↓

Image Optimization

↓

Brotli Compression

↓

1.5 Second TTI
```

The application becomes interactive much sooner.

---

# Common Interview Questions

## 1. What Is Time To Interactive (TTI)?

**Answer:**

TTI is the time from when a page starts loading until it becomes fully interactive, meaning users can click buttons, type in inputs, and interact with the UI without significant delays.

---

## 2. What Usually Causes a High TTI?

**Answer:**

Common causes include:

- Large JavaScript bundles
- Long JavaScript execution
- Heavy rendering
- Large images
- Too many initial API requests
- Excessive third-party libraries

---

## 3. How Can You Reduce TTI?

**Answer:**

- Code Splitting
- Lazy Loading
- Tree Shaking
- Dynamic Imports
- Minification
- Gzip/Brotli Compression
- Image Optimization
- Virtualization
- Reducing unnecessary re-renders
- Server-Side Rendering
- Streaming SSR
- Selective Hydration

---

## 4. Does SSR Reduce TTI?

**Answer:**

SSR helps users **see content sooner**, improving the perceived loading experience. However, true interactivity still depends on JavaScript being downloaded and hydrated. Combining SSR with code splitting and selective hydration can significantly improve the overall experience.

---

## 5. Which Optimization Usually Has the Biggest Impact?

**Answer:**

For many React applications, the biggest improvements come from:

1. Code Splitting
2. Lazy Loading
3. Tree Shaking
4. Compression (Brotli/Gzip)
5. Image Optimization
6. Reducing unnecessary JavaScript execution

---

# Senior Interview Tip ⭐

If asked:

> **"Your React application takes 8 seconds before users can click anything. What would you do?"**

A strong answer would be:

1. Measure the application using **Lighthouse** and the **Chrome Performance Panel**.
2. Profile rendering with the **React DevTools Profiler**.
3. Reduce the initial JavaScript bundle with **Code Splitting**, **Tree Shaking**, and **Lazy Loading**.
4. Compress assets using **Brotli/Gzip** and optimize images.
5. Reduce unnecessary API calls and defer non-critical data.
6. Eliminate unnecessary re-renders with `React.memo`, `useMemo`, and `useCallback` where appropriate.
7. Consider **SSR**, **Streaming SSR**, and **Selective Hydration** for faster initial rendering.
8. Measure again to verify that TTI has improved.

---

# Easy Memory Trick

Imagine opening a new laptop.

Without optimization

```
Power On

↓

Wait

↓

Wait

↓

Wait

↓

Desktop Ready
```

High TTI.

---

With optimization

```
Power On

↓

Desktop Ready

↓

Background Apps Load Later
```

Low TTI.

React applications work the same way.

- **Tree Shaking = Remove Unused Apps**
- **Code Splitting = Install in Smaller Parts**
- **Lazy Loading = Open Apps Only When Needed**
- **Compression = Smaller Downloads**
- **SSR = Show the Screen Earlier**
- **Selective Hydration = Make Important Buttons Work First**

---

# Summary

- **Time To Interactive (TTI)** measures how long it takes for a webpage to become fully responsive to user interactions.
- High TTI is commonly caused by large JavaScript bundles, expensive JavaScript execution, excessive rendering, heavy images, and too many initial network requests.
- The most effective ways to reduce TTI include:
  - **Code Splitting**
  - **Lazy Loading**
  - **Tree Shaking**
  - **Minification**
  - **Brotli/Gzip Compression**
  - **Image Optimization**
  - **Virtualization**
  - **Reducing unnecessary re-renders**
  - **Server-Side Rendering (SSR)**
  - **Streaming SSR**
  - **Selective Hydration**
- Always **measure first**, optimize based on evidence, and **measure again** to confirm improvements.