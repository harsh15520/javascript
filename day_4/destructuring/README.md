# Extraction of Values in JavaScript: Destructuring Assignment Explained

In JavaScript, **destructuring assignment** is the preferred and most flexible way to extract values from arrays and objects into variables.

***

## Array Destructuring

Extract items from an array by declaring variables in the desired position:

```js
const arr = [1, 2, 3];
const [x, y, z] = arr;
// x = 1, y = 2, z = 3
```
- You can skip items by leaving blanks:
  ```js
  const [a, , c] = arr; // a = 1, c = 3
  ```
- Or unpack remaining elements into a separate array using the **rest operator**:
  ```js
  const [first, ...rest] = arr; // first = 1, rest = [2, 3]
  ```
- Swap variables quickly:
  ```js
  let a = 4, b = 7;
  [a, b] = [b, a]; // a = 7, b = 4
  ```


***

## Object Destructuring

Extract properties by declaring variables matching property names:

```js
const user = { name: "Sara", age: 25, city: "Delhi" };
const { name, age } = user;
// name = "Sara", age = 25
```

- The order does not matter; only the property name must match.
- Use **aliasing** to assign to a different variable name:
  ```js
  const { city: homeTown } = user; // homeTown = "Delhi"
  ```
- Set **default values** for missing properties:
  ```js
  const { country = "India" } = user; // country = "India" (defaulted if missing)
  ```
- Extract the rest of the object:
  ```js
  const { name, ...other } = user;
  // name = "Sara", other = { age: 25, city: "Delhi" }
  ```


***

## Accessing Object Properties

There are two main access styles:

- **Dot notation (`object.property`)**
  - Use for property names that are valid identifiers (no spaces, special chars, starts with letter).
  - Example: `user.name`

- **Bracket notation (`object["property"]`)**
  - Use for property names with spaces, symbols, or from variables.
  - Example: `user["city"]`, `user[propName]`

***

## Built-in Extraction Methods

JavaScript provides methods to extract object data for iteration or transformation:

- `Object.keys(obj)` — returns array of property names.
- `Object.values(obj)` — returns array of property values.
- `Object.entries(obj)` — returns array of `[key, value]` pairs.

```js
const data = { apple: 1, banana: 2, cherry: 3 };
Object.keys(data);   // ['apple', 'banana', 'cherry']
Object.values(data); // [1, 2, 3]
Object.entries(data);// [['apple', 1], ['banana', 2], ['cherry', 3]]
```


***

## Mutability with Arrays

Declaring an array as `const` means the variable cannot be reassigned, but its contents can be mutated:
```js
const nums = [10, 20];
nums.push(30);      // [10, 20, 30] (allowed)
nums[0] = 5;        // [5, 20, 30] (allowed)
```


***

## Practical Examples with Array Methods

Many array operations rely on extraction as part of processing:

```js
const numbers = [1, 2, 3];
numbers.forEach((num) => { console.log(num); }); // extracts each value

// Filtering and mapping:
const names = ["John", "Sara", "Mike"];
const upperCasedNames = names.map(name => name.toUpperCase());
// ["JOHN", "SARA", "MIKE"]
```


***
