 
## Agenda
 + Modules in Javascript
 + BigInt data type
 + Symbol datatype 
 + Proxies in javascript
 + Map and Objects
 + Sets
 + Weak references
     + Use case of weekSet
 + Weak map
 + Generator
## Modules in JavaScript

### Definition

A **module** in JavaScript is a self-contained and reusable piece of code that encapsulates variables, functions, and classes. It helps organize and manage code by keeping related functionality together in separate files.

### Example

Suppose you have an HTML file `index.html` with two JavaScript scripts `script1.js` and `script2.js`. In `script1.js`, we define variables, and in `script2.js`, we access those variables.

```htmlembedded
<!-- index.html -->
<!DOCTYPE html>
<html lang = "en">
<head>
  <title>Module Example</title>
</head>
<body>
  <script src = "script1.js"></script>
  <script src = "script2.js"></script>
</body>
</html>
```

```javascript
// script1.js
let name = "Learner";
function greet(){
    console.log("Hello")
}
```

```javascript
// script2.js
greet()
console.log(name);
```

### Output and Explanation

**Output:**
```plaintext
hello Learner
```
**Explanation:**
  + The HTML file serves as the entry point of our application. The JavaScript included in the HTML file represents separate modules that interact with each other.
 + In the `script1.js` file, we define functions and variables.
 + In the `script2.js` file, we access the variables `script1.js` and display the output. 
 + Despite being in a separate module, we can access the variables because the variables' scope is limited to the module in which they were defined.


**[Ask the learners]**

What is a potential **disadvantage** of organising code using separate scripts in an HTML file?

**Answer:** One disadvantage is that the variables in `script1.js` are in the global scope, making them accessible to other scripts and potentially leading to **naming conflicts** and **unintended modifications**.

### Example
Let us see another example to demonstrate the naming conflicts and how they can affect the data. Let us update the scripts as follows:

```javascript
// script1.js
let count = 0;

function increment() {
  count++;
}
```

```javascript
// script2.js
let count = 10; // or count=10;
increment(); 

console.log(count); // Output: 11
```

### Explanation
 + Both `script1.js` and `script2.js` have a variable named `count`. 
 + When `increment()` is called from `script2.js`, it modifies the `count` variable from `script1.js`, resulting in unintended behavior and potential naming conflicts.

Using **modules** helps mitigate such problems by encapsulating variables within module **scopes**, preventing interference with other parts of the code.

### Modules 

ES6 introduced a standardised way to **organize** and **structure** JavaScript code using modules. Modules allow us to split our code into separate files, making it more maintainable and scalable. 


Modules allow us to **export** and **import** values from one module to another. There are two types of exports: 
 + Default exports.
 + Named exports.

**Named Exports:** 
  + Named exports involve explicitly specifying the items we want to export from a module. 
  + These items can include functions, variables, or classes. 
  + When using named exports, we wrap each export with curly braces `{}` when importing them into another module.

**Default Exports:** 
 + Default exports allow us to designate a single `default` export for a module. 
 + This is especially useful when there's a primary functionality or class that the module provides. 
 + When importing a default export, we can assign it any name we choose.

### Example
Consider that, we have two files: `utils.js` and `app.js`. These files represent different modules, showcasing various types of exports and import techniques.

```javascript
// utils.js

// Named Exports
export function greet(name) {
  return `Hello, ${name}!`;
}

export const PI = 3.14159;

// Default Export
export default function multiply(a, b) {
  return a * b;
}
```

The above code defines both named exports (`greet` function and `PI` variable) and a default export (`multiply` function).


```javascript
// app.js

// Importing Named Exports
import { greet, PI } from './utils.js';

console.log(greet('Alice')); // Output: "Hello, Alice!"
console.log(`Value of PI: ${PI}`); // Output: "Value of PI: 3.14159"

// Importing Default Export
import customMultiply from './utils.js';

console.log(customMultiply(2, 3)); // Output: 6
```

In `app.js`, we import the named exports using destructuring and the default export using any name of our choice (`customMultiply`).

### Imports are Cached

In JavaScript, when a module is imported into another module, the imported module's contents are cached after the **first import** and subsequent imports of the same module will **not execute** the module code again; instead, they will receive a reference to the **cached** module. 

#### Example

Consider two modules: `counter.js` and `main.js`. We'll explore how importing and caching work using a simple counter as example.

```javascript
// counter.js
let count = 0;

export function increment() {
  count ++ ;
}

export function getCount() {
  return count;
}
```

```javascript
// main.js
import { increment, getCount } from './counter.js';

console.log(getCount()); // Output: 0
increment();
console.log(getCount()); // Output: 1
```

```javascript
import { getCount } from './counter.js';

console.log(getCount()); // Output: 1
```
#### Explanation

 + The `counter.js` module exports functions to manipulate a counter. 
 + When `main.js` imports and runs the functions, it increments the counter. 
 + Notice that even though we import `getCount` again in a different part of `main.js`, the count remains at 1. 
 + This is because the module `counter.js` was cached after the initial import, preserving its state.

**[Ask the learners]:**
Why does the count remain at 1 even after importing and incrementing the counter in a different part of the code?

**Answer:**
Imports are **cached** in JavaScript. When a module is imported, its contents are cached after the first import. Subsequent imports of the same module refer to the cached version, preserving the state and behavior of the module across different parts of the code.

## BigInt DataType

### Definition

The BigInt data type was introduced to handle integer values that **exceed** $(2^{63} -1)$ or **limits** of the Number data type. 

 + Traditional Number type in JavaScript has limitations due to its 64-bit representation, and BigInt addresses this limitation by allowing the representation of arbitrarily **large integers** with precision.
 + BigInt literals are **suffixed** with 'n' to distinguish them from regular Number literals.
 + BigInt is stored in **heap** data type.
 + BigInt ensures that **precision** is **not lost** even with very large numbers.

#### Example

Let us create two BigInt values using different methods. Then we can perform basic mathematical operations on these BigInts.

```javascript
// Creating BigInts
const largeNumber = 12345678901234567890890n;
const fromString = BigInt("98765432109609876543210");

// BigInt Operation
const product = largeNumber * fromString;
console.log("Product:", product);

```
**Output:**
```plaintext
Product: 1219326311366925771283513183853664053650356900n
```
The output shows that BigInts retain precision even with very large values.


## Symbol DataType

### Definition
The Symbol data type in JavaScript is a **unique** and **immutable** value that is guaranteed to be a unique identifier and can be used as a property key in objects. They are often used to prevent property key collisions when working with objects.

### Example
**Comparison of Primitive Data Types**

```javascript
const string1 = "hello";
const string2 = "hello";

console.log(string1 === string2); // Output: true

const array1 = [1, 2, 3];
const array2 = [1, 2, 3];

console.log(array1 === array2); // Output: false
```

**[Ask the learners]:**
Why does the comparison of two identical primitive data types (`string1` and `string2`) result in `true`, while the comparison of two identical non-primitive data types (`array1` and `array2`) results in `false`?

**Answer:**
Primitive data types (like strings) are compared by their values, so when their values are the same, the comparison results in `true`. Non-primitive data types (like arrays and objects), on the other hand, are compared by reference, not by value. Hence, even if their contents are the same, the reference is different, leading to a comparison result of `false`.

## Example
Let us explore the properties of Symbol datatype with an example.

```javascript
// Creating Symbols
const symbol1 = Symbol("description");
const symbol2 = Symbol("description");

// Using Symbols as Object Properties
const person = {
  name: "Learner",
  age: 30,
  [symbol1]: "A person"
};

console.log(person[symbol1]); 
console.log(person[symbol2]); 

// Checking Symbol Description
console.log(symbol1.toString()); 
console.log(symbol2.description); 
```

**Output:**
```plaintext
"A person"
undefined
"Symbol(description)"
"description"
```


#### Explanation

 + We have created two symbols (`symbol1` and `symbol2`) with the same description. 
 + However, symbols are always unique, so `symbol1` and `symbol2` are not equal. 
 + We then use a symbol as a property key in an object (`person`), demonstrating how symbols can be used to avoid property key collisions.
 + Additionally, we show how to access the description of a symbol.
## Proxies in JavaScript

### Definition
Proxies are objects that allow you to **intercept** and **customize** operations performed on objects, such as reading, writing, and deleting properties. 

 + Proxies are used to define custom behavior for fundamental operations on an object, giving more control over its interactions.
 + Proxies can be used to validate input and prevent certain operations.

## Example

In this example, let us create an object `obj` with properties `eng` and `math`. We then define a `handler` object containing custom behavior for the `get` and `set` operations using the Proxy. 
```javascript
// Object
let obj = { eng: "English", math: "Mathematics" };
// custom handler
let handler = {
  get(target, prop) {
    if (prop in target) {
      return target[prop];
    } else {
      throw new Error("Property does not exist");
    }
  },
  set(target, prop, value) {
    if (prop in target) {
      target[prop] = value;
      return true;
    } else {
      throw new Error("Cannot add new property");
    }
  }
};

// Create Proxy
let proxy = new Proxy(obj, handler);

console.log(proxy.eng);
console.log(proxy.science); // Throws: Error - Property does not exist

proxy.history = "History"; // Throws: Error - Cannot add new property
```
**Output:**

```plaintext
"English"
```

**Explanation:**

+ A Proxy is created with the object and the `handler ` defines how operations on the Proxy are intercepted and handled.
 + The `get` handler intercepts property access. If the property exists in the target object (`obj`), it returns the property value. If not, it throws an error.
 + The `set` handler intercepts property assignment. If the property exists in the target object, it sets the property value and returns `true`. If not, it throws an error.

**[Ask the learners]:**
How can Proxies be used to hide certain properties of an object?

**Answer:**
Proxies can be used to hide properties by selectively allowing or disallowing access to them. By customizing the `get` and `set` handlers, you can control whether certain properties are visible or accessible, enhancing privacy and security.
## Map and Objects

## Definition


**Objects** in JavaScript are collections of **key-value pairs**, where keys are strings or symbols.

A **Map** is a collection of **key-value** pairs, where keys can be of any data type. Unlike objects, keys in maps maintain their original **order**.


In JavaScript, both Maps and Objects are used to store collections of key-value pairs. However, they have different behaviors and use cases. 

### Example 1: Creating a Map and an Object

Let us create a simple object and Map to understand the syntax, properties, and methods.

```javascript
const cap = {
  name: "Steve",
  occupation: "Super Hero",
  age: 35
};

cap.newProp = "Hello";
delete cap.name;

const personMap = new Map();
personMap.set('name', "Jasbir");
personMap.set('occupation', 'Super Hero');
personMap.set('age', 35);

console.log("Object:", cap);
console.log("Map:", personMap);
```

 + In this example, we start by creating an object `cap` with properties like `name`, `occupation`, and `age`. 
 + We add a new property using dot notation and delete the `name` property using the `delete`  keyword which is slower compared to the map.
 + We need to use the `Map()` class to create a map object. 
 + Then, we create a Map `personMap` and use the `set` method to add key-value pairs. 
 + Finally, we print both the object and the map.

**Output:**

```plantext
Object: { occupation: 'Super Hero', age: 35, newProp: 'Hello' }

Map: Map(3) { 'name' => 'Jasbir', 'occupation' => 'Super Hero', 'age' => 35 }
```
The output in the Map shows arrow marks(`=>`), but both outputs are relatively the same.

Let's dive into the differences between Maps and Objects and explore some examples to understand them better.

### Example 2: Iterating through Map and Object

 + We can iterate over a map using any type of iteration except the `for in` loop, 
 + We can iterate over an object using only the `for in` loop and the other method doesn't work.

```javascript
cap.forEach((value, key) => {
  console.log("Object Key:", key, "Value:", value);
}); // throws error


personMap.forEach((value, key) => {
  console.log("Map Key:", key, "Value:", value);
}); // produce output

for (let [key, value] of Object.entries(cap)) {
  console.log("Object Key:", key, "Value:", value);
} // throws error 

for (let [key, value] of personMap) {
  console.log("Map Key:", key, "Value:", value);
} // produce output

for(let key in cap){
    console.log("Object Key:", key, "Value:", cap[key]); 
} // produce ouptut

```
 + We can use the `forEach` method to iterate over the map and it works like a charm, but the same can't be done for objects.
 + We also use the `Object.entries` method to iterate through the object and the destructuring assignment to iterate through the map.

**Output:**

```plaintext
Map Key: name Value: Jasbir
Map Key: occupation Value: Super Hero
Map Key: age Value: 35
Map Key: name Value: Jasbir
Map Key: occupation Value: Super Hero
Map Key: age Value: 35
Object Key: occupation Value: Super Hero
```

### Example 3: Datatype of Keys

Map can have keys as any datatype, but Objects are restricted to keys of type String, Number, and Symbol only.

```javascript
// user objects 
const user1 = { name: 'Bob' };
const user2 = { name: 'Charlie' };
const user3 = { name: { there: 'Alice' } };
// values
const preferencesObj1 = { theme: 'dark', language: 'en' };
const preferencesObj2 = { theme: 'light', language: 'fr' };
const preferencesObj3 = { theme: 'dark', language: 'en' };
// Create a Map
const preferenceMap = new Map();
// Use objects as keys
preferenceMap.set(user1, preferencesObj1);
preferenceMap.set(user2, preferencesObj2);
preferenceMap.set(user3, preferencesObj3);

console.log("User 1's name:", user1.name);
console.log("User 1's preferences:", preferenceMap.get(user1));
```

 + We create objects representing users and their preferences. 
 + We then use a Map `preferenceMap` to associate users with their preference objects.
 + We print the name of a user and retrieve their preferences using the Map.

**Output:**

```plaintext
User 1's name: Bob
User 1's preferences: { theme: 'dark', language: 'en' }
```

### Example 4: Using `JSON.stringify` with Map and Object

The stringify method is used to convert the object and the map into **JSON format**.

In this example, let me show the result of using `JSON.stringify` with both an object and a Map. 

```javascript
console.log("Object JSON:", JSON.stringify(cap));
console.log("Map JSON:", JSON.stringify([...personMap]));
```
**Output:**

```plaintext
Object JSON: {"occupation":"Super Hero","age":35,"newProp":"Hello"}
Map JSON: [{"name":"Jasbir"},{"occupation":"Super Hero"},{"age":35}]
```

**[Ask the learners]** What is the advantage of using a Map over an object?

**Answer:** Maps allow keys of any data type and maintain the order of keys.

Sure, let me explain the Set in JavaScript and provide an example with explanations and output.

---

## Sets in JavaScript

In JavaScript, a Set is a built-in data structure that allows you to store a collection of **unique** values. Unlike arrays, Sets ensure that **no duplicate values** are stored, making them a great choice when you need to work with a collection of items without repetition.

### Example: Working with a Set

```javascript
// Creating a new Set
const uniqueSet = new Set();

// Adding values to the Set
uniqueSet.add(1);
uniqueSet.add(2);
uniqueSet.add(3);
uniqueSet.add(1); // Duplicate value won't be added

// Deleting a value from the Set
uniqueSet.delete(2);

// Checking if a value exists in the Set
console.log("Does the Set have 2?", uniqueSet.has(2)); // Output: false

// Displaying the Set
console.log("Set contents:", uniqueSet);
```

#### Explanation

1. We start by creating an empty Set called `uniqueSet`.
2. Using the `add` method, we add values `1`, `2`, and `3` to the Set. Notice that even though we attempt to add `1` again, it won't be added as Sets only store unique values.
3. We use the `delete` method to remove the value `2` from the Set.
4. The `has` method checks whether the value `2` exists in the Set. Since we previously deleted `2`, it returns `false`.
5. Finally, we print the contents of the Set using `console.log`.

**Output:**

```plaintext
Does the Set have 2? false
Set contents: Set(2) { 1, 3 }
```

The number 2 is not present in the set as it was deleted using the `delete()` method and we can also see that only unique values are present in the set.

**[Ask the learners]** How is a Set different from an array?

**Answer:** Sets store unique values and automatically remove duplicates, while arrays can contain duplicates.


---

## Weak References and Garbage Collection

Weak references and garbage collection play crucial roles in managing memory efficiently in JavaScript. Let's explore these concepts through examples.

### Example 1: Weak References and Garbage Collection

```javascript
let john = {};
john.age = 25;
console.log("john 5", john);
john = null;
console.log("john 7", john);
```

In this example, we create an object `john` and add a property `age` to it. We log the object's state and then set `john` to `null`. The object becomes unreachable and is a candidate for garbage collection.

**Output:**

```plaintext
john 5 { age: 25 }
john 7 null
```

### Example 2: Weak References and Arrays

```javascript
let john = {};
let arr = [john];
john.age = 25;
console.log("john 13", john);
john = null;
console.log("john 15", john);
console.log("john 17", arr[0]);
```

In this example, we create an object `john` and an array `arr` containing a reference to `john`. After adding a property to `john`, we log its state, set `john` to `null`, and then check both `john` and the object inside the array.

**Output:**

```plaintext
john 13 { age: 25 }
john 15 null
john 17 { age: 25 }
```

**[Ask the learners]** Are the outcomes of the two examples the same?

**No**, in the first example, the object `john` becomes eligible for garbage collection when set to `null`, while in the second example, the reference to the object inside the array (`arr[0]`) keeps the object alive even after setting `john` to `null`.
### Garbage Collection and Memory Management

Garbage collection is a process where the JavaScript engine automatically reclaims memory occupied by objects that are no longer reachable. This prevents memory leaks and ensures efficient memory management.
### Sequence of Storage: Stack and Heap

In Example 2, the sequence of storage and the role of the garbage collector can be explained as follows:

1. The variable `john` is created in the stack, pointing to an empty object in the heap.
2. The array `arr` is created in the stack, containing a reference to the same object in the heap.
3. The `age` property is added to the object in the heap.
4. The reference to the object inside `arr` keeps it alive, even after `john` is set to `null`.
5. When `john` is set to `null`, the original reference to the object is removed, but the reference in `arr` keeps the object from being garbage collected.
### Strong References in Map

```javascript
let john = { name: "John" };
let map = new Map();
map.set(john, "hi");
john = null;
console.log(map.get(john));
```

In this example, even after setting `john` to `null`, the Map retains a strong reference to the object, preventing it from being garbage collected.

**Output:**

```plaintext
hi
```

### WeakMap: Weak References

```javascript
let john = { name: "John" };
let john2 = { name: "Steve" };
let weakMap = new WeakMap();
weakMap.set(john, "hi");
weakMap.set(john2, "hello");
john = null;
console.log(weakMap.get(john));
console.log(weakMap.get(john2));
```

In this example, we use a WeakMap, which allows its keys to be garbage collected when no other references to the keys exist.

**Output:**

```plaintext
undefined
hello
```
As john is set to `null`, unlike Strong reference, the output value becomes `undefined` due to weak maps.

---

## Use Case of WeakMap: Caching Process Results

Due to weak references in weakMap, if an object only exists as a key in a WeakMap and **no other references** to it exist, it can be garbage collected. This property makes WeakMap suitable for scenarios like **caching**, where you want to store results but still allow them to be garbage collected if the associated objects are no longer needed.

### Example: Caching Process Results

The `module.js` is where the processing and caching logic is defined.

```javascript
let cache = new WeakMap();

function process(obj) {
  if (!cache.has(obj)) {
    // processing
    let futDate = Date.now() + 2000;
    while (Date.now() < futDate) {
      let result = obj;
      cache.set(obj, result);
      return result;
    }
  }
  return cache.get(obj);
}

module.exports = process;
```

The `client.js` is where the caching logic is used with different objects.

```javascript
const process = require("./module.js");

let obj1 = { task: "Hello" };
let obj2 = { task: "Bye" };

let result1 = process(obj1);
console.log("result 1:", result1);

let result2 = process(obj1);
console.log("result 2:", result2);

let result3 = process(obj2);
console.log("result 3:", result3);

obj1 = null;
```

#### Explanation:

1. In `module.js`, a WeakMap `cache` is created to store processed results. The `process` function takes an object as an argument.
2. If the object is not already in the cache, the function starts processing by entering a loop for a short time (2 seconds). During this time, it stores the object itself as a result in the cache using `cache.set(obj, result)`.
3. After processing, the function returns the result, and the cache retains a weak reference to the object.
4. In `client.js`, the `process` function is imported. Two objects, `obj1` and `obj2`, are created.
5. `obj1` is processed, and its result is stored in `result1`. The result is then logged to the console.
6. The `process` function is called again with `obj1`, but this time, it retrieves the result from the cache instead of recomputing it. The cached result is logged as `result 2`.
7. The `process` function is called with `obj2`, and its result is logged as `result 3`.
8. Finally, `obj1` is set to `null` to remove the strong reference to it.

**Output:**

```plaintext
result 1: { task: 'Hello' }
result 2: { task: 'Hello' }
result 3: { task: 'Bye' }
```


---

## WeakSet in JavaScript

A WeakSet is a specialized type of collection that holds **weak references** to objects. This means that if an object is only referenced by a WeakSet, it can be garbage collected even if the WeakSet still contains a reference to it. 

### Example: Using WeakSet for Visited Users

```javascript
let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

let visitedSet = new WeakSet();

visitedSet.add(john); // John visited us
visitedSet.add(pete); // Then Pete
visitedSet.add(john); // John again

// visitedSet now has 2 users

john = null; // John is not a visitor anymore

console.log("John visited?", visitedSet.has(john));
console.log("Mary visited?", visitedSet.has(mary));
visitedSet.delete(pete);
```

#### Explanation:

  + We have created three objects: `john`, `pete`, and `mary`, each with a `name` property, and used a weekSet. 
  + Since `visitedSet` holds weak references, even though we set `john` to `null`, it doesn't prevent `john` from being garbage collected.
 + The `has` method is used to check if `john` and `mary` visited.

**Output:**

```plaintext
John visited? false
Mary visited? false
```


**[Ask the learners]** When would you use WeakSets and WeakMaps instead of regular Sets and Maps?

**Answer:** WeakSets and WeakMaps are useful when you want objects to be eligible for garbage collection when they are no longer needed elsewhere in your code.

Certainly, I'll explain Generators using the provided example code and provide detailed explanations for each step along with the expected output.

---
## Generators in JavaScript

Generators are a special type of function in JavaScript that allow you to **pause** their **execution** and later **resume** it. 

They are defined using the `function*` syntax and include one or more `yield` statements that temporarily pause the execution and yield a value to the caller. 

### Example

We can define a generator function `fn` using the `function*` syntax. Inside the function, we have three `console.log` statements to indicate each yield step.

```javascript
function * fn() {
  console.log("First yield");
  yield 10;
  console.log("Second yield");
  yield 20;
  console.log("Third yield");
  return 30;
}

let generator = fn();

console.log("Before");
console.log("Between", generator.next().value);
console.log("Between", generator.next().value);
console.log("After");
```

#### Explanation:

 + A generator instance named `generator` can be created by invoking `fn()`.
 + We call `generator.next().value` for the first time. The execution starts, and it reaches the first `yield` statement.
 +  We call `generator.next().value` again. The execution resumes from where it was paused and reaches the second `yield` statement. 
 + We log `Between` followed by the value returned by the second `yield` statement, which is `20`.
8. We log `After` to indicate the completion of the generator.

**Output:**

```plaintext
Before
First yield
Between 10
Second yield
Between 20
After
```

**[Ask the learners]** How do generators differ from regular functions?

**Answer:** Generators can be paused and resumed, maintaining their state between calls, which is not possible with regular functions.

