

## Agenda
**Topics to cover in Javascript:**

1. **Promises as a Concept:** Promises represent the outcome of asynchronous operations in JavaScript.
2. **Consuming Promises:** Use `.then()` to handle resolved Promises and `.catch()` for errors.
3. **Chaining Promises:** Chain `.then()` to sequence async tasks more cleanly.
4. Convert cb based fucntion to promise based
5. **Promises vs. Callbacks:** Promises offer structured async handling and are part of the MicroTask Queue for prioritized execution.

### Callback hell
Callback hell, also known as "Pyramid of Doom," is a term used in programming to describe a situation where you have multiple nested callbacks within asynchronous functions. It occurs when you have a series of asynchronous operations that depend on the results of previous operations, resulting in deeply nested callback functions. Callback hell can make your code difficult to read, understand, and maintain. It often arises in scenarios where you're working with asynchronous I/O operations, such as reading files, making API requests, or handling database queries, and you need to ensure that these operations happen in a specific sequence.


Let's consider a scenario where you want to read the contents of three files (`f1.txt`, `f2.txt`, and `f3.txt`) using the Node.js `fs` module. In this example, we'll illustrate how callback hell can occur when you're dealing with nested callbacks for reading these files sequentially. Here's how callback hell might look in this context:

```javascript
const fs = require('fs');

fs.readFile('f1.txt', 'utf8', (error1, data1) => {
  if (error1) {
    console.error(error1);
  } else {
    // Successfully read f1.txt, now read f2.txt
    fs.readFile('f2.txt', 'utf8', (error2, data2) => {
      if (error2) {
        console.error(error2);
      } else {
        // Successfully read f2.txt, now read f3.txt
        fs.readFile('f3.txt', 'utf8', (error3, data3) => {
          if (error3) {
            console.error(error3);
          } else {
            // Successfully read f3.txt, now you can use data1, data2, and data3
            console.log('Contents of f1.txt:', data1);
            console.log('Contents of f2.txt:', data2);
            console.log('Contents of f3.txt:', data3);
          }
        });
      }
    });
  }
});
```

In this code:

1. You start by reading `f1.txt`. Inside the callback for `f1.txt`, you read `f2.txt`, and then within the callback for `f2.txt`, you read `f3.txt`.

2. Each step introduces a new level of indentation and callback, making the code difficult to read and maintain, especially when error handling and additional processing are involved.

3. If an error occurs at any step, you need to handle it within that specific callback, leading to error handling code spread throughout the codebase.

Callback hell can make your code less readable, harder to debug, and prone to errors. This is why modern JavaScript development encourages the use of Promises or async/await to handle asynchronous operations more cleanly and avoid the nesting problem seen in callback hell.


Creating separate functions (`f1cb`, `f2cb`, `f3cb`) to encapsulate the file reading logic is a step in the right direction to make the code more organized and less nested. This approach can help mitigate the callback hell problem to some extent. Here's how it might look:

```javascript
const fs = require('fs');

function f1cb(error, data) {
  if (error) {
    console.error(error);
  } else {
    console.log('Contents of f1.txt:', data);
    readFile2();
  }
}

function f2cb(error, data) {
  if (error) {
    console.error(error);
  } else {
    console.log('Contents of f2.txt:', data);
    readFile3();
  }
}

function f3cb(error, data) {
  if (error) {
    console.error(error);
  } else {
    console.log('Contents of f3.txt:', data);
    // You can perform further processing here
  }
}

function readFile1() {
  fs.readFile('f1.txt', 'utf8', f1cb);
}

function readFile2() {
  fs.readFile('f2.txt', 'utf8', f2cb);
}

function readFile3() {
  fs.readFile('f3.txt', 'utf8', f3cb);
}

// Start the process by reading f1.txt
readFile1();
```

In this code:

1. Each `readFileX` function is responsible for reading one of the files and passing the result to its respective callback (`f1cb`, `f2cb`, `f3cb`).

2. By separating the file reading logic into individual functions, you reduce nesting and make the code more modular.

3. You still need to handle errors in each callback, but it's less nested and easier to manage.

While this approach improves code organization and readability, it doesn't completely eliminate the callback hell problem, especially when dealing with a more complex sequence of asynchronous operations. For more complex scenarios, using Promises or async/await can provide even cleaner and more maintainable code.

## Intuition of Promise
A Promise is a fundamental concept in JavaScript that represents the eventual completion or failure of an asynchronous operation. It provides a more structured and manageable way to work with asynchronous code compared to traditional callbacks. Promises are used to handle tasks such as making HTTP requests, reading files, or any other operation that doesn't immediately return a result.

Certainly, let's explain Promises using the restaurant analogy:

Imagine you're a customer at a restaurant:

1. **Traditional Ordering (Callback Approach):**
   - You tell the waiter what you want to order, and they give you a number and say, "We'll call you when your meal is ready."
   - You have to wait around, frequently checking with the waiter, or even standing at the counter to know when your meal is prepared.
   - This is similar to how traditional callback-based asynchronous code works. You initiate an operation, and a callback function is used to notify you when it's done. You need to actively wait and check for completion.

2. **Modern Ordering (Promise Approach):**
   - You place your order with the waiter, and they give you a numbered buzzer.
   - They say, "When your meal is ready, the buzzer will vibrate, and you can collect your meal from the counter."
   - With this system, you can sit at your table, engage in other activities, and only return to the counter when your buzzer notifies you.
   - This is similar to how Promises work in JavaScript. When you initiate an asynchronous operation, you receive a Promise that represents the eventual completion of that operation. You can continue with other tasks and respond to the completion notification when it occurs.

In both cases, you get your meal, but the Promise approach is more structured, allowing you to be more efficient with your time while waiting for the operation to complete. Promises make asynchronous code more organized, readable, and easier to manage compared to the callback-based approach.



In JavaScript, Promises come in three main states: **Pending**, **Fulfilled** (also known as **Resolved**), and **Rejected**. You can think of these states and their analogies in terms of tokens in various real-world scenarios:

1. **Pending Promise (Token at a Waiting Area):**
   - A Pending Promise is like a token you receive when you're in a waiting area, such as a queue at a restaurant or a ticket at a service center.
   - You hold this token, knowing that something is expected to happen, but you're not sure when.
   - You have the potential to get your request or service fulfilled, but it hasn't happened yet.

2. **Fulfilled (Resolved) Promise (Token Vibrates or Lights Up):**
   - A Fulfilled (Resolved) Promise is like a token that suddenly vibrates or lights up, signaling that your request or service is now ready.
   - It's similar to when your restaurant buzzer starts vibrating or a display at the service center shows your number as ready.
   - At this point, the Promise has successfully completed, and you can proceed with the result.

3. **Rejected Promise (Token with an Error Message):**
   - A Rejected Promise is like a token that comes back with an error message, indicating that your request or service could not be fulfilled as expected.
   - It's similar to receiving a notification that your request at the restaurant couldn't be fulfilled due to an issue with the kitchen or a service center telling you they couldn't process your request.
   - The Rejected Promise provides information about why the operation failed.

In all these scenarios, the key concept is that the token (or Promise) represents the status of your request or operation. It starts as pending, and depending on the outcome, it either becomes fulfilled (resolved) or rejected. This makes it easier to handle asynchronous operations and gracefully respond to success or failure, similar to how tokens help manage expectations and communication in real-world situations.




```javascript
const fs = require("fs");
console.log("before");
const promise = fs.promises.readFile("./f1.txt");
console.log(promise);
console.log("After");
```

This code uses Node.js's `fs.promises.readFile()` method to read the contents of a file (`f1.txt`) asynchronously using Promises.

When you run this code, it performs the following steps:

1. It imports the Node.js `fs` module, which provides file system-related functionality.

2. It logs "before" to the console. This is a synchronous operation, so it executes immediately.

3. It initiates the asynchronous file reading operation by calling `fs.promises.readFile("./f1.txt")`. This method returns a Promise that represents the eventual completion or failure of the file read operation. However, it does not block the execution of the subsequent code.

4. It logs the `promise` variable to the console. At this point, the `promise` variable holds the Promise object, but the file reading operation has not yet completed. The output will show the Promise object's details, including its state (which will likely be "pending" at this stage).

5. It logs "After" to the console. Like the first `console.log`, this is also a synchronous operation, so it executes immediately after the previous line.

The output might look something like this (order can vary depending on the execution speed of the file read operation):

```
before
After
Promise { <pending> }
```

In this example, "before" and "After" are logged immediately, while the Promise is still pending. The state of the Promise is initially "pending" until the file read operation is finished.

Certainly, here's the code you provided along with comments explaining its behavior:

```javascript
const fs = require("fs");
console.log("before");

// Initiating an asynchronous file reading operation
const promise = fs.promises.readFile("./f1.txt");

console.log(promise);
console.log("After");

// Scheduling a callback to execute after a 2-second delay
setTimeout(() => {
    console.log("i am file read");
    console.log(promise);
}, 2000);
```

Here's the expected sequence of execution and output:

1. `console.log("before");` is a synchronous operation, so it logs "before" immediately.
2. `const promise = fs.promises.readFile("./f1.txt");` initiates an asynchronous file reading operation. This operation starts but doesn't block further execution.
3. `console.log(promise);` logs the `promise` variable immediately after the file reading operation is initiated. At this point, the Promise is likely in the "pending" state.
4. `console.log("After");` is another synchronous operation, so it logs "After" immediately after the previous `console.log`.
5. `setTimeout(() => { ... }, 2000);` schedules a callback function to execute after a 2-second delay.

Now, the expected output:

- "before" is logged first.
- The `promise` variable is logged, showing its initial state as "pending."
- "After" is logged immediately after the previous line.
- After a 2-second delay, the `setTimeout` callback is executed, logging "i am file read" and the `promise` variable again. The state of the `promise` may have changed depending on whether the file reading operation has completed within that time.

The final output will depend on the timing of the file reading operation. If the file reading operation takes longer than 2 seconds, the `setTimeout` callback will still execute, and the `promise` variable will show its state at that moment.


You can use the `.then()` method to specify what should happen when the Promise is resolved (when the file reading operation is successful). Here's the updated code with the `.then()` method and the specified behavior:

```javascript
const fs = require("fs");
console.log("before");

// Initiating an asynchronous file reading operation
const promise = fs.promises.readFile("./f1.txt");

console.log(promise);
console.log("After");

// Using .then() to handle the resolved Promise
promise.then(futurevalue => {
    console.log("data inside the file is:", futurevalue);
});

// Scheduling a callback to execute after a 2-second delay
setTimeout(() => {
    console.log("i am file read");
    console.log(promise);
}, 2000);
```

With this modification, the code will log "data inside the file is:" followed by the content of the file once the file reading operation is successfully completed. The rest of the code remains the same, including the `setTimeout` callback to log "i am file read" and the `promise` variable after a 2-second delay.

Here's the expected sequence of output:

- "before" is logged first.
- The `promise` variable is logged, showing its initial state as "pending."
- "After" is logged immediately after the previous line.
- Once the file reading operation is complete (which may take some time), the `.then()` callback is executed, logging "data inside the file is:" followed by the content of the file.
- After a 2-second delay, the `setTimeout` callback is executed, logging "i am file read" and the `promise` variable again. The state of the `promise` may have changed by this point.

You can add a `.catch()` block to handle errors that might occur during the file reading operation. Here's the updated code with both `.then()` and `.catch()`:

```javascript
const fs = require("fs");
console.log("before");

// Initiating an asynchronous file reading operation
const promise = fs.promises.readFile("./f1.txt");

console.log(promise);
console.log("After");

// Using .then() to handle the resolved Promise
promise
  .then(futurevalue => {
    console.log("data inside the file is:", futurevalue);
  })
  .catch(error => {
    console.error("An error occurred:", error);
  });

// Scheduling a callback to execute after a 2-second delay
setTimeout(() => {
  console.log("i am file read");
  console.log(promise);
}, 2000);
```

With this addition, the code will now handle errors that may occur during the file reading operation. If the file cannot be read, the `.catch()` block will be executed, logging an error message.

Here's the expected sequence of output:

- "before" is logged first.
- The `promise` variable is logged, showing its initial state as "pending."
- "After" is logged immediately after the previous line.
- Once the file reading operation is complete (which may take some time), the `.then()` callback is executed, logging "data inside the file is:" followed by the content of the file.
- If an error occurs during the file reading operation, the `.catch()` block is executed, logging an error message.
- After a 2-second delay, the `setTimeout` callback is executed, logging "i am file read" and the `promise` variable again. The state of the `promise` may have changed by this point.

This updated code handles both successful and error scenarios when reading the file.

---

##  Chaining  Promises

In the code you are reading the contents of three files (`f1.txt`, `f2.txt`, and `f3.txt`) using nested callbacks, which can result in callback hell or deeply nested code. To improve code readability and maintainability, you can introduce Promises and chaining. Here's the same code refactored using Promises and chaining:

```javascript
const fs = require('fs').promises;

function readFile(filePath) {
  return fs.readFile(filePath, 'utf8');
}

function handleError(error) {
  console.error(error);
}

readFile('f1.txt')
  .then(data1 => {
    console.log('Contents of f1.txt:', data1);
    return readFile('f2.txt');
  })
  .then(data2 => {
    console.log('Contents of f2.txt:', data2);
    return readFile('f3.txt');
  })
  .then(data3 => {
    console.log('Contents of f3.txt:', data3);
  })
  .catch(handleError);
```

Here's how the code works with Promises and chaining:

1. We create a `readFile` function that returns a Promise to read a file using `fs.promises.readFile`.

2. We define a `handleError` function to handle errors consistently.

3. We start by reading `f1.txt` using `readFile('f1.txt')`. When that Promise resolves, we log the contents of `f1.txt` and return another Promise to read `f2.txt`.

4. Similarly, we read `f2.txt` and log its contents when the Promise resolves. We then return a Promise to read `f3.txt`.

5. Finally, we read `f3.txt`, log its contents when the Promise resolves, and complete the chain of operations.

6. If any error occurs at any point in the chain, it will be caught by the `.catch(handleError)` block at the end.

By using Promises and chaining, you can avoid callback hell and create more structured, readable, and maintainable code. Each step in the chain represents a sequential operation, making the code flow more naturally.

---
## Converting a callback function to promise based

Here is an example of how to modify the `readFileAsync` function to explicitly use `resolve` and `reject`:

```javascript
const fs = require('fs');

// Function to promisify fs.readFile
function readFileAsync(filePath, encoding) {
  return new Promise((resolve, reject) => {
    fs.readFile(filePath, encoding, (err, data) => {
      if (err) {
        reject(err); // Reject the Promise if an error occurs
      } else {
        resolve(data); // Resolve the Promise with the data
      }
    });
  });
}

console.log("before");

// Use the promisified function to read a file
const promise = readFileAsync('./f1.txt', 'utf8');

console.log(promise);
console.log("After");

// Using .then() to handle the resolved Promise
promise
  .then(data => {
    console.log("data inside the file is:", data);
  })
  .catch(error => {
    console.error("An error occurred:", error);
  })

// Scheduling a callback to execute after a 2-second delay
setTimeout(() => {
  console.log("i am file read");
  console.log(promise);
}, 2000);
```

In this code:

- I've added a `.finally()` block at the end of the Promise chain. This block will execute regardless of whether the Promise is resolved or rejected. It's used here to log a message indicating that the Promise has been resolved or rejected.

With this modification, the `readFileAsync` function resolves the Promise with the file data when the file is successfully read and rejects the Promise with an error if there is any issue. The `.then()` block handles the resolved Promise, the `.catch()` block handles the rejected Promise, and the `.finally()` block always executes.

The output will be similar to the previous version of the code, with the addition of the "Promise has been resolved or rejected" message in the `.finally()` block.




1. **Inversion of Control (IoC):**

   - **Issue:** In traditional callback-based code, the control flow is often inverted, which means that you don't have control over when a callback function will be executed. T

   - **Solution with Promises:** Promises provide a structured way to handle asynchronous operations, eliminating callback hell. In the code provided, Promises allow you to write asynchronous code in a more linear and readable fashion. Each `.then()` block represents a step in the process, and you can chain them together to create a sequence of asynchronous operations.

   - **Example:** In your code, you read three files sequentially, and the control flow is clear. You start with `readFile('f1.txt')`, then continue with `readFile('f2.txt')`, and finally `readFile('f3.txt')`. This sequence of steps is much easier to follow than nested callbacks.

2. **Trust and Error Handling:**

   - **Issue:** Callback-based code often has trust and error-handling issues. It's challenging to ensure that all errors are properly handled, and you may have inconsistent error-handling practices.

   - **Solution with Promises:** Promises have built-in error-handling mechanisms. You can use `.catch()` to handle errors that occur at any point in the Promise chain. This allows you to centralize error handling and ensure that all errors are consistently addressed. Additionally, Promises make it explicit when an operation has succeeded (resolved) or failed (rejected).

   - **Example:** In your code, you handle errors with the `.catch()` block. If any of the file reading operations encounters an error, it will be caught and logged with the "An error occurred" message. This ensures that errors are properly handled, and you have control over how to respond to them.

By using Promises, you gain better control over the flow of asynchronous operations, make your code more readable, and ensure consistent error handling. These improvements enhance code maintainability and reliability, making it easier to reason about and debug asynchronous code.

---
## Problems with callbacks



Here's the function you provided:

```javascript
function runMlalgo(amount, cb) {
  console.log("running ml algo");
  console.log("processing payment");
  setTimeout(function () {
    console.log("payment done");
    cb();
    cb();
    cb();
    cb();
    cb();
  }, 1000);
}

module.exports = {
  runMlalgo,
}
```

In this function, `runMlalgo`, you have two main tasks: running a machine learning algorithm and processing a payment. However, the function itself doesn't control when or how these tasks are executed; it delegates the responsibility to the caller.

Here's how this function can be related to IoC:

1. **Callback Function (cb)**: The function accepts a callback function (`cb`) as an argument. This is a form of IoC because it allows the caller to specify what action should be taken once the payment processing is completed. The caller can pass any function they want as `cb`, effectively injecting behavior into the `runMlalgo` function.

2. **Asynchronous Operation (setTimeout)**: The `setTimeout` function is used to introduce asynchrony into the code. This means that `runMlalgo` doesn't block the program's execution while waiting for the payment processing to complete. Instead, it delegates the control flow to the JavaScript event loop. When the timeout expires, the provided callback function (`cb`) is executed multiple times. This is another form of IoC because the timing and execution of the callback are controlled by the event loop, not `runMlalgo`.

In summary, while the `runMlalgo` function itself doesn't represent a classic example of IoC, it demonstrates some IoC principles:

1. It allows the caller to specify behavior (the callback) that will be executed later.
2. It relies on the event loop to determine when to execute the callback, relinquishing control over the execution flow.


## Promises vs Callbacks

Here's the comparison of Promises over Callbacks:

| Aspect                         | Promises                                 | Callbacks                               |
|--------------------------------|------------------------------------------|----------------------------------------|
| Resolution Handling             | Promises can be resolved or rejected once in their lifetime, providing clear success or failure handling. | Callbacks are called multiple times and may not provide a clear distinction between success and failure. |
| Error Handling                  | Promises have built-in error-handling mechanisms, allowing centralized error handling with `.catch()`. | Error handling in callbacks can be scattered and inconsistent. |
| Readability and Maintainability | Promises offer a more structured and readable way to handle asynchronous code with `.then()`. | Callbacks can lead to callback hell, making code harder to read and maintain. |
| Microtask Queue                 | Callbacks of Promises go to a separate queue known as the microtask queue, which has higher priority than the regular callback queue. | Callbacks are executed in the regular callback queue, which may have lower priority and can lead to blocking behavior. |

Promises provide a more robust and organized approach to handling asynchronous operations compared to traditional callbacks. They offer clear success and error handling, enhance code readability, and ensure smoother event-loop execution with the microtask queue.

* Promises and MicroTask Queue
* promise as an object 
