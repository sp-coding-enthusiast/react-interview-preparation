# 81. What is Infinite Scroll?

## Simple Definition

**Infinite Scroll** is a technique where **more data is automatically loaded as the user scrolls down the page**, instead of requiring the user to click a "Next Page" button.

In simple words,

> **As you reach the bottom of the page, the application automatically fetches and displays more data.**

This creates a smooth, uninterrupted browsing experience.

---

# Why Do We Need Infinite Scroll?

Imagine an e-commerce website with:

```
1,000,000 Products
```

Should the browser load all products at once?

```
Load 1,000,000 Products

↓

Huge Download

↓

High Memory Usage

↓

Slow Website
```

Definitely not.

Instead, load products gradually.

---

# Real-Life Analogy: Buffet Restaurant

Imagine you're at a buffet.

Without Infinite Scroll

The waiter brings:

```
100 Plates

↓

Your Table
```

Impossible to manage.

---

With Infinite Scroll

The waiter brings:

```
5 Plates

↓

You Finish

↓

5 More Plates

↓

Repeat
```

You only receive what you need.

React applications follow the same principle.

---

# Traditional Pagination

```
Page 1

↓

20 Products

↓

Next Page

↓

20 Products

↓

Next Page
```

The user must manually navigate between pages.

---

# Infinite Scroll

```
20 Products

↓

User Scrolls

↓

Load 20 More

↓

Scroll

↓

Load 20 More

↓

Scroll

↓

Load 20 More
```

The experience feels continuous.

---

# Visual Example

Initially

```
Product 1

Product 2

...

Product 20
```

User scrolls to the bottom.

React requests

```
Products 21–40
```

The page becomes

```
Product 1

...

Product 40
```

Scroll again.

React loads

```
Products 41–60
```

The list keeps growing.

---

# How Infinite Scroll Works

```
User Scrolls

↓

Reached Bottom?

↓

Yes

↓

Call API

↓

Receive Data

↓

Append New Items

↓

Continue Scrolling
```

The cycle repeats until there is no more data.

---

# Example

Imagine an API.

```
GET /products?page=1
```

Returns

```
20 Products
```

User scrolls.

```
GET /products?page=2
```

Returns

```
Next 20 Products
```

The application combines both pages.

---

# React Example

```jsx
const [products, setProducts] = useState([]);

const [page, setPage] = useState(1);

async function loadMore() {

  const response = await fetch(

    `/api/products?page=${page}`

  );

  const data = await response.json();

  setProducts(prev => [

    ...prev,

    ...data

  ]);

  setPage(prev => prev + 1);

}
```

Each API response is appended to the existing list.

---

# Detecting the Bottom

There are two common approaches.

### 1. Scroll Event

```
User Scrolls

↓

Check Scroll Position

↓

Reached Bottom?

↓

Load More
```

Simple but can fire very frequently, so it often needs throttling or debouncing.

---

### 2. Intersection Observer ✅ (Recommended)

React watches a small invisible element near the bottom of the list.

```
Last Element Visible?

↓

Yes

↓

Load More
```

This is more efficient than constantly listening to scroll events.

---

# Real-World Applications

Infinite Scroll is commonly used in:

- Instagram
- Facebook
- LinkedIn
- X (Twitter)
- YouTube recommendations
- TikTok
- Amazon search results
- Pinterest

---

# Infinite Scroll vs Pagination

| Pagination | Infinite Scroll |
|------------|-----------------|
| User clicks "Next" | Loads automatically |
| Separate pages | Continuous list |
| Easier to jump to a page | Better for continuous browsing |
| Good for search results and reports | Good for feeds and social media |

---

# Infinite Scroll vs Virtualization

Many people confuse these.

They solve different problems.

### Infinite Scroll

Controls

```
How Much Data Is Loaded
```

---

### Virtualization

Controls

```
How Much Data Is Rendered
```

---

Example

Infinite Scroll

```
Load

20

↓

40

↓

60

↓

80 Products
```

Virtualization

```
Only Render

20 Visible Products
```

Even if

```
80 Products
```

have already been loaded.

---

# Together

Modern applications often combine both.

```
Infinite Scroll

↓

Load More Data

↓

Virtualization

↓

Render Only Visible Items
```

This provides excellent scalability.

---

# Challenges

### Duplicate Requests

If the user scrolls quickly,

multiple API calls may occur.

Prevent this using a loading flag.

```jsx
if (loading) return;
```

---

### Loading Indicator

Show feedback while data is loading.

```
Loading...

↓

New Items
```

This improves user experience.

---

### End of List

When there is no more data,

display

```
No More Products
```

instead of making more API calls.

---

### Error Handling

If the request fails,

show

```
Couldn't load more products.

Try Again.
```

---

# Best Practices

- Fetch data in small batches.
- Use **Intersection Observer** instead of scroll events when possible.
- Prevent duplicate API requests.
- Display loading indicators.
- Handle API errors gracefully.
- Stop requesting data when the end of the list is reached.
- Combine with virtualization for very large datasets.

---

# Popular React Libraries

- `react-intersection-observer`
- `react-infinite-scroll-component`
- `TanStack Query` (for data fetching and pagination)
- `TanStack Virtual` (when combined with virtualization)

---

# Complete Flow

```
Load First Page

↓

Display Items

↓

User Scrolls

↓

Bottom Reached

↓

API Request

↓

Append Items

↓

Continue Scrolling

↓

Repeat
```

---

# Common Interview Questions

## 1. What Is Infinite Scroll?

**Answer:**

Infinite Scroll is a UI pattern where additional data is automatically fetched and appended as the user reaches the end of the currently displayed content.

---

## 2. Why Is Infinite Scroll Used?

**Answer:**

It improves the browsing experience by loading data gradually, reducing initial load time and allowing users to continue scrolling without manually changing pages.

---

## 3. How Do You Detect When to Load More Data?

**Answer:**

The recommended approach is to use the **Intersection Observer API**, which detects when a target element enters the viewport. Scroll event listeners are another option but are generally less efficient.

---

## 4. Is Infinite Scroll Better Than Pagination?

**Answer:**

It depends on the use case.

- **Infinite Scroll** is excellent for social feeds and continuous browsing.
- **Pagination** is better when users need to jump to specific pages, compare results, or know the total number of pages.

---

## 5. What Is the Difference Between Infinite Scroll and Virtualization?

**Answer:**

- **Infinite Scroll** controls **how much data is loaded**.
- **Virtualization (Windowing)** controls **how much of the loaded data is rendered**.

They solve different problems and are often used together.

---

## 6. What Problems Can Infinite Scroll Cause?

**Answer:**

Potential issues include:

- Duplicate API requests
- Memory growth if too much data is kept in memory
- Difficulty navigating back to a specific position
- Accessibility concerns
- Poor SEO if not implemented carefully
- Users may never reach the footer because new content keeps loading

---

# Senior Interview Tip ⭐

If you're asked:

> **"How would you build an infinite scrolling product list?"**

A strong answer would be:

1. Load the first page of products.
2. Use an **Intersection Observer** to detect when the user reaches the bottom.
3. Fetch the next page from the API.
4. Append the new items to the existing list.
5. Prevent duplicate requests with a loading flag.
6. Stop requesting when there is no more data.
7. Combine with **virtualization** to render only visible items.
8. Use a data-fetching library such as **TanStack Query** to manage caching, loading states, retries, and pagination.

---

# Easy Memory Trick

Imagine watching videos on a streaming app.

You don't download every video ever made.

Instead,

```
Watch Video

↓

Scroll

↓

Next Video Loads

↓

Scroll

↓

Another Video Loads
```

Only the next content is loaded as needed.

React Infinite Scroll works the same way.

- **Infinite Scroll = Load More Data**
- **Virtualization = Show Only Visible Data**
- **Windowing = Another name for Virtualization**

---

# Summary

- **Infinite Scroll** automatically loads additional data as users reach the end of the current content.
- It reduces the initial loading time and creates a seamless browsing experience.
- The **Intersection Observer API** is the preferred way to detect when more data should be loaded.
- Infinite Scroll controls **data loading**, while **Virtualization (Windowing)** controls **data rendering**.
- Common best practices include preventing duplicate requests, showing loading indicators, handling errors, and stopping when no more data is available.
- For very large datasets, Infinite Scroll is often combined with **Virtualization** to achieve excellent performance and scalability.