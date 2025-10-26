Template literals in JavaScript (also known as template strings) extend the power of traditional string creation by enabling interpolation, multi-line formatting, and tagged template functions.[1][2][5][7]

## 1. Syntax and Structure

Template literals are defined using **backticks (`)** instead of single (`'`) or double (`"`) quotes.

```js
`string text`
`string text ${expression} more text`
tagFunction`string text ${expression}`
```
- **Backticks (`)** — indicate that the string is a template literal.
- **${expression}** — used to embed variables or evaluate expressions.
- **tagFunction** — allows advanced preprocessing when using tagged templates.[2][1]

## 2. String Interpolation

Backticks allow variables and expressions to be inserted directly within strings via `${ ... }`. The result of the expression becomes part of the final string.

### Variable Interpolation Example
```js
const name = "Alice";
console.log(`Hello, ${name}!`);
// Output: Hello, Alice!
```

### Expression Interpolation Example
```js
const a = 10, b = 5;
console.log(`The sum of ${a} and ${b} is ${a + b}.`);
// Output: The sum of 10 and 5 is 15.
```

You can embed **any valid JavaScript expression**, including function calls and ternary operators.[4][7]

```js
const user = "John";
console.log(`User type: ${user === "Admin" ? "Administrator" : "Guest"}`);
```

## 3. Multi-line Strings

Template literals allow multi-line strings without `\n` escape characters.

```js
const message = `This is line one
This is line two
This is line three`;
console.log(message);
```
This preserves both formatting and indentation, making it ideal for generating text blocks, emails, or HTML templates.[5][7]

## 4. Contrast with Regular Strings

Regular quoted strings (`'` or `"`) require concatenation or escaping:
```js
// Traditional method
const name = "Bob";
console.log("Hello, " + name + "!"); // Tedious

// Template literal
console.log(`Hello, ${name}!`); // Simpler and cleaner
```
They also cannot span multiple lines without special syntax:
```js
// Invalid:
"Line 1
Line 2"; // ❌ SyntaxError

// Works only with backticks:
`Line 1
Line 2`; // ✅
```

## 5. Advanced Usage: Tagged Templates

Tagged templates allow you to attach a **custom function** to process the literal and its substitutions. The function receives:
- **An array of string parts** (between expressions)
- **The evaluated expressions** as subsequent arguments

### Example
```js
function highlight(strings, ...values) {
  return strings.map((str, i) => `${str}<strong>${values[i] ?? ""}</strong>`).join("");
}

const name = "Alice", age = 25;
const result = highlight`My name is ${name} and I am ${age} years old.`;

console.log(result);
// Output: My name is <strong>Alice</strong> and I am <strong>25</strong> years old.
```
The tag function has full control over how to merge and transform values, enabling use cases like:
- Syntax highlighting
- Dynamic string sanitization
- Localization or formatting rules.[6][7]

## 6. Escaping Backticks

To include a backtick inside a template literal, use a **backslash**:
```js
console.log(`To escape a backtick, use \` like this.`);
```

## 7. Real-World Use Cases

- **Dynamic content generation:**
  ```js
  const user = { name: "Emma", id: 108 };
  console.log(`Welcome ${user.name}, your ID is ${user.id}.`);
  ```
- **HTML templating:**
  ```js
  const title = "Dashboard";
  const html = `
    <div class="header">
      <h1>${title}</h1>
    </div>`;
  ```
- **Tagged template sanitization (security use):**
  ```js
  safeHTML`<p>${userInput}</p>`;
  ```
- **Multi-line console output:**
  ```js
  console.log(`
  ===== Report =====
  Name: John
  Score: 92
  ==================
  `);
  ```

## Summary Table

| Feature | Simple Quotes (' / ") | Backticks (`) |
|----------|----------------------|---------------|
| Variable Interpolation | No | Yes (`${}`) |
| Expression Embedding | No | Yes |
| Multi-line Support | No (requires `\n`) | Yes |
| Tagged Templates | No | Yes |
| Escaping Backticks | Not applicable | `\`` |
