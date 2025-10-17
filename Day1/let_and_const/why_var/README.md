## Why var Still Exists

var persists solely for **backward compatibility** with legacy JavaScript code written before ES6 (2015). Modern JavaScript engines must continue supporting var to avoid breaking millions of existing websites.

## The Only "Cases" for var (All Discouraged)

### 1. Legacy Browser Support (Obsolete in 2025)
**Historical context only:** Before 2015-2017, some developers used var because older browsers (IE 10 and below, Safari 7-9) didn't support let/const. However, in 2025, this is no longer relevant - all modern browsers support ES6, and transpilers like Babel can convert let/const to var for ancient browsers if absolutely necessary.

### 2. Intentional Hoisting Exploitation (Bad Practice)
**Anti-pattern:** Some developers argue var's hoisting to undefined is useful for accessing variables before declaration. However, this is universally considered poor practice - modern code should declare variables before use.

```js
console.log(x); // undefined with var
var x = 5;

console.log(y); // ReferenceError with let
let y = 5;
```

### 3. Global Object Property Creation (Anti-pattern)
**Anti-pattern:** var at the global level creates a property on the window object, while let/const do not. Some legacy code exploits this, but modern practices avoid polluting the global object.

```js
var globalVar = "accessible";
console.log(window.globalVar); // "accessible"

let blockScoped = "not accessible";
console.log(window.blockScoped); // undefined
```

### 4. Function Scope Instead of Block Scope (Anti-pattern)
**Anti-pattern:** Deliberately using var to leak variables outside blocks. This is the opposite of what modern programming recommends - variables should have the narrowest scope possible.

## Modern JavaScript Best Practices

**Industry consensus** for code written in 2025:

1. **Never use var** - there is no good reason to use it in new code
2. **Use const by default** - for all values that don't need reassignment
3. **Use let only when necessary** - for variables that must be reassigned
4. **Avoid var completely** - even in legacy code refactoring, convert var to let/const

## Teaching Context

The only legitimate reason to **learn** about var (not use it) is educational:
- Understanding legacy codebases you may encounter at work
- Comprehending how JavaScript evolved and why let/const were introduced[6]
- Recognizing var's problems to appreciate modern solutions[7]
