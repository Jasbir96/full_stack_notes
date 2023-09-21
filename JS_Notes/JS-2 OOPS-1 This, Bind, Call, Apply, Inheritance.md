
# Agenda
- Host and Native Object
- How this behaves in JS
- Chaining in JS
- JS inheritance and prototype
- bind, call and apply in JS
- use-case of call bind apply
- polyfills of call bind apply



---

# Host objects
Objects that are provided by the environment are known as host objects. Examples of host objects in the browser are window, document, local storage, etc. Examples of host objects in nodejs are global, os, process, etc. Host objects are dependent on the environment.

# Native objects
Objects that are provided by the language are known as host objects. Examples of native objects are date, JSON, etc.


---

# Example Code

Let us take an example of another small code:
```javascript
let firstVar = "created using let";
var secondVar = "created using var";
console.log("hello from", this);
```

- This code lies in the global section, as there is no function, so firstly execution context is created. 
- First hosting(allocation of memory for function and variables) happens, then gets a global object, in our case it is a window then next this is calculated. 
- For this execution context then we have the execution phase.

So let us assume we have a call stack.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/321/original/upload_6614f9c5d71c58588cf6f03f658c84d2.png?1695154328)

We have firstVar(FV) and secondVar(SV)
, and both are undefined initially.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/322/original/upload_fdfc65dca7151e6ba6c8f02bf8948bbc.png?1695154354)

And we will get a window object. As for the global execution context, this will be equal to the window.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/323/original/upload_7b0b167e03fc492d601d63974d1f41ee.png?1695154394)

Again for this, we will get a window.
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/324/original/upload_2b42174647c91274b49a799b68248894.png?1695154421)

Now next phase is code execution. firstVar(FV) and secondVar(SV) will be replaced by its value.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/325/original/upload_dabe4053b0169cfa85d755dc339b0467.png?1695154466)

Now output for log `this` is a window, as for global execution `this` is a window.

## Difference in the allocation of memory of let and var in the global execution context
In the global execution context, var will go to the global object. And let will go script object.
So for displaying secondVar we can write,
```javascript
console.log(secondVar)
```
OR
```javascript
console.log(window.secondVar)
```

In our case, the global object is a **window**.


## Javascript Code
```javascript
let cap = {
    // property
    firstName : "Steve",
    // method
    sayHi : function(){
        console.log("hi from", this.firstName);
    }
}
cap.sayHi();
let sayHiAdd = cap.sayHi;
sayHiAdd();
```

### Explanation
First of all, let us create a call stack:

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/326/original/upload_7f53ffd50dedab28fcda387e67513ba7.png?1695154573)

Inside the call stack, we have variables with undefined values.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/327/original/upload_ebda1f98b13245e10b2032496d5c299b.png?1695154601)


Now code execution phase, the first line is `cap.sayHi()`,  execution context for sayHi() will be created. Whenever the execution context is created we will get a window and now we have to determine this for it. And again here cap will be undefined.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/328/original/upload_a29e2628bbfe7b75afd9976e5eee1a9d.png?1695154649)

Now cap will get memory in the heap let us suppose it is 20k.
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/329/original/upload_bec1aaace89803ac78426fa7122013e2.png?1695154700)


**Whenever a method is invoked with an object like `cap.sayHi()`, then that object becomes this.**

So inside sayHi() when `cap.sayHi()` calls sayHi(), the value of this is `cap`.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/330/original/upload_bb384289e4d70074180d2052c38be166.png?1695154751)

So the execution of `cap.sayHi()`, simply prints **hi from Steve**.

Now when the `let sayHiAdd = cap.sayHi;`  statement is executed then we are calling `cap.sayHi` and storing it into `sayHiAdd`. So `sayHiAdd` will be having the address of `sayHi`.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/331/original/upload_8a9a4ba6c74a648696e23f1fec0b613e.png?1695154788)

After that `sayHiAdd()` statement is executed which will again create the execution context and it will get a window object and now we have to determine this for it.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/332/original/upload_163f9a7dffddaaa66379159f1d583d0a.png?1695154832)

**For normal function call, means function call without object this will be equal to the window object.**


Now this will again be a window object.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/333/original/upload_7ab16949c66d99e6a8f2f9e6e7a53f66.png?1695154873)

So now when `sayHiAdd()` is executed then this will be a window and when `console.log("hi from ",this.firstName);` is executed, then the window.firstName is required to be displayed but it is undefined. 
So will get **hi from undefined.**

### Output
hi from Steve
hi from undefined.

## Another Javascript Code
```javascript
let cap = {
    // property
    firstName : "Steve",
    // method
    sayHi : function(){
        console.log("hi from", this.firstName);
    }
}
cap.sayHi();
var firstName = "Loki"
let sayHiAdd = cap.sayHi;
sayHiAdd();
```


### Output
hi from Steve
hi from Loki.
**Explanation:**
As we have defined firstName as var and var will default goto window.

---
# Rules for this
- For Global Execution context this will be a window object.
- For Execution context created with method call(calling with object), this will be that object.
- For Execution context created with a function call(calling without object), this will be that window.


---

# Question
Output of following javascript code:
```javascript
let cap = {
    firstName : "Steve",
    sayHi : function(){
        console.log("53", this.firstName);
        const iAmInner = function(){
            console.log("55",this.firstName);
        }
        iAmInner();
    }
}
cap.sayHi();
```


## Question Explanation
- When `cap.sayHi()` calls `sayHi()`, then this will be a cap, as it is a method call(means calling a method with an object). So it will print **53 Steve**. 
- Now inside this when `iAmInner()` is called. Then this will be a window object as it is a function call. 
- Then it will be print **55 undefined.** As we don't have firstName in the window object.

### Output
53 Steve
55 undefined



---

# Question
Output of following javascript code:
```javascript
let cap = {
    firstName : "Steve",
    sayHi : function(){
        console.log("53", this.firstName);
        const iAmInner = () => {
            console.log("55",this.firstName);
        }
        iAmInner();
    }
}
cap.sayHi();
```


## Question Explanation
- When `cap.sayHi()` calls `sayHi()`, then this will be a cap, as it is a method call(means calling a method with an object). So it will print **53 Steve**. 
- Arrow function does not have its own this. And arrow function uses this from outside. 
- **Arrow function does not follow these rules, it takes this from outside:**
    - GEC -> this -> window object
    - EC is created with -> method call -> this will be that object
    - EC is created with -> function call -> this will be that window.

So when `iAmInner()` is called, as it is an arrow function it will take from outside so then this becomes `cap`, that's why **55 Steve** will be printed.

### Output
53 Steve
55 Steve



---
# Question
Output of following javascript code:
```javascript
var firstName = "Loki"
let cap = {
    firstName : "Steve",
    sayHi : () => {
        console.log("53", this.firstName);
        const iAmInner = () => {
            console.log("55", this.firstName);
        }
        iAmInner();
    }
}
cap.sayHi();
```


## Question Explanation
- `iAmInner()` does not have its own this. So it will go to `sayHi()` to get their this, and `sayHi()` is also an arrow function so it will also not have its own this.
-  So then it will go to the global execution context, GEC has a window object which has `window.firstName="Loki"`, so this will become a window for both `sayHi()` and `iAmInner()`.

We have hierarchy
```javascript
gec has a windows object
gec{
    sayHi => (){
        iAmInner => (){
            
        }
    }
}
```

### Output
53 Loki
55 Loki




---



# Question
Output of following javascript code:
```javascript
var firstName = "Loki"
let cap = {
    firstName : "Steve",
    sayHi : function(){
        console.log("53", this.firstName);
        const subInner = () => {
            console.log("54", this.firstName);
            const iAmInner = ()=> {
                console.log("55", this.firstName);
            }
            iAmInner();
        }
        subInner();
    }
}
cap.sayHi();
```


## Question Explanation
- `cap.sayHi()` is executed, it has a cap as this, now it will print **53 Steve**, inside this `subInner()` is called, but it is an arrow function so it doesn't have its own this so it will take cap as this, so it will also print **53 Steve**, now again inside this `iAmInner()` is called, now again it is an arrow function so it doesn't have its own this. 
- So it will `subInner()` for this, but it is also an arrow function so it will also not have its own this, so it will go to `sayHi()`, it has `cap` as this, so `iAmInner()` take `cap` as this and it will also print **53 Steve**.


### Output
53 Steve
53 Steve
53 Steve

---
# Behaviour of this
|                |      non-strict       |        strict         |
|:--------------:|:---------------------:|:---------------------:|
|      GEC       |     Window Object     |        window         |
| Function call  |     Window Object     |       undefined       |
|  Method call   |    Current object     |    current object     |
| Arrow function | this from outer scope | this from outer scope |

In **strict mode**, we can not use any variable without declaration.

```javascript
"use strict";
let cap = {
    // property
    firstName: "Steve",
    // method
    sayHi : function(){
        console.log("hi from", this.firstName);
    }
}
cap.sayHi();
var firstName = "Loki"
let sayHiAdd = cap.sayHi;
sayHiAdd();
```

This code will first print **hi from Steve**, as in case of the method call in strict mode this will be a current object, but when `sayHiAdd()` is executed then it will give an error, as in this case **this is undefined** in strict mode, so it will not able to read properties of this.


---


# Output of javascript code
```javascript
let ladder = {
    stop:0,
    up(){
        this.stop ++ ;
    },
    down(){
        this.stop -- ;
    }
    showStep: function(){
        console.log(this.stop);
    }
}
ladder.up();
ladder.up();
ladder.up();
ladder.down();
ladder.showStep();
```

## Output
2


## Explanation
We are calling `up()`, `down()` and `showStep()` on ladder object.

Now in place of
```javascript
ladder.up();
ladder.up();
ladder.up();
ladder.down();
ladder.showStep();
```
we want to write like this`ladder.up().up().up().down().showStep()`, So for that, we need to return an object of the ladder from every function, we will return this. So we will need to write the below code.

```javascript
let ladder = {
    stop:0,
    up(){
        this.stop ++ ;
        return this;
    },
    down(){
        this.stop -- ;
        return this;
    }
    showStep: function(){
        console.log(this.stop);
        return this;
    }
}
ladder.up().up().up().down().showStep()
```

**Output:**
2

---


# Inheritance : High Level Intro
- Inheritance is related to OOPS. All OOPS languages are Java-based.
- Because we need a class to create an object just like for creating a house we need the architecture of the house.
- Suppose we have architecture having 1 door and 2 rooms. Now we want to create a new house with this architecture along with it we also want a swimming pool.
- So we will inherit all the properties of the old architecture and add a swimming pool to it.
- Then we will create a house with that new architecture.


---
title: Prototypal OOPS
description: 
duration: 300
card_type: cue_card
---

# Prototypal OOPS
- We have a base object and let us take an example we have a base model of a Tata company that works as a prototype.
- Now a child object inherits properties from a base object just like the mescom car is made by inheriting properties from Tata company with one new feature.
- Now another object is created by inheriting properties from the child object which becomes the grandchildren of the base object, just like safari is made by inheriting properties from mexcom by adding a new feature of 8 seater.

Prototypal OOPS is we will create a base object, and directly inherit properties from it to create a new object.

But in Java, we have to make a class then another class inherit the properties of that class, and then we will create an object but in Prototypal OOPS we will directly inherit from the object.


---

# OOPS followed by JS
JS follows prototypal OOPS. 
- We have a parent object from which we can create a child object.

```javascript
let arr = [1, 2, 3, 4];
console.log(typeof arr); //print object
```

- As the type of arr is an object, it has some parent object also. 
- arr has parent Array.
-  Array has all the methods required by every array. Parent of Array of Object.


So everything starts from an Object, and it has children like `Array` which will inherit all the properties of `Object`, and it can have its property. Now when we create an array `[]`, then it has all the properties of its parent i.e. `Array` and grandparent i.e. `Object`. To verify it will try to access any method of `Object` i.e. grandparent by array. We have a `toString()` method of object class.

```javascript
let arr = [1, 2, 3, 4];
console.log(arr.toString()); // prints 1,2,3,4
```

---

# Advantages of Inheritance
- Reuse of code
- Multiple child objects can access the single method of parent.
- Save memory space


If we want to add any method in the object that will be available for children, then we use `prototype` to add it:
```javascript
Array.prototype.sum =  function(){
    let sum = 0;
    for(let i = 0; i < this.length ; i ++ ){
        sum = sum + this[i];
    }
    console.log(sum);
}

arr = [1, 2, 3, 4]
arr2 = [3, 4, 1, 8, 9]
arr.sum()
arr2.sum()
```

Now we have added a method in the parent object using `prototype`, so it can accessed by both children.



---

# Bind call and apply

## call 
Borrowing a function from the object and can be used for another object without actually adding to it. 
```javascript
let cap = {
    name: "Steve",
    team: "cap",
    petersTeam: function(mem1, mem2){
        console.log(`Hey ${this.name} am your neighborhood spiderman and I belong to ${this.team}'s team`);
        console.log(`I am working with ${mem1} & ${mem2} `);
    }
}
let ironMan = {
    name: "Tony", 
    team: "iron man"
}

//  borrowing a function and can use it for another object without actually adding into it. 
//We can also pass parameters for that function.
cap.petersTeam.call(ironMan, "natasha", "war machine");


```

## apply
- Apply is used to borrow function for n number of parameters.
- The only difference between call and apply is that we will pass parameters in the form of an array in apply.
- When we are not aware of a number of parameters then we use `apply`.
```javascript
let cap = {
    name: "Steve",
    team:"cap",
    petersTeam: function(mem1, mem2, ...otherMan){
        console.log(`Hey ${this.name} am your neighborhood spiderman and I belong to ${this.team}'s team`);
        console.log(`I am working with ${mem1} & ${mem2} with ${otherMan}`);
    }
}
let ironMan = {
    name: "Tony", 
    team: "iron man"
}

// apply
cap.petersTeam.apply(ironMan, ["natasha", "war machine", "doctor strange", "loki"])
```


## bind
- Copies the function that you call later with the same this.
- When we want to use the method multiple times.
```javascript
let cap = {
    name: "Steve",
    team: "cap",
    petersTeam: function(mem1, mem2, ...otherMan){
        console.log(`Hey ${this.name} am your neighborhood spiderman and I belong to ${this.team}'s team`);
        console.log(`I am working with ${mem1} & ${mem2} with ${otherMan}`);
    }
}
let ironMan = {
    name: "Tony", 
    team: "iron man"
}


// bind
let ironManStolenMan =  cap.petersTeam.bind(ironMan);
ironManStolenMan("natasha", "war machine", "doctor strange")
ironManStolenMan("natasha", "war machine")
```

# Use case of bind, apply and call
- Borrow features.

## call
We use `call` when we want to borrow a method only once with a defined number of parameters.

**Example:**
```javascript
let cap = {
    name: "Steve",
    team: "cap",
    petersTeam: function(mem1, mem2){
        console.log(`Hey ${this.name} am your neighborhood spiderman and I belong to ${this.team}'s team with members ${mem1} and ${mem2}`);
    }
}
let ironMan = {
    name: "Tony", 
    team: "iron man"
}

cap.petersTeam.call(ironMan, "thor", "loki");
```

## apply
We use `apply` when we want to borrow a method only once with n number of parameters.

**Example:**
```javascript
cap.petersTeam.apply(ironMan, ["thor", "loki"]);
```

## bind
We use `bind` when we want to use a method multiple times and we want to make a permanent copy of that method.

**Example:**
```javascript
const boundFn = cap.petersTeam.bind(ironMan);
boundFn("thor", "loki");
boundFn("cap", "war machines");
```

---

# Polyfills of bind, apply and call
- Every array is built using `Array`.
- Every object is built using `Object`.
- Every function has a parent `Function`.

`cap.petersTeam.call()`, here `call` is on function.
- `call`, `bind`, and `apply` is on function.
- `this` in `call`, `bind`, and `apply` is a function.
- `call`, `bind`, and `apply` receive an object on which this function is to be called.

## Polyfills of call

- `call` is used to call a function as a part of the object that was defined in another object.
- We will receive a function to be invoked as `this`.
- The object on which the function is to be invoked is received as a parameter.

**Example:**
```javascript
let cap = {
    name: "Steve",
    team: "cap",
    petersTeam: function(mem1, mem2){
        console.log(`Hey ${this.name} am your neighborhood spiderman and I belong to ${this.team}'s team with members ${mem1} and ${mem2}`);
    }
}
let ironMan = {
    name: "Tony", 
    team: "iron man"
}
// building a polyfill of call
//Here we will receive an object as a parameter on which function to be called
function.prototype.myCall = function(objOnWhichReqFnToBeInvoked){
    //Here this is cap.petersTeam
    // console.log("value of this:", this);
    // required function to be invoked is in this
    let requiredFn = this;
    // adding requiredFn(cap.petersTeam) in the object(ironMan) on which we want to call the requiredFn(cap.petersTeam)
    objOnWhichReqFnToBeInvoked.requiredFn = requiredFn;
    // calling function with that object ironMan
    objOnWhichReqFnToBeInvoked.requiredFn();
    // deleted added function from the object ironMan
    delete objOnWhichReqFnToBeInvoked.requiredFn;
}
cap.petersTeam.myCall(ironMan);
```

**If we want to pass parameters to the function to be called**
Then simply we need to pass it as a rest parameter in `myCall` so that it can accept n number of parameters.

**Example:**
```javascript
let cap = {
    name: "Steve",
    team: "cap",
    petersTeam: function(mem1, mem2){
        console.log(`Hey ${this.name} am your neighborhood spiderman and I belong to ${this.team}'s team with members ${mem1} and ${mem2}`);
    }
}
let ironMan = {
    name: "Tony", 
    team: "iron man"
}
// building a polyfill of call
//Here we will receive an object as a parameter on which function to be called
function.prototype.myCall = function(objOnWhichReqFnToBeInvoked, ...args){
    //Here this is cap.petersTeam
    // console.log("value of this:", this);
    // required function to be invoked is in this
    let requiredFn = this;
    // adding requiredFn(cap.petersTeam) in the object(ironMan) on which we want to call the requiredFn(cap.petersTeam)
    objOnWhichReqFnToBeInvoked.requiredFn = requiredFn;
    // calling a function with that object ironMan
    objOnWhichReqFnToBeInvoked.requiredFn(...args);
    // deleted added function from the object ironMan
    delete objOnWhichReqFnToBeInvoked.requiredFn;
}
cap.petersTeam.myCall(ironMan, "loki", "thor");
```


## Polyfills of apply

- Polyfills of `apply` is created similar to `call`, we will array as a single argument.
- And then spread it while calling the function

**Example:**
```javascript
let cap = {
    name: "Steve",
    team: "cap",
    petersTeam: function(mem1, mem2){
        console.log(`Hey ${this.name} am your neighborhood spiderman and I belong to ${this.team}'s team with members ${mem1} and ${mem2}`);
    }
}
let ironMan = {
    name: "Tony", 
    team: "iron man"
}
// building a polyfill of apply
function.prototype.myApply = function(objOnWhichReqFnToBeInvoked, args){
     let requiredFn = this;
    objOnWhichReqFnToBeInvoked.requiredFn = requiredFn;
    objOnWhichReqFnToBeInvoked.requiredFn(...args);
    
    delete objOnWhichReqFnToBeInvoked.requiredFn;
}
cap.petersTeam.myApply(ironMan,["Loki", "Thor"]);
```


## Polyfills of bind

- Polyfills of `bind` can be easily built using `call` or `apply`.

**Example:**
```javascript
let cap = {
    name: "Steve",
    team: "cap",
    petersTeam: function(mem1, mem2){
        console.log(`Hey ${this.name} am your neighborhood spiderman and I belong to ${this.team}'s team with members ${mem1} and ${mem2}`);
    }
}
let ironMan = {
    name: "Tony", 
    team: "iron man"
}

//Building a polyfill of bind
function.prototype.myBind = function(objOnWhichReqFnToBeInvoked){
    const requiredFn = this;
    return function(...args){
        requiredFn.call(objOnWhichReqFnToBeInvoked,...args);
    }
    
}
const boundFn = cap.petersTeam.myBind(ironMan);
boundFn("loki", "thor");
```

---
