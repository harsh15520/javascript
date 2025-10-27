# Building Greeting Messages with Template Literals

Here are several examples of how to build greeting messages using JavaScript template literals, ranging from simple to advanced:

## Basic Greeting with Variables

```js
const name = "Alice";
const greeting = `Hello, ${name}!`;
console.log(greeting);
// Output: Hello, Alice!
```

## Time-Based Greeting with Expressions

```js
const name = "John";
const hour = new Date().getHours();
const timeOfDay = hour < 12 ? "morning" : hour < 18 ? "afternoon" : "evening";

const greeting = `Good ${timeOfDay}, ${name}! How are you today?`;
console.log(greeting);
// Output: Good afternoon, John! How are you today!
```

## Personalized Greeting with Multiple Variables

```js
const user = {
  firstName: "Emma",
  lastName: "Watson", 
  age: 28,
  city: "London"
};

const personalGreeting = `
Welcome back, ${user.firstName} ${user.lastName}!
You are ${user.age} years old and live in ${user.city}.
Have a wonderful day!
`;
console.log(personalGreeting);
```

## Dynamic Greeting with Conditional Logic

```js
const userName = "Alice";
const isNewUser = false;
const loginCount = 15;

const greeting = `
${isNewUser ? "Welcome" : "Welcome back"}, ${userName}!
${isNewUser ? "Thanks for joining us!" : `This is your ${loginCount}th visit.`}
`;
console.log(greeting);
```

## Advanced Greeting with Tagged Template

```js
function greetUser(strings, name, status) {
  const now = new Date();
  const currentHour = now.getHours();
  const timeOfDay = currentHour < 12 ? "morning" : currentHour < 17 ? "afternoon" : "evening";
  
  return `Good ${timeOfDay}, ${name}! Your account status is: ${status.toUpperCase()}${strings[2]}`;
}

const userName = "Bob";
const accountStatus = "premium";
const result = greetUser`Hello ${userName}, status: ${accountStatus}. Enjoy your session!`;
console.log(result);
// Output: Good afternoon, Bob! Your account status is: PREMIUM. Enjoy your session!
```

## Interactive Greeting Function

```js
function createGreeting(name, age, hobby) {
  return `
🌟 Hello there, ${name}!
📅 At ${age} years old, you're in your prime!
🎯 I heard you enjoy ${hobby} - that's awesome!
🚀 Ready to make today amazing?
  `;
}

console.log(createGreeting("Sarah", 25, "photography"));
```

## Benefits of Template Literals for Greetings:

- **Cleaner syntax** - no more string concatenation with `+`
- **Multi-line support** - perfect for formatted messages
- **Expression embedding** - calculations and function calls inline
- **Improved readability** - easier to see the final message structure
- **Dynamic content** - conditional logic right in the string
