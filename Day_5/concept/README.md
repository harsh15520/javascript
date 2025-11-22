# Spread & Rest Operators - Complete Guide

The `...` operator has two roles depending on context: **spreading** (unpacking) or **resting** (collecting).

---

## Part 1: Spread Operator (Unpacking)

The spread operator **unpacks** elements from arrays/objects or arguments.

### 1.1 Array Spreading

#### Copying Arrays
```javascript
const original = [1, 2, 3];
const copy = [...original];

console.log(copy);           // [1, 2, 3]
console.log(copy === original); // false (different reference)

// Modifying copy doesn't affect original
copy.push(4);
console.log(original);       // [1, 2, 3]
console.log(copy);           // [1, 2, 3, 4]
```


#### Adding Elements
```javascript
const numbers = [2, 3, 4];

// Add at beginning
const withStart = [1, ...numbers];
console.log(withStart); // [1, 2, 3, 4]

// Add at end
const withEnd = [...numbers, 5];
console.log(withEnd); // [2, 3, 4, 5]

// Add at both ends
const withBoth = [0, 1, ...numbers, 5, 6];
console.log(withBoth); // [0, 1, 2, 3, 4, 5, 6]
```

#### Spreading Strings
```javascript
const str = "hello";
const chars = [...str];
console.log(chars); // ['h', 'e', 'l', 'l', 'o']

// Practical use
const unique = [...new Set(str)];
console.log(unique); // ['h', 'e', 'l', 'o']
```

#### Array in Array Operations
```javascript
// Find max/min
const numbers = [5, 2, 9, 1, 7];
console.log(Math.max(...numbers)); // 9
console.log(Math.min(...numbers)); // 1

// Old way (verbose)
console.log(Math.max.apply(null, numbers)); // 9
```

### 1.2 Object Spreading

#### Overriding Properties
```javascript
const defaults = {
  theme: 'light',
  fontSize: 14,
  notifications: true
};

const userPrefs = {
  theme: 'dark',
  fontSize: 16
};

// Later spreads override earlier ones
const settings = { ...defaults, ...userPrefs };
console.log(settings);
// { theme: 'dark', fontSize: 16, notifications: true }

// Order matters!
const reversed = { ...userPrefs, ...defaults };
console.log(reversed);
// { theme: 'light', fontSize: 14, notifications: true }
```

#### Adding/Updating Properties
```javascript
const user = { name: 'Alice', age: 30 };

// Add new property
const withEmail = { ...user, email: 'alice@example.com' };
console.log(withEmail);
// { name: 'Alice', age: 30, email: 'alice@example.com' }

// Update existing property
const olderUser = { ...user, age: 31 };
console.log(olderUser);
// { name: 'Alice', age: 31 }

// Multiple changes
const updated = { 
  ...user, 
  age: 31, 
  city: 'NYC',
  verified: true 
};
console.log(updated);
// { name: 'Alice', age: 31, city: 'NYC', verified: true }
```

#### Removing Properties (with destructuring)
```javascript
const user = { 
  id: 1, 
  name: 'Alice', 
  password: 'secret123', 
  age: 30 
};

// Remove password
const { password, ...userWithoutPassword } = user;
console.log(userWithoutPassword);
// { id: 1, name: 'Alice', age: 30 }

// Remove multiple properties
const { password: pwd, age, ...minimal } = user;
console.log(minimal);
// { id: 1, name: 'Alice' }
```

---

## Part 2: Rest Operator (Collecting)

The rest operator **collects** multiple elements into an array.

### 2.1 Rest in Function Parameters

#### Combining Regular and Rest Parameters
```javascript
function introduce(greeting, ...names) {
  return `${greeting} ${names.join(', ')}!`;
}

console.log(introduce('Hello', 'Alice'));
// "Hello Alice!"

console.log(introduce('Welcome', 'Bob', 'Charlie', 'David'));
// "Welcome Bob, Charlie, David!"

// Rest must be last parameter
// function invalid(...rest, last) {} // ❌ SyntaxError
```

#### Multiple Parameters with Rest
```javascript
function createUser(name, age, ...roles) {
  return {
    name,
    age,
    roles,
    roleCount: roles.length
  };
}

console.log(createUser('Alice', 30, 'admin', 'editor', 'viewer'));
// {
//   name: 'Alice',
//   age: 30,
//   roles: ['admin', 'editor', 'viewer'],
//   roleCount: 3
// }
```

#### Rest vs Arguments Object
```javascript
// Old way with arguments
function oldSum() {
  // arguments is array-like, not real array
  const args = Array.from(arguments);
  return args.reduce((acc, n) => acc + n, 0);
}

// Modern way with rest
function modernSum(...numbers) {
  // numbers is a real array
  return numbers.reduce((acc, n) => acc + n, 0);
}

console.log(oldSum(1, 2, 3));      // 6
console.log(modernSum(1, 2, 3));   // 6
```

### 2.2 Rest in Array Destructuring

#### Basic Rest in Arrays
```javascript
const [first, ...rest] = [1, 2, 3, 4, 5];
console.log(first); // 1
console.log(rest);  // [2, 3, 4, 5]

// Empty rest
const [a, b, c, ...remainder] = [1, 2, 3];
console.log(remainder); // []
```

#### Skipping Elements
```javascript
const numbers = [1, 2, 3, 4, 5, 6];

// Skip first, collect rest
const [, ...withoutFirst] = numbers;
console.log(withoutFirst); // [2, 3, 4, 5, 6]

// Get first and last, skip middle
const [first, ...middle] = numbers;
const last = middle.pop();
console.log(first, last); // 1 6
```

#### Practical Array Processing
```javascript
// Head and tail pattern
function processQueue([current, ...remaining]) {
  console.log('Processing:', current);
  if (remaining.length > 0) {
    console.log('Remaining:', remaining);
  }
}

processQueue([1, 2, 3, 4]);
// Processing: 1
// Remaining: [2, 3, 4]

// Split first from rest for recursion
function recursiveSum([head, ...tail]) {
  if (head === undefined) return 0;
  return head + recursiveSum(tail);
}

console.log(recursiveSum([1, 2, 3, 4])); // 10
```

### 2.3 Rest in Object Destructuring

#### Basic Rest in Objects
```javascript
const user = { 
  id: 1, 
  name: 'Alice', 
  email: 'alice@example.com',
  age: 30 
};

const { id, ...userWithoutId } = user;
console.log(id);              // 1
console.log(userWithoutId);   // { name: 'Alice', email: '...', age: 30 }
```

#### Extracting Specific Properties
```javascript
const config = {
  host: 'localhost',
  port: 3000,
  username: 'admin',
  password: 'secret',
  database: 'mydb',
  ssl: true
};

// Extract credentials, keep rest as connection config
const { username, password, ...connectionConfig } = config;
console.log(connectionConfig);
// { host: 'localhost', port: 3000, database: 'mydb', ssl: true }
```

#### Removing Multiple Properties
```javascript
const article = {
  id: 1,
  title: 'JavaScript Tips',
  content: '...',
  author: 'Alice',
  createdAt: '2024-01-01',
  updatedAt: '2024-01-15',
  internal_id: 'xyz',
  _private: 'data'
};

// Remove internal/private fields
const { internal_id, _private, ...publicArticle } = article;
console.log(publicArticle);
// { id: 1, title: '...', content: '...', author: '...', ... }
```

#### Nested Rest
```javascript
const data = {
  user: {
    id: 1,
    name: 'Alice',
    email: 'alice@example.com',
    profile: {
      bio: 'Developer',
      avatar: 'avatar.jpg',
      private: true
    }
  }
};

// Extract and clean nested data
const {
  user: {
    id,
    profile: { private: isPrivate, ...publicProfile },
    ...userDetails
  }
} = data;

console.log(publicProfile);
// { bio: 'Developer', avatar: 'avatar.jpg' }
console.log(userDetails);
// { name: 'Alice', email: 'alice@example.com' }
```

---

## Part 3: Combining Spread & Rest

### 3.1 Array Manipulations

#### Inserting Elements
```javascript
const original = [1, 2, 3, 4, 5];

// Insert at position 2
function insertAt(arr, index, ...items) {
  return [
    ...arr.slice(0, index),
    ...items,
    ...arr.slice(index)
  ];
}

console.log(insertAt(original, 2, 99, 100));
// [1, 2, 99, 100, 3, 4, 5]
```

#### Removing Elements
```javascript
const numbers = [1, 2, 3, 4, 5];

// Remove element at index
function removeAt(arr, index) {
  return [
    ...arr.slice(0, index),
    ...arr.slice(index + 1)
  ];
}

console.log(removeAt(numbers, 2));
// [1, 2, 4, 5]

// Remove multiple elements
function removeRange(arr, start, count) {
  return [
    ...arr.slice(0, start),
    ...arr.slice(start + count)
  ];
}

console.log(removeRange(numbers, 1, 3));
// [1, 5]
```

#### Replacing Elements
```javascript
const arr = [1, 2, 3, 4, 5];

// Replace at index
function replaceAt(arr, index, ...newItems) {
  return [
    ...arr.slice(0, index),
    ...newItems,
    ...arr.slice(index + 1)
  ];
}

console.log(replaceAt(arr, 2, 99, 100));
// [1, 2, 99, 100, 4, 5]
```

### 4.3 Common Pitfalls

```javascript
// ❌ Shallow copy issue
const original = { a: 1, b: { c: 2 } };
const copy = { ...original };
copy.b.c = 99;
console.log(original.b.c); // 99 (oops!)

// ✅ Deep copy with nested spread
const properCopy = {
  ...original,
  b: { ...original.b }
};
properCopy.b.c = 99;
console.log(original.b.c); // 2 (safe!)

// ❌ Rest must be last
// const { a, ...rest, b } = obj; // SyntaxError

// ✅ Correct
const { a, b, ...rest } = { a: 1, b: 2, c: 3, d: 4 };
```

The spread and rest operators are fundamental to modern JavaScript, enabling clean, immutable data manipulation and flexible function signatures!
