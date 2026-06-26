# 20. What are Keys?

## Simple Definition

A **Key** is a **special unique identifier** that React uses to identify each item in a list.

Think of a Key as an **ID card** for every element.

It helps React know:

- Which item is new
- Which item was removed
- Which item was updated
- Which item stayed the same

---

# Real-Life Analogy: Student Roll Numbers

Imagine a classroom.

Instead of calling students by their position,

```
Student 1

Student 2

Student 3
```

The school gives every student a unique roll number.

```
101 → Rahul

102 → John

103 → Priya
```

Even if students change seats,

their roll numbers remain the same.

React Keys work exactly the same way.

---

# Why Does React Need Keys?

Suppose you display a list.

```
Apple

Banana

Orange
```

React creates three UI elements.

Later the list becomes

```
Apple

Banana

Orange

Mango
```

React needs to know

```
Which item is new?
```

The Key tells React:

```
Apple → Same

Banana → Same

Orange → Same

Mango → New
```

So React only creates the Mango element.

Everything else stays untouched.

---

# Without Keys

Imagine a classroom.

Students are identified by seat numbers.

```
Seat 1 → Rahul

Seat 2 → John

Seat 3 → Priya
```

Rahul leaves.

Now everyone shifts.

```
Seat 1 → John

Seat 2 → Priya
```

The teacher gets confused because seat numbers changed.

---

# With Keys

Students have IDs.

```
101 Rahul

102 John

103 Priya
```

Rahul leaves.

```
102 John

103 Priya
```

The IDs never change.

The teacher immediately knows only Rahul left.

React works exactly like this.

---

# Example Without Keys

```jsx
const fruits = [
  "Apple",
  "Banana",
  "Orange"
];

function App() {
  return (
    <>
      {fruits.map(fruit => (
        <li>{fruit}</li>
      ))}
    </>
  );
}
```

React gives a warning:

```
Each child in a list should have
a unique "key" prop.
```

---

# Correct Example

```jsx
const fruits = [
  {
    id: 1,
    name: "Apple"
  },
  {
    id: 2,
    name: "Banana"
  },
  {
    id: 3,
    name: "Orange"
  }
];

function App() {
  return (
    <>
      {fruits.map(fruit => (
        <li key={fruit.id}>
          {fruit.name}
        </li>
      ))}
    </>
  );
}
```

Now every item has a unique Key.

---

# Visual Representation

Without Keys

```
Apple

Banana

Orange
```

React doesn't know which one changed.

---

With Keys

```
1 Apple

2 Banana

3 Orange
```

Now React can track every item individually.

---

# What Happens When a New Item Is Added?

Original List

```
1 Apple

2 Banana

3 Orange
```

New List

```
1 Apple

2 Banana

3 Orange

4 Mango
```

React compares the Keys.

```
1 Same

2 Same

3 Same

4 New
```

Only Mango is created.

Everything else stays untouched.

---

# What Happens When an Item Is Removed?

Original

```
1 Apple

2 Banana

3 Orange
```

Updated

```
1 Apple

3 Orange
```

React immediately knows

```
Key 2 disappeared.

Remove Banana.
```

Very efficient.

---

# Common Sources for Keys

Usually use:

```jsx
key={product.id}
```

or

```jsx
key={user.id}
```

or

```jsx
key={employee.employeeId}
```

Any value that is:

- Unique
- Stable
- Doesn't change between renders

---

# Where Are Keys Used?

Mostly while rendering lists.

```jsx
users.map(user => (
    <User
        key={user.id}
        user={user}
    />
))
```

---

# Important Note

Keys are used by React internally.

They are **not available as Props**.

Example

```jsx
<User key={1} />
```

Inside User component

```jsx
function User(props) {
    console.log(props.key);
}
```

Output

```
undefined
```

If you need the value, pass it separately.

```jsx
<User
    key={user.id}
    id={user.id}
/>
```

---

# Summary

- Keys uniquely identify list items.
- React uses Keys to efficiently update the UI.
- Keys should be unique among siblings.
- Keys are mainly used with `.map()`.
- Keys are not passed as Props.

---

# 21. Why Should Keys Be Unique?

## Simple Definition

Every Key should be **unique within its list**.

If two items share the same Key,

React cannot correctly identify them.

Think of Keys like Aadhaar numbers or Passport numbers.

Two people should never have the same ID.

---

# Real-Life Analogy: Airport Boarding Pass

Imagine two passengers.

```
Passenger A

Seat 21A
```

```
Passenger B

Seat 21A
```

Chaos!

The airline doesn't know who belongs in seat 21A.

React faces the same problem if Keys are duplicated.

---

# Example of Duplicate Keys

Wrong

```jsx
const products = [
    { id: 1, name: "Laptop" },
    { id: 1, name: "Phone" }
];
```

Both items have

```
id = 1
```

React cannot reliably distinguish them.

---

# Correct Example

```jsx
const products = [
    { id: 1, name: "Laptop" },
    { id: 2, name: "Phone" }
];
```

Every item has its own identity.

---

# Why React Uses Unique Keys

Imagine a shopping list.

```
1 Milk

2 Bread

3 Eggs
```

Later

```
1 Milk

2 Bread

3 Eggs

4 Butter
```

React compares

```
1 Same

2 Same

3 Same

4 New
```

Only Butter is added.

---

# If Keys Aren't Unique

```
1 Milk

1 Bread

1 Eggs
```

React can't tell them apart.

Unexpected behavior can occur during updates.

---

# Do Keys Need to Be Globally Unique?

No.

Keys only need to be unique **among siblings in the same list**.

Example

Products

```
1 Laptop

2 Phone
```

Orders

```
1 Order

2 Order
```

This is perfectly fine because they belong to different lists.

---

# Good Keys

```jsx
key={user.id}

key={product.id}

key={order.id}
```

---

# Bad Keys

```jsx
key={Math.random()}
```

Changes every render.

---

```jsx
key={Date.now()}
```

Also changes every render.

React treats every item as brand new each time.

---

# Summary

- Keys should be unique within a list.
- Stable Keys help React efficiently update the UI.
- Duplicate Keys can cause incorrect rendering.
- IDs from a database are usually the best Keys.

---

# 22. Index as Key: Why Is It Dangerous?

## Simple Definition

The **index** is the position of an item in an array.

Example

```
0 Apple

1 Banana

2 Orange
```

Here

```
Apple → Index 0

Banana → Index 1

Orange → Index 2
```

You can write

```jsx
fruits.map((fruit, index) => (
    <li key={index}>
        {fruit}
    </li>
))
```

React allows this.

But it is **often a bad idea**.

---

# Why?

Because indexes change.

IDs usually don't.

---

# Real-Life Analogy: Marathon Runners

Imagine runners.

Initially

```
Position 1 → Rahul

Position 2 → John

Position 3 → David
```

A new runner joins at the front.

```
Position 1 → Alex

Position 2 → Rahul

Position 3 → John

Position 4 → David
```

Everyone's position changed.

If you identify runners only by position,

you'll think Rahul became Alex!

That's exactly what happens when using indexes as Keys.

---

# Example

Original List

```
0 Apple

1 Banana

2 Orange
```

Insert

```
Mango
```

at the beginning.

New List

```
0 Mango

1 Apple

2 Banana

3 Orange
```

Every index changed.

React thinks

```
Apple became Mango

Banana became Apple

Orange became Banana
```

Instead of recognizing that only Mango was added.

This causes unnecessary updates.

---

# Example Code

Wrong

```jsx
const fruits = [
  "Apple",
  "Banana",
  "Orange"
];

function App() {
  return (
    <>
      {fruits.map((fruit, index) => (
        <li key={index}>
          {fruit}
        </li>
      ))}
    </>
  );
}
```

If items are inserted, removed, or reordered, React may reuse the wrong component instances.

---

# Real Problem: Input Fields

Imagine this list.

```
John

David

Emma
```

Each row has a text box.

You type

```
John → Developer

David → Tester

Emma → Manager
```

Now a new user is inserted at the top.

```
Alex

John

David

Emma
```

If the **index** is the Key,

React may keep the text box state with the position instead of the person.

Result

```
Alex → Developer ❌

John → Tester ❌

David → Manager ❌
```

The entered values appear under the wrong people.

This is one of the most common bugs caused by using indexes as Keys.

---

# Correct Solution

Use a stable ID.

```jsx
users.map(user => (
    <User
        key={user.id}
        user={user}
    />
))
```

Now React knows

```
101 John

102 David

103 Emma
```

Even if the order changes,

the IDs remain the same.

Everything stays correct.

---

# When Is Using Index Acceptable?

Using the index is generally acceptable only when **all** of these are true:

- The list is static.
- Items are never added.
- Items are never removed.
- Items are never reordered.
- Items don't have their own local state.

Example

```jsx
const days = [
  "Monday",
  "Tuesday",
  "Wednesday"
];
```

If this list will never change, using the index is usually fine.

---

# Interview Questions

## 1. What are Keys in React?

**Answer:**

Keys are unique identifiers that help React identify list items during rendering. They allow React to efficiently determine which items were added, removed, or updated.

---

## 2. Why Should Keys Be Unique?

**Answer:**

Unique Keys let React correctly match elements between renders. Duplicate Keys can lead to incorrect updates, rendering issues, and unexpected behavior.

---

## 3. Why Is Using the Index as a Key Dangerous?

**Answer:**

Indexes change when items are inserted, removed, or reordered. This can cause React to associate the wrong component or state with the wrong data, leading to bugs such as incorrect input values or unexpected UI behavior.

---

## 4. What Is the Best Key to Use?

**Answer:**

A stable, unique identifier such as a database ID.

Example:

```jsx
key={product.id}
```

---

## 5. Can We Use `Math.random()` as a Key?

**Answer:**

No.

`Math.random()` generates a new value on every render, so React treats every item as new and recreates the entire list, hurting both performance and correctness.

---

# Easy Memory Trick

Imagine a library.

Every book has a unique barcode.

```
Book A → 1001

Book B → 1002

Book C → 1003
```

Even if the books are moved to different shelves,

their barcodes never change.

React Keys work the same way.

They identify items by a **stable identity**, not by their position.

---

# Summary

- Keys are unique identifiers used by React to track list items.
- They help React efficiently update only the elements that changed.
- Keys should be unique among sibling elements and should remain stable between renders.
- Database IDs are the best choice for Keys.
- Avoid duplicate Keys because they confuse React's reconciliation process.
- Avoid using array indexes as Keys for dynamic lists because positions change when items are inserted, removed, or reordered.
- Use the array index only for simple, static lists that never change order or content.