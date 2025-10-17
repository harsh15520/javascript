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

## Block Scope in Loops
Using `let` in loops creates a new variable for each iteration, which is especially useful in closures:

```js
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Logs: 0, 1, 2
```

If you use `var`, all closures share the same variable, often leading to unexpected results:

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Logs: 3, 3, 3
```

## The Temporal Dead Zone (TDZ)
Variables declared with `let` and `const` are hoisted but not initialized. Accessing them before their declaration in the block causes a **ReferenceError**. This period is called the *temporal dead zone* (TDZ).

```js
{
  // console.log(a); // ReferenceError
  let a = 1;
}
```

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
