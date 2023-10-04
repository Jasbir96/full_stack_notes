
# Agenda
- spread, rest and default parameter
- call by ref and call by value
- copy in JS : Assignment
- Shallow Copy in JavaScript
    - Shallow Copy an object in JavaScript using spread operator
- Deep Copy in JavaScript
- Polyfill of DeepCopy
- How to Flatten An Array

---
# Example
```javascript
function fn(param1, param2, param3){
    console.log("hi params are ", param1, param2, param3);
}
fn("hi", "hello", "hola");
fn("hi", "hello");
```

**Output:**
hi params are  hi hello hola
hi params are  hi hello undefined

**Explanation:**
We can see that if we are not passing parameters then its value becomes undefined.

# Default parameter
We can assign a default value to the parameters, that value will be assigned to the parameter if we do not pass any value for that parameter during function call.
```javascript
function fn(param1, param2, param3 = "defaultValue"){
    console.log("hi params are ", param1, param2, param3);
}
fn("hi", "hello", "hola");
fn("hi", "hello");
```

**Output:**
hi params are  hi hello hola
hi params are  hi hello defaultValue

**Explanation:**
Now as in the second function calling we have not passed any value to the param3, so it assigns a default value to it.

# spread operator

Let us take an example of an array, And assign that array to another array, then both the arrays have the same reference.
```javascript
let arr = [1, 2, 3, 4, 5];
let arr2 = arr;
arr2.pop();
arr2.push(100);
arr2[2] = 200;
console.log("arr", arr);
```

**Output:**
[1, 2, 200, 4, 100]

Here we can see that both arrays have the same reference, As we have performed operations on `arr2`, but all the operations are reflected in `arr` also.

**With assignment operator reference remains the same.**

## Spread Operator
**To overcome this problem we use the spread operator.**
The spread operator copies all the values from one array to another array on only the first level.

### Example
```javascript
let arr = [1, 2, 3, 4, 5];
// assigning one array to another using spread operator
let arr2 = [...arr];
arr2.pop();
arr2.push(100);
arr2[2] = 200;
console.log("arr", arr);
```

**Output:**
[1, 2, 3, 4, 5]

Now we can see that the original array has not changed.

**Array Inside Array:**
```javascript
let arr = [1, 2, [3, 4], 4, 5];
// assigning one array to another using spread operator
let arr2 = [...arr];
arr2.pop();
arr2.push(100);
arr2[2][0] = 200;
console.log("arr", arr);
```

**Output:**
[1, 2, [200, 4], 4, 5]

Now we can see that the original array is also changed.
**So in spread changes at the top level in the new array are not reflected in the original array, but if we make any changes at the next level, then it will also reflected in the original array.**
# rest
rest is used to collect n number of params. Syntax of rest and spread are the same, but the places where it was used are different. rest is used while accepting parameters in a function.
- Rest is used to collect the remaining parameters.
- It is used as a parameter of function.

## Example
```javascript
function fn(param1, ...param2){
    console.log("params are ", param1);
    console.log("rest params ", param2);
}
fn("hi", "hello", "hola");
fn("hi", "hello");
```

**Output:**
params are hi
rest params ["hello", "hola"]
params are hi
rest params ["hello"]

---



#  Call by reference and call by value

**Example**
```javascript
let arr = [1, 2, 3, 4, 5];
let arr2 = arr;
arr2.pop();
arr2 = 10;
console.log(arr);
```

We have a stack and heap, we have arr and arr2 in the stack, and arr is stored at 10k in the heap, and in arr2 we are storing the address of arr.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/335/original/upload_29be90abbaea5eaa6a82a85c4deb215a.png?1695155816)

- Now when we do arr2.pop(), as it has 10k address, so it will pop element 5.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/336/original/upload_46f4928f55d8cb0d6409f23440e998ef.png?1695155856)

- Now when we update arr2, then it has value arr2 = 10.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/337/original/upload_cc62079bf25db95d11994a80361db179.png?1695155884)


- So `console.log(arr)` will print [1, 2, 3, 4]

**Example**
```javascript
function modifer(a, b){
    console.log("1", a, b)
    a = 10
    b = 20
    console.log("2", a, b)
}
let p = [4, 7, 9]
let q = [3, 6, 8]
console.log("3", p, q);
modifier(p, q)
console.log("4", p, q);
```
In the above program, we are not using any pointer/reference.
**Output**
3 [ 4, 7, 9 ] [ 3, 6, 8 ]
1 [ 4, 7, 9 ] [ 3, 6, 8 ]
2 10 20
4 [ 4, 7, 9 ] [ 3, 6, 8 ]
## References
It is an address or a pointer. 

**So in javascript everything is call by sharing.**

- For primitive we have the actual value
- For non-primitive, we are passing the value of the reference.

---
##  How Copy works in general in JS

It is important to understand how copy work in different JavaScript data types before we move on to shallow and deep copy values.

In programming, variables serve as data containers and data storage. Eight data types are defined in the latest version of ECMAScript,

Primitive data types include:

- Boolean
- null
- undefined
- Number
- BigInt 
- String 
- Symbol

JavaScript has only one non-primitive/composite data type:

- Object 

When we copy primitive data types, it will be a values are copied.

Example:

```javascript
let a = 15;
let b = a; // this is a copy
a = 20;
console.log(a) //Output: 20
console.log(b)//Output : 15
```
As b is a copy of a, changes made to a will not affect b.

If the same applies to non-primitive types, then changes in the source object will affect the cloned object values. It's because types refer to addresses rather than values in memory.

```javascript
eg. let object = {
    a : 20
}
let clonedObject = object // this is a copy
object.a = 30;
console.log(object.a); // Output : 30
console.log(clonedObject.a) // Output : 30
```

ClonedObject is the duplicate of object, changing object would affect clonedObject's properties.

---

---

##  Shallow Copy in JavaScript

* Shallow Copy refers to the process of transferring a bit-by-bit copy of an object to a specified object.
* In shallow copying, an exact copy of an object is pasted into another object.
* Generally, it is used for copying one-dimensional array elements, which include only the first level elements.
* There is only a copy of the reference addresses.

### 2.1 Shallow Copy an object in JavaScript using spread operator

It is possible to shallow copy an object using the "spread" operator. It is represented by three consecutive dots "...".

**Syntax:**

`let objectVariable= {...objectValue};`

Here, the spread operator will copy the key-value pair of “objectVariable” to “objectValue”.

**Example: Copying an object shallowly in JavaScript using the spread operator**

Let's start by creating an object called "student" that has two key-value pairs:

```javascript
let arr = [1, 2, 3, 4, [10, 12], 5, 6];
// spreadArray now contains the same elements as arr, but it's a new array with a separate reference.
let spreadArray = [...arr];
spreadArray[2] = 100;
spreadArray[4] = 200;
spreadArray[4][1] = 300;
console.log("outputs ", spreadArray, arr); 
// Ouput:
// outputs  [
//     1, 2, 100, 4,
//   200, 5,   6
// ] [ 1, 2, 3, 4, [ 10, 12 ], 5, 6 ]
```

**Explanation:**

* The modification spreadArray[2] = 100; only affects spreadArray, so the third element in spreadArray is changed to 100 while leaving arr unchanged.
* Similarly, spreadArray[4] = 200; affects spreadArray but does not impact arr. So, the fifth element in spreadArray becomes 200, but arr remains the same.
* The modification spreadArray[4][1] = 300; changes the second element of the subarray within the fifth element of spreadArray. This also indirectly modifies the corresponding subarray in arr because both arrays share the same subarray reference. So, the second element of the subarray within the fifth element in arr becomes 300 as well.
* In summary, spreading an array creates a new array with the same elements but a separate reference.

### `Ask a question`:  why does shallow copy exist not deep copy ?
Answer : We can have array and object that can have n level if nesting an there size is only limited by js heap so for performance point of view it good to have shallow copy

---

---
##  Shallow Copy in JavaScript using Object.assign() method

JavaScript's "Object.assign()" method shallow copies the "key-value" pair of an already created object into another one. By using the object.assign() method, you will not alter the original object.

**Syntax:**

`Object.assign(target, source)`

In this case, "target" represents the JavaScript object whose key-value pair will be copied, and "source" represents the object into which the copied key-value pair will be pasted.

**Example: JavaScript shallow copy using Object.assign()**

By invoking "Object.assign()", we will shallow copy the "student" object to “studentCopy”:

```javascript
let person = {
     firstName: 'John',
     lastName: 'Doe',
     address: {
         street: 'North 1st street',
         city: 'San Jose',
         state: 'CA',
         country: 'USA'
    },
};
let copiedObject = Object.assign({}, person);
copiedObject.lastName = "Odinson";
copiedObject.address.street = "south 1st street";
console.log("person", person); 
// Output:
// person {
//   firstName: 'John',
//   lastName: 'Doe',
//   address: {
//     street: 'south 1st street',
//     city: 'San Jose',
//     state: 'CA',
//     country: 'USA'
//   }
// }
console.log("copiedObject", copiedObject);
// Output:
// copiedObject {
//   firstName: 'John',
//   lastName: 'Odinson',
//   address: {
//     street: 'south 1st street',
//     city: 'San Jose',
//     state: 'CA',
//     country: 'USA'
//   }
// }
```

**Explanation:**

* Object.assign({}, person) is used to create a shallow copy of the person object. Object.assign() is a method that takes one or more source objects and copies their properties into a target object (in this case, {} which creates an empty object). 
* This results in the creation of a new object called copiedObject that initially has the same properties and values as the person object.
* The code creates a shallow copy of the person object using Object.assign(), which means that primitive properties are copied, but nested objects (like address) are still shared between the original and copied objects. 
* Changes made to the nested objects in copiedObject will also affect the nested objects in the person object.

---


---

##  Deep Copy in JavaScript

JSON.stringify() and JSON.parse() are methods that are used for deep copying a JavaScript object.

**Syntax:**

`let object= JSON.parse(JSON.stringify(objectCopy));`

Stringify() converts JavaScript "object" into a string, and then parse() performs the parsing operation and returns which will be stored in "objectCopy”.

**Example: Deep copying a JavaScript object using JSON.stringify() and JSON.parse()**

This example uses the stringify() and parse() methods to copy "student" to "studentCopy". Using "JSON.stringify()" the "employee" object will be converted into a "string" and parsed with "JSON.parse()" to return a JavaScript object:

Only downside is : JSON.parse requires regex matching that ie very slow so we usually avoid it whereever possible

```javascript
let person = {
     firstName: 'John',
     lastName: 'Doe',
     address: {
         street: 'North 1st street',
         city: 'San Jose',
         state: 'CA',
         country: 'USA'
     },
};
// convert obj to string 
let stringSyntaxOfobject = JSON.stringify(person);
console.log(typeof stringSyntaxOfobject,stringSyntaxOfobject)
// Output:
// string {"firstName":"John","lastName":"Doe","address":{"street":"North 1st street","city":"San Jose","state":"CA","country":"USA"}}
// /**deep copy -> object like string*/
let deepClonedObj = JSON.parse(stringSyntaxOfobject);

deepClonedObj.lastName = "Odinson";
deepClonedObj.address.street = "south 1st street";
console.log("person", person);
// Output:
// person {
//   firstName: 'John',
//   lastName: 'Doe',
//   address: {
//     street: 'North 1st street',
//     city: 'San Jose',
//     state: 'CA',
//     country: 'USA'
//   }
// }
console.log("copiedObject", deepClonedObj);
// Output:
// copiedObject {
//   firstName: 'John',
//   lastName: 'Odinson',
//   address: {
//     street: 'south 1st street',
//     city: 'San Jose',
//     state: 'CA',
//     country: 'USA'
//   }
// }
```

**Explanation:**

* The JSON.stringify() method is used to convert the person object into a JSON-formatted string.
* The typeof operator is used to check the data type of stringSyntaxOfobject, and the result is logged to the console.
* JSON.parse() creates a deep copy of the person object. The deep cloned object is assigned to the deepClonedObj variable.
* This code creates a deep clone of the person object using JSON serialization and deserialization and allows you to make changes to the clone without affecting the original object, demonstrating the concept of deep copying.

**Example: Deep copying a JavaScript Array using JSON.stringify() and JSON.parse()**

```javascript
let arr = [1, 2, 3, 4, [10, 12], 5, 6];
let stringArr = JSON.stringify(arr);
const deepArr = JSON.parse(stringArr);
deepArr[2] = 100;
deepArr[4][1] = 300;
console.log("outputs ", deepArr, arr);
// Ouput:
// outputs  [ 1, 2, 100, 4, [ 10, 300 ], 5, 6 ] [ 1, 2, 3, 4, [ 10, 12 ], 5, 6 ]
```
**Explanation:**

* JSON.stringify is used to convert the arr array into a JSON-formatted string. 
* After converting arr to a string, the code then uses JSON.parse to convert that string back into a JavaScript array. This effectively creates a new array, deepArr, with the same values as arr. 
* However, it's important to note that this new array is a deep copy of the original array, meaning it contains entirely new instances of the nested array and its elements.
* The changes we made only affects deepArr and does not impact the original arr
* This code demonstrates how to create a deep copy of a nested array and make changes to the copied array without affecting the original array.

---

---

## Polyfill of DeepCopy

An example of how to perform ployfil of DeepCopy is shown below

```javascript
let person = {
    firstName: 'John',
    lastName: 'Doe',
    address: {
        street: 'North 1st street',
        city: 'San Jose',
        state: 'CA',
        country: 'USA'
    },
    friends: ["Steve", "Nikola", "Ray", { name: "Jai", lastName: "Roy" }]
};

let superClone = (object) => {
    let isArr = Array.isArray(object);
    let cloning = isArr ? [] : {};
    for (let prop in object) {
        if (Array.isArray(object[prop])) {
            cloning[prop] = [...object[prop]];
            for (let i = 0; i < cloning[prop].length; i++) {
                if (cloning[prop][i] == "object") {
                    cloning[prop][i] = superClone(object[prop][i]);
                }
            }
        } else if (typeof object[prop] === "object") {
            cloning[prop] = superClone(object[prop]);
        } else {
            cloning[prop] = object[prop];
        }
    }
    return cloning;
};

let deeplyClonedObj = superClone(person);
deeplyClonedObj.lastName = "Odinson";
deeplyClonedObj.address.street = "south 1st street";
console.log("person", person);
// Output:
// person {
//   firstName: 'John',
//   lastName: 'Doe',
//   address: {
//     street: 'North 1st street',
//     city: 'San Jose',
//     state: 'CA',
//     country: 'USA'
//   },
//   friends: [ 'Steve', 'Nikola', 'Ray', { name: 'Jai', lastName: 'Roy' } ]
// }
console.log("copiedObject", deeplyClonedObj);
// Output:
// copiedObject {
//   firstName: 'John',
//   lastName: 'Odinson',
//   address: {
//     street: 'south 1st street',
//     city: 'San Jose',
//     state: 'CA',
//     country: 'USA'
//   },
//   friends: {
//     '0': 'Steve',
//     '1': 'Nikola',
//     '2': 'Ray',
//     '3': { name: 'Jai', lastName: 'Roy' }
//   }

```

**Explanation**

* The  above JavaScript code defines an object person representing information about an individual, and it includes nested objects and arrays. 
* A function called superClone is defined to create a deep copy of any object, including nested objects and arrays. 
* Finally, it demonstrates how to use the superClone function to create a deeply cloned copy of the person object.
* The key takeaway here is that the superClone function clones an object deep, so that any changes you make to it won't affect the original.

---
---
##  How to Flatten An Array

The below code defines a JavaScript function called flatten that takes a nested array as input and transforms it into a single-level array containing all the numbers from the input array


**Example:**

```javascript
// input  -> nested array
let input = [1, 2, 3, [4, 5], [6, 7, 8, [9, 10, 11]]];
// output -> single level of array with num 
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11];

// [4, 5] -> [4,5]
// [6, 7, 8, [9, 10, 11]] -> [6, 7, 8, 9, 10, 11]

function flatten(srcArr) {
    let newArr = [];
    for (let i = 0; i < srcArr.length; i++) {
        // check if elemnt -> array -> 
        let elem = srcArr[i];
        let isElemArr = Array.isArray(elem);
        if (isElemArr) {
            // flatten it again
            let smalleFlattenArr = flatten(elem);
            newArr.push(...smalleFlattenArr);
        } else {
            //push it to newArr;
            newArr.push(elem);
        }
    }
    return newArr;
}
let flattenedArr = flatten(input);
console.log(flattenedArr);
// Output:
// [
//    1, 2, 3, 4,  5,
//    6, 7, 8, 9, 10,
//   11
// ]

```

**Note:** The question that will be asked in interviews is how does Array.prototype.flat() method works. We would be asked to write a code to create a method over Array object so that every array would get their own flatten function that has the option of levels also

Answer: It works the same way as the flatten function with one difference which is we can pass the levels inside it  like 1,2. If we pass 1 as parameter it would remove the first layer and when we provide 2 it would remove 2 layers it is ideak to use Infinity when we want to remove all the layers. An example is shown below

**Example:**

```javascript
let input = [1, 2, 3, [4, 5], [6, 7, 8, [9, 10, 11]]];

let flattenOutput = input.flat(Infinity);
console.log(flattenOutput); 
// Ouput:
// [
//    1, 2, 3, 4,  5,
//    6, 7, 8, 9, 10,
//   11
// ]
```
Questions which can be asked based on this flatten a array are 

 * simple deep clone/copy
 * deep copy /clone with nested objects and array HW
 * flatten an array 
 * Array.prototype.flat() HW
 * flatten an object 

**Give Array.prototype.flat method() which and deep clone with nested object and properties as homework**

**Object before flattening**

```
let person = {
    firstName: 'John',
    lastName: 'Doe',
    address: {
        street: 'North 1st street',
        city: 'San Jose',
        state: 'CA',
        country: 'USA',
        postCodes: {
            firstBlock: 10,
            secondBlock: 12
        }
    }
}
```

**Object after flatening**

```
person = {
    firstName: 'John',
    lastName: 'Doe',
    "address.street": 'North 1st street',
    "address.city": 'San Jose',
    "address.state": 'CA',
    "address.country": 'USA',
    "address.postCodes.firstBlock": 10,
    "address.postCodes.secondBlock": 12
}
```


---

