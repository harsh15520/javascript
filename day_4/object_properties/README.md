# Destructuring a Complex Nested Object

Let me work with your complex nested structure and show various destructuring patterns.

## Your Object Structure

```javascript
const object1 = {
  integer1: 200,
  object2: {
    object3: {
      integer2: 42,
      string1: "bob",
      list1: [
        // object4
        { integer3: 1, string2: 'first post' },
        // object5
        { integer4: 2, string3: 'second post' }
      ]
    }
  }
};
```

## 1. Basic Nested Destructuring

### Extract Deeply Nested Values
```javascript
// Extract values at different nesting levels
const {
  integer1,
  object2: {
    object3: {
      integer2,
      string1,
      list1
    }
  }
} = object1;

console.log(integer1);  // 200
console.log(integer2);  // 42
console.log(string1);   // "bob"
console.log(list1);     // Array with both posts
```

### Extract Array Elements
```javascript
// Destructure the nested array
const {
  object2: {
    object3: {
      list1: [firstPost, secondPost]
    }
  }
} = object1;

console.log(firstPost);   // { integer3: 1, string2: 'first post' }
console.log(secondPost);  // { integer4: 2, string3: 'second post' }
```

### Extract Values from Array Objects
```javascript
// Destructure objects within the array
const {
  object2: {
    object3: {
      list1: [
        { integer3, string2 },
        { integer4, string3 }
      ]
    }
  }
} = object1;

console.log(integer3);  // 1
console.log(string2);   // 'first post'
console.log(integer4);  // 2
console.log(string3);   // 'second post'
```

## 2. Using Rest Pattern with Nested Arrays

```javascript
// Get first post and rest of posts
const {
  object2: {
    object3: {
      list1: [firstPost, ...remainingPosts]
    }
  }
} = object1;

console.log(firstPost);        // { integer3: 1, string2: 'first post' }
console.log(remainingPosts);   // [{ integer4: 2, string3: 'second post' }]
```

```javascript
// Get specific fields from first post, keep rest of array
const {
  object2: {
    object3: {
      string1,
      list1: [{ string2 }, ...otherPosts]
    }
  }
} = object1;

console.log(string1);      // "bob"
console.log(string2);      // 'first post'
console.log(otherPosts);   // [{ integer4: 2, string3: 'second post' }]
```

## 3. Creating Both Parent and Nested Variables

```javascript
// Keep intermediate objects AND extract nested values
const {
  object2,                           // Full object2
  object2: {
    object3,                         // Full object3
    object3: {
      integer2,
      string1,
      list1                          // Full array
    }
  }
} = object1;

console.log(object2);   // Full nested structure
console.log(object3);   // { integer2: 42, string1: "bob", list1: [...] }
console.log(list1);     // Array of posts
console.log(string1);   // "bob"

// Use cases:
// - Pass object3 to a function
// - Use string1 directly in logic
// - Iterate over list1
```

```javascript
// Keep array AND extract elements
const {
  object2: {
    object3: {
      list1,                              // Full array
      list1: [firstPost, secondPost]      // Individual elements
    }
  }
} = object1;

console.log(list1.length);        // 2
console.log(firstPost.string2);   // 'first post'
console.log(secondPost.string3);  // 'second post'
```

## 4. Renaming Variables While Destructuring

```javascript
// Rename to more meaningful variable names
const {
  integer1: userId,
  object2: {
    object3: {
      integer2: age,
      string1: username,
      list1: posts
    }
  }
} = object1;

console.log(userId);     // 200
console.log(age);        // 42
console.log(username);   // "bob"
console.log(posts);      // Array of posts
```

```javascript
// Rename nested array elements
const {
  object2: {
    object3: {
      list1: [
        { integer3: firstId, string2: firstTitle },
        { integer4: secondId, string3: secondTitle }
      ]
    }
  }
} = object1;

console.log(firstId);      // 1
console.log(firstTitle);   // 'first post'
console.log(secondId);     // 2
console.log(secondTitle);  // 'second post'
```

## 5. Default Values for Missing Properties

```javascript
// Add default values in case properties don't exist
const {
  integer1 = 0,
  object2: {
    object3: {
      integer2 = 0,
      string1 = "anonymous",
      string4 = "default value",  // Doesn't exist in object
      list1 = []
    } = {}
  } = {}
} = object1;

console.log(string4);  // "default value" (used default)
```

## 6. Practical Use Cases

### Function Parameter Destructuring
```javascript
// Extract specific nested values in function parameters
function displayUserPosts({
  integer1: userId,
  object2: {
    object3: {
      string1: username,
      list1: posts
    }
  }
}) {
  console.log(`User ${username} (ID: ${userId}) has ${posts.length} posts:`);
  posts.forEach(post => {
    const title = post.string2 || post.string3;
    console.log(`- ${title}`);
  });
}

displayUserPosts(object1);
// User bob (ID: 200) has 2 posts:
// - first post
// - second post
```

### Processing First Post Only
```javascript
function getFirstPostDetails({
  object2: {
    object3: {
      string1,
      list1: [{ integer3, string2 }]
    }
  }
}) {
  return {
    author: string1,
    postId: integer3,
    title: string2
  };
}

console.log(getFirstPostDetails(object1));
// { author: 'bob', postId: 1, title: 'first post' }
```

### Combining Multiple Patterns
```javascript
// Complex real-world scenario
const {
  integer1,                              // Top level value
  object2: {
    object3,                             // Keep full object3
    object3: {
      string1: author,                   // Rename
      list1,                             // Keep full array
      list1: [
        firstPost,                       // Keep full first post
        { integer4, string3: lastTitle } // Extract from second post
      ]
    }
  }
} = object1;

console.log('User ID:', integer1);           // 200
console.log('Author:', author);              // "bob"
console.log('Full object3:', object3);       // Full nested object
console.log('All posts:', list1);            // Full array
console.log('First post:', firstPost);       // Full first post object
console.log('Last post ID:', integer4);      // 2
console.log('Last post title:', lastTitle);  // 'second post'
```

### Updating Posts with Destructuring
```javascript
function updatePost(data, postIndex, updates) {
  const {
    object2: {
      object3: {
        list1: posts
      }
    }
  } = data;
  
  // Create updated post
  const updatedPost = { ...posts[postIndex], ...updates };
  
  // Return new structure with updated post
  return {
    ...data,
    object2: {
      ...data.object2,
      object3: {
        ...data.object2.object3,
        list1: posts.map((post, idx) => 
          idx === postIndex ? updatedPost : post
        )
      }
    }
  };
}

const updated = updatePost(object1, 0, { string2: 'Updated first post' });
console.log(updated.object2.object3.list1[0].string2);
// 'Updated first post'
```

## 7. Skipping Nested Elements with Rest

```javascript
// Skip intermediate objects, extract what you need
const {
  object2: {
    object3: {
      list1: [, ...restPosts]  // Skip first post
    }
  }
} = object1;

console.log(restPosts);  // [{ integer4: 2, string3: 'second post' }]
```

```javascript
// Skip and extract with rest at multiple levels
const {
  integer1,
  object2: {
    object3: {
      integer2,
      list1: [, { integer4, ...restOfSecondPost }]
    }
  }
} = object1;

console.log(integer1);              // 200
console.log(integer2);              // 42
console.log(integer4);              // 2
console.log(restOfSecondPost);      // { string3: 'second post' }
```

The key is understanding that destructuring patterns can be combined at any depth, and you can mix object destructuring, array destructuring, rest patterns, renaming, and default values all in a single expression!
