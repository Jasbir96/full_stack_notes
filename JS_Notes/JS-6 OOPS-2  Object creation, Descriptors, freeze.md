
## Agenda

- Creating objects in JS
    - Using Object Literals
    - How are we able to use methods on primitive data types even though have no method or properties associated with them
    - Using Object.create()
       - Creating an object without any parent
       - Creating an object from another object
- How to iterate using for in loop 
 - How to iterate using Object.Keys() method
- Understanding Inheritance
    - Using the function Constructor Method in JavaScript before the introduction of es6
    - Using Class Constructor after the introduction of ES6
-  Prototypal Inheritance 
-  Object Descriptors
Preventing : Reassignment, Create, update & delete 
    - const 
    - Object.seal 
    - Object.freeze 
    - Object.preventExtension


## 1. Creating objects in JS:

In JavaScript, objects are non-primitive data types that allow you to store multiple collections of data.

**Example:**

```javascript
const candidate = { 
    firstName: 'Muthu', 
    class: 12
};
console.log(candidate.firstName) // Output: Muthu
console.log(candidate.class) // Output : 12
```

In this case, candidate is an object that stores strings and numbers as part of its values.

### 1.1 Using Object Literals

JavaScript's simplest method of creating an object is to initialize it. 

Defining and creating an object can be done in a single line. 

A key-value pair phenomenon is used to assign values separated by commas. 

Values are assigned in curly braces:

**Syntax**

```javascript
var object = {
   propertyName:propertyValue
}
```

A property name refers to the name of the property, and a property value refers to its value.

**Example:**

```javascript
// Creating an object using Object literals
var candidate = {
    firstName:"Muthu",
    lastName:"Annamalai"
};
console.log(candidate.firstName); // Output: Muthu
```

**Code Explanation:**

* A candidate object is defined, and various properties are assigned to it.
* These properties are then assigned different values.
* Using JavaScript's console.log() method, candidate.firstName is finally displayed.

### 1.1.1 How are we able to use methods on primitive data types even though have no method or properties associated with them

* Although primitive types don't have methods, except for null and undefined, there are object equivalents that wrap the primitive values, so we can use methods on them.
* For string primitives, there's String, for number primitives, there's Number, and for others, there's Boolean, BigInt, and Symbol.
* When a method is called, Javascript automatically converts primitives into their corresponding objects. The Javascript wraps the primitive and calls the method.
* Here's how a String object looks with its primitive value and proto (which is outside of our scope here but is related to the prototype of its object) 
![strings](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/338/original/upload_f47ee274ac33fc6aae53d606a4270608.png?1695156454)
* This is how we can access properties like length and methods like indexOf and substring when we work with string primitives.
* Whenever Javascript finds an object that should have a primitive value, it calls the valueOf method to convert it back to a primitive value.
![valueOf method JavaScript](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/339/original/upload_fd7011ea0f29ba0da374e38482fc2b2e.png?1695156559)


### 1.2 Using Object.create()

Object.create() creates a new object and links it to the prototype of an existing object.

**Syntax:**

**Object.create(proto, propertiesObject)**

The create() method takes in:

* proto - Represents the prototype of new object.
* propertiesObject (optional) - It is used to specify which property descriptors should be added to the newly-created object. 

```javascript
// creating the prototype for the object that will be created later
function greeting() {
   this.greeting = 'Hello Muthu!';
}
// using the object.create() method to create a function whose object inherits properties from the prototype
function greetMuthu() {
   greeting.call(this);
}
// creating an greetMuthu function object with the prototype object's properties (such as greeting)
greetMuthu.prototype = Object.create(greeting.prototype);
const app = new greetMuthu();
// Displaying the object created
console.log(app.greeting); //Output : Hello Muthu!
```

In this example, there are two functions “greeting” and “greeetMuthu”.

We have created a new greetMuthu instance named "app" with a prototype and property of "greeting", e.g. this.greeting = 'Hello World'.

#### 1.2.1 Creating an object without any parent

We can create an object without any parent by using the below synatx

`var a = Object.create(null)`

This creates an object that doesn't inherit anything, i.e., an empty object with no prototype.
![Creating an object without any parent](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/340/original/upload_cb3c5905fad2c6082695cf2a776bf8c9.png?1695156588)

#### 1.2.2 Creating an object from another object

An example of how to create an object from another object is shown below

```javascript
let candidate = {
    name: "Muthu",
    age: 24,
    job: "developer"
}
let pickNameAndAge = function(record) {
  return {
    name: record.name,
    age: record.age
  }
}
let muthu = pickNameAndAge(candidate)
console.log(muthu) // Output : { name: 'Muthu', age: 24 }
```

#### 1.2.3 How to iterate using for in loop 

A for...in statement iterates over an object's properties. We will make a simple candidate object with a few name:value pairs to demonstrate.

```javascript
const candidate = {
  firstName: "Muthu",
  lastName: "Annamalai"
}
```

By using a for...in loop, we can access the names of all the properties easily.

```javascript
// It will print the property names of candidate object
for (attribute in candidate) {
   console.log(attribute); // Output : firstName,lastName
}
```

By using the property name as the object's index value, we can also access each property's value.

```javascript
// It will print the property values of object
for (attribute in candidate) {
   console.log(candidate[attribute]); //Output : Muthu, Annamalai
}
```
Using them together, we can access an object's names and values.

```javascript
// It will print the names and values of object properties
for (attribute in candidate) {
   console.log(`${attribute}`.toUpperCase() + `: ${candidate[attribute]}`); // Output: FIRSTNAME: Muthu, LASTNAME: Annamalai
}
```
The property name was modified using the toUpperCase() method, followed by the property value. The for...in method is very useful for iterating through the properties of an object.

#### 1.2.4 How to iterate using Object.Keys() method

The Object.keys() method returns an array of an object's enumerable properties.

**keys() Syntax:**

`Object.keys(obj)`

```javascript
let Candidate = {
  name: "Muthu",
  age: 20
};
```

```javascript
// get all keys of Candidate
let std1 = Object.keys(Candidate);
console.log(std1); // Output: [ 'name', 'age']
```

#### 1.2.5 Understanding Inheritance

* Using inheritance, you can define classes that inherit all the functionality of their parents and add new functionality.
* A class can inherit all the methods and properties of another class by using class inheritance.
* A useful feature of inheritance is that it allows reusability of code.
* Class inheritance is achieved by using the extends keyword. For example

**Example:**

```javascript
// parent class
class Candidate { 
    constructor(name) {
        this.name = name;
    }
    greet() {
        console.log(`Hello World I am ${this.name}`);
    }
}
// inheriting parent class
class User extends Candidate {

}
let candidate1 = new Candidate('Muthu');
candidate1.greet(); // Output : Hello World I am Muthu
```

**Code Explanation:**

* In the example above, the Candidate class inherits all the methods and properties of the Person class. 
* Thus, the Candidate class now has a name property and a greet() method.
* Then, we accessed the greet() method of Candidate class by creating a candidate1 object

### 1.3 Using the function Constructor Method in JavaScript before the introduction of es6

* In JavaScript, the constructor method can also be utilized to create an object. 
* An object instance of the class is created by the method. 
* In this method, an object type is defined using the constructor method:

**Syntax:**

```javascript
function Constructor(property) {
this.property = property;}
let newObject = new Constructor('objectValue');
```

Following is a description of the parameters.

* Constructor: a method for initializing class objects.
* newObject: Represents An object that has just been created
* property: Describes the existing properties of an object
* objectValue: Identifies the value assigned to the object.

**Example:**

```javascript
// Example of creating an object using a constructor
function Class(name, age) {
    this.name = name;
    this.age = age;
}

let candidate1 = new Class('Muthu', 20);
let candidate2 = new Class('Annamalai', 25)
console.log(candidate1.name); // Output : Muthu
console.log(candidate2.name); // Output : Annamalai
```

**In this code:**

* A constructor is called by passing a property name and an age parameter.
* Then, two objects named candidate1 and candidate2 are created.
* By calling the constructor, various values can be assigned to them

### 1.4 Using Class Construtor after the introduction of ES6

* JavaScript ES6 supports the concept of classes. Using a class to create the object is quite similar to using a constructor. 
* In JavaScript, the methods are replaced with classes by providing the functionality in the ES6 version. 
* Below is the syntax for creating this method:

**Syntax**

```javascript
Class className{
    constructor(property) {
        this.property = property;
    }
}
let newObject = new className('objectValue');
```

In the above syntax:

* ClassName specifies the class name.
* Aftert the above step we will pass the property to the constructor.
* Then JavaScript assigns the objectValue to the newObject variable.

Example

```javascript
// An example of how to create an object using classes
class Student {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}
let candidate1 = new Student('Muthu', 20);
let candidate2 = new Student('Annamalai', 25)
console.log(candidate1.name); // Output : Muthu
console.log(candidate2.age); // Output : 25
```

In this code:

* Class Student has two properties: name and age
* Additionally, two objects are created: candidate1 and candidate2.
* Then, candidate1 and candidate2 are assigned different values.
* Lastly, use JavaScript's console.log() method to display the information.

---
title: Prototypal Inheritance 
description: Intro to prototypal inheritance
duration: 600
card_type: cue_card
---

## 2. Prototypal Inheritance 

* In Javascript, prototypal inheritance is used to add methods and properties to objects. 
* It allows one object to inherit the properties and methods of another. 
* We typically use Object.getPrototypeOf and Object.setPrototypeOf to get and set the prototype of an object. 
* Nowadays, proto is used to set it in modern languages.

**Example:**

```javascript
let candidate = {
    fly: true,
    Cantalk() {
        return "Sorry, Can't talk";
    },
};

// Object User
let user = {
    CanCook: true,
    CanCode() {
        return "Can't Code";
    },
    //  Inheriting the properties and methods of candidate
    __proto__: candidate, 
};
// Printing on console the properties of candidate
console.log("Can a user fly: " + user.fly); // Output : Can a user fly: true
// Method of candidate
console.log("Can a user talk: " + user.Cantalk()); // Output: Can a user talk: Sorry, Can't talk
// Property of user
console.log("Can a user cook: " + user.CanCook); // Ouput : Can a user cook: true
// Method of user
console.log("Can a user code: " + user.CanCode()); // Output : Can a user code: Can't Code
```

---
title: Object Descriptors
description: All about Object Descriptors
duration: 300
card_type: cue_card
---

## 3. Object Descriptors

* In the Object.defineProperty() static method, a new property is defined directly on an object, or an existing property is modified, and the object is returned.
* An object's property descriptor consists of the following attributes:
    1. value: This is the value associated with the property
    2. writable: It indicates whether the property can be changed. If the property can be manipulated, it returns true
    3. enumerable: Returns true if the property is visible during enumeration of the properties of the corresponding object.
    4. Configurable: Indicates whether the property descriptor can be modified or removed.

**Example:**

```javascript
const object1 = {};
Object.defineProperty(object1, 'property1', {
  value: 25,
  writable: false,
});
object1.property1 = 100;
// Throws an error in strict mode
console.log(object1.property1);
// Expected output: 25
```


## Preventing : Reassignment, Create, update & delete 

Initially, we need to comprehend how does reassignment, creating, deleting, or updating a property works.

Here's an example:
```javascript!
let config = {
    appName: "scaler.com",
    database: {
        host: "127.0.0.1",
        name: "mainDB",
        user: "root",
        password: "admin"
    }
}
/**
 * mutation/changes we can do on an object ->
 * 1. reassign,
 * 2. create a prop
 * 3. update a property
 * 4. remove a property
 */

// reassign
//config=10;
console.log(config);

// create a new property
config.tempServer = "127.0.018";

// delete a property
delete config.database.password;

// update a property
config.appName = "InterviewBit.com";
```

**Ask the learners**
What if we need to prevent the modification of the object and its properties?

Let's look at some of the probable solutions that are actually not going to prevent the modification.
- `const`: **Instruction:** Change the `let` keyword to `const` for the variable config in the above code and check if the properties can be modified in any way.
- `const` does not prevent the modification. 
> Indeed, `const` only freezes the address of the object and not its properties.

So, there's should be an alternative. We can prevent extensions with the help of `Object.preventExtensions()`.

**Code:**
```javascript!
let config = {
    appName: "scaler.com",
    database: {
        host: "127.0.0.1",
        name: "mainDB",
        user: "root",
        password: "admin"
    }
}

let unextendableObject = Object.preventExtensions(config);
unextendableObject.tempServer = "127.0.0.18";
console.log(unextendableObject);
```
**Output:**
```
{
  appName: 'scaler.com',
  database: {
    host: '127.0.0.1',
    name: 'mainDB',
    user: 'root',
    password: 'admin'
  }
}
```

So, with `Object.preventExtensions()`, new properties cannot be added. However, you can still update or delete a property.


**The problem** with `Object.preventExtensions()` is that it would work only on the first level. Therefore, any new property can be created at the inner level.

Here's an example:
```javascript!
let config = {
    appName: "scaler.com",
    database: {
        host: "127.0.0.1",
        name: "mainDB",
        user: "root",
        password: "admin"
    }
}

let unextendableObject = Object.preventExtensions(config);
// new property cannot be added at the first level
unextendableObject.tempServer = "127.0.0.18";

// new properties can be introduced at the inner level
unextendableObject.database.newpwd = "fake";

// You can still update or delete a property, even at the first level
unextendableObject.appName = "interviewbit.com";
delete unextendableObject.database.password;

console.log(unextendableObject);
```

**Output:**
```
{
  appName: 'interviewbit.com',
  database: { host: '127.0.0.1', name: 'mainDB', user: 'root', newpwd: 'fake' }
}
```

In order to prevent any deep extensions, we can manage to use `Object.preventExtensions()` for a particular property of an object.

Here's an example for the password of the database:
```javascript!
let config = {
    appName: "scaler.com",
    database: {
        host: "127.0.0.1",
        name: "mainDB",
        user: "root",
        password: "admin"
    }
}

let unextendableObject = Object.preventExtensions(config);
// new property cannot be added at the first level
unextendableObject.database = Object.preventExtensions(unextendableObject.database);
unextendableObject.tempServer = "127.0.0.18";

// new properties cannot be introduced at the inner level
unextendableObject.database.newpwd = "fake";

console.log(unextendableObject);
```

**Output:**
```
{
  appName: 'scaler.com',
  database: {
    host: '127.0.0.1',
    name: 'mainDB',
    user: 'root',
    password: 'admin'
  }
}
```

We also have some other methods.
`Object.seal`: `Object.seal` allows you to seal an object, preventing new properties from being added or existing properties from being removed. However, it still allows you to modify the values of existing properties.

Let's see what happens when we user `Object.seal`:
```javascript
const config = {
    appName: "scaler.com",
    database: {
        host: "127.0.0.1",
        name: "mainDB",
        user: "root",
        password: "admin"
    },
    extra: 10
}

let unextendableObject = Object.seal(config);

unextendableObject.database = Object.seal(unextendableObject.database);
unextendableObject.tempServer = "127.0.0.18"; //cannot add new property
unextendableObject.database.newpwd = "fake"; //cannot add new property
delete unextendableObject.extra; //cannot delete any property
unextendableObject.appName = "interviewbit.com"; //existing properties can be modified
console.log(unextendableObject);
```

**Output:**
```
{
  appName: 'interviewbit.com',
  database: {
    host: '127.0.0.1',
    name: 'mainDB',
    user: 'root',
    password: 'admin'
  },
  extra: 10
}
```

With `Object.freeze`, you cannot update, delete, or add any property.

```
/**
 * mutation/changes we can do on an object ->
 * 1. reassign -> const,
 * 2. create a prop -> object.preventExtension
 * 3. update and create -> Object.seal
 * 4. prevent create, update, delete -> Object.freeze
 */