# 4. What is SPA (Single Page Application)?

## Simple Definition

A **Single Page Application (SPA)** is a web application where the browser loads **one HTML page initially**, and after that, only the required data is fetched from the server.

The page does not reload completely when the user navigates between screens.

Popular SPA frameworks:

- React
- Angular
- Vue.js
- Blazor WebAssembly

---

## Real-Life Example

Imagine entering a shopping mall.

### Traditional Way (MPA)

Every time you want to visit another shop:

- Exit the mall
- Re-enter
- Go through security again

This takes time.

### SPA Way

You enter the mall once.

Now you can move between:

- Clothing Store
- Food Court
- Electronics Store

without leaving the mall.

Much faster.

---

## Example: Amazon

When you:

1. Search for a product
2. Open product details
3. Add to cart
4. View cart

Most parts update instantly.

The entire page is not refreshed.

This behavior is similar to SPA.

---

## How SPA Works Internally

### First Request

Browser requests:

```html
index.html
```

Server returns:

```html
<div id="root"></div>
```

along with:

```javascript
app.js
```

React application starts.

---

### User Clicks "Products"

Instead of:

```text
GET /Products
```

React changes the screen itself.

Only product data is fetched:

```text
GET /api/products
```

Server returns JSON:

```json
[
  {
    "id": 1,
    "name": "Laptop"
  }
]
```

React updates UI.

No full page refresh.

---

## SPA Flow

```text
Browser
   |
Load index.html once
   |
React Application Starts
   |
User Clicks Menu
   |
API Call
   |
JSON Data Returned
   |
UI Updates
```

---

## Advantages of SPA

### Faster User Experience

Only data is transferred.

Not entire pages.

---

### Smooth Navigation

Feels like a mobile app.

---

### Less Server Load

Server sends data only.

Not complete HTML pages repeatedly.

---

### Better User Experience

No page flickering.

No full reload.

---

### Reusable APIs

Same APIs can be used by:

- React Web App
- Mobile App
- Desktop App

---

## Disadvantages of SPA

### Initial Load Can Be Heavy

Large JavaScript bundle must be downloaded.

---

### SEO Challenges

Search engines may struggle if not configured properly.

Solutions:

- Server Side Rendering (SSR)
- Next.js

---

### JavaScript Dependency

If JavaScript fails:

Application may not work properly.

---

## Example SPA Technologies

| Technology | Type |
|------------|------|
| React | SPA Framework |
| Angular | SPA Framework |
| Vue.js | SPA Framework |
| Blazor WASM | SPA Framework |
| Next.js | React + SSR |
| Nuxt.js | Vue + SSR |

---

# 5. SPA vs MPA (Multi Page Application)

## What is MPA?

MPA stands for:

**Multi Page Application**

Every page request loads a completely new page from the server.

Examples:

- Traditional ASP.NET MVC
- PHP Websites
- Old E-commerce Sites
- Government Portals

---

## Real-Life Analogy

### SPA

Netflix App

Switching screens is instant.

---

### MPA

Reading a physical book.

To go to another chapter:

- Close current page
- Open another page

Everything reloads.

---

# Architecture Comparison

## MPA

```text
Browser
   |
Request Page
   |
Server Generates HTML
   |
Returns Full Page
```

Every navigation repeats this process.

---

## SPA

```text
Browser
   |
Load App Once
   |
User Clicks
   |
API Call
   |
JSON Returned
   |
UI Updated
```

No full page reload.

---

# Example: Product Page

## MPA

User clicks Product.

```text
GET /Product/1
```

Server returns:

```html
<html>
 Product Details
</html>
```

Entire page reloads.

---

## SPA

User clicks Product.

```text
GET /api/products/1
```

Server returns:

```json
{
  "id": 1,
  "name": "Laptop"
}
```

React updates only product section.

No page reload.

---

# Visual Comparison

## MPA

```text
Page A
  ↓ Reload
Page B
  ↓ Reload
Page C
```

---

## SPA

```text
Single Page
   ↓
Screen A
   ↓
Screen B
   ↓
Screen C
```

Same page, UI changes dynamically.

---

# SPA vs MPA Comparison Table

| Feature | SPA | MPA |
|----------|----------|----------|
| Full Page Reload | No | Yes |
| Speed After Load | Very Fast | Slower |
| Initial Load | Heavy | Light |
| User Experience | App-like | Traditional |
| SEO | Harder | Easier |
| Development Complexity | Higher | Lower |
| API Usage | Extensive | Less |
| Server Load | Lower | Higher |
| Mobile-like Experience | Excellent | Average |
| Popular Frameworks | React, Angular, Vue | ASP.NET MVC, PHP, JSP |

---

# Interview Answer (Short)

**SPA (Single Page Application)** loads a single HTML page initially and dynamically updates content using JavaScript without reloading the entire page. Examples include React, Angular, and Vue applications.

**MPA (Multi Page Application)** loads a new HTML page from the server for every navigation request. Examples include traditional ASP.NET MVC and PHP applications.

The main difference is that SPA provides a faster, app-like experience by updating only the required data, while MPA reloads the entire page on every request.