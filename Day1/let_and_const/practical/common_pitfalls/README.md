Migrating from var to let and const in JavaScript is a crucial step for writing safer, clearer, and more maintainable code, as it enforces block scoping and immutability where appropriate.

### Rewriting Strategy: const First Principle

- **Use const by default:** If a variable’s reference should never change, declare it with const. This makes your intent clear and prevents accidental reassignment.
- **Use let only when necessary:** If a variable’s value will change (such as loop counters or accumulators), use let. Both let and const are block-scoped, preventing variable leakage outside their intended block.

### Common Pitfalls When Replacing var

#### 1. Hoisting and the Temporal Dead Zone (TDZ)
- **var** is hoisted and initialized as undefined, so accessing it before its declaration does not throw an error.
- **let/const** are hoisted but not initialized, so accessing them before their declaration throws a ReferenceError due to the TDZ.
- **Best practice:** Always declare and initialize let/const variables before using them to avoid runtime errors.

#### 2. Loop Variable Leakage and Closure Capture
- **var** leaks loop variables outside the block, which can cause bugs, especially with closures.
- **let** confines the variable to the loop block, preventing leakage and making closures behave as expected.
  ```js
  for (let i = 0; i < 10; i++) { /* ... */ }
  // i is not accessible outside the loop
  ```

#### 3. Conditional Function Declarations
- Function declarations inside blocks are block-scoped in strict mode, unlike var which is function-scoped.
- **Correct pattern:** Use a function expression assigned to a let variable declared outside the block for conditional logic.
  ```js
  let welcome;
  if (age < 18) {
    welcome = function() { alert("Hello!"); };
  } else {
    welcome = function() { alert("Greetings!"); };
  }
  welcome();
  ```

#### 4. Misunderstanding const Immutability
- **const** prevents reassignment of the variable reference, but does not make objects or arrays immutable.
  ```js
  const user = { name: "Alice" };
  user.name = "Charlie"; // Allowed
  user = { name: "Bob" }; // Error: reassignment forbidden
  ```

### Summary Table

| Keyword | Scope         | Hoisting | Reassignment | Redeclaration | TDZ      | Immutability |
|---------|--------------|----------|--------------|---------------|----------|--------------|
| var     | Function/global | Yes      | Yes          | Yes           | No       | No           |
| let     | Block         | Yes      | Yes          | No            | Yes      | No           |
| const   | Block         | Yes      | No           | No            | Yes      | Reference only|

Switching to let and const makes code more predictable, prevents accidental variable leakage, and enforces best practices for modern JavaScript development.
