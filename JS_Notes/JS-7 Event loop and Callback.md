
# Agenda

-  Intuition behind Asynchronous Programming
-   Async JS with callbacks
-   All about the event loop with a callback queue
-   Serial and Parallel Execution of Async code
- Timers : setTimeout, setInterval, clearInterval, clearTimeout 
- SetInterval and clearInterval polyfill 

---
## Intuition of Async JS

**Case Scenario**
Let's assume that there's the Big Billion Day Sale on Flipkart and there's a huge discount on the iPhone 14 Pro. Moreover, there are three users who arrive on the website in quick succession (after an interval of 1 second each).


- The User 1 arrives at 8:00 PM and requests for the page of iPhone 14 Pro
- The Flipkart server requests the page from the database through a query
- It takes 2 seconds to fetch the answer and say, the request comprises a minimal time of 0.1 second only.

**Ask the learners**
- Can user 2 make his request right at the moment he arrives or does he need to wait until the request-response finishes for user 1?
- **Answer:** User 2 will have a waiting time of 1 sec, following which he'd be able to make a request.

Now, user 2 arrives and asks for the Nokia's page and gets the response back at 8:00 PM and 4 seconds (since he made a request at 8:00 PM & 2 seconds).

Eventually, the user 3 will eventually have a waiting time of 2 seconds and also a delay in response. This continues to increase for the newly arriving users.

> So, this illustrates the fact that this approach is not appropriate for handling numerous requests.

**Solution:**
Assume we have a group of workers where the Flipkart server can delegate the tasks to the workers.

The workers will fetch the answer from the database and get the response which is then sent back to the user.

This is how the multiple requests can be handled when it's solely the responsibility of workers to fetch the response.

Here's how the synchronous code works:
```javascript!
/**
 * Synchronous code -> the code that executes line by line
 */

console.log("Before");
function fn() {
    console.log("I am fn");
}

fn();
console.log("After");
```

**Output:**
```
Before
I am fn
After
```
So, here the statement "Before" gets printed and the function `fn()` is allocated a memory space and is then executed. The statement "After" gets printed at the end. 

---
##  Async JS with callbacks


Now, let's see what happens in the case of Asynchronous programming:
```javascript!
/**
 * Asynchronous code -> piece of code that's executed at the current point of time
 * and other piece of code is executed on later part
 */

//1
console.log("Before");

function fn() {
    console.log("I am fn");
}
//3
setTimeout(fn, 2000);
//2
console.log("After");
```
**Output:**
```
Before
After
I am fn
```
Here, the function `setTimeout` had been called earlier but since there's a delay of 2000 milliseconds, it would be executed once the "Before" and "After" get printed.

**Ask the learners**
- Is `console.log()` a part of the JavaScript language?
- **Answer:** No, it's not.
- Is window a part of the JavaScript language?
- **Answer:** The `window` object is not actually a part of the JavaScript language itself. Instead, it is a feature provided by web browsers to enable interaction between JavaScript code and the browser environment.

Similarly, the `document`, `fetch`,`eventListeners`, and even`I/O` are also not part of JS. Contrarily, they are all provided by the browser itself; they are part of the Web API.

On the other hand, there's NodeJS. `fs`, `http`, and `child_process` are all a part of NodeJS.

>  the Environment provides the features whereas JavaScript provides us the logic.

As for the browser, the features are provided by the Web API and logic is provided by JavaScript. However, Node provides both Node API as well as JS logic which enables to use the same logic for both frontend as well as backend.

**Ask the learners**
- **Generic question:** Why was there a need for Java when there's already C++? (for a beginner as well)
- **Answer:** Because it eradicates the hassle of taking care of pointers. Similarly, if you jump from Java to JavaScript, you'd observe that you don't have to worry about threads.

**Inference:** As a programmer, one cannot create asynchronous functions. They are provided by the Environment itself.

Now, there's a simple following what we've discussed.

---
## quiz 1

Here's the code. And what will be the output here? (What gets printed in the console)

```javascript!
let a = true;
console.log("Before");
setTimeout(() => {
    a = false;
console.log("I broke the while loop");
}, 1000);
console.log("After");

while(a){
    
}
```
**Output:**
```
Before
After
(the while loop never terminates)
```

Now, let's perceive why it happens.
**Note:** You'll get the same output in every environment.

**Explanation:**
We need to have clarity of each of the lines in the code.
- a's value is set to true in the callstack
- "Before" gets printed in the console
- Now, `setTimeout` is a feature provided by the Web/Node API, so it goes to the Node API. Moreover, it contains a function with some logic.
- Even if the `setTimeout` wishes to go to the callstack, it cannot until the callstack is empty.
- However, the value of `a` will be true forever and therefore, the while loop never terminates.

---
## Quiz 2  

What will be the output of this code?
```javascript!
console.log("Before");

const cb2 = () => {
        console.log("Set timeout 1");
        while(1){

        }
}

const cb1 = () => console.log("hello");

setTimeout(cb2, 1000)

setTimeout(cb1, 2000)

console.log("After");
```

**Output:**
```
Before
After
Set timeout 1
(while loop continues)
```
**Explanation:**
- "Before" gets printed and then we have `const` variable `cb2` and `cb1`.
- Both the `setTimeout` are sent to the Web API. 
- We also have a callback queue where `cb2` goes first and is followed by `cb1`.
- Since the callstack is empty, the setTimeout with `cb2` goes to the callstack.
- It contains a while loop that won't  terminate, so the code continues to be in the same state waiting for the termination of the loop.


---
## Quiz 3  


What will be the output if we modify the code to be this way?
```javascript!
console.log("Before");

const cb2 = () => {
        console.log("Set timeout 1");
        let timeInFuture = Date.now() + 5000;
        while(Date.now() < timeInFuture){

        }
}

const cb1 = () => console.log("hello");

setTimeout(cb2, 1000)

setTimeout(cb1, 2000)

console.log("After");
```
**output:**
```
Before
After
Set timeout 1
.
.
.
hello (after 6 seconds)
```

**Explanation:** The "hello" gets printed after 6 seconds because the `cb2` has a `setTimeout` of 1 second, followed by the wait for 5 more seconds in the while loop.
The `cb2` that has a waiting time for 2 seconds can now get executed immediately and "hello" gets printed.

---
##  Event loop with callback queue


The event loop is a fundamental concept in JavaScript that governs how asynchronous operations are managed and executed within a single-threaded environment.

The event loop continuously checks the call stack and the callback queue. If the call stack is empty, it takes the first function from the callback queue and pushes it onto the call stack for execution. 

This process ensures that the asynchronous code is executed when the call stack is clear.

**Takeaways:** 
- JavaScript is single-threaded. It provides us the logic whereas the Web API provides the features.
- **Callback queue:** When an asynchronous operation (like a `setTimeout` or an event listener) is ready to be executed, its callback function is placed in the callback queue.
- **Event loop:** The event loop continuously checks the call stack and the callback queue and pushes the function from the callback to the callstack.

---
##  Serial and Parallel Execution of Async code  

Let's understand the meaning of synchronous, serial, parallel, and async.

There's a common distinction: 
- When we talk about tasks, they can either be serial or parallel.
- When we discuss the code, it can either be synchronous or asynchronous.

Now, what are serial tasks?
**Serial tasks:** Tasks that are dependent.
**Parallel tasks:** Tasks that are independent.

Serial tasks are tasks that are executed one after the other, in a sequential manner. In other words, each task starts only after the previous one has completed.

Parallel tasks are tasks that are executed simultaneously, or at least concurrently, meaning they can be worked on at the same time. Parallel tasks do not have to wait for each other to complete; they can progress independently.


Let us understand the difference between synchronous and asynchronous code.
**Synchronous code:**
Create a file `f1.txt` with a content of your choice:
```
Hello, I'm f1
```

Now, write the JS code in this manner:
```javascript!
const fs = require("fs");
// Synchronous function provided by nodejs to read a file

console.log("Before");
const buffer = fs.readFileSync("./f1.txt");
console.log(""+buffer);
console.log("After");
```

**Output:**
```
Before
Hello, I'm f1
After
```
**Asynchronous code:**
```javascript!
const fs = require("fs");
// Asynchronous function provided by nodejs to read a file

console.log("Before");
const buffer = fs.readFile("./f1.txt", function (err, data){
    console.log("" + data);
});
console.log("After");
```
**Output:**
```
Before
After
Hello, I'm f1
```

**Explanation:** Here, the `fs.readFile` function is called to asynchronously read the contents of the file named `f1.txt`. The function takes two arguments: the path to the file and a callback function that will be executed when the file reading is complete or encounters an error.

Let's see the issue with an asynchronous code:
```javascript!
/* You can block the main thread 
when you've got 2 files to read
and return the concatenated result in the order for the file supplied */

const fs = require("fs");

console.log("Before");
let content1 = fs.readFileSync("./f1.txt");
let content2 = fs.readFileSync("./f2.txt");
console.log("" + content1 + "\n" + content2);
console.log("After");
```

**Output:**
```
Before
Hello, I'm f1
Hello, I'm f2
After
```

If the same code is used for handling multiple requests, it won't be able to do so until one request gets fulfilled.
```javascript!
const fs = require("fs");

const {Server} = require("http");
console.log("Before");
let content1 = fs.readFileSync("./f1.txt");
let content2 = fs.readFileSync("./f2.txt");
console.log("" + content1 + "\n" + content2);
console.log("After");

Server.get("/",cb);
Server.post("/",cb);
```
If your server routes are blocking, then you can only listen to one route at a time.

**Ask the learners:**
- Now, for an asynchronous code, if you wish to read the next file after the first file, where are we supposed to the write the code for the next file?
- **Answer:** By writing about the next file inside the function for the first file.

**Code:**
```javascript!
/* You cannot block the main thread 
when you've got 2 files to read
and return the concatenated result in the order for the file supplied 
async function -> callback -> it confirms that your function has been executed*/

const fs = require("fs");

console.log("Before");

fs.readFile("./f1.txt", f1cb);

function f1cb(err, data) {
    let content1 = data;
    fs.readFile("./f2.txt", f2cb);
    function f2cb(err, data){
        let content2 = data;
        console.log("" + content1 + " " + content2);
    }
    
}

console.log("After");
```

---
## Quiz 4  



What if we cannot block the main thread and have two files to read and print the output in any manner?

**Answer:**
```javascript!
/* You cannot block the main thread 
when you've got 2 files to read
and print the output in any manner */

const fs = require("fs");

console.log("Before");

fs.readFile("./f1.txt", function(err, data){
    console.log("" + data);
});
fs.readFile("./f2.txt", function(err, data){
    console.log("" + data);
});


console.log("After");
```
**Output:**
```
Before
After
Hello, I'm f1
Hello, I'm f2
```

Here, the order of execution is uncertain and can be produced in any order for the files.




---

## setTimeout and clearTimeout
**Ask the learners**
- Are timers part of JS?
- **Answer:** No, they aren't. They are not a core part of the JavaScript language specification itself but are provided by these environments to enable scheduling of code execution at specific times or after specific intervals.

When you use `setTimeout` to schedule a function to run after a specified delay, the function is not executed immediately when the timer elapses if the call stack is not empty. Instead, the function is placed in a queue (known as the "callback queue" or "message queue") until the call stack becomes empty.

**Example**
Let's start with an example here:

```javascript
/*****SetTimeout****/
console.log("Before");
// Calling a function at a delay of 3000 milliseconds
function cb(){
    console.log("setTimeout cb has been called");
}
setTimeout(cb, 3000)
console.log("After");
```

**Output:**
```plaintext
Before
After
setTimeout cb has been called
```

Now, if you wish to cancel the timeout, what should be done in this case?

With `setTimeout`, we also have an attached method called `clearTimeout`. As the name suggests, it basically cancels the `setTimeout`.

Let's see how we can cancel the `setTimeout`. So, everytime `setTimeout` is called, it returns an `id` which we can further use to call a `clearTimeout` function.

---
### Question
What will be the output of the following code?

```javascript
/*****SetTimeout -> clearTimeout****/
console.log("Before");
// Calling a function at a delay of 3000 milliseconds
function cb(){
    console.log("setTimeout cb has been called");
}
const timerID = setTimeout(cb, 3000);

function canceller() {
    console.log("Cancelling the timeout");
    clearTimeout(timerID);
}
setTimeout(canceller, 2000);
console.log("After");
```

**Output:**
```plaintext
Before
After
Cancelling the timeout
```
**Explanation:** The timeout will be canceled out in 2s.

-   The `cb` function is scheduled to run after 3000 milliseconds (3 seconds).
-   The `canceller` function is scheduled to run after 2000 milliseconds (2 seconds).
- The 2000-millisecond timer elapses, and the `canceller` function is executed. 
- It logs "Cancelling the timeout" and clears the timeout scheduled for the `cb` function and the 3000-millisecond timer for the `cb` function has not yet elapsed, but the `clearTimeout` function prevented it from running.

---
### Quiz
What does the timestamp in the output justify?

```javascript!
/*****SetInterval****/
console.log("Before");

function cb(){
    console.log("Time stamp is", Date.now());
}
// Calling function cb at an interval of 1-second
setInterval(cb, 1000);
console.log("After");
```

**Output:**
```
Before
After
Time stamp is  1660712345678
Time stamp is  1660712346679
Time stamp is  1660712347680
Time stamp is  1660712348681
.
.
... continues after every 1-second 
```

**Explanation:** The timestamp is obtained using the `Date.now()` function. This function returns the current timestamp in milliseconds EPOCH since January 1, 1970 UTC.

---

### Quiz
What will be the output of the code given below?
```javascript
/*****setInterval, clearInterval****/
console.log("Before");

function cb(){
    console.log("Interval called");
}

// Setting up an interval to call function cb every 1000 milliseconds (1 second)
const timerID = setInterval(cb, 1000);

// Function to cancel the interval
function cancelInterval() {
    console.log("Cancelling the interval timer");
    clearInterval(timerID); // Clearing the interval using clearInterval
}

// Setting up a timeout to cancel the interval after 3000 milliseconds (3 seconds)
setTimeout(cancelInterval, 3000);

console.log("After");
```
**Output:**
```plaintext
Before
After
Interval called
Interval called
Cancelling the interval timer
```

**Explanation:**

**Web API:** Both the `setInterval` and `setTimeout` functions are part of the Web API. They schedule the execution of `cb` and `cancelInterval` functions.


When the Global Execution Context (GEC) comes into the picture, it allocates the memory to the variables. So, the "Before" gets printed, and "After" get printed. The `timerID` was present in the callstack and it gets removed along with GEC. 



However, the `timerID` survives because of hoisting (memory allocation) and closure. 

When` cancelInterval` gets its memory, it forms **closure** over its outer variable. Therefore, even if the GEC is removed, the `cancelInterval` will always have access to `timerID`.

> **Note:** You cannot do asynchronous programming without closures.

Back to our question, the `cb` gets added to the queue and since the callstack is empty, it'll go to the callstack where it be executed and "Interval called" gets printed. The same process repeats after a second.



Now that the "Interval called" has been printed twice, what would happen next?

Indeed, the `setInterval` has a built-in `setTimeout`. Alternatively, it appears (although not true) that the behavior of`setInterval` can be emulated by calling `setTimeout` infinitely. 

The `setTimeout` had already been called before the `setInterval` gets called for the third time. Therefore, the callback of `setTimeout` has higher precedence than the callback of `setInterval` and "Cancelling the interval timer" gets printed.


> **Note:** Moving forward, emphasize why the precedence of `setTimeout` is more than that of `setInterval`.

- **Ask the learners** if the concept of setInterval and clearInterval is clear.

---
title: setInterval polyfill
description: 
duration: 2000
card_type: cue_card
---

We'll now implement our own **`setInterval`**.


**Ask the learners**
- What does the `setInterval` take up as parameters?
- **Answer:** callback and time.
- Now, what would you do when you have the following code and you need to call `cb` only one?
```javascript!
function mysetInterval(cb, time){

}

function cb() {
    console.log("I'll be called on every interval")
}

mysetInterval(cb, 1000)
```

- **Answer:** We need to call `setTimeout` inside `mysetInterval`.

```javascript!
/* Create polyfill of setInterval with the help of setTimeout */
function mysetInterval(cb, delay){
    function myfunc{
        cb();
    }

    setTimeout(myfunc, delay)
}

function cb() {
    console.log("I'll be called on every interval")
}

mysetInterval(cb, 1000)
```

Here, the function `myfunc` does nothing but calls `cb`.

**Ask the learners**
- How can you call `cb` again after a particular delay?
- **Answer:** By internally calling `setTimeout` in the function `myfunc`.

**Code:**
```javascript!
/* Create polyfill of setInterval with the help of setTimeout */
function mysetInterval(cb, delay){
    function myfunc(){
        cb();
        // Calling setTimeout again after a fixed interval
        setTimeout(myfunc, delay);
    }

    setTimeout(myfunc, delay)
}

function cb() {
    console.log("I'll be called on every interval")
}

mysetInterval(cb, 1000)
```
**Output:**
```
I'll be called on every interval
I'll be called on every interval
I'll be called on every interval
I'll be called on every interval
.
.
...
```

Let's understand how we can work with `clearTimeout`.
We'll take an example where a statement gets printed until a boolean is not changed from true to false.
So, here we must take an object which has a boolean instance variable.

But, how about considering the boolean variable itself instead of the hassle of creating an object?

Here's a demonstration of its consequence:
```javascript!
/* Create polyfill of setInterval with the help of setTimeout */
function mysetInterval(cb, delay){
    // primitive
    let flag = true;
    function myfunc(){
        cb();
        if(flag == true)
        setTimeout(myfunc, delay);
    }

    setTimeout(myfunc, delay);
    return flag;
}
function clearMyInterval(flag) {
    flag = false;
}

function cb() {
    console.log("I'll be called on every interval")
}

let flag = mysetInterval(cb, 1000);

function clearCb() {
    clearMyInterval(flag);
}

setTimeout(clearCb, 2000);
```
Since a boolean variable will be primitive, a copy of `flag` is sent and modified. This doesn't have any impact on the original `flag`. The `flag` will have the same value in the scope where it has been initialized.


With objects, it's easier to deal with because it provides a reference to the same object and its properties. Therefore, we have an object to do our modification and not its copy.

```javascript!
/* Create polyfill of setInterval with the help of setTimeout */
function mysetInterval(cb, delay){
    // primitive
    let obj = {
        flag : true
    }
    function myfunc(){
        if(obj.flag == true)
        {
            cb();
            setTimeout(myfunc, delay);
        }
        
    }
    setTimeout(myfunc, delay);
    return obj;
}
function clearMyInterval(obj) {
    obj.flag = false;
}

function cb() {
    console.log("I'll be called on every interval")
}

let obj = mysetInterval(cb, 1000);

function clearCb() {
    console.log("Cancelling the cb");
    clearMyInterval(obj);
}

setTimeout(clearCb, 3000);
```

**Output:**
```
I'll be called on every interval
I'll be called on every interval
Cancelling the cb
```

**Instruction:** Break down of the code explanation shall be taught to the learners for this output.

**Takeaways:**
- `setInterval` is more like a `setTimeout` that is called at a given interval repeatedly. 
- Non-primitive data types are the way to have a single source of truth
- The `clearInterval` is a simple function that receives an object and changes its properties to false, whereas
- `setInterval` only executes the further `setTimeout` functions on the basis of the value of the flag



