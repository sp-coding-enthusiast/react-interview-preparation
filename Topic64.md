# 89. Explain Core Web Vitals

## Simple Definition

**Core Web Vitals** are a set of performance metrics created by **Google** to measure **how fast, stable, and responsive a website feels to real users.**

In simple words,

> **Core Web Vitals answer three important questions:**

1. **How quickly does the main content appear?**
2. **How quickly does the page respond when the user interacts?**
3. **Does the page stay visually stable while loading?**

Instead of measuring only technical speed, Core Web Vitals measure **actual user experience.**

---

# Why Were Core Web Vitals Introduced?

Imagine two websites.

### Website A

```
Loads Fast

↓

Buttons Work Immediately

↓

Nothing Moves
```

Users are happy.

---

### Website B

```
Loads Slowly

↓

Button Doesn't Respond

↓

Page Keeps Moving
```

Users get frustrated.

Both websites may technically "load," but their user experience is very different.

Google introduced Core Web Vitals to measure that experience.

---

# The Three Core Web Vitals

```
Core Web Vitals

↓

LCP

↓

INP

↓

CLS
```

Each measures a different aspect of performance.

---

# Easy Way to Remember

Imagine visiting a restaurant.

```
Restaurant

↓

Food Arrives Quickly?

↓

Waiter Responds Quickly?

↓

Table Doesn't Shake?
```

These correspond to:

```
Food Speed

↓

LCP

------------------

Waiter Response

↓

INP

------------------

Stable Table

↓

CLS
```

---

# 1. Largest Contentful Paint (LCP)

Measures:

> **How long it takes for the largest visible content to appear.**

Usually the largest content is:

- Hero image
- Banner
- Large heading
- Main content image
- Large video thumbnail

---

Example

```
User Opens Website

↓

Header Appears

↓

Logo Appears

↓

Hero Image Appears

↓

LCP Recorded
```

---

### Good LCP

```
≤ 2.5 Seconds
```

---

### Needs Improvement

```
2.5–4 Seconds
```

---

### Poor

```
> 4 Seconds
```

---

# 2. Interaction to Next Paint (INP)

Measures:

> **How quickly the page responds after the user interacts with it.**

Examples:

- Clicking a button
- Opening a menu
- Typing in a textbox
- Selecting a dropdown
- Dragging a slider

---

Example

```
Click Button

↓

JavaScript Runs

↓

UI Updates

↓

INP Recorded
```

---

### Good INP

```
≤ 200 ms
```

---

### Needs Improvement

```
200–500 ms
```

---

### Poor

```
> 500 ms
```

---

# 3. Cumulative Layout Shift (CLS)

Measures:

> **How much the page unexpectedly moves while loading.**

Example

```
User Tries To Click

↓

Button Moves

↓

User Clicks Wrong Item
```

Very frustrating.

---

### Good CLS

```
≤ 0.1
```

---

### Needs Improvement

```
0.1–0.25
```

---

### Poor

```
> 0.25
```

---

# Visual Example

Bad CLS

```
Login

Password

Submit

↓

Advertisement Loads

↓

Everything Moves Down
```

User accidentally clicks the advertisement.

---

Good CLS

```
Login

Password

Submit
```

Nothing shifts.

---

# Real-Life Analogy

Imagine eating dinner.

### LCP

```
How Fast

Food Arrives
```

---

### INP

```
How Fast

Waiter Responds
```

---

### CLS

```
Does The Table Move

While You're Eating?
```

---

# Why Core Web Vitals Matter

Benefits

- Better user experience
- Faster websites
- Higher user satisfaction
- Lower bounce rate
- Better engagement
- Can positively influence Google Search rankings

---

# How React Developers Improve Core Web Vitals

Improve LCP

- Code Splitting
- Lazy Loading
- Image Optimization
- CDN
- Compression
- Server-Side Rendering (SSR)
- Streaming SSR

---

Improve INP

- Reduce JavaScript execution
- Avoid unnecessary re-renders
- Use `React.memo`
- Use `useMemo`
- Use `useCallback`
- Break long-running tasks into smaller chunks
- Use Web Workers for CPU-intensive work

---

Improve CLS

- Always specify image width and height
- Reserve space for advertisements and banners
- Avoid inserting content above existing content
- Use stable layouts
- Load fonts efficiently to reduce layout shifts

---

# Measuring Core Web Vitals

Popular tools

- Chrome Lighthouse
- Chrome DevTools
- PageSpeed Insights
- Chrome User Experience Report (CrUX)
- `web-vitals` library

---

# 90. LCP, CLS, INP Explained in Detail

---

# 1. Largest Contentful Paint (LCP)

## What Is LCP?

LCP measures

> **How quickly the largest visible element appears on the screen.**

It focuses on the content users care about most.

---

## Example

```
Open Website

↓

Background

↓

Header

↓

Navigation

↓

Large Hero Image

↓

LCP Recorded
```

---

## What Counts as the Largest Element?

Examples include:

- Hero image
- Large heading
- Large block of text
- Video poster image

The browser measures whichever is the largest visible content element.

---

## Why Can LCP Be Slow?

Reasons include:

- Large JavaScript bundles
- Slow server response
- Large images
- Render-blocking CSS or JavaScript
- Slow network

---

## How to Improve LCP

- Optimize images
- Use WebP or AVIF
- Compress assets with Brotli or Gzip
- Use a CDN
- Code Splitting
- Lazy Loading (for non-critical resources)
- Server-Side Rendering
- Streaming SSR
- Preload critical assets when appropriate

---

# 2. Interaction to Next Paint (INP)

## What Is INP?

INP measures

> **How quickly the page responds after a user interaction and updates the screen.**

It reflects the responsiveness of your application.

---

## Example

```
Click Login

↓

JavaScript Executes

↓

React Updates UI

↓

Browser Paints

↓

INP Recorded
```

---

## Why Can INP Be Slow?

Reasons include:

- Heavy JavaScript
- Expensive React rendering
- Large synchronous loops
- Too many re-renders
- Blocking the main thread

---

## How to Improve INP

- Reduce unnecessary re-renders
- Use `React.memo`
- Use `useMemo`
- Use `useCallback`
- Virtualize large lists
- Split long tasks
- Use Web Workers for heavy computation
- Optimize event handlers

---

# 3. Cumulative Layout Shift (CLS)

## What Is CLS?

CLS measures

> **How much visible content unexpectedly shifts while the page is loading or updating.**

---

## Example

Before

```
Login

Password

Submit
```

Advertisement loads

After

```
Advertisement

Login

Password

Submit
```

Everything moved.

CLS increases.

---

## Why Does CLS Happen?

Common causes:

- Images without dimensions
- Ads loading without reserved space
- Dynamic banners inserted above content
- Late-loading fonts causing text to shift

---

## How to Improve CLS

- Set `width` and `height` (or CSS `aspect-ratio`) for images
- Reserve space for ads and embeds
- Avoid inserting new content above existing content
- Use efficient font loading strategies
- Design stable layouts

---

# Comparison Table

| Metric | Measures | Good Score | Common Fixes |
|----------|----------|------------|--------------|
| **LCP** | Loading performance (largest visible content) | ≤ 2.5 s | Optimize images, SSR, CDN, Code Splitting |
| **INP** | Interaction responsiveness | ≤ 200 ms | Reduce JavaScript, optimize rendering, break long tasks |
| **CLS** | Visual stability | ≤ 0.1 | Reserve layout space, specify image sizes, avoid layout shifts |

---

# Complete Loading Timeline

```
User Opens Website

↓

HTML Downloaded

↓

CSS Loaded

↓

JavaScript Loaded

↓

Largest Content Appears

↓

LCP

↓

User Clicks Button

↓

React Updates UI

↓

INP

↓

No Layout Movement

↓

Good CLS
```

---

# Common Interview Questions

## 1. What Are Core Web Vitals?

**Answer:**

Core Web Vitals are Google's user-centric performance metrics that measure loading performance (LCP), interaction responsiveness (INP), and visual stability (CLS).

---

## 2. What Is LCP?

**Answer:**

LCP measures how long it takes for the largest visible content element on the page to render. A good LCP is **2.5 seconds or less**.

---

## 3. What Is INP?

**Answer:**

INP measures how responsive a page is after a user interaction. It captures the delay until the browser can present the next visual update. A good INP is **200 milliseconds or less**.

---

## 4. What Is CLS?

**Answer:**

CLS measures unexpected layout shifts while the page is loading or updating. A good CLS score is **0.1 or lower**.

---

## 5. Which Core Web Vital Is Most Important?

**Answer:**

All three are important because they measure different aspects of user experience:

- **LCP** → Loading speed
- **INP** → Responsiveness
- **CLS** → Visual stability

Improving all three provides the best overall experience.

---

## 6. How Do You Measure Core Web Vitals?

**Answer:**

You can use:

- Lighthouse
- PageSpeed Insights
- Chrome DevTools
- Chrome User Experience Report (CrUX)
- The `web-vitals` library for real-user measurements

---

# Senior Interview Tip ⭐

If you're asked:

> **"How would you improve Core Web Vitals in a React application?"**

A strong answer would be:

### Improve LCP

- Optimize images
- Use a CDN
- Enable Brotli/Gzip compression
- Apply Code Splitting and Lazy Loading
- Use SSR or Streaming SSR
- Preload critical resources

### Improve INP

- Reduce unnecessary re-renders
- Optimize event handlers
- Use `React.memo`, `useMemo`, and `useCallback` where appropriate
- Break long-running tasks into smaller chunks
- Move CPU-intensive work to Web Workers

### Improve CLS

- Reserve space for images and ads
- Specify image dimensions or use `aspect-ratio`
- Prevent unexpected content insertion
- Load fonts efficiently

Finally, validate improvements using **Lighthouse**, **PageSpeed Insights**, and **real-user monitoring**.

---

# Easy Memory Trick

Imagine ordering food at a restaurant.

```
Food Arrives Quickly?

↓

LCP

----------------------

Waiter Responds Quickly?

↓

INP

----------------------

Table Doesn't Shake?

↓

CLS
```

Or remember:

```
L

↓

Load

----------------------

I

↓

Interact

----------------------

C

↓

Content Stability
```

---

# Summary

- **Core Web Vitals** are Google's key metrics for measuring real-world user experience.
- **LCP (Largest Contentful Paint)** measures how quickly the largest visible content loads. Target: **≤ 2.5 seconds**.
- **INP (Interaction to Next Paint)** measures how responsive the page is after user interactions. Target: **≤ 200 milliseconds**.
- **CLS (Cumulative Layout Shift)** measures visual stability and unexpected layout movement. Target: **≤ 0.1**.
- React developers improve these metrics using techniques such as **Code Splitting**, **Lazy Loading**, **Tree Shaking**, **image optimization**, **SSR**, **Streaming SSR**, **reducing unnecessary re-renders**, and **stable layouts**.
- Use tools like **Lighthouse**, **PageSpeed Insights**, **Chrome DevTools**, and **real-user monitoring** to measure and continuously improve Core Web Vitals.