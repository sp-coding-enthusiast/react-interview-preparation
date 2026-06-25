# What are React Elements?

React Elements are the **smallest building blocks of a React application**.

Think of them as **blueprints (instructions)** that tell React:

> "What should appear on the screen?"

A React Element is simply a JavaScript object that describes a UI component.

---

# Real-Life Analogy

Imagine you want to build a house.

### Blueprint

The architect creates a blueprint:

- Living room
- Kitchen
- Bedroom

The blueprint is **not the actual house**.

It only describes what the house should look like.

Similarly:

- React Element = Blueprint
- Real DOM Element = Actual House

React uses React Elements to build the actual UI in the browser.

---

# Simple Example

```jsx
const element = <h1>Hello World</h1>;