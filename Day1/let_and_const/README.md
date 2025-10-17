# Block-Scoped Variables in JavaScript

**Block scope** is a key feature of modern JavaScript, introduced with ES6, that allows variables to be limited to the nearest enclosing block (curly braces `{}`), rather than just functions or the global context. Let's break down how this works and why it's important for writing safe, predictable code.

## Declaring Block-Scoped Variables
- **let**: Declares a block-scoped variable. You can update its value, but you cannot redeclare it in the same block.
- **const**: Declares a block-scoped constant. You must initialize it at declaration, and you cannot reassign it. However, if the value is an object or array, you can still modify its contents.

```js
if (true) {
  let a = 10;
  const b = 20;
  // a and b are only accessible inside this block
}
// a and b are not accessible here
```

## Block Scope vs. var (Function Scope)
- **var**: Variables declared with `var` are *not* block-scoped. They are scoped to the function or global context, which can lead to bugs if you expect them to be limited to a block.

```js
if (true) {
  var x = 5;
}
console.log(x); // 5 (accessible outside the block)
```
### Key Differences in Loop Scoping

- **var (Function/Global Scope):**
  - A variable declared with var inside a loop is not limited to the loop block; it is accessible throughout the enclosing function or globally if not in a function.
  - After the loop, the variable retains its final value and can be accessed or modified outside the loop.

- **let (Block Scope):**
  - A variable declared with let in the loop header or body is only accessible within the loop block.
  - Attempting to access the variable outside the loop results in a ReferenceError, preventing accidental use or modification.

### Example Comparison

```javascript
// Using var
for (var i = 0; i < 3; i++) {
  // loop body
}
console.log(i); // 3 (i is accessible and retains its value)[web:45][web:48]

// Using let
for (let j = 0; j < 3; j++) {
  // loop body
}
console.log(j); // ReferenceError: j is not defined (j is block-scoped)[web:45][web:48]
```

### Why let is Preferred in Loops

- Using let keeps the loop variable local to the loop, preventing it from leaking into the surrounding scope and reducing the risk of bugs.
- This aligns with modern coding practices that favor minimizing global variables and maximizing variable locality for safer, more predictable code.

### 1. Definition and Cause
- The TDZ starts at the beginning of a block (curly braces `{}`) and ends when the let or const variable is declared and initialized.
- Accessing the variable before its declaration line throws a ReferenceError, even though the variable technically exists in the scope.
- Example:
  ```js
  {
    // TDZ for 'a' starts here
    // console.log(a); // ReferenceError
    let a = 10; // TDZ ends here
  }
  ```

### 2. Contrast with var (Hoisting)
- Variables declared with var are hoisted and initialized to undefined at the top of their scope, so accessing them before their declaration does not throw an error, but returns undefined.
- let and const are also hoisted, but not initialized, which is why accessing them before their declaration triggers the TDZ and a ReferenceError.
- Example:
  ```js
  console.log(b); // undefined
  var b = 5;

  console.log(c); // ReferenceError
  let c = 10;
  ```

### 3. Declaration Requirements and Initialization
- let and var can be declared without an initializer, but let variables are still subject to the TDZ until their declaration is processed.
- const must always be initialized at declaration, and is also subject to the TDZ.
- Example:
  ```js
  {
    // console.log(d); // ReferenceError
    const d = 20; // Must be initialized here
  }
  ```

### Summary Table

| Keyword | Hoisted | Initialized Before Declaration | TDZ Applies | Access Before Declaration |
|---------|---------|-------------------------------|-------------|--------------------------|
| var     | Yes     | Yes (undefined)               | No          | undefined                |
| let     | Yes     | No                            | Yes         | ReferenceError           |
| const   | Yes     | No (must initialize)          | Yes         | ReferenceError           |


## Block Scope and Function Declarations (Strict Mode)
In strict mode, function declarations inside blocks are only visible within that block. To use a function outside a conditional block, assign a function expression to a `let` variable declared outside the block.

```js
let fn;
if (condition) {
  fn = function() { /* ... */ };
}
// fn is accessible here
```

## Quick Review
- Use `let` and `const` for block-scoped variables.
- Prefer `const` for values that shouldn't change.
- Avoid `var` unless you specifically need function/global scope.
- Remember the TDZ: don't access `let`/`const` variables before declaration.

**Check your understanding:**
- What happens if you try to access a `let` variable before it's declared?
- Why is using `let` in loops safer than `var`?
