# Advanced Post Processing and Updates with Destructuring

Let me show you various patterns for processing posts and updating data.

## 1. Variety of Processing First Post Only

### First Post with Validation
```javascript
function validateFirstPost({
  object2: {
    object3: {
      list1: [firstPost = null]
    } = {}
  } = {}
} = {}) {
  if (!firstPost) {
    return { valid: false, error: 'No posts found' };
  }
  
  const hasId = 'integer3' in firstPost;
  const hasTitle = 'string2' in firstPost;
  
  return {
    valid: hasId && hasTitle,
    post: firstPost,
    errors: [
      ...(!hasId ? ['Missing post ID'] : []),
      ...(!hasTitle ? ['Missing post title'] : [])
    ]
  };
}

console.log(validateFirstPost(object1));
// { valid: true, post: {...}, errors: [] }

console.log(validateFirstPost({}));
// { valid: false, error: 'No posts found' }
```
### First Post with Related Data
```javascript
function getFirstPostWithContext({
  integer1: userId,
  object2: {
    object3,                              // Keep full context
    object3: {
      string1: author,
      list1: [
        firstPost,
        ...otherPosts
      ]
    }
  }
}) {
  return {
    currentPost: firstPost,
    nextPost: otherPosts[0] || null,
    author,
    userId,
    postsRemaining: otherPosts.length,
    allPostsContext: object3  // Full object for other operations
  };
}

console.log(getFirstPostWithContext(object1));
// {
//   currentPost: { integer3: 1, string2: 'first post' },
//   nextPost: { integer4: 2, string3: 'second post' },
//   author: 'bob',
//   userId: 200,
//   postsRemaining: 1,
//   allPostsContext: {...}
// }
```

### Conditional First Post Processing
```javascript
function processFirstPostConditionally(data, options = {}) {
  const {
    object2: {
      object3: {
        string1: author,
        list1: [firstPost]
      }
    }
  } = data;
  
  const { includeAuthor = true, uppercase = false } = options;
  
  let result = { ...firstPost };
  
  if (includeAuthor) {
    result.author = author;
  }
  
  if (uppercase && firstPost.string2) {
    result.string2 = firstPost.string2.toUpperCase();
  }
  
  return result;
}

console.log(processFirstPostConditionally(object1, { uppercase: true }));
// { integer3: 1, string2: 'FIRST POST', author: 'bob' }

console.log(processFirstPostConditionally(object1, { includeAuthor: false }));
// { integer3: 1, string2: 'first post' }
```

## 2. Updating Posts with Destructuring - Advanced Patterns

### Update Specific Post by Index
```javascript
function updatePostByIndex(data, index, updates) {
  const {
    object2: {
      object3: {
        list1: posts
      }
    }
  } = data;
  
  // Create new posts array with updated post
  const updatedPosts = posts.map((post, idx) =>
    idx === index ? { ...post, ...updates } : post
  );
  
  return {
    ...data,
    object2: {
      ...data.object2,
      object3: {
        ...data.object2.object3,
        list1: updatedPosts
      }
    }
  };
}

const updated1 = updatePostByIndex(object1, 0, { 
  string2: 'Updated First Post',
  edited: true 
});

console.log(updated1.object2.object3.list1[0]);
// { integer3: 1, string2: 'Updated First Post', edited: true }
```

### Add New Post
```javascript
function addPost(data, newPost) {
  const {
    object2: {
      object3: {
        list1: existingPosts
      }
    }
  } = data;
  
  // Generate new ID based on existing posts
  const maxId = Math.max(
    ...existingPosts.map(p => p.integer3 || p.integer4 || 0)
  );
  
  const postWithId = {
    ...newPost,
    integer3: maxId + 1
  };
  
  return {
    ...data,
    object2: {
      ...data.object2,
      object3: {
        ...data.object2.object3,
        list1: [...existingPosts, postWithId]
      }
    }
  };
}

const withNewPost = addPost(object1, { string2: 'third post' });
console.log(withNewPost.object2.object3.list1);
// [
//   { integer3: 1, string2: 'first post' },
//   { integer4: 2, string3: 'second post' },
//   { integer3: 3, string2: 'third post' }
// ]
```

### Delete Post
```javascript
function deletePost(data, postId) {
  const {
    object2: {
      object3: {
        list1: posts
      }
    }
  } = data;
  
  const filteredPosts = posts.filter(post => {
    const currentId = post.integer3 || post.integer4;
    return currentId !== postId;
  });
  
  return {
    ...data,
    object2: {
      ...data.object2,
      object3: {
        ...data.object2.object3,
        list1: filteredPosts
      }
    }
  };
}

const afterDelete = deletePost(object1, 1);
console.log(afterDelete.object2.object3.list1);
// [{ integer4: 2, string3: 'second post' }]
```

### Update First Post Only
```javascript
function updateFirstPost(data, updates) {
  const {
    object2: {
      object3: {
        list1: [firstPost, ...restPosts]
      }
    }
  } = data;
  
  const updatedFirst = { ...firstPost, ...updates };
  
  return {
    ...data,
    object2: {
      ...data.object2,
      object3: {
        ...data.object2.object3,
        list1: [updatedFirst, ...restPosts]
      }
    }
  };
}

const updatedFirst = updateFirstPost(object1, { 
  string2: 'Brand New First Post',
  featured: true 
});

console.log(updatedFirst.object2.object3.list1[0]);
// { integer3: 1, string2: 'Brand New First Post', featured: true }
```

### Conditional Update
```javascript
function updatePostsWhere(data, condition, updates) {
  const {
    object2: {
      object3: {
        list1: posts
      }
    }
  } = data;
  
  const updatedPosts = posts.map(post =>
    condition(post) ? { ...post, ...updates } : post
  );
  
  return {
    ...data,
    object2: {
      ...data.object2,
      object3: {
        ...data.object2.object3,
        list1: updatedPosts
      }
    }
  };
}

// Update posts with specific ID
const conditionalUpdate = updatePostsWhere(
  object1,
  post => post.integer3 === 1,
  { highlighted: true }
);

console.log(conditionalUpdate.object2.object3.list1[0]);
// { integer3: 1, string2: 'first post', highlighted: true }
```

### Reorder Posts
```javascript
function reorderPosts(data, fromIndex, toIndex) {
  const {
    object2: {
      object3: {
        list1: posts
      }
    }
  } = data;
  
  const newPosts = [...posts];
  const [movedPost] = newPosts.splice(fromIndex, 1);
  newPosts.splice(toIndex, 0, movedPost);
  
  return {
    ...data,
    object2: {
      ...data.object2,
      object3: {
        ...data.object2.object3,
        list1: newPosts
      }
    }
  };
}

const reordered = reorderPosts(object1, 0, 1);
console.log(reordered.object2.object3.list1);
// Second post is now first, first post is now second
```
These patterns demonstrate how destructuring makes complex data manipulation more readable and maintainable, especially when working with deeply nested structures!
