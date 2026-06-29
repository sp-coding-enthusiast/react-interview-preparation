# 101. What are React Server Components (RSC)?

> **Interview Note ⭐**
>
> React Server Components (RSC) are one of the biggest architectural changes introduced to React.
>
> They are **not the same as Server-Side Rendering (SSR).**
>
> This is one of the most common interview questions.
>
> **SSR renders HTML on the server.**
>
> **Server Components execute on the server and send a React component tree (serialized data), not JavaScript for that component, to the browser.**

---

# Simple Definition

A **React Server Component** is a component that **runs only on the server**.

It is **never sent as JavaScript to the browser**.

Instead, the server executes the component and sends the rendered result (React's serialized component payload) to the client.

In simple words,

> **The browser receives the result, not the component's JavaScript code.**

---

# Why Were Server Components Introduced?

Imagine a shopping website.

```
Product Page

↓

Product Details

↓

Reviews

↓

Recommendations
```

Traditionally,

the browser downloads JavaScript for all of these components.

Even if most of them simply display data.

That increases:

- Bundle size
- Download time
- Parse time
- Execution time

---

React Server Components solve this problem.

```
Server Executes

↓

Browser Receives Result

↓

No JavaScript Needed

For That Component
```

---

# Real-Life Analogy

Imagine ordering food.

### Client Component

Restaurant sends:

```
Ingredients

↓

Recipe

↓

Cooking Instructions
```

You cook everything yourself.

---

### Server Component

Restaurant sends:

```
Cooked Food
```

You simply eat.

Less work for you.

---

# Basic Example

Server Component

```jsx
async function Products() {

  const products = await getProducts();

  return (

    <ul>

      {products.map(product => (

        <li key={product.id}>

          {product.name}

        </li>

      ))}

    </ul>

  );

}
```

Notice:

No `useEffect`

No loading state

No client-side fetch

The data is fetched directly on the server.

---

# What Happens?

```
Browser Requests Page

↓

Server Runs Component

↓

Database Query

↓

React Builds Output

↓

Sends Result

↓

Browser Displays
```

---

# Why Is This Faster?

Traditional React

```
Download JS

↓

Execute JS

↓

Fetch API

↓

Wait

↓

Render
```

---

Server Components

```
Server Fetches

↓

Server Renders

↓

Browser Displays
```

Less JavaScript.

Less work.

---

# Can Server Components Use Hooks?

No.

They **cannot** use client-only Hooks like:

```jsx
useState()
```

```jsx
useEffect()
```

```jsx
useReducer()
```

```jsx
useRef()
```

because they never run in the browser.

---

# What Can Server Components Do?

They can:

- Fetch data
- Access databases
- Read files
- Access secure environment variables
- Call backend services
- Render UI
- Import other Server Components
- Import Client Components

---

# What Can't They Do?

They cannot:

- Handle click events
- Access the DOM
- Use browser APIs
- Use state
- Use effects
- Access `window`
- Access `document`

Because none of these exist on the server.

---

# Client Component

Interactive components must run in the browser.

Example

```jsx
"use client";

function Counter() {

  const [count, setCount] =

    useState(0);

  return (

    <button

      onClick={() =>

        setCount(count + 1)

      }

    >

      {count}

    </button>

  );

}
```

The `"use client"` directive tells frameworks like **Next.js App Router** that this component should be rendered as a Client Component.

---

# Mixing Components

This is the recommended architecture.

```
Server Component

↓

Fetch Data

↓

Pass Props

↓

Client Component

↓

Handle Clicks
```

Example

```
Product Page

↓

Server Component

↓

Product Data

↓

Client Component

↓

Add To Cart Button
```

---

# Internally

```
Request

↓

Server Component Executes

↓

Database

↓

React Payload

↓

Browser

↓

Client Component Hydrates

↓

Interactive
```

---

# Benefits

- Smaller JavaScript bundles
- Faster page loads
- Better SEO
- Faster data fetching
- Secure backend access
- Less client-side work
- Better performance

---

# 102. Server Components vs Client Components

---

# Simple Comparison

| Server Components | Client Components |
|-------------------|-------------------|
| Run on the server | Run in the browser |
| No JavaScript shipped for that component | JavaScript downloaded to the browser |
| Can fetch data directly | Usually fetch data via APIs |
| Cannot use state | Can use state |
| Cannot use effects | Can use effects |
| Cannot handle events | Can handle events |
| Cannot access DOM | Can access DOM |
| Best for static/data-driven UI | Best for interactive UI |

---

# Real-Life Analogy

Imagine a bakery.

---

### Server Component

```
Cake Already Baked

↓

Customer Receives Cake
```

---

### Client Component

```
Customer Receives

Flour

Sugar

Recipe

↓

Bake At Home
```

Much more work.

---

# Data Fetching

Server Component

```
Server

↓

Database

↓

Component

↓

Browser
```

No API call from the browser is required.

---

Client Component

```
Browser

↓

API

↓

Server

↓

Database

↓

Browser

↓

Render
```

More network round trips.

---

# Bundle Size

Server Component

```
Products.jsx

↓

Runs Server

↓

0 KB JS

Sent To Browser
```

---

Client Component

```
Products.jsx

↓

Downloaded

↓

Executed

↓

Hydrated
```

More JavaScript.

---

# Example

Server Component

```jsx
async function ProductList() {

  const products =

    await getProducts();

  return (

    <ul>

      {products.map(product => (

        <li key={product.id}>

          {product.name}

        </li>

      ))}

    </ul>

  );

}
```

---

Client Component

```jsx
"use client";

function SearchBox() {

  const [text, setText] =

    useState("");

  return (

    <input

      value={text}

      onChange={(e) =>

        setText(e.target.value)

      }

    />

  );

}
```

---

# Which Should You Choose?

Choose **Server Components** when:

- Fetching data
- Displaying reports
- Product listings
- Blog articles
- Dashboards
- Static content
- Reading files
- Accessing databases

---

Choose **Client Components** when:

- Buttons
- Forms
- Search boxes
- Dropdowns
- Animations
- Drag & Drop
- Modals
- User interactions

---

# Best Practice

Build pages like this.

```
Server Component

↓

Fetch Data

↓

Render Layout

↓

Client Components

↓

Handle User Actions
```

This minimizes JavaScript while keeping the UI interactive.

---

# Common Mistakes

### Mistake 1

Thinking Server Components replace Client Components.

❌ Wrong.

Both are needed.

---

### Mistake 2

Using `useState()` in Server Components.

❌ Not allowed.

---

### Mistake 3

Using `window` in Server Components.

❌ Doesn't exist on the server.

---

### Mistake 4

Making every component a Client Component.

❌ Leads to larger bundles.

Use Client Components only when interactivity is required.

---

# Real-World Example

Imagine Amazon.

```
Product Details

↓

Server Component

---------------------

Add To Cart

↓

Client Component

---------------------

Reviews

↓

Server Component

---------------------

Quantity Selector

↓

Client Component
```

Only interactive pieces need client-side JavaScript.

---

# Complete Architecture

```
Browser

↓

Request

↓

Server Components

↓

Database

↓

React Payload

↓

Browser

↓

Client Components Hydrate

↓

Interactive UI
```

---

# Server Components vs SSR

This is another common interview question.

| Server Components | Server-Side Rendering (SSR) |
|-------------------|-----------------------------|
| Execute only on the server | Renders HTML on the server |
| Reduce JavaScript sent to the browser | JavaScript is still sent for the rendered components so they can hydrate |
| Focus on component execution | Focus on initial HTML generation |
| Can directly access backend resources | Can also fetch data, but the resulting page still hydrates on the client |
| Can be combined with SSR | Often used together with Server Components in modern frameworks |

**Important:** Server Components and SSR are **complementary**, not competing technologies.

---

# Common Interview Questions

## 1. What Are React Server Components?

**Answer:**

React Server Components are components that execute only on the server. Their JavaScript is not shipped to the browser, reducing bundle size and allowing direct access to backend resources like databases and file systems.

---

## 2. Can Server Components Use `useState()`?

**Answer:**

No.

Server Components cannot use client-only Hooks such as `useState`, `useEffect`, `useReducer`, or `useRef` because they never execute in the browser.

---

## 3. Can Server Components Handle Click Events?

**Answer:**

No.

Event handlers require browser-side JavaScript, so interactive behavior belongs in Client Components.

---

## 4. What Is the Difference Between Server Components and Client Components?

**Answer:**

Server Components are optimized for data fetching and rendering on the server, while Client Components provide interactivity through state, effects, event handlers, and browser APIs.

---

## 5. Why Are Server Components Faster?

**Answer:**

They reduce the amount of JavaScript downloaded and executed by the browser, move data fetching closer to the data source, and decrease client-side work.

---

## 6. Can Server Components Import Client Components?

**Answer:**

Yes.

A Server Component can render a Client Component and pass it props. This is the recommended architecture in frameworks such as Next.js App Router.

---

# Senior Interview Tip ⭐

If you're asked:

> **"How would you structure a modern React application using Server Components?"**

A strong answer is:

> "I use Server Components for data fetching, database access, layouts, and static content because they reduce client-side JavaScript and improve performance. I use Client Components only for interactive features like forms, buttons, modals, and search inputs. This keeps bundles small while preserving a rich user experience."

This reflects the architecture encouraged by modern React frameworks.

---

# Easy Memory Trick

Imagine building a house.

### Server Component

```
Factory Builds Furniture

↓

Delivers Ready

↓

Place It Inside
```

Minimal work on-site.

---

### Client Component

```
Wood

↓

Tools

↓

Instructions

↓

Assemble On Site
```

More work at the destination.

React works the same way.

- **Server Component = Ready-Made**
- **Client Component = Build in Browser**

---

# Summary

## React Server Components

- Execute **only on the server**.
- Their JavaScript is **not shipped to the browser**.
- Can fetch data directly from databases and backend services.
- Cannot use state, effects, browser APIs, or event handlers.
- Reduce bundle size and improve performance.

## Server Components vs Client Components

| Server Components | Client Components |
|-------------------|-------------------|
| Server execution | Browser execution |
| Data fetching | User interaction |
| No state/effects | Supports state/effects |
| No event handlers | Supports event handlers |
| Smaller bundles | Adds client-side JavaScript |

## Recommended Architecture

- Use **Server Components** for:
  - Data fetching
  - Layouts
  - Product listings
  - Reports
  - Static content

- Use **Client Components** for:
  - Forms
  - Buttons
  - Search
  - Animations
  - Modals
  - Any interactive UI

Together, they provide the best balance of **performance**, **security**, and **user experience**.