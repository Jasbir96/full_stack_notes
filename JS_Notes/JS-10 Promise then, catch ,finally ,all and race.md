## Agenda
+ Promises
+ Promise then ,catch , finally 
+ Promise chaining 
+ Promise methods (all ,race)
+ Pollyfills of Promise.race 
+  Create promise API

---
##  Promises

A promise is a special object in JavaScript that represents the eventual completion (or failure) of an asynchronous operation. Promises provide a structured way to manage asynchronous tasks, making it easier to handle and coordinate complex operations without getting lost in callback hell.

## Explaining then

The `then` method is used to attach success callbacks to promises. It allows you to specify what should happen when the promise is resolved successfully. Multiple `then` callbacks can be attached to a promise, creating a chain of actions.

### Example
```javascript
// Example: Reading data from f1.txt
let fs = require("fs");
let promise = fs.promises.readFile("f1.txt");

promise.then(function(data) {
    console.log("Hi, the data is:", data);
});

promise.then(function(data) {
    console.log("Buffer format is:", data);
});

promise.then(function() {
    console.log("I am not accepting");
});
```

### Explanation:
- The data in `f1.txt` is "Hi, I am F1"
- The first two `then` callbacks log the data read from the file and the buffer format.
- The third `then` callback logs "I am not accepting".

### Output
```plaintext
Hi, the data is: Hi, I am F1
Buffer format is: Hi, I am F1
I am not accepting
```

### Explaining `catch`:

The `catch` method is used to handle errors that occur while executing a promise. It allows you to define what should happen when the promise is rejected. Multiple `catch` callbacks can be attached to handle different types of errors.
### Example

```javascript
// Example: Catching error when f1.txt does not exist
let fs = require("fs");
let promise = fs.promises.readFile("f1.txt");

promise.catch(function(err) {
    console.log("err is 1:", err);
});

promise.catch(function(err) {
    console.log("err is 2:", err);
});

promise.catch(function(err) {
    console.log("I am not accepting");
});
```

**[Ask the learners]**: What will be the output if the file "f1.txt" does not exists.

 - If the file does not exist, the first two `catch` callbacks log the error message from the failed promise, and the third `catch` callback logs "I am not accepting". The **output** will be:

```plaintext
err is 1: Error: no such file or directory
err is 2: Error: no such file or directory
I am not accepting
```

### Using the then Function with Separate Callbacks

Here's an alternative way to use the `then` function with separate success (`scb`) and error (`fcb`) callbacks:

```javascript
promise.then(scb, fcb);

function scb(data) {
    console.log("The data is: ", data);
}

function fcb(error) {
    console.log("Inside Catch: ", error.message);
}
```

In this example, the `scb` function is executed when the promise is resolved successfully, and the `fcb` function is executed if the promise is rejected.

**[Ask the learners]**: How would you implement the `promise.catch` functionality using the `fcb` function in this alternative approach?
 - You can implement the `promise.catch` functionality by using the `fcb` function as the error callback in the `then` function.
**Output**:

```plaintext
The data is: [contents of the file]
```
### Explaining  finally:

The `finally` method is used to attach callbacks that execute regardless of whether the promise is resolved or rejected. It's commonly used to perform cleanup or finalization actions.

### Example:
```javascript
// Example: Execution of finally
let fs = require("fs");
let promise = fs.promises.readFile("f1.txt");

promise.finally(function(err) {
    console.log("err is 1", err);
});

promise.finally(function(err) {
    console.log("err is 2", err);
});

promise.finally(function() {
    console.log("I am not accepting");
    console.log("Second line of finally");
});
```

**[Ask the learners]**: What do you think will be the order of execution for the `finally` callbacks? Will the order change if the promise is resolved or rejected?

 - The `finally` callbacks will execute in the order they are defined, and this order will not change regardless of whether the promise is resolved or rejected. The `finally` callbacks are designed to run regardless of the promise's outcome. All three `finally` callbacks will execute regardless of whether the promise is resolved or rejected with no data or `undefined` in `err`

### Output
```plaintext
err is 1 undefined
err is 2 undefined
I am not accepting
Second line of finally
```


---
## Definition of Resolve and Reject in Promises

In promises, `resolve` and `reject` are two fundamental actions that determine the outcome of a promise. When a promise is **resolved**, it means that the asynchronous operation associated with the promise has **successfully** completed. On the other hand, when a promise is **rejected**, it signifies that the operation has encountered an **error** or failure.

## Example
Let's explore an example that illustrates the concepts of resolving and rejecting promises. We'll use the `Promise.resolve` and `Promise.reject` methods to create and work with promises.

```javascript
const promise = Promise.resolve("Resolved value");

promise.then(function(value) {
    console.log("Resolved: ", value);
});

const promiseReject = Promise.reject("some error");

promiseReject.then(function() {
    console.log("This will not be executed");
}).catch(function(err) {
    console.log("Caught an error: ", err.message);
});
```

In this example, we create a promise using `Promise.resolve` with the value `"Resolved value"`. We then attach a success callback using the `then` method, which will be executed when the promise is resolved.

Additionally, we create another promise using `Promise.reject` with a custom error. We try to attach a success callback to this promise using `then`, but since the promise is rejected, the `catch` method is triggered, catching and handling the error.

**Output**:
```plaintext
Resolved: Resolved value
Caught an error: some error
```

In the first part of the example, the promise created using `Promise.resolve` is resolved with the value `"Resolved value"`. When the `then` callback is executed, the resolved value is logged to the console, resulting in the output "Resolved: Resolved value".

In the second part, the promise created using `Promise.reject` is intentionally rejected with a custom error using `new Error("Promise rejected")`. When the promise is rejected, the `catch` callback is invoked, catching the error and logging its message to the console. This leads to the output "Caught an error: Promise rejected".


## Chained Promises Example

Chained promises involve linking multiple asynchronous operations in a sequence, where the output of one operation feeds as input to the next. Each step in the chain is connected through `.then()` callbacks, enabling the handling of data transformation, error management, and more.

**Example:**
```javascript
let promise = Promise.resolve(10);
promise.then(function(data) {
    console.log("Step 1:", data); // Output: Step 1: 10
    return data * 2;
}).then(function(firstThenValue) {
    console.log("Step 2:", firstThenValue); // Output: Step 2: 20
    return firstThenValue + 3;
}).then(function(secondThenValue) {
    console.log("Step 3:", secondThenValue); // Output: Step 3: 23
    return secondThenValue * 2;
}).then(function(thirdThenValue) {
    console.log("Step 4:", thirdThenValue); // Output: Step 4: 46
});
```
### Step 1:
The promise is resolved with the value `10`. In the first `then` callback, the value `10` is logged to the console ("Step 1: 10"). The `return` statement in this callback passes the result `10` to the next `then` callback.

### Step 2:
The value `10` (from the previous `then`) is multiplied by `2` in the second `then` callback, resulting in `20`. This value is logged to the console ("Step 2: 20"), and `return data + 3` passes `23` to the next `then` callback.

### Step 3:
The value `23` (from the previous `then`) is logged to the console ("Step 3: 23"). Then, it is multiplied by `2` in the third `then` callback, resulting in `46`. This value is returned to be used in the next `then` callback.

### Step 4:
The final value `46` (from the previous `then`) is logged to the console ("Step 4: 46").




---

## Skipping in Promises

Skipping in promises refers to the behavior where if an error occurs in a `catch` block, the subsequent `then` block(s) in the chain will still be executed.

### Example
```javascript
Promise.resolve(100)
    .catch((err) => {
        console.log("err", err);
    })
    .then((data) => {
        console.log("data", data);
    });
```

**[Ask the learners]** What do you think will be the output of the above code? 

**Pause for learner responses**
 - The output might be surprising. Even though there is a `catch` block, since the `catch` block doesn't throw an error or return a rejected promise, the subsequent `then` block will still execute.

**Output**:
```plaintext
data 100
```


## Different Ways to Create Rejected Promises

In JavaScript, promises can be rejected using various methods. Let's explore each of these methods along with a simple example for each.

### 1. Promise.reject

The `Promise.reject` method allows you to explicitly create a rejected promise with a specified reason.

```javascript
const rejectedPromise1 = Promise.reject("Explicit rejection");
rejectedPromise1.catch((reason) => {
    console.log("Caught:", reason);
});
```

### 2. Getting a Rejected Value from a Promise-Based Function

If a promise-based function encounters an error or throws an error, the returned promise will be rejected.

```javascript
let fs = require("fs");
let Filepromise = fs.promises.readFile("f1.txt");

Filepromise.catch((error) => {
    console.log("Caught:", error);
});
```

### 3. Throwing a New Error

#### Throwing an Error from then

Consider the following example:

```javascript
Promise.resolve("Initial value")
    .then((data) => {
        console.log("Data:", data);
        throw new Error("Error from then");
    })
    .catch((err) => {
        console.log("Caught:", err.message);
    });
```

In this example, the promise is resolved with the value "Initial value". The `then` block logs the data and then throws an error. The subsequent `catch` block catches and logs the error.


### b) Throwing an Error from `catch`


```javascript
Promise.reject("Rejected value")
    .catch((data) => {
        console.log("Caught:", data);
        throw new Error("Error from catch");
    })
    .catch((err) => {
        console.log("Caught:", err.message);
    });
```

**[Ask the learners]** What will be the output of the code this time?  
**Pause for learner responses**

- The output will be:

```plaintext
Caught: Rejected value
Caught: Error from catch
```


### 4. Chained Catch: then Returns a Rejection

### a) First then Returns an Error

```javascript
Promise.resolve("Initial data")
    .then((data) => {
        console.log("1st then:", data);
        throw new Error("Error from first catch");
    })
    .catch((err) => {
        console.log("1st catch:", err.message);
    });
```

**Explanation:** In this case, the first `then` block logs the data and then throws an error. Since the error is thrown within the `then` block, the subsequent `catch` block will handle it.

**Output:**
```
1st then: Initial data
1st catch: Error from first catch
```

### b) First then Returns Promise.reject

```javascript
Promise.resolve("Initial data")
    .then((data) => {
        console.log("1st then:", data);
        return Promise.reject("Rejected from first then");
    })
    .catch((err) => {
        console.log("1st catch:", err);
    });
```

**Explanation:** Here, the first `then` block returns a promise that is immediately rejected. As a result, the subsequent `catch` block will handle the rejection.

**Output:**
```plaintext
1st then: Initial data
1st catch: Rejected from first then
```

### c) First then Returns a Promise that Could Be Rejected


```javascript
Promise.resolve("Initial data")
    .catch((err) => {
        console.log("1st catch:", err);
    })
    .then((data) => {
        console.log("2nd then:", data);
        return fs.promises.readFile("file.txt", "utf8");
    })
    .catch((err) => {
        console.log("2nd catch:", err.message);
    });
```

**Explanation:** In this example, the first `catch` block is not executed since there is no rejection before the first `then` block. The second `then` block reads a file asynchronously. If the file read encounters an error, it will be caught by the subsequent `catch` block.

**Output:**
```plaintext
2nd then: Initial data
2nd catch: ENOENT: no such file or directory, open 'file.txt'
```

### 5. Chained Catch: catch Returns a Rejection

Exactly like 4th case.


## Rules of finally in Promises

The `finally` block is a powerful tool in promise chains that allows you to execute code regardless of whether the promise is resolved or rejected. However, there are some important rules to keep in mind when working with the `finally` block.

### 1) finally Does Not Take Any Parameter

In the `finally` block, you don't receive any parameter related to the outcome of the parent promise. The block is executed irrespective of whether the parent promise is resolved or rejected.

```javascript
Promise.resolve("Initial data")
    .finally(() => {
        console.log("Finally block executed");
    })
    .then((data) => {
        console.log("Resolved:", data);
    });
```

### 2) If finally Returns a Rejection/Error/Promise

If the `finally` block returns a rejected promise, throws an error, or returns a promise that will be rejected, the error from the `finally` block will be the outcome of the chain.

```javascript
Promise.reject("Initial rejection")
    .finally((data) => {
        console.log(data); // undefined
        throw new Error("I am an error");
        // return Promise.reject("An error from finally");
    })
    .catch((err) => {
        console.log("Caught:", err.message);
    });
```

### 3) If finally Returns a Value/Promise

When the `finally` block returns a value or a promise that will not be rejected, it doesn't influence the outcome of the parent promise. The resolved value of the parent promise will continue down the chain.

```javascript
Promise.resolve("Initial data")
    .finally((data) => {
        console.log(data); // 1 undefined
        return "Finally resolved";
    })
    .then((data) => {
        console.log("Resolved:", data); // Resolved: Initial data
    });
```

### 4) finally Receives a Rejection/Error

If the `finally` block encounters a rejection or throws an error, the `finally` block executes itself, and if it doesn't return anything (i.e., a value or a promise), the error will be propagated down the chain.

```javascript
Promise.resolve("Initial data")
    .finally((data) => {
        console.log(data); // 4 undefined
        return "Something";
        // throw new Error("An error in finally");
    })
    .catch((err) => {
        console.log("Caught:", err.message); // Caught: Something
    });
```


## Promise.all Definition

The `Promise.all` method is used to wait for all promises in an iterable to be resolved, or for any to be rejected. It returns a single promise that is resolved with an array of resolved values or rejected with the reason of the first promise that was rejected.

### 1) When All Promises Are Resolved

```javascript
const p0 = Promise.resolve(3);
const p1 = 42;
const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('foo');
    }, 100);
});

Promise.all([p0, p1, p2])
    .then((res) => {
        console.log("Resolved:", res);
    });
```

**Output:**
```plaintext
Resolved: [3, 42, "foo"]
```

In this case, all promises are resolved, and the `Promise.all` returns an array containing the resolved values of `p0`, `p1`, and `p2`.

### 2) When Either of the Promises Gets Rejected

Now, let's see what happens when one of the promises gets rejected:

```javascript
const p0 = Promise.resolve(30);
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        reject('An error occurred!');
    }, 200);
});
const p2 = 42;

Promise.all([p0, p1, p2])
    .then((res) => {
        console.log("Resolved:", res);
    })
    .catch((err) => {
        console.log("Error:", err);
    });
```

**Output:**
```plaintext
Error: An error occurred!
```


## Prototype in Promises: Custom myPromiseALL Method

In JavaScript, you can extend built-in objects using prototypes to add custom functionality. Here, we'll explore how to create a custom method `myPromiseALL` on the `Promise` prototype to mimic the behavior of `Promise.all`.

### Example

```javascript
Promise.prototype.myPromiseALL = function (arrayOfPromises) {
    return new Promise(function (resolve, reject) {
        const unresolved = arrayOfPromises.length;
        const resolvedPromiseArr = [];
        
        if (unresolved === 0) {
            resolve([]);
            return;
        }

        arrayOfPromises.forEach(async (promise) => {
            try {
                const value = await promise;
                resolvedPromiseArr.push(value);
                unresolved -- ;

                if (unresolved === 0) {
                    resolve(resolvedPromiseArr);
                }
            } catch (err) {
                reject(err);
            }
        });
    });
};

const p0 = Promise.resolve(30);
const p1 = 42;
const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('foo');
    }, 100);
});

Promise.myPromiseALL([p0, p1, p2])
    .then((res) => {
        console.log("Resolved:", res);
    })
    .catch((err) => {
        console.log("Error:", err);
    });
```
### Explanation

In this example, we've added a custom method `myPromiseALL` to the `Promise` prototype. This method mimics the behavior of `Promise.all`. It takes an array of promises as input and returns a new promise. Inside the custom method, we iterate through each promise in the array using `forEach`.

For each promise, we use `await` to resolve the promise's value. If the promise resolves successfully, we push the value into the `resolvedPromiseArr` array and decrement the `unresolved` count. Once all promises are resolved (i.e., `unresolved` becomes 0), we call the `resolve` function with the array of resolved values.

**Output:**
```plaintext
Resolved: [30, 42, "foo"]
```




## Definition of Promise.race

`Promise.race` is a method in JavaScript that takes an array of promises as input and returns a new promise. This new promise resolves or rejects as soon as any of the input promises settles, whether it's resolved or rejected. It's useful when you want to know which promise among several is the first to complete.

## Example:

Let's explore an example that illustrates the usage of `Promise.race` to determine the first settled promise among a set of promises.

```javascript
const firstPromise = new Promise((res, rej) => {
    setTimeout(() => res('one'), 2000);
});

const secondPromise = new Promise((res, rej) => {
    setTimeout(() => res('two'), 1500);
});

Promise.race([firstPromise, secondPromise])
    .then((result) => {
        console.log("First settled promise:", result);
    });
```

## Explanation:

- Two promises, `firstPromise` and `secondPromise`, are created using the `Promise` constructor and `setTimeout`.
- `firstPromise` resolves after a timeout of 2000 milliseconds with the value `'one'`.
- `secondPromise` resolves after a timeout of 1500 milliseconds with the value `'two'`.
- `Promise.race` is used to wait for the first promise to settle (resolve or reject).
- The `.then` callback logs the result of the first settled promise.

## Output:

Since `secondPromise` settles (resolves) before `firstPromise`, the output will be `First settled promise: two`.
## Create Promise API

Let's walk through the process of creating a custom promise API step by step. We'll start from basic promise usage and gradually build up to a custom promise implementation.

### 1) Basic Promise Usage

```javascript
function promSetTimeout(delay) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve("Hey then");
        }, delay);
    });
}

promSetTimeout(1000)
    .then((data) => {
        console.log(data);
    });
```

**Explanation:** We begin by creating a basic promise function `promSetTimeout` that uses the built-in `Promise` constructor. It wraps `setTimeout` and resolves the promise after the specified delay. When the promise is resolved, the attached `then` block logs the resolved data.

**Output:** After approximately 1 second: `Hey then`

### 2) Creating a Basic Custom Promise

```javascript
const executorFn = (resolve, reject) => {
    setTimeout(() => {
        const value = "Hey then";
        resolve(value);
    }, 1000);
};

const myPromise = new CustomPromise(executorFn);

myPromise.then((data) => {
    console.log(data);
});
myPromise.catch((err) => {
    console.log(err);
});
```

**Explanation:** We move on to creating a basic custom promise named `myPromise` using an executor function. We have placeholders for the `resolve` and `reject` functions inside the executor. We attach `then` and `catch` blocks to our custom promise to handle resolved and rejected states.

**Output:** After approximately 1 second: `Hey then`

### 3) Custom Promise Implementation
Now we're going to create a basic custom promise named `CustomPromise`. This custom promise implementation will mimic some aspects of the native `Promise` API, allowing us to better understand how promises work under the hood.

#### Defining Constants

```javascript
const PENDING = "pending";
const RESOLVED = "resolved";
const REJECTED = "rejected";
```

We start by defining three constants: `PENDING`, `RESOLVED`, and `REJECTED`. These constants represent the possible states of a promise.

#### Custom Promise Constructor

```javascript
function CustomPromise(executorFn) {
    let state = PENDING;
    let value = undefined;
    let scbArr = [];
    let fcbArr = [];

}
```

The `CustomPromise` constructor function takes an `executorFn` as its parameter. Inside the constructor, we declare some variables:
- `state`: Tracks the current state of the promise (initially set to `PENDING`).
- `value`: Holds the resolved or rejected value.
- `scbArr`: An array to store success (`then`) callbacks.
- `fcbArr`: An array to store failure (`catch`) callbacks.

#### Adding then and catch Methods

```javascript
this.then = (cb) => {
    if (state === RESOLVED) {
        cb(value);
    } else {
        scbArr.push(cb);
    }
};

this.catch = (cb) => {
    if (state === REJECTED) {
        cb(value);
    } else {
        fcbArr.push(cb);
    }
};
```

We define the `then` and `catch` methods within the prototype of the `CustomPromise` constructor. The `then` method adds the provided success callback (`cb`) to the `scbArr` if the promise is pending. If the promise is already resolved (`state === RESOLVED`), the success callback is immediately invoked with the resolved value.

The `catch` method behaves similarly, adding the provided error callback (`cb`) to the `fcbArr` if the promise is pending. If the promise is already rejected (`state === REJECTED`), the error callback is immediately invoked with the rejected value.

#### Resolving and Rejecting

```javascript
const resolve = (val) => {
    state = RESOLVED;
    value = val;
    scbArr.forEach(cb => cb(value));
};

const reject = (err) => {
    state = REJECTED;
    value = err;
    fcbArr.forEach(cb => cb(value));
};

executorFn(resolve, reject);
```

We define `resolve` and `reject` functions that change the state of the promise and invoke the appropriate callbacks. When `resolve` is called, the promise state is updated to `RESOLVED`, the resolved value is stored, and all success callbacks are executed. Similarly, when `reject` is called, the promise state is updated to `REJECTED`, the rejected value is stored, and all error callbacks are executed.

Finally, we call the `executorFn` (the function passed to the constructor) with the `resolve` and `reject` functions as arguments. This simulates the actual execution of the asynchronous operation that the promise represents.

#### Using the Custom Promise

```javascript
const myPromise = new CustomPromise(executorFn);

const cb = (data) => {
    console.log(data);
};

myPromise.then(cb);
myPromise.then((data) => {
    console.log("I am the second then");
});
myPromise.catch((err) => {
    console.log(err);
});
```

We create an instance of `CustomPromise` named `myPromise` by calling the constructor and passing in the `executorFn`. We define a success callback `cb` that logs the resolved data.

We demonstrate how to use the custom promise by attaching two `then` blocks and a `catch` block to the `myPromise` instance. The first `then` block (`myPromise.then(cb)`) logs the resolved data. The second `then` block logs `"I am the second then"`. The `catch` block logs any rejected value.

#### Output

After approximately 1 second,

```plaintext
Hey then
I am the second then
```
