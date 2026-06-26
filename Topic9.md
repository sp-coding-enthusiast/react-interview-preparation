# 14. What are Props?

## Simple Definition

**Props** (short for **Properties**) are **data that a parent component sends to a child component**.

Think of Props as **information or instructions passed from one component to another**.

A child component receives the data and displays or uses it, but it **should not directly modify it**.

---

# Real-Life Analogy: Ordering Food

Imagine you go to a restaurant.

You tell the waiter:

- Pizza
- Large Size
- Extra Cheese

The waiter takes your order to the chef.

The chef prepares the pizza based on the information received.

```
Customer
      │
      │ Order Details
      ▼
Waiter
      │
      │ Passes Information
      ▼
Chef
```

In React:

- Parent Component = Customer
- Props = Order Details
- Child Component = Chef

The child component simply uses the information it receives.

---

# Another Analogy: Gift Box

Imagine you receive a gift box.

```
Gift Box

Inside:

🎁 Toy
🎁 Chocolate
🎁 Card
```

The box carries information.

Similarly,

Props carry data from one component to another.

---

# Why Do We Need Props?

Without Props, every component would display the same content.

Example:

```
Product Card

Laptop
₹50,000

Product Card

Laptop
₹50,000

Product Card

Laptop
₹50,000
```

Every product is identical.

Not useful.

---

# With Props

We create one Product component.

Then pass different information.

```
Product

Name = Laptop
Price = ₹50,000

Product

Name = Mobile
Price = ₹20,000

Product

Name = Headphones
Price = ₹3,000
```

Same component.

Different data.

---

# Basic Example

Parent Component

```jsx
function App() {
  return (
    <Welcome name="Saurabh" />
  );
}
```

Child Component

```jsx
function Welcome(props) {
  return <h1>Hello {props.name}</h1>;
}
```

Output

```
Hello Saurabh
```

---

# How It Works

```
Parent Component

↓

name = "Saurabh"

↓

Child Component

↓

props.name

↓

Hello Saurabh
```

---

# Multiple Props

Parent

```jsx
function App() {
  return (
    <Student
      name="Rahul"
      age={24}
      city="Bangalore"
    />
  );
}
```

Child

```jsx
function Student(props) {
  return (
    <>
      <h2>{props.name}</h2>
      <p>{props.age}</p>
      <p>{props.city}</p>
    </>
  );
}
```

Output

```
Rahul

24

Bangalore
```

---

# Props Are Like Function Parameters

Normal JavaScript Function

```javascript
function greet(name) {
    return "Hello " + name;
}

greet("John");
```

Output

```
Hello John
```

React Component

```jsx
function Welcome(props) {
    return <h1>Hello {props.name}</h1>;
}

<Welcome name="John" />
```

Output

```
Hello John
```

Very similar idea.

Functions receive parameters.

Components receive Props.

---

# Visual Representation

```
App Component

↓

name = "John"

↓

Welcome Component

↓

props.name

↓

Hello John
```

---

# Example: Product Card

Parent

```jsx
function App() {
  return (
    <>
      <Product
        name="Laptop"
        price={50000}
      />

      <Product
        name="Phone"
        price={20000}
      />

      <Product
        name="Tablet"
        price={30000}
      />
    </>
  );
}
```

Child

```jsx
function Product(props) {
  return (
    <div>
      <h2>{props.name}</h2>
      <p>₹{props.price}</p>
    </div>
  );
}
```

Output

```
Laptop

₹50000

----------------

Phone

₹20000

----------------

Tablet

₹30000
```

Notice that we wrote the Product component only once.

React reused it three times with different Props.

---

# Destructuring Props (Recommended)

Instead of writing:

```jsx
function Welcome(props) {
  return <h1>{props.name}</h1>;
}
```

You can write:

```jsx
function Welcome({ name }) {
  return <h1>{name}</h1>;
}
```

Or with multiple props:

```jsx
function Student({ name, age, city }) {
  return (
    <>
      <h2>{name}</h2>
      <p>{age}</p>
      <p>{city}</p>
    </>
  );
}
```

This is cleaner and is the preferred style in modern React.

---

# Passing Different Data Types

## String

```jsx
<User name="John" />
```

---

## Number

```jsx
<User age={25} />
```

---

## Boolean

```jsx
<User isAdmin={true} />
```

---

## Array

```jsx
<User skills={["React", "C#", ".NET"]} />
```

---

## Object

```jsx
<User user={{
    name: "John",
    age: 25
}} />
```

---

## Function

```jsx
<Button onClick={handleClick} />
```

Functions are commonly passed as Props so that child components can notify the parent when something happens (like a button click).

---

# Props Are Read-Only

One of the most important React rules:

A child component **must not modify its Props**.

Wrong

```jsx
function Welcome(props) {
    props.name = "Rahul";
}
```

This is not allowed.

Correct

```jsx
function Welcome(props) {
    return <h1>{props.name}</h1>;
}
```

The child only reads the Props.

Think of Props as a sealed package.

You can open it and use what's inside.

You should not change its contents.

---

# Parent Controls the Data

```
Parent

↓

Sends Props

↓

Child

↓

Displays Data
```

If the parent changes the Props, React automatically updates the child.

Example:

Initially

```jsx
<Welcome name="John" />
```

Output

```
Hello John
```

Later

```jsx
<Welcome name="David" />
```

Output

```
Hello David
```

The child didn't change anything.

The parent sent new Props.

---

# Default Props

Sometimes a parent doesn't send a value.

You can provide a default.

```jsx
function Welcome({ name = "Guest" }) {
  return <h1>Hello {name}</h1>;
}
```

Usage

```jsx
<Welcome />
```

Output

```
Hello Guest
```

---

# Props vs Variables

Variable

```javascript
let name = "John";
```

Only available inside the current function.

Props

```jsx
<Welcome name="John" />
```

Passed from one component to another.

---

# Props vs State (Quick Preview)

| Props | State |
|--------|-------|
| Passed by Parent | Managed by Component |
| Read-only | Can be updated |
| Used to pass data | Used to store changing data |
| Controlled by Parent | Controlled by the Component |

You'll learn **State** in the next topic, and that's where components can store and update their own data.

---

# Common Interview Questions

## 1. What are Props?

**Answer:**

Props are read-only inputs passed from a parent component to a child component. They allow components to receive and display dynamic data.

---

## 2. Can a Child Component Modify Props?

**Answer:**

No.

Props are immutable (read-only). Only the parent component should change the values passed as Props.

---

## 3. Why Are Props Important?

**Answer:**

Props make components reusable. Instead of creating multiple similar components, we create one component and pass different data through Props.

---

## 4. Can We Pass Functions as Props?

**Answer:**

Yes.

Passing functions as Props is a common React pattern. It allows child components to trigger actions in the parent component, such as handling button clicks or updating data.

---

# Real-Life Examples

Think of Props like:

- 📦 A courier package carrying items.
- 🍔 A food order sent to the kitchen.
- 🎫 A movie ticket containing your seat number.
- 📄 A job application form filled with your details.
- 🚚 A delivery slip that tells the destination.

In every case, information is **passed** from one place to another without the receiver changing it.

---

# Summary

- Props stand for **Properties**.
- Props are used to pass data from a parent component to a child component.
- They make components reusable and dynamic.
- Props are read-only and should never be modified by the child.
- Props can hold strings, numbers, booleans, arrays, objects, and even functions.
- Modern React commonly uses **destructuring** for cleaner access to Props.
- Props are one of the fundamental building blocks of React applications.