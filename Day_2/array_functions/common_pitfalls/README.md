# Common Pitfalls with JavaScript Array Methods

Based on the search results and common development issues, here are the most frequent mistakes developers make when working with array methods:

## 1. Not Using the Returned Value

**Most common mistake:** Calling array methods like `map()`, `filter()`, or `reduce()` but forgetting that they return a **new array** and don't modify the original.

```js
// Wrong - result is ignored
let numbers = [1, 2, 3];
numbers.map(n => n * 2);
console.log(numbers); // Still [1, 2, 3]

// Correct - capture the new array
let doubled = numbers.map(n => n * 2);
console.log(doubled); // [2, 4, 6]
```

## 2. Using the Wrong Method

Confusing which method to use for different operations:

```js
// Wrong - using map when you need filter
let users = ['Alice', 'Bob', 'Charlie'];
let filtered = users.map(name => name.length > 3);
// Result: [true, false, true] - not what you want!

// Correct - use filter
let filtered = users.filter(name => name.length > 3);
// Result: ['Alice', 'Charlie']
```

**Quick reference:**
- `map()` - Transform each element, returns new array of same length
- `filter()` - Keep only matching elements, returns new array (possibly shorter)
- `reduce()` - Combine all elements into single value
- `forEach()` - Execute function for each element, returns nothing

## 3. Forgetting to Return Values in Callbacks

Arrow functions without curly braces return automatically, but with braces you need explicit `return`:

```js
// Wrong - no return statement
let doubled = [1, 2, 3].map(n => {
  n * 2; // This doesn't return anything!
});
console.log(doubled); // [undefined, undefined, undefined]

// Correct
let doubled = [1, 2, 3].map(n => {
  return n * 2;
});
// Or use implicit return
let doubled = [1, 2, 3].map(n => n * 2);
```

## 4. Misunderstanding Object References

Arrays store references to objects, not copies:

```js
let original = [{name: 'John'}];
let copy = original;
copy[0].name = 'Jane';
console.log(original[0].name); // 'Jane' - original changed too!

// Correct - create shallow copy
let copy = [...original];
// Or deep copy for nested objects
let copy = JSON.parse(JSON.stringify(original));
```

## 5. Array Length Pitfalls

Array length is based on the highest index, not the number of elements:

```js
let arr = [];
arr[100] = 'value';
console.log(arr.length); // 101, but only 1 element exists!

// Better - use push()
arr.push('value'); // Adds at the end
```

## 6. Using for...in Instead of for...of

`for...in` iterates over object properties (including inherited ones), not just array elements:

```js
let arr = [1, 2, 3];
arr.customProp = 'test';

// Wrong - includes custom properties
for (let key in arr) {
  console.log(key); // 0, 1, 2, customProp
}

// Correct - only array elements
for (let value of arr) {
  console.log(value); // 1, 2, 3
}
// Or use forEach
arr.forEach(value => console.log(value));
```

## 7. Modifying Arrays During Iteration

Changing an array while looping through it causes unpredictable results:

```js
// Dangerous - removing items during iteration
let arr = [1, 2, 3, 4];
for (let i = 0; i < arr.length; i++) {
  if (arr[i] % 2 === 0) {
    arr.splice(i, 1); // Skips elements!
  }
}

// Correct - use filter to create new array
let filtered = arr.filter(n => n % 2 !== 0);
```

## 8. Not Handling Empty Arrays or Undefined Values

Always check for edge cases:

```js
// Risky
let total = users.reduce((sum, user) => sum + user.salary, 0);
// Fails if any user.salary is undefined

// Better - provide defaults
let total = users.reduce((sum, user) => sum + (user.salary || 0), 0);
```

## 9. Chaining Methods Inefficiently

Multiple array iterations can be combined for better performance:

```js
// Inefficient - 3 separate loops
let result = arr
  .filter(n => n > 0)
  .map(n => n * 2)
  .filter(n => n < 100);

// Better - combine filters when possible
let result = arr
  .filter(n => n > 0 && n * 2 < 100)
  .map(n => n * 2);
```

## 10. Treating Arrays as Associative Arrays

JavaScript arrays shouldn't be used with string keys:

```js
// Wrong - abusing arrays
let arr = [];
arr['name'] = 'John'; // Works but wrong!
console.log(arr.length); // 0 - doesn't count non-numeric keys

// Correct - use objects for key-value pairs
let obj = {};
obj['name'] = 'John';
// Or modern Map
let map = new Map();
map.set('name', 'John');
```

## Best Practices Summary

✅ **Always capture return values** from map, filter, reduce
✅ **Choose the right method** for your task
✅ **Use explicit returns** with curly braces
✅ **Create copies** when you don't want to mutate originals
✅ **Use for...of or forEach** for iteration, not for...in
✅ **Handle edge cases** like empty arrays and undefined values
✅ **Use push()** instead of direct index assignment
✅ **Test with edge cases** (empty arrays, single elements, undefined values)
