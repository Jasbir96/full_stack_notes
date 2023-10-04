## Agenda
- Lexical Scope
- Intro to closures
- Nested closure
- Application of closure 
    - Infinite Currying
    - Creating Private variables using closure
    -  Creating Memoized function

## 1. Lexical Scope

* A lexical scope allows a function scope to access variables in the outer scope. The outer scope is deteremined wrt to `function definition`  only were we have defined the function code in js file  .
* Note : fn call is not used to determine the outer scope  

**Example:**

```javascript
var varName = 10;
/**fn def*/
function b() {
    console.log("in b",varName);
}
function fn() {
    var varName = 20;
    /**fn call*/
    b();
    console.log(varName);  //20
}

fn(); 
// Output
// in b 10
// 20
```
**Code Explanation:**

* In the above code varName has a global scope since it is defined outside the function 
* Inside the fn function, there's a local variable also named varName. This local variable takes precedence over the global one when referenced within the fn function. 
* When the b function is called from within the fn function, it references the varName variable. Since there is no varName declared within the b function's scope, it looks for the nearest enclosing scope, which is the `fn function definition`. It finds the local varName declared in fn and uses that value.

### 1.1 Quiz Question
```
var varName = 10;
function b() {
    console.log( varName);
}
function fn() {
    var varName = 20;
    b();
    console.log(varName);
}
fn();
```
What will the output
#### Choices
- [ ] error
- [ ] 20 10
- [x] 10 20
- [ ] 20 10

**Explanation:** This code defines two functions, b and fn, and a global variable varName with a value of 10. Inside fn, a local variable varName with a value of 20 shadows the global one. When fn is called, it logs the local varName, which is 20, then calls b. b logs the global varName, which is 10.

## 2. Intro to closures

* Closure is a JavaScript lexical scoping technique used to preserve variables from a function's outer scope in its inner scope.
* In lexical scoping, the scope of a variable is defined by its position in the source code.
* Whenever you define a function, the variables it contains are only accessible within the function. 
* If you try to access variables within a function from outside it will result in a scope error. Closure can solve this problem.

**Example: **

```javascript
function outerFunction() {
    let count = 0;
    function innerFunction() {
        count++;
        return count
    }
    return innerFunction
}
const innerFunc = outerFunction();
console.log(innerFunc()) // Output: 1
console.log(innerFunc()) // Output: 2 
```

**Code Explanation:**

* This code defines two functions, outerFunction and innerFunction, and demonstrates closure in JavaScript.
* outerFunction is defined, which initializes a variable count to 0.
* Inside outerFunction, there's an innerFunction defined. This inner function increments count by 1 each time it's called and returns the updated value of count.
* outerFunction then returns innerFunction, but crucially, it retains access to the count variable due to closure.
* const innerFunc = outerFunction(); calls outerFunction and assigns the returned innerFunction to the innerFunc variable. At this point, innerFunc is essentially a reference to innerFunction with its own enclosed count variable.
* When innerFunc() is called the first time, it increments the count (which starts at 0) to 1 and returns it, leading to the first console.log outputting 1.
* When innerFunc() is called again, it increments the count (which was retained in the closure) to 2 and returns it, resulting in the second console.log outputting 2.
* In essence, this code demonstrates how inner functions can maintain access to variables defined in their outer functions even after those outer functions have completed execution, thanks to closure.

## 3. Closure Question 1

Give the output of the below code:

```javascript
function createCounter(init, delta) {
    function count() {
        init = init + delta;
        return init
    }
    return count
}
let c1 = createCounter(10,5)
let c2 = createCounter(5,2)
console.log(c1()) // 15
console.log(c2()) // 7
console.log(c1()) // 20
console.log(c2()) // 9
```
**Output:**

```
15
7
20
9
```

**Here's a step-by-step explanation of the code:**

* The createCounter function takes two parameters, init and delta, which represent the initial value of the counter and the amount it should be incremented by when called.
* In the count function, the init value is incremented by the delta value and then the updated init value is returned.
* Count is returned by the createCounter function. Basically, when you call createCounter, it doesn't return a value directly, but instead a reference to the count function.
* A counter function c1 is then created by calling createCounter with an initial value of 10 and a delta of 5. Whenever you call c1, it will start at 10 and increment by 5 every time.
* Similarly, we create another counter function c2 with an initial value of 5 and a delta of 2. Each time it is called, it will start at 5 and increment by 2.
* As soon as you call c1(), the count function is invoked with the values that were specified during the call to createCounter(10, 5) in the createCounter() function. 
* The value is incremented by 5 (10 + 5 = 15) and the console is logged with 15.
* In the same way, when you call c2(), it invokes the count function created with the values specified in createCounter(5, 2). In this example, the value is incremented by 2 (5 + 2 = 7) and the result is logged to the console 
* Then we repeat the same process again.
## 4. Nested Closures

* A nested closure occurs when an inner function references variables or parameters defined in an outer function, and the inner function references the outer function. 
* Despite the outer function's completion, these inner functions still have access to its variables.

**Example:**

```javascript
let iamINGEC = 200;
function getFirstName(firstName) {
    console.log("I have got your first Name");
    return function getLastName(lastName) {
        console.log("I have got Your last Name");
        return function greeter() {
            console.log(`Hi I am ${firstName} ${lastName}`);  // closure 
            console.log("Hi GEC", iamINGEC) // LExical scope
        }
    }
}


const fnNameRtrn = getFirstName("Jasbir");
// console.log(fnNameRtrn); // getLastname

const lnNameRtrn = fnNameRtrn("Singh");
// console.log(lnNameRtrn);  // greeter

lnNameRtrn();
// Output:
// I have got your first Name
// I have got Your last Name
// Hi I am Jasbir Singh
// Hi GEC 200
```

**Explanation:**

* The above code defines a function getFirstName which takes a firstName argument. When called, it logs a message and returns another function, which in turn takes a lastName argument. 
* The second function getLastName also logs a message and returns a third function called greeter. 
* The greeter function logs a message that includes the firstName and lastName passed to the outer functions, creating a closure over these variables. It also logs the value of the iamINGEC variable, which is in the lexical scope.
* This code showcases the concept of closures and lexical scope in JavaScript, allowing functions to remember and access variables from their outer scopes even after those outer functions have finished executing.


## 5. Application of Closures

Listed below are the application of closure

### 5.1 Currying

* Currying involves splitting up a function that accepts multiple arguments into several functions that only accept one parameter each. 
* It allows you to provide some arguments upfront while delaying the provision of others.

```javascript
function getFirstName(firstName) {
    console.log("I have got your first Name");
    return function getLastName(lastName) {
        console.log("I have got Your last Name");
        return function greeter() {
            console.log(`Hi I am ${firstName} ${lastName}`);  // closure 
        }
    }
}

getFirstName("Jasbir")("Singh")();

const getLastName = getFirstName("Jasbir");
const greeter = getLastName("Singh");
greeter();
console.log("Task in between");
console.log("Task in between");
// Output:
// I have got your first Name
// I have got Your last Name
// Hi I am Jasbir Singh
// I have got your first Name
// I have got Your last Name
// Hi I am Jasbir Singh
// Task in between
// Task in between
```

**Code Explanation:**

* The getFirstName function takes a single argument firstName and returns a function (getLastName) that expects another argument (lastName).
* The getLastName function, in turn, returns a function (greeter) that doesn't take any additional arguments.
* This structure follows the currying pattern
* In the code we first provide "Jasbir" as an argument to getFirstName, which returns a function that expects "Singh" as an argument. Then, we call that function with "Singh" to get the greeter function, and finally, you call greeter().

### 5.2 Closure with Async code

* The setTimeout() method is commonly used if you want to run your function after a specified amount of time has passed. 
* Here is the general syntax of the method:

**Syntax:**

`setTimeout(expression, timeout);`

**Example:**

```
let a=100;
console.log("Before");
function cb() {
    console.log("I will explode",a);
}
setTimeout(cb, 2000);
console.log("After");
```

**Explanation:**

* "Before" is logged to the console immediately.
* The setTimeout function is called with cb as the function to execute and a delay of 2000 milliseconds. This means that after approximately 2 seconds, the cb function will be executed.
* "After" is logged to the console immediately.
* The code continues to run. However, there's a delay of 2 seconds specified for the cb function execution, so the "I will explode" message, along with the value of a (which is 100), will be logged to the console after 2 seconds.

---
Give the output of the below code:

```javascript
function outer() {
    let arrFn = [];
    for (var i=0;i<3;i++) {
        arrFn.push(function fn() {
            console.log(i);
        })
    }
    return arrFn;
}
let arrFn = outer();
arrFn[0]();
arrFn[1]();
arrFn[2]();
```

**Output:**

```
3
3
3
```

**Explanation:**

* When arrFn[0]() is called the current value of i is logged to the console, which is 3. Since the for loop has completed and i is now 3.
* When arrFn[1]() is called it also logs the value of i, which is still 3.
* When arrFn[2]() is called, and it logs the value of i, which is still 3.
* All three functions reference the same i variable, and they access its value at the end of the loop when it's 3.

---

## 7. Closure Question 3 

Give the scope of arrFn in the below code

```javascript
function outer() {
    let arrFn = [];
    for (var i=0;i<3;i++) {
        arrFn.push(function fn() {
            i++;
            console.log(i);
        })
    }
    return arrFn;
}
let arrFn = outer();
arrFn[0]();
arrFn[1]();
arrFn[2]();
```
**Answer:**


* When declared inside a block (for example, inside a loop or if statement), variables declared with let have block scope, but arrFn is declared within outer's function body, so its scope is limited to the function body. 
* As a result, it can be accessed throughout the entire function, but not outside of it.
 
#### 9.1 Follow Up Question

Give the output of the below code:

```javascript
function outer() {
    let arrFn = [];
       let i = 0
       for (i=0;i<3;i++) {
           arrFn.push(function fn() {
           console.log(i);
        })
    }
    return arrFn;
}
let arrFn = outer();
arrFn[0]();
arrFn[1]();
arrFn[2]();
```

**Output:**

```
3
3
3
```

**Explanation:**

* Calling arrFn[0] logs the value of i, which is 3, to the console.
* Calling arrFn[1] also logs 3 to the console.
* ArrFn[2] also logs 3 to the console when called.
* Since i is incremented in the loop, you might expect these functions to log 0, 1, and 2, respectively. All of them, however, log 3. This id due to JavaScript's closure system:



---
title: Closure Question 4
description: How to solve the question
duration: 360
## 8. Closure Question 4

Give the output of the below code:

```javascript
function outer() {
    let arrFn = [];
    
    for (let i=0;i<3;i++) {
        arrFn.push(function fn() {
            console.log(i);
        })
    }
    return arrFn;
}
let arrFn = outer();
arrFn[0]();
arrFn[1]();
arrFn[2]();
```

**Output:**

```
0
1
2
```

**Explanation:**

* arrFn[0] : This line calls the first function in arrFn, which prints the value of i captured for that function, 0 in this case.
* arrFn[1] : This line prints the value of i captured for the second function in the array, which is 1.
* arrFn[2] : Calls the third function in the array, which prints the value captured for i in that function, which is 2.

---


## 9. Infinite Currying

* With infinite currying, you can curry a function indefinitely, adding arguments one by one. 
* In order to accomplish this, a new function is returned for each argument until all arguments are collected, and then the original function is invoked with all arguments.

**Example:**

```javascript
function counter(args) {
    // write code only inside this function
    let count = 0;
    count++;
    if (args == 0) {
        return count;
    } else {
        return function inner(args) {
            count++;
            if (args == 0) {
                return count;
            } else {
                return inner;
            }
        }
    }
}
console.log(counter(0)); // Output :  1
console.log(counter()(0)); // Output : 2
console.log(counter()()()()(0)); // Output : 3 
```  
**Explanation:**

* In the above code, the counter function returns either a number (when args is 0) or another function (when args is not 0). 
* This creates a chain of nested function calls, where each function returned by counter is itself capable of returning another function or a number.
* This chain of nested functions effectively allows you to "curry" the function indefinitely, meaning you can keep calling it with additional parentheses to create deeper levels of nesting.

---
title: Creating Private variables using closure
description: Explain how to create private varibales using closure
duration: 360

## 10. Creating Private variables using closure

* JavaScript allows you to create private variables using the closure method. 
* A closure is a function that can access variables defined in the scope of its parent function even after the parent function has finished and returned. 
* Defining a variable inside a function makes it a private variable that is only accessible by code inside that function.

```javascript
function createEvenStack() {
    let items= [];
    return {
        push(item) {
            if (item % 2 == 0) {
                console.log("Is pushed");
                items.push(item);
            }
            else {
                console.log("Please input even value");
            }
        },
        pop() {
            return items.pop();
        }
    }; 
}
const stack = createEvenStack();
stack.push(10);
stack.push(5);
console.log(stack.items); // the items array is not exposed as a property of the stack object, so it returns undefined
// Output:
// Is pushed
// Please input even value
// undefined

```
**Explanation:**

* When we declare a variable within a function (in this case, items), that variable is typically only accessible within the function's scope. 
* However, by returning an object with methods that reference items, you effectively create a closure. The methods within the returned object can access and manipulate the items variable even after the createEvenStack function has finished executing.
* This encapsulation of items within the closure means that items is not directly accessible from outside the function, making it effectively private. It can only be accessed or modified through the methods provided by the returned object (push and pop in this case).

---
title: Creating Memoized function
description: Explain how to create memoized function using closure
duration: 720

## 11. Creating Memoized function

* Memorized functions improve performance when they are repeatedly called with the same arguments. 
* Here is a very simple example of how to create a memoized function:

```
function calc(n) {
    let sum = 0;
    for (let i = 0; i < n; i++) {
        sum += n;
    }
    return sum;
}


function memoize(fn) {
    let cache = {};
    return function (n) {
        //  cache -> res -> present
        let istheInputPresent = cache[n]==undefined;
        if (istheInputPresent) {
            return cache[n];
        } else {
            const result = fn(n);
            cache[n] = result;
            return result;
        }
        //  it is not  -> call the actual fn and 
        // add the res to the cache
        //then return  the result
    }
}
let efficentCalcFN = memoize(calc);
console.time(); 
efficentCalcFN(5);
console.timeEnd();
console.time();
efficentCalcFN(5);
console.timeEnd();
// Output:
// default: 0.079ms
// default: 0.008ms
```
**Explanation:**

* calc(n) computes the sum of n itself n times.
* memoize(fn) is a higher-order function that takes any function fn and returns a memoized version of it. It caches results in an object called cache.
* The code then memoizes the calc function and measures its execution time for two calls with the same input, 5. 
* The first call computes the result and stores it in the cache, while the second call retrieves the cached result, significantly reducing execution time. 
* This showcases the efficiency of memoization in avoiding redundant calculations for the same input.