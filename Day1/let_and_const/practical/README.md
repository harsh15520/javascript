Rewriting var to let and const is necessary in several real-world scenarios where var's function-scoping and hoisting behavior causes bugs or maintenance issues.

## Real-World Rewrite Cases

### 1. Event Handlers in Loops
**Problem:** When attaching event listeners inside loops, var causes all handlers to reference the same variable.

```js
// Old code with var (buggy)
for (var i = 0; i < buttons.length; i++) {
  buttons[i].onclick = function() {
    console.log('Button ' + i + ' clicked');
  };
}
// All buttons log the same final value of i

// Rewritten with let (correct)
for (let i = 0; i < buttons.length; i++) {
  buttons[i].onclick = function() {
    console.log('Button ' + i + ' clicked');
  };
}
// Each button logs its own index
```

### 2. Asynchronous Operations (setTimeout/Promises)
**Problem:** var in loops with async operations captures the wrong value.

```js
// Old code with var
for (var i = 1; i <= 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
// Prints: 4, 4, 4

// Rewritten with let
for (let i = 1; i <= 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
// Prints: 1, 2, 3
```

### 3. Configuration and Constants
**Problem:** var allows accidental reassignment of values that should remain fixed.

```js
// Old code with var
var API_KEY = "xyz123";
var MAX_RETRY = 3;
var BASE_URL = "https://api.example.com";

// Later in code, accidental reassignment
API_KEY = "wrong-key"; // No error

// Rewritten with const
const API_KEY = "xyz123";
const MAX_RETRY = 3;
const BASE_URL = "https://api.example.com";

API_KEY = "wrong-key"; // Error: Assignment to constant variable
```

### 4. Shopping Cart or Accumulator Functions
**Problem:** Mixing mutable and immutable values with only var provides no semantic distinction.

```js
// Old code with var
function calculateTotal(prices) {
  var total = 0;
  for (var i = 0; i < prices.length; i++) {
    var price = prices[i];
    total += price;
  }
  var discount = 0.1;
  var finalTotal = total - (total * discount);
  return finalTotal;
}

// Rewritten with let/const
function calculateTotal(prices) {
  let total = 0; // Changes in loop
  for (let i = 0; i < prices.length; i++) {
    const price = prices[i]; // Doesn't change per iteration
    total += price;
  }
  const discount = 0.1; // Fixed value
  const finalTotal = total - (total * discount); // Computed once
  return finalTotal;
}
```

### 5. Conditional Function Creation
**Problem:** Function declarations inside blocks behave unpredictably with var in strict mode.

```js
// Old code (breaks in strict mode)
if (userAge < 18) {
  function welcome() {
    alert("Hello!");
  }
} else {
  function welcome() {
    alert("Greetings!");
  }
}
welcome(); // Error: welcome is not defined

// Rewritten with let and function expressions
let welcome;
if (userAge < 18) {
  welcome = function() {
    alert("Hello!");
  };
} else {
  welcome = function() {
    alert("Greetings!");
  };
}
welcome(); // Works correctly
```

### 6. Module-Level Variables
**Problem:** var at global level pollutes the window object, causing naming conflicts.

```js
// Old code with var (pollutes global)
var userId = 12345;
var userName = "John";
console.log(window.userId); // 12345

// Rewritten with const/let (doesn't pollute global)
const userId = 12345;
let userName = "John";
console.log(window.userId); // undefined
```

### 7. Block-Scoped Temporary Variables
**Problem:** var leaks temporary variables outside their intended scope.

```js
// Old code with var
function processData(data) {
  if (data.valid) {
    var result = data.value * 2;
    var temp = result + 10;
  }
  console.log(result); // Accessible (leaks out)
  console.log(temp);   // Accessible (leaks out)
}

// Rewritten with let/const
function processData(data) {
  if (data.valid) {
    const result = data.value * 2;
    const temp = result + 10;
  }
  console.log(result); // ReferenceError: result is not defined
}
```

## Best Practice Summary

- **Use const** for: API keys, configuration values, fixed references to functions/objects, calculated values that won't change
- **Use let** for: loop counters, accumulators, variables that need reassignment
- **Avoid var** in modern code: only maintain it in legacy codebases, otherwise migrate to let/const
