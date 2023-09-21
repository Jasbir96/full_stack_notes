
## Agenda
- Arrays and its use cases
    - splice
    - slice
    - concat
    - split
    - join
    - trim
-  **Arrays** are object disguised as an array
- Functions and it's significance
     - Function Definition
     - Function Behaving Like Objects
     - Significance of Function
     - Functions can be assigned to variables
     - Passing a paramater to a function
     - Passing a function as a parameter to another function
     - Function returning another function
- Problem Explanation
- Intro to Call Back Function and Higher Order Functions
- Higher Order Function Methods
    - The .forEach() Method
    - The .map() Method
        - Polyfill of Map
    - The .filter() Method
    - The .reduce() Method
        - Polyfill of Reduce
    - The Sort method()

---

## 1. Arrays and its use cases

JavaScript arrays are ordered lists of values. Each value is called an element based on its index:

### 1.1 Array Methods

#### 1.1.1 splice()

With the splice() method, you can modify an array (add, remove, and replace elements).

**Syntax:**

`array.splice(start,deleteCount, item1, ..., itemN)`

**Example:**

```javascript
let arr = [1, 2, 3, 4, 5, 6, 7, 8];
let splicedArr = arr.splice(2, 3);
console.log("splicedArr: ", arr,splicedArr); // Output: splicedArr:  [ 1, 2, 6, 7, 8 ] [ 3, 4, 5 ]
```
#### 1.1.2 slice()

slice() returns a new array object containing a shallow copy of a portion of an array.

**Syntax:**

**arr.slice(start,a end)**

**Example:**

```javascript
let arr = [1, 2, 3, 4, 5, 6, 7, 8];
let slicedArr = arr.slice(2, 4);
console.log("slicedArr: ", slicedArr); // Output: slicedArr:  [ 3, 4 ]
```

#### 7.1.3 concat()

The concat() method concatenates given arguments to the given string.

**Syntax:**

`str.concat(str1, ..., strN)`

**Example:**

```javascript
let arr = [1, 2, 3, 4, 5, 6, 7, 8];
let concatArr = arr.concat([100, 200, 300]);
console.log("concatArr: ", concatArr);
 // Output: concatArr:  [
//     1, 2, 3,   4,   5,
//     6, 7, 8, 100, 200,
//   300
// ]
console.log("arr",arr);
// Output : arr [
//   1, 2, 3, 4,
//   5, 6, 7, 8
// ]
```

#### 1.1.4 split()

With the split() method, a string is split (divided) into two or more substrings according to the splitter (or divider).

**Syntax:**

`strings.Split(divider)`

**Example: **

```javascript
let str = "Hi i am google"
let arrStr = str.split(" ");
console.log(arrStr); // Output : [ 'Hi', 'i', 'am', 'google' ]
```

#### 1.1.5 trim()

Using the "trim()" method, we remove both whitespaces from the provided value without affecting its original form.

**Syntax: **

`string.trim()`

**Example:**

```javascript
let str = "Hi i am google";
let arrStr = str.trim();
console.log(arrStr); // Output : Hi i am google
```

#### 1.1.6 join()

The join() method returns a new string by concatenating all of the elements in an array, separated by a specified separator.

**Syntax:**

`arr.join(separator)`

**Example:**

```javascript
let str = "Hi i am google";
let arrStr = str.split(" ");
// joins the array of string into  string on the basis of delimiter
let joinedStr = arrStr.join("+");
console.log("joinedStr",joinedStr); // Output : joinedStr Hi+i+am+google
```

---







---


## 2. Introduction to Functions 

Functions are blocks of code that accomplish specific tasks.

Let's say you need to write a program that creates a rectangle and colors it. To solve this problem, you can create two functions:

- a function to draw the rectangle
- function to color the rectangle

Taking complex problems and breaking them down into smaller pieces makes your program easier to understand and reuse.

**Syntax:**

```javascript
function <function-name>(arg1, arg2, arg3,...)
{
    //write function code here
};
```

### 2.1 Function Definition

The following defines a function which when called prints out "Hi I am a fn"

**Example: Defining a function**

```javascript
//fn definiton 
function fn() {
    // code 
    console.log("Hi I am a fn");
}
// function call
fn(); // Output: Hi I am a fn
```

### 2.2 Function Behaving like Objects

* We can add properties and methods to functions similar to objects as functions behave like objects  
* An example of how an function behaves like object is shown below

**Example:**

```javascript
function fn() {
    // code 
    console.log("Hi I am a fn");
}
fn.count = 0;
fn.printCount = function () {
    console.log("count is ", this.count);
}
console.log("count prop", fn.count);
fn.printCount();
fn()
// Output:
// count prop 0
// count is  0
// Hi I am a fn
```


### 2.3 Functions can be assigned to variables:

Just like strings and numbers, functions can be assigned to variables. An example is shown below

**Example:**

```javascript
const fnAddress = function () {
    console.log("Hello i am a fn  expression");
}
fnAddress(); // Output: Hello i am a fn  expression
```

### 2.4 Passing a paramater to a function

JavaScript allows to pass a parameter to functions

Example: 

```javascript
function fn(param) {
    console.log("param is ", param); 
}
fn("Hello");
fn([1, 2, 3, 4]);
fn({ name: "Jasbir" });
// Output:
// param is  Hello
// param is  [ 1, 2, 3, 4 ]
// param is  { name: 'Jasbir' }
```

### 2.5 Passing a function as a parameter to another function

In JavaScript, we can pass a function to another function. An example is shown below

```javascript
function fn(param) {
    console.log("param is ", param); //[Function: param]
    const rValue = param();
    console.log("rValue is ", rValue);
    return "From outer fn";
}
const outerFnRVal = fn(smaller);
console.log(outerFnRVal);
function smaller() {
    console.log(" I am smaller");
    return "hello";
}
// Output:
// param is  [Function: smaller]
//  I am smaller
// rValue is  hello
// From outer fn
```
**Explanation:**

* The above code defines a function fn that takes another function as a parameter, calls that function (smaller in this case), logs the return value of the function, and then returns a specific string. 
* It demonstrates how you can pass functions as arguments to other functions in JavaScript. The result of calling fn(smaller) is the string "From outer fn", which is stored in the variable outerFnRVal.

### 2.6 Function returning another function

In JavaScript, we can retunr a function inside another function. An example is shown below

**Example:**

```javascript
function fn() {
    console.log(" I am fn I am returning a fn");  
    return function inner() {
        console.log("Returned from fn");  
    }
}
const Rval = fn();
console.log("Rval", Rval()); 
// Ouput:
//  I am fn I am returning a fn
// Returned from fn
// Rval undefined
```
**Explanation:**

* The above code defines a function fn that returns another function inner. 
* When fn is called and assigned to Rval, it logs two messages to the console. 
* When Rval is called, it logs the "Returned from fn" message and returns undefined, which is why you see "Rval undefined" in the output.

**Note: **

When we execute the below code we will get You both are wasted as output as the last line will only be considered. But this is not a best practice. So to avoid we will be going with function expression in that case you cannot reuse a function name which is decalred  already

**Example: Without function expression**

```javascript
function real () { console.log("I am real. Always run me"); }
function real () { console.log("No I am real one "); }
function real () { console.log("You both are wasted"); }
real();
// Output:
// You both are wasted
```

**Example: With function expression**

When we try to run this code we will be getting a error called **Identifier 'real' has already been declared**

```javascript
const real = function () { console.log("I am real. Always run me"); }
const real = function () { console.log("No I am real one "); }
const real = function () { console.log("You both are wasted"); }
real();
```

---


## 3.  Arrays in js 

In JavaScript, arrays are used to store a list of values. You can store JavaScript array objects in variables and deal with them as you would any other type of data.

**Example:**

```javascript=
let arr = [
    1,
    1.1
    , true,
    null,
    "Hello",
    [1, 2, 3, 4, 5],
    { name: "Jasbir" },
    function sayhi() {
        console.log(" I am a fn inside an array")
    }
]
console.log("arr", arr);
// Used to accrss the 3 inside [1,2,3,4,5]
console.log(arr[5][2]); // Output: 3
// Used to access the name 
console.log(arr[6]["name"]); // Output : Jasbir
// Used to access the function sayHi()
arr[7](); // Output: I am a fn inside an array
```

### 3.1 Arrays are object diguised as an array

* An array is essentially an object that specializes in storing a collection of elements, usually of the same data type, and provides various methods for manipulating and accessing those elements. 
* An example is shown below to demonstrate how arrays are object disguised. Traversin objects also looks very similar to this 

```javascript
let arr = [
    1,
    1.1
    , true,
    null,
    "Hello",
    [1, 2, 3, 4, 5],
    { name: "Jasbir" },
    function sayhi() {
        console.log(" I am a fn inside an array")
    }
]
for (let key in arr) {
    console.log("key : ", key, "value : ", arr[key]);
}
// Output:
// key :  0 value :  1
// key :  1 value :  1.1
// key :  2 value :  true
// key :  3 value :  null
// key :  4 value :  Hello
// key :  5 value :  [ 1, 2, 3, 4, 5 ]
// key :  6 value :  { name: 'Jasbir' }
// key :  7 value :  [Function: sayhi]
```

Since arrays are internally a object we will be able to add the value of Hello like in the example below

```javascript
let arr = [
    1,
    1.1
    , true,
    null,
    "Hello",
    [1, 2, 3, 4, 5],
    { name: "Jasbir" },
    function sayhi() {
        console.log(" I am a fn inside an array")
    }
]
arr['Hello'] = 200
for (let key in arr) {
    console.log("key : ", key, "value : ", arr[key]);
}
// Output:
// key :  0 value :  1
// key :  1 value :  1.1
// key :  2 value :  true
// key :  3 value :  null
// key :  4 value :  Hello
// key :  5 value :  [ 1, 2, 3, 4, 5 ]
// key :  6 value :  { name: 'Jasbir' }
// key :  7 value :  [Function: sayhi]
// key :  Hello value :  200
```

### `Ask a Question`: What will the output of console.log(arr[25]) and the length of the array

```javascript
let arr = [
    1,
    1.1
    , true,
    null,
    "Hello",
    [1, 2, 3, 4, 5],
    { name: "Jasbir" },
    function sayhi() {
        console.log(" I am a fn inside an array")
    }
]
console.log(arr[25]); // Output: Undefined
arr[30] = 600;
console.log("144", arr.length); // Output: Length = 31
```
Answer: The output of console.log(arr[25]) is undefined and the length of the array is 31

### `Follow Up Question`: What will be the length of the array now

```javascript
let arr = [
    1,
    1.1
    , true,
    null,
    "Hello",
    [1, 2, 3, 4, 5],
    { name: "Jasbir" },
    function sayhi() {
        console.log(" I am a fn inside an array")
    }
]
console.log(arr[25]); // Output: Undefined
arr[30] = 600;
console.log("144", arr.length);
arr["Hello"] = 200;
console.log("146", arr.length);// Output: Length = 31
```
Answer: 
* The length still remains 31 as the length is determined by the highest numeric index in the array, and non-numeric keys do not affect the length of the array. 
* When you set arr["Hello"] = 200;, you are adding a property with the key "Hello" to the array object, but this does not affect the length of the array because this property is not a numeric index.

Now when add a element at 75 after hello now the length would become 76

```javascript
let arr = [
    1,
    1.1
    , true,
    null,
    "Hello",
    [1, 2, 3, 4, 5],
    { name: "Jasbir" },
    function sayhi() {
        console.log(" I am a fn inside an array")
    }
]
console.log(arr[25]); // Output: undefined
arr[30] = 600;
console.log("144", arr.length); // Output: 144, 31
arr["Hello"] = 200;
arr[75] = 800;
console.log("146", arr.length);// Output: 146, 76
```

---


## 4. Intro Higher Order Functions
### 4.1 Problem Explanation

### `Ask a Question`: Write a code to square every element in the array [2, 3, 4, 5] without using map, filter and reduce

Answer: 

```javascript
let arr = [2, 3, 4, 5];
for (let i = 0; i < arr.length; i++) {
    arr[i] = arr[i] * arr[i]
}
console.log("arr",arr) // Output: arr [ 4, 9, 16, 25 ]
```

### `Follow Up Question`: Write a code to cube every element in the array [2, 3, 4, 5]

```javascript
let arr = [2, 3, 4, 5];
for (let i = 0; i < arr.length; i++) {
    arr[i] = arr[i] * arr[i] * arr[i];
}
console.log("arr",arr); // Ouput: arr [ 8, 27, 64, 125 ]
```
* The kind of behaviour we do in the above codes is called transformation
* When have an array of movies and every object has details about the movies like runtime,ratings, year release and the actors involved and in that we have to filter only the ratings of the movie. In that case we do not need the entire data so we do transformations
* When we practice writing code for each and everything like square and cube when both the functionlity is same we are repeating the code which is not a good idea so to avoid we will have a transformer function like the below example

**Example:**

```javascript
let arr = [2, 3, 4, 5];

function transformer(arr, logic) {
    let newArr = [];
    for (let i = 0; i < arr.length; i++) {
        let ans = logic(arr[i]);
        newArr.push(ans);
    }
    return newArr;
}

function squarer(elem) {
    return elem * elem;
}
function cuber(elem) {
    return elem * elem * elem;
}

const squaredArr = transformer(arr, squarer);
const cubedArr = transformer(arr, cuber);

console.log("squaredArr", squaredArr); // Ouput: squaredArr [ 4, 9, 16, 25 ]
console.log("cubedArr", cubedArr); // Output: cubedArr [ 8, 27, 64, 125 ]
```

**Explanation**

* In the above code we have defined a higher-order function transformer, which takes an array and a transformation function (logic) as input and applies the logic function to each element of the input array, storing the results in a new array newArr. 
* Two transformation functions are defined: squarer, which squares a number, and cuber, which cubes a number.
* Then we use the transformer function to create two new arrays: squaredArr and cubedArr. squaredArr contains the result of applying the squarer function to each element of arr, and cubedArr contains the result of applying the cuber function to each element of arr. Finally, it logs both squaredArr and cubedArr to the console.

### 4.2 Intro to Callback Functions and Higher Order Functions

**Call Back Functions**

In JavaScript, a callback function is a function that is passed into another function as an argument. This function can then be invoked during the execution of that higher order function (that it is an argument of).

Functions can be passed as arguments in JavaScript since they are objects.

```javascript
const isEven = (n) => {
  return n % 2 == 0;
}
let printMessage = (evenFunc, num) => {
  const isNumEven = evenFunc(num);
  console.log(`The number ${num} is an even number: ${isNumEven}.`)
}

// Pass in isEven as the callback function
printMessage(isEven, 6); // Output: The number 6 is an even number: true
```

**Higher-Order Functions**

In Javascript, functions can be assigned to variables in the same way that strings or arrays can. They can be passed into other functions as parameters or returned from them as well.

Higher-order functions take functions as parameters and return functions.

---


## 5. Imp Higher Order Function Methods:

### 5.1 The .forEach() Method

The for each method is used to traverse the arrays. An example on how to implement it is shown below

```javascript
let arr = [2, 3, 4, 5];
function printELem(elem) {
    console.log(elem * elem);
}
let rVal = arr.forEach(printELem) 
// Output
// 4
// 9
// 16
// 25
```
### 5.2 The .map() Method

* Map() executes a callback function on each element in an array. The callback function returns a new array with the return values.

* This method does not alter the original array, and the returned array may contain different elements than the original array.

* Each member of the finalParticipantsArr array has a 'joined the contest.' string added by the .map() method.

```javascript
let arr = [2, 3, 4, 5];

function squarer(elem) {
    return elem * elem;
}
function cuber(elem) {
    return elem * elem * elem;
}
let squaredArr = arr.map(squarer);
console.log("squaredArr", squaredArr); // Output: squaredArr [ 4, 9, 16, 25 ]
let cubedArr = arr.map(cuber);
console.log("cubedArr", cubedArr); // Output: cubedArr [ 8, 27, 64, 125 ]
```

### 5.2.1 PolyFill of Map

An example of how to perform an polyfill of map is shown below

```javascript
let arr = [2, 3, 4, 5];

Array.prototype.myMap = function (logic) {
    let newArr = [];
    for (let i = 0; i < this.length; i++) {
        let ans = logic(this[i]);
        newArr.push(ans);
    }
    return newArr;
}
function squarer(elem) {
    return elem * elem;
}
function cuber(elem) {
    return elem * elem * elem;
}
let squaredArr = arr.myMap(squarer);
console.log("squaredArr", squaredArr); // Output: squaredArr [ 4, 9, 16, 25 ]
let cubedArr = arr.myMap(cuber);
console.log("cubedArr", cubedArr); // Output: cubedArr [ 8, 27, 64, 125 ]
```

**Explanation:**

* The above extends the functionality of JavaScript's built-in Array object by adding a custom method called myMap. 
* This custom myMap method behaves similarly to the standard Array.prototype.map() method, which is used for transforming elements of an array and creating a new array based on the results of applying a given function to each element of the original array.

### 5.3 The .filter() Method

The .filter() method calls a callback function on each element of an array. Each element's callback function must return either true or false. Any elements returned by the callback function are included in the new array.

```javascript
let elems = [1, 2, 3, 11, 4, 5, 34, 12];

function isOdd(elem) {
    return elem % 2 == 1;
}

function isgtr5(elem) {
    return elem > 5;
}
// odd values
let oddvaluesArr = elems.filter(isgtr5);
console.log("oddvaluesArr", oddvaluesArr); // Output: oddvaluesArr [ 11, 34, 12 ]
console.log("elem", elems); 
// Output: elem [
//   1, 2,  3, 11,
//   4, 5, 34, 12
// ]
```

**Note: Give polyfill of filter method as HomeWork**

### 5.4 The .reduce() Method

* In .reduce(), an array is iterated through and a single value is returned.

* It takes two parameters (accumulator and currentValue) as arguments. Iteration after iteration, accumulator represents the value returned by the last iteration, and currentValue represents the current element.

```javascript
let elems = [1, 2, 3, 4, 5];
function sum(acc, elem) {
    return acc + elem;
}
function product(acc, elem) {
    return acc * elem;
}
const sumValues = elems.reduce(sum); 
console.log("sum", sumValues); // Output: sum 15
const prodValues = elems.reduce(product);
console.log("prod", prodValues); // Ouput: prod 120
```

### 5.4.1 Polyfill of reduce

Intuition approach of polyfill of reduce

**Intution 1**

```javascript
function sumArr(arr) {
    // init
    let sum = 0;
    for (let i = 0; i < arr.length; i++) {
        // collects the results
        sum = sum + arr[i];
    }
    return sum;
}
```
**Explanation:**

In the above code, sumArr function takes an array as input, iterates through its elements, and calculates the sum of those elements, which it then returns as the result

**Intution 2**

```javascript
let elems = [1, 2, 3, 4, 5];
// cb 
function sum(acc, elem) {
    return acc + elem;
}
function product(acc, elem) {
    return acc * elem;
}
function reducer(arr, cb) {
    let acc = arr[0];
    for (let i = 1; i < arr.length; i++) {
        acc = cb(acc, arr[i]);
    }
    return acc;
}
const sumValues = reducer(elems,sum); 
console.log("sum", sumValues); // Output: sum 15
const prodValues = reducer(elems,product);
console.log("prod", prodValues); // Ouput: prod 120
```

**Explanation:**

In the above code, the use of a reducer function to perform different operations (sum and product) on an array by passing different callback functions to the reducer.

Whenever we are asked to write the polyfill of reduce we can start with these initial 2 intuitions and move to the final version

**Final Verion of polyfill of reduce**

```javascript

let elems = [1, 2, 3, 4, 5];
Array.prototype.reducer = function (cb, defVal) {
    let acc = defVal != undefined ? defVal : this[0];
    let sidx = defVal != undefined ? 0: 1;
    for (let i = sidx; i < this.length; i++) {
        acc = cb(acc, this[i]);
    }
    return acc;
}
function sum(acc, elem) {
    return acc + elem;
}
function product(acc, elem) {
    return acc * elem;
}
function reducer(arr, cb) {
    let acc = arr[0];
    for (let i = 1; i < arr.length; i++) {
        acc = cb(acc, arr[i]);
    }
    return acc;
}
const sumVal = elems.reducer(sum, 10);
console.log("sumVall", sumVal); // Output: sumVall 25
const prodVal = elems.reducer(product, 100);
console.log("prodVal", prodVal);// Output: prodVal 12000
```


An example of how to perform an polyfill of reduce is shown below

```javascript
// define custom reduce()

if (!Array.prototype.customReduce) {
   Array.prototype.customReduce = function(callback, initialValue) {
    let accumulator;
    let firstIndex;
    //We will get only the callback param,in this case we know initialValue is not pass and 
    //Here we will set the accumulator to be with the first item and set firstIndex to be 1
    if (arguments.length === 1) {
      accumulator = this[0];
      firstIndex = 1;
    } 
    //Here we will get both callback and initialValue
    //In this case we set the accumulator to initialValue and firstIndex to be 0
    else {
      accumulator = initialValue;
      firstIndex = 0;
    }
    //Here we will iterate on each item in the array (depend what we set for the firstIndex)
    //and each time we keep the new accumulator
    for (let index = firstIndex; index < this.length; index++) {
      accumulator = callback(accumulator, this[index], index);
    }
    //when iteration is done we return the accumulator
    return accumulator;
  }
} 
// declare an array
var numbers = [6, 7, 8, 9, 10]
// call custom reduce() on array to get sum of all elements
console.log(numbers.customReduce((total,num) => total + num, 0)); // Output: 40
```
### 5.5 The Sort method()

* Used to sort items in an array (either in ascending or descending order).

```
var animals = ['hen',
    'elephant',
    'llama',
    'leopard',
    'ostrich',
    'whale',
    'octopus',
    'rabbit',
    'lion',
    'dog'];

let sortedArr = animals.sort();
console.log("sortedArr", sortedArr);
// Output:
// sortedArr [
//   'dog',     'elephant',
//   'hen',     'leopard',
//   'lion',    'llama',
//   'octopus', 'ostrich',
//   'rabbit',  'whale'
// ]
```

By default, we can't sort numbers based on their numeric value because all non-undefined elements are converted to strings before sorting.

Here's how we can implement this using a custom function.

```javascript
let numArr = [1, 2, 3, 4, 5, 11,];

function sortHelper(a, b) {
    /**increasing order*/ 
    return b-a;
}
let sortedArr = numArr.sort(sortHelper);
console.log("sortedArr", sortedArr); // Output: sortedArr [ 11, 5, 4, 3, 2, 1 ]
```


