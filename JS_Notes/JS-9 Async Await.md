
## Agenda
**Topics to cover in Javascript:**

- **Queues in Browser** :Queues refer to various queues  that browsers use to manage and execute async tasks efficiently.
- **Async/Await**: 
- **Different Types of Errors**: 

We will try to cover most of these topics in today's sessions and the remaining in the next.

It is going to be a bit challenging, advanced, but very interesting session covering topics that are asked very frequently in interviews.

So let's start.

---
title: Queues in Browser
description: Discussion about the task queue, microtasks queue, call stack, ui rendering etc in detail. 
duration: 2100
card_type: cue_card
---
## Queues in Browser

Some key concepts related to JavaScript execution and rendering in the browser. Let me explain each of them in more detail:

1. **Tasks Queue:** The tasks queue, often referred to as the "task queue" or "callback queue," is a part of the event loop in JavaScript. It's responsible for managing asynchronous code execution. When asynchronous events like I/O operations, timers (e.g., `setTimeout`), or event listeners (e.g., click events) have their callback functions ready to run, those callbacks are placed in the tasks queue.

   The event loop continuously checks if the call stack is empty. If it is, it takes the next task from the tasks queue and pushes it onto the call stack for execution.

2. **Microtasks Queue:** The microtasks queue is a separate queue that handles microtasks, which have a higher priority than tasks. Promises and certain other asynchronous operations, like `process.nextTick` in Node.js or `MutationObserver` in the browser, schedule callbacks to run as microtasks. Microtasks are executed before the next rendering, and they are processed entirely before tasks.

   This means that if there are microtasks in the microtasks queue, they will be executed before any tasks in the tasks queue.

3. **Call Stack:** The call stack is a data structure that keeps track of function calls in a JavaScript program. It follows the Last-In-First-Out (LIFO) principle. When a function is called, it's pushed onto the call stack, and when it returns or completes, it's popped off the stack. This stack determines the order in which synchronous code is executed.

   For asynchronous code, when the call stack is empty, the event loop checks the tasks queue for pending tasks and executes them.

4. **UI Rendering:** The browser's rendering engine is responsible for rendering web pages. The rendering process involves various steps, including layout, paint, and composite. The browser tries to render at a consistent frame rate, typically 60 frames per second (FPS) for smooth animations.

   The `requestAnimationFrame` API is a tool that helps synchronize JavaScript animations with the browser's rendering cycle. When you use `requestAnimationFrame`, your animation code is executed just before the next repaint of the browser's content, ensuring smooth and efficient animations without causing layout thrashing.

![](https://hackmd.io/_uploads/HJ4FUWzJa.png)

### Microtask Queue

The microtask queue is a queue in the JavaScript event loop where microtasks are stored and executed. Microtasks are a category of asynchronous tasks that have higher priority than regular tasks (or macrotasks). They are designed to be executed immediately after the current task completes, but before rendering or other tasks from the regular task queue (also known as the "callback queue").

Some common sources of microtasks include:

1. **Promise Resolutions:** When a Promise is resolved or rejected, the associated callbacks (`.then()` and `.catch()`) are queued as microtasks.

2. **`queueMicrotask()` Function:** You can use the `queueMicrotask()` function to queue a custom function as a microtask. This is often used for tasks that should have high priority but aren't directly related to Promises.

Microtasks are used to ensure that certain asynchronous operations, particularly those related to promises, are executed as soon as possible, making them useful for maintaining precise control flow and reducing latency in asynchronous code.

Here's an example:

```javascript
console.log('Start');

// Microtask created using a Promise
Promise.resolve().then(() => {
  console.log('Microtask 1');
});

// Microtask created using queueMicrotask
queueMicrotask(() => {
  console.log('Microtask 2');
});

console.log('End');
```

The output of this code will be:

```
Start
End
Microtask 1
Microtask 2
```

As you can see, the microtasks (`Microtask 1` and `Microtask 2`) are executed immediately after the main task (`Start` and `End`) but before any other asynchronous tasks or rendering. This behavior allows you to maintain precise control over the order of execution in your asynchronous code.



Let's break down the execution of the code step by step:

```javascript
console.log('A syn stack'); // Executed immediately, synchronous
requestAnimationFrame(function () {
  console.log('B requestAnimationFrame');
});

setTimeout(function () {
  console.log('C task queue');
}, 0);

queueMicrotask(function () {
  console.log('D microtask');
});

console.log('E syn stack');
```

1. `console.log('A syn stack');`: This line is executed immediately and logs `'A syn stack'` to the console. It's a synchronous operation, so it runs in the main stack.

2. `requestAnimationFrame(function () { console.log('B requestAnimationFrame'); });`: This registers a function to be executed before the next repaint (rendering frame). The function (`'B requestAnimationFrame'`) will be logged once the browser is ready to repaint. It is asynchronous and doesn't block the main stack.

3. `setTimeout(function () { console.log('C task queue'); }, 0);`: This sets up a timer with a delay of 0 milliseconds. However, it doesn't guarantee immediate execution. Instead, it schedules the function (`'C task queue'`) to be executed in the task queue after the current stack is clear.

4. `queueMicrotask(function () { console.log('D microtask'); });`: This schedules a microtask with the function (`'D microtask'`) to be executed. Microtasks have a higher priority than tasks. It will be executed before the next frame but after the current stack is cleared.

5. `console.log('E syn stack');`: This line is executed immediately and logs `'E syn stack'` to the console. It's synchronous and runs in the main stack.

Now, let's consider the order of the actual output:

```
A syn stack
E syn stack
D microtask
B requestAnimationFrame
C task queue
```

- 'A syn stack' and 'E syn stack' are logged first because they are synchronous and executed immediately.
- 'D microtask' is logged next because microtasks have higher priority than tasks, and it's scheduled to run after the current stack is cleared.
- 'B requestAnimationFrame' is logged when the browser is ready to repaint (typically in the next frame).
- 'C task queue' is logged last because it's scheduled in the task queue and will run after the microtask ('D microtask').


### Promise microtasks


 let's include a `Promise.resolve().then()` block in your code and explain the updated execution:

```javascript
console.log('A syn stack'); // Executed immediately, synchronous
requestAnimationFrame(function () {
  console.log('B requestAnimationFrame');
});

setTimeout(function () {
  console.log('C task queue');
}, 0);

queueMicrotask(function () {
  console.log('D microtask');
});

console.log('E syn stack');

Promise.resolve().then(function () {
  console.log('F Promise Microtask');
});
```

Now, let's break down the execution with the updated code:

1. `console.log('A syn stack');`: This line is executed immediately and logs `'A syn stack'` to the console. It's a synchronous operation and runs in the main stack.

2. `requestAnimationFrame(function () { console.log('B requestAnimationFrame'); });`: This registers a function to be executed before the next repaint (rendering frame). The function (`'B requestAnimationFrame'`) will be logged once the browser is ready to repaint. It is asynchronous and doesn't block the main stack.

3. `setTimeout(function () { console.log('C task queue'); }, 0);`: This sets up a timer with a delay of 0 milliseconds. However, it doesn't guarantee immediate execution. It schedules the function (`'C task queue'`) to be executed in the task queue after the current stack is clear.

4. `queueMicrotask(function () { console.log('D microtask'); });`: This schedules a microtask with the function (`'D microtask'`) to be executed. Microtasks have higher priority than tasks and run before the next frame but after the current stack is cleared.

5. `console.log('E syn stack');`: This line is executed immediately and logs `'E syn stack'` to the console. It's synchronous and runs in the main stack.

6. `Promise.resolve().then(function () { console.log('F Promise Microtask'); });`: This code creates a Promise and immediately resolves it. The `.then()` callback (`'F Promise Microtask'`) is scheduled as a microtask and will be executed after `'D microtask'`.

Now, let's consider the order of the actual output:

```
A syn stack
E syn stack
D microtask
F Promise Microtask
B requestAnimationFrame
C task queue
```

- 'A syn stack' and 'E syn stack' are logged first because they are synchronous and executed immediately.
- 'D microtask' and 'F Promise Microtask' are logged next because microtasks have higher priority than tasks and are executed before the next frame.
- 'B requestAnimationFrame' is logged when the browser is ready to repaint (typically in the next frame).
- 'C task queue' is logged last because it's scheduled in the task queue and runs after the microtasks are executed.

The key takeaway here is that microtasks, including Promise microtasks, are executed before regular tasks, ensuring precise control over asynchronous code execution.

In JavaScript's event loop, different types of tasks and callbacks have different priorities. Here's the general priority order from highest to lowest:

1. **Synchronous Code:**
   Synchronous code has the highest priority in the event loop. This includes code that runs as part of the main execution thread. Synchronous code runs to completion before any other tasks are executed. It's the core execution of your JavaScript program.

2. **Microtasks:**
   Microtasks have a higher priority than regular tasks (macrotasks). Microtasks include things like Promise resolution handlers (`.then()` and `.catch()` callbacks), `queueMicrotask()` callbacks, and other browser-specific microtask sources like `MutationObserver`. Microtasks are executed immediately after the current synchronous code completes but before the next frame or task from the task queue is processed. This makes them useful for tasks that need to be executed as soon as possible.

3. **Callback Queue (Task Queue):**
   The callback queue (sometimes called the task queue) holds tasks that are ready to be executed. These tasks are usually the result of asynchronous operations like timers (`setTimeout`, `setInterval`) and I/O operations (e.g., handling file I/O, network requests, or user interactions like click events). Tasks in the callback queue are executed in the order they were added, after all microtasks have been executed.

4. **`requestAnimationFrame`:**
   The `requestAnimationFrame` function is a special case. It is used for scheduling animations and other operations that require smooth rendering. Code scheduled with `requestAnimationFrame` runs just before the browser performs a repaint, typically at the next frame. While it's conceptually similar to a task, it's often given a slightly higher priority than regular tasks to ensure smooth animations.

To summarize, the priority order can be generalized as follows:

1. Synchronous Code
2. Microtasks
3. `requestAnimationFrame`
4. Callback Queue (Task Queue)

Keep in mind that the exact behavior and priorities can vary slightly depending on the JavaScript environment and browser implementation, but this general order helps you understand how different types of tasks are prioritized and executed in the event loop.

---

## Async/Await
Here's the code for reference:

```javascript
const fs = require("fs");
console.log("Before"); 
const promise = fs.promises.readFile("./f1.txt");
promise
  .then(function (futurevalue) {
    console.log("Data inside the file is " + futurevalue);
  })
  .catch(function (err) {
    console.log("Error:", err);
  });
console.log("After"); 
```

Here's how the code with promises without `async/await` differs from the code with `async/await`:

1. **Promise Chain**: In the original code, promises were used in a chain with `.then()` and `.catch()`. This means that the code for handling the result of the promise and handling errors were separate and chained together. In contrast, with `async/await`, the code looks more linear, as error handling is placed near the code that might throw an error.

2. **Synchronous Appearance**: The original code appears to be more asynchronous because the "After" message is logged before the promise has resolved. This is due to the nature of promises and the event loop. With `async/await`, you can ensure that the "After" message is logged after the promise has resolved by using `await` inside the `async` function.

3. **Explicit Promise Handling**: In the original code, the handling of promises using `.then()` and `.catch()` is more explicit, which can sometimes make the code harder to read and follow, especially when multiple promises are involved. `async/await` simplifies this by allowing you to write asynchronous code in a more synchronous style.

4. **Function Structure**: With `async/await`, the code is structured as a single `async` function, making it clear that all the operations are related to the same task. In the original code, promises and callbacks could be spread across different parts of your code, making it harder to understand the flow.



Using **`async/await`** to make it more synchronous and easier to read. Here's the code with **`async/await`** and an explanation of how it works:

```javascript
const fs = require("fs");

async function readFileAsync() {
  console.log("Before");

  try {
    const fileData = await fs.promises.readFile("./f1.txt", "utf-8");
    console.log("Data inside the file is " + fileData);
  } catch (err) {
    console.log("Error:", err);
  }

  console.log("After");
}

readFileAsync();
```

Explanation:

1. We define an `async` function named `readFileAsync`. The use of `async` in the function declaration allows us to use the `await` keyword inside the function to pause its execution until a promise is resolved.

2. We log "Before" to the console to indicate that the code is about to start.

3. Inside the `try` block, we use `await` with `fs.promises.readFile` to asynchronously read the file "./f1.txt" and store the result in the `fileData` variable. The `await` keyword ensures that the code execution will pause until the promise returned by `fs.promises.readFile` is resolved, making the code appear more synchronous.

4. We log the data from the file using `console.log`.

5. We use a `catch` block to handle any errors that may occur during the file reading operation. If an error occurs, it will be caught, and we log the error message.

6. Finally, we log "After" to indicate that the code execution has reached this point.

7. We call the `readFileAsync` function to start the asynchronous file reading operation.

By using `async/await`, we make the code look more linear and easier to follow, as if it were synchronous, while still preserving the asynchronous behavior of file reading. This approach simplifies error handling with the use of `try/catch`, making it clear where errors are expected to occur and how to handle them.


You can achieve the same sequential file reading using `async/await` syntax, which provides a more concise and synchronous-like code structure. Here's the same example with `async/await`:

```javascript
const fs = require('fs/promises'); // Using fs.promises for promise-based file operations

// Function to read a file and return a Promise
async function readFileAsync(filename) {
  try {
    const data = await fs.readFile(filename, 'utf8');
    return data;
  } catch (error) {
    throw error;
  }
}

// Function to read files sequentially
async function readFilesSequentially() {
  try {
    const data1 = await readFileAsync('f1.txt');
    console.log('File 1 Contents:', data1);

    const data2 = await readFileAsync('f2.txt');
    console.log('File 2 Contents:', data2);

    const data3 = await readFileAsync('f3.txt');
    console.log('File 3 Contents:', data3);
  } catch (error) {
    console.error('Error:', error);
  }
}

// Call the function to read files sequentially
readFilesSequentially();
```

In this updated code:

1. We define the `readFileAsync` function as an `async` function that reads a file and returns its content. We use `await` to make asynchronous file reading appear synchronous.

2. We create an `async` function named `readFilesSequentially` that reads the files one after the other using `await` for each file read operation.

3. Inside the `readFilesSequentially` function, we use `await` to read each file sequentially and then log its content.

4. Any errors that occur during file reading are caught using `try...catch` blocks, and we handle them in the `catch` block.

**Assuming the same content for the files as mentioned earlier,** the expected output will be:

```
File 1 Contents: Contents of f1.txt
File 2 Contents: Contents of f2.txt
File 3 Contents: Contents of f3.txt
```

Using `async/await`, the code is structured more linearly and resembles synchronous code, making it easier to read and understand while achieving the same sequential file reading behavior.


## What will be output of this code.
also provide an explanation of what the code is doing. :

```javascript
function resolveAfterNSeconds(n, x) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(x);
    }, n);
  });
}

(function () {
  let a = resolveAfterNSeconds(1000, 1);
  a.then(async function (x) {
    let y = await resolveAfterNSeconds(2000, 2);
    let z = await resolveAfterNSeconds(1080, 3);
    let p = resolveAfterNSeconds(2000, 4);
    let q = resolveAfterNSeconds(1000, 5);
    // Output, total time
    console.log(x + y + z + await p + await q);
  })();
})();
```

Now, let's break down what this code does:

Certainly! Let's break down the execution of the provided code and explain the output:

1. The `resolveAfterNSeconds` function is defined to create a Promise that resolves after a specified number of milliseconds `n` with the value `x`.

2. Inside the immediately invoked function expression (IIFE), the following steps are performed:

   - `let a = resolveAfterNSeconds(1000, 1);`: This creates a Promise `a` that will resolve after 1000 milliseconds (1 second) with the value `1`.

   - `a.then(async function (x) { ... })`: We use `.then()` to handle the resolved value of `a`, which is `x`. The callback function inside `.then()` is marked as `async`.

   - `let y = await resolveAfterNSeconds(2000, 2);`: This line waits for the resolution of a Promise created by `resolveAfterNSeconds(2000, 2)` (2 seconds delay) and assigns the resolved value `2` to `y`.

   - `let z = await resolveAfterNSeconds(1080, 3);`: This line waits for the resolution of a Promise created by `resolveAfterNSeconds(1080, 3)` (1.08 seconds delay) and assigns the resolved value `3` to `z`.

   - `let p = resolveAfterNSeconds(2000, 4);`: This creates a Promise `p` that will resolve after 2000 milliseconds (2 seconds) with the value `4`. Note that we don't use `await` here, so `p` is not awaited.

   - `let q = resolveAfterNSeconds(1000, 5);`: This creates a Promise `q` that will resolve after 1000 milliseconds (1 second) with the value `5`. Again, we don't use `await`, so `q` is not awaited.

   - `console.log(x + y + z + await p + await q);`: The `console.log` statement calculates and logs the sum of `x` (1), `y` (2), `z` (3), `await p` (4), and `await q` (5).

3. Now, let's calculate the total time it takes for the entire execution:

   - `a` takes 1000 milliseconds (1 second) to resolve.
   - `y` takes an additional 2000 milliseconds (2 seconds) to resolve.
   - `z` takes an additional 1080 milliseconds (1.08 seconds) to resolve.
   - `p` takes 2000 milliseconds (2 seconds) to resolve.
   - `q` takes 1000 milliseconds (1 second) to resolve.

The total time is the sum of these delays, which is `1000 + 2000 + 1080 + 2000 + 1000 = 6080 milliseconds` or `6.08 seconds`.

4. The final `console.log` statement calculates the sum: `1 + 2 + 3 + 4 + 5 = 15`.

So, the output of the code will be:

```
15
```

This output represents the sum of `x`, `y`, `z`, `await p`, and `await q`.

