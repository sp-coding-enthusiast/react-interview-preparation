# 84. What is Tree Shaking?

## Simple Definition

**Tree Shaking** is a build optimization technique that **removes unused JavaScript code from the final production bundle.**

In simple words,

> **If your application doesn't use some code, don't include it in the final bundle.**

Tree Shaking makes your application:

- Smaller
- Faster
- More efficient

It happens **during the build process**, not while the application is running.

---

# Why Is It Called "Tree Shaking"?

Imagine a tree full of branches.

```
            Tree

          /  |   \

      Branch Branch Branch

      /   \

   Leaf  Leaf
```

Now shake the tree.

Only the **dead leaves** fall.

The healthy branches remain.

Tree Shaking works the same way.

```
Application Code

↓

Unused Code

↓

Removed

↓

Used Code Remains
```

---

# Why Do We Need Tree Shaking?

Imagine you install a utility library.

```javascript
import {
  add,
  subtract,
  multiply,
  divide,
  squareRoot,
  cubeRoot,
  factorial
} from "./math";
```

But your application only uses

```javascript
add()
```

Without Tree Shaking,

the browser downloads

```
add

subtract

multiply

divide

squareRoot

cubeRoot

factorial
```

Most of this code is never used.

---

# With Tree Shaking

The bundler analyzes the code.

```
Used

↓

add()
```

Unused

```
subtract()

multiply()

divide()

factorial()
```

Removed.

Final bundle contains only

```
add()
```

Smaller bundle.

---

# Real-Life Analogy: Travel Bag

Imagine you're going on a two-day trip.

Without Tree Shaking

```
Pack

Winter Clothes

Summer Clothes

Shoes

Umbrella

Books

Laptop

Gaming Console

Kitchen Utensils
```

Huge suitcase.

---

With Tree Shaking

```
Pack

Only What You Need
```

Much lighter.

React applications work the same way.

---

# Example

Suppose

```javascript
// math.js

export function add() {}

export function subtract() {}

export function multiply() {}

export function divide() {}
```

Application

```javascript
import { add } from "./math";

add();
```

Production bundle becomes

```
add()
```

Only.

The other functions are removed.

---

# How Tree Shaking Works

```
Source Code

↓

Analyze Imports

↓

Find Used Code

↓

Remove Unused Exports

↓

Generate Final Bundle
```

---

# When Does Tree Shaking Happen?

Tree Shaking happens during

```
npm run build
```

or

```
vite build
```

It is performed by the bundler.

It **does not happen during development**.

---

# Which Bundlers Support Tree Shaking?

Modern bundlers include support for Tree Shaking.

- Webpack
- Rollup
- Vite (uses Rollup for production builds)
- Parcel
- esbuild

---

# ES Modules Are Important

Tree Shaking works best with **ES Modules (ESM)**.

Example

```javascript
import { add } from "./math";
```

Because imports and exports are static, bundlers can determine what is actually used.

---

CommonJS example

```javascript
const math = require("./math");
```

This is harder to analyze, so Tree Shaking is less effective.

---

# Real-World Example

Imagine importing

```javascript
import _ from "lodash";
```

Without optimization,

the entire Lodash library might be included.

Better

```javascript
import debounce from "lodash/debounce";
```

or (depending on the project setup)

```javascript
import { debounce } from "lodash-es";
```

Only the required functionality is included, allowing the bundler to eliminate unused code more effectively.

---

# Tree Shaking vs Code Splitting

Many developers confuse these.

### Tree Shaking

```
Remove

Unused Code
```

---

### Code Splitting

```
Split

Remaining Code

Into Chunks
```

Example

```
100 Files

↓

Remove

40 Unused Files

↓

Split

60 Remaining Files

Into Bundles
```

Different optimizations.

---

# Tree Shaking vs Minification

### Tree Shaking

```
Remove

Unused Code
```

---

### Minification

```
Shorten

Variable Names

↓

Remove Spaces

↓

Compress Code
```

Both reduce bundle size, but in different ways.

---

# Complete Build Process

```
Source Code

↓

Tree Shaking

↓

Remove Unused Code

↓

Code Splitting

↓

Create Bundles

↓

Minification

↓

Compress Code

↓

Production Build
```

---

# Benefits

Tree Shaking provides:

- Smaller bundle size
- Faster downloads
- Faster page load
- Lower bandwidth usage
- Better performance
- Improved caching efficiency

---

# Does Tree Shaking Remove Everything?

No.

Tree Shaking removes only code that can be proven to be unused.

It **cannot safely remove** code that may have **side effects**.

Example

```javascript
console.log("Application Started");
```

Removing this could change the program's behavior, so the bundler must be careful.

---

# Side Effects

Some files execute code simply by being imported.

```javascript
import "./setupAnalytics";
```

Even if no functions are imported,

this file performs work immediately.

Bundlers usually keep such files because removing them could break the application.

Many libraries indicate whether they are free of side effects using the `sideEffects` field in `package.json`.

---

# Common Interview Questions

## 1. What Is Tree Shaking?

**Answer:**

Tree Shaking is a build optimization that removes unused JavaScript exports from the production bundle, reducing the application's size.

---

## 2. When Does Tree Shaking Occur?

**Answer:**

It occurs during the production build process, not while the application is running.

---

## 3. Which Module System Works Best with Tree Shaking?

**Answer:**

**ES Modules (ESM)**, because their static `import` and `export` syntax allows bundlers to determine which code is used.

---

## 4. Is Tree Shaking the Same as Code Splitting?

**Answer:**

No.

- **Tree Shaking** removes unused code.
- **Code Splitting** divides the remaining code into smaller bundles.

---

## 5. Does Tree Shaking Improve Performance?

**Answer:**

Yes.

By reducing the amount of JavaScript that users download and parse, it can improve load time and application performance.

---

## 6. Can Tree Shaking Remove Side Effects?

**Answer:**

Generally, no.

Bundlers avoid removing code that may produce side effects unless it is explicitly marked as safe to eliminate.

---

# Senior Interview Tip ⭐

If you're asked:

> **"How does React reduce the size of a production build?"**

A strong answer would be:

1. **Tree Shaking** removes unused exports.
2. **Code Splitting** divides the application into smaller bundles.
3. **Lazy Loading** downloads bundles only when needed.
4. **Minification** compresses the remaining JavaScript.
5. **Image optimization** and compression reduce asset sizes.
6. **Gzip** or **Brotli** compression further reduce download size.
7. Serving assets through a **CDN** improves delivery speed.

This demonstrates an understanding of the complete production optimization pipeline.

---

# Easy Memory Trick

Imagine cleaning your wardrobe.

Without Tree Shaking

```
Keep

Everything
```

Your wardrobe stays full of clothes you never wear.

---

With Tree Shaking

```
Remove

Unused Clothes

↓

Keep

Only What You Wear
```

Your wardrobe becomes lighter and easier to manage.

React's production build follows the same principle.

- **Tree Shaking = Remove Unused Code**
- **Code Splitting = Divide Remaining Code**
- **Lazy Loading = Download It Later**
- **Minification = Compress the Final Code**

---

# Summary

- **Tree Shaking** removes unused JavaScript code from the production bundle.
- It is performed during the **build process** by bundlers such as **Webpack**, **Rollup**, **Vite**, **Parcel**, and **esbuild**.
- Tree Shaking works best with **ES Modules (ESM)** because imports and exports are statically analyzable.
- It is different from **Code Splitting**, **Lazy Loading**, and **Minification**, although these techniques are often used together.
- Tree Shaking reduces bundle size, improves download speed, and helps create faster React applications.