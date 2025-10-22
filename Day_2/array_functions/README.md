# Detailed Arrow Function Syntax

Arrow functions in JavaScript provide a concise way to write function expressions using the `=>` syntax. Here's a comprehensive breakdown of all syntax variations:

## Basic Syntax

```js
const functionName = (param1, param2, ...paramN) => {
  // function body
  return result;
};
```

## Syntax Variations

### 1. Multiple Parameters
Standard syntax with parentheses:
```js
const add = (a, b) => a + b;
const multiply = (x, y) => x * y;
```

### 2. Single Parameter
Parentheses are **optional** with exactly one parameter:
```js
// With parentheses
const square = (x) => x * x;

// Without parentheses (preferred)
const square = x => x * x;
const double = n => n * 2;
```

### 3. No Parameters
Empty parentheses are **required** when there are no parameters:
```js
const sayHi = () => "Hello!";
const getRandomNumber = () => Math.random();
```

### 4. Single Expression (Implicit Return)
When the function body contains a single expression, you can omit curly braces and the `return` keyword:
```js
const sum = (a, b) => a + b;
const isEven = n => n % 2 === 0;
const greet = name => `Hello, ${name}!`;
```

### 5. Multiple Statements (Explicit Return)
Use curly braces for multiple statements and include explicit `return`:
```js
const calculateTotal = (price, tax) => {
  const subtotal = price * 1.1;
  const total = subtotal + tax;
  return total;
};
```

### 6. Returning Object Literals
Wrap object literals in parentheses to distinguish from function body braces:
```js
const createUser = (name, age) => ({ name: name, age: age });
// or with shorthand
const createUser = (name, age) => ({ name, age });
```

## Complete Syntax Examples

### No Arguments
```js
const greet = () => console.log("Hello!");
```

### One Argument
```js
const double = n => n * 2;
const isAdult = age => age >= 18;
```

### Multiple Arguments
```js
const max = (a, b) => a > b ? a : b;
const fullName = (first, last) => `${first} ${last}`;
```

### Multi-line Function
```js
const processData = (data) => {
  const cleaned = data.trim();
  const upper = cleaned.toUpperCase();
  return upper;
};
```

### Returning Objects
```js
// Wrong - JavaScript thinks {} is function body
const makePerson = (name) => { name: name }; // Error

// Correct - wrap in parentheses
const makePerson = (name) => ({ name: name });
```

## Common Use Cases

### Array Methods
```js
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(n => n * 2);
const evens = numbers.filter(n => n % 2 === 0);
const sum = numbers.reduce((acc, n) => acc + n, 0);
```

### Callbacks
```js
setTimeout(() => console.log("Delayed"), 1000);
button.addEventListener('click', () => alert('Clicked!'));
```

### Promise Chains
```js
fetch('/api/data')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

## Syntax Comparison Chart

| Parameters | Syntax | Example |
|------------|--------|---------|
| 0 params | `() => expression` | `() => 42` |
| 1 param | `param => expression` | `x => x * 2` |
| 2+ params | `(a, b) => expression` | `(a, b) => a + b` |
| Multi-line | `(params) => { ... }` | `(x) => { return x * 2; }` |
| Return object | `() => ({ ... })` | `() => ({ id: 1 })` |

## Key Syntax Rules

1. **Parentheses around parameters:**
   - Required for 0 or 2+ parameters
   - Optional for exactly 1 parameter

2. **Return statement:**
   - Implicit when no curly braces (single expression)
   - Explicit `return` required with curly braces

3. **Function body:**
   - No braces for single expression
   - Curly braces required for multiple statements

4. **Object literal returns:**
   - Must wrap in parentheses: `() => ({ key: value })`
