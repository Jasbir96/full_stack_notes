# Full Stack LLD: React-9: Testing ,Deployment and React Interview questions-1

---
title: Agenda of the lecture
description: What will be covered in the topic?

---

## Agenda
* How does React render in our browser?
* Custom React Render
* useState, useReducer [React]

---
title: How React Renders 
description: Understand various terminologies related to rendering

---

## How react Renders 
JSX is not understood by the browser so it is converted to a JS file
**JSX -> JS (transpilation)**
* Babel is a tool to convert -> JSX -> JS
* React: does the  conversion of your UI into a JSON object -> VDOM 

**Architectures: Fiber architecture**

1. Initial Render: VDOM is constructed
    * **VDOM**: virtual DOM: it is an in-memory representation of the original DOM of the browser

2. Update a few changes: update the state 
    * React will create another VDOM and then it will compare with the previous virtual DOM.
    * This process of comparing our old VDOM with the new virtual is Known as `Reconciliation`
    * React uses an algorithm known as the `Diffing algorithm` to compare your Old VDOM with your new VDOM.
    * It does all the comparisons based on the Time complexity of our Diffing Algorithm  -> O(n)
    * This gets the minimum number of changes that are required

3. Updating the Real DOM: 
    * It will be using those minimum set of changes and with those changes, it is going to 
    * update your real dom 

**Question asked in Interview**
Let's say we are given this VDOM and we are asked to convert it to a Real DOM

---
title: React Rendering
description: React rendering

---

## React Rendering
* We will create an HTML file called index.html to give a feel of the normal script file
* we will create a javascript file called `customRendrer.js`
* Inside this we will have a function `render` that takes an object as an input and this returns a finalHTML.
* Now we will have an event listener on the document to at least have the content loaded.
* Now we will use `querySelector` to get the root and we will call the render function and pass the object to the `wholeApp`.
* Next we will `appendChild(wholeApp)`
```javascript
document.addEventListener("DOMContentLoaded", function () {
    const rootElem = document.querySelector("#root");
    const wholeApp = render(obj);
    console.log("whole app", wholeApp);
    // rootElem.appendChild(wholeApp);
})
```
* Our render function takes an object and returns an HTML. This HTML can then be appended to the root.

---
title: Understanding Obj
description: Understanding the format of the object

---

## Understanding obj
* The first thing that we have is a `type` - here can we say the type is a custom component.
* It has two possibilities - It is a function that can accept an object as well as a prop and that prop can be used.

Here is an example of an object and we are supposed to convert this to HTML.

```javascript
const obj =
{
    type: 'div',
    
// or 
    
type: (props) => ({
    type: "button", props:
        { children: props.counter + "Clicks" }
}),
```
1. **type :** 
    * string: html element:  document.create element 
    * function: custom component: call that function and if we have props then pass that also

2. **props:**
    * always an object - will have children
    * values ->  strings  : normal attributes ex:  class, id -> setAttribute 
    * Children:
    * arrays 
can have value as a string 
can have value as a function : Custom element
can have an object : normal element
```javascript
const obj =
{
    type: 'div',
    props: {
        class: "container",
        children: [
            {
                type: 'h1', props: {
                    children: ' this is '
                }
            },
            {
                type: 'p', props: {
                    class: 'paragraph',
                    children: [
                        ' I am ',
                        {
                            type: (props) => ({
                                type: "button", props:
                                    { children: props.counter + "Clicks" }
                            }),
                            props: { counter: 1 }
                        }
                        ,
                        ' from'
                    ]
                }
            }
        ]
    }
}

/*** OUTPUT
 * <div   class="container">
 * <h1>this is </h1>
 * <p class ="paragraph">I am <button>1 clicks</button> from</p>
 * </div > 
 * */
```

* Go through each element and ask learners what will be the HTML output for this
* Gist - We have an element, its attribute, and content inside it.
* HTML has these three things and these are enough to represent an element
* HTML as a tag name, list of attributes, and content inside them.

---
title: Writing Code
description: Writing code for converting objects to HTML

---

## Writing Code for Converting Objects to HTML
* Firstly we will read the type of the object and check if it is a string
* If the condition is satisfied then we will createElement
* Let us check for a prop, we will loop through all the props. Now we will check for the type of the prop and check if it is a "children"
* Firstly, check if the children is an array if so, iterate through this array
* Now this array may contain a string or another object
* For string we will createTextNode and we will append this 
* else it can be a normal object - so will pass it to render and get a HTML and append it to the parent.
* If it's not a children then check if that is a string
* We will set this attribute and its value using setAttribute.
* Now if the object's type is not string and instead is a function, then we will get parameters from the props of the object.
* Next we will have to call a function and pass these parameters to it.
* so basically this returns an element.
* and then pass this element to render.
```javascript
function render(obj) {
    let element;
    // if your type is string -> It is normal element

    if (typeof obj.type === "string") {
        element = document.createElement(obj.type);
    }
    // if you custom component -> call that fn with props 

    if (typeof obj.type === "function") {
        const props = obj["props"];

        let elementObj = obj.type(props);
        console.log(elementObj)
        return render(elementObj);
    }
    const props = obj.props;

    for (let prop in props) {

        if (prop == "children") {
            const children = props[prop];
            let isArray = Array.isArray(children);
            console.log("isArray ", isArray);
            if (isArray) {
                for (let i = 0; i < children.length; i++) {
                    const innerChild = children[i];
                    if (typeof innerChild == "string") {
                        const textNode = document.createTextNode(innerChild);
                        element.appendChild(textNode);
                    } else {
                        const childElem = render(children[i]);
                        element.appendChild(childElem);
                    }
                }
            } else {
                const textNode = document.createTextNode(props[prop]);
                element.appendChild(textNode);
            }
        }
        else if (typeof props[prop] == "string") {
            element.setAttribute(prop, props[prop]);
        }




    }


    return element;
}

document.addEventListener("DOMContentLoaded", function () {
    const rootElem = document.querySelector("#root");
    const wholeApp = render(obj);
    console.log("whole app", wholeApp);
    // rootElem.appendChild(wholeApp);
})
```
* revise the entire code discussed.
* take time to code and explain this
* check how it is working and verify if it's acting as we are expecting it to work (test cases)
* Lastly, check if there are any issues

---
title: useState Example
description: where to use the useState variable

---

## useState Example
* Firstly create a button, on pressing the button the count should increment.
* This is the ideal usecase of the useState
* Now instead of waiting for the eventListener set a timer of 1000 milliseconds to increment the value of the count. 
* The value of the count doesn't increment
* Because the function executes but each time it takes the same `0` value of the variable.
* So to solve this thing we can rather than directly using the value of count in set count we can use a function inside `setCount`
```javascript
import React from 'react'

function Counter() {
    const [count, setCount] = useState(0);

    const handleCount = () => {
        setInterval(() => {
            // console.log(count)

            //It is always better to use you useState with cb  when you are updating 
            //With the help of a previous state
            setCount((count) => { return count + 1 });
            // setCount(count+1)
        }, 1000)
    }


    return (
        <>
            <h3>{count}</h3>
            <button onClick = {handleCount}>Increment</button>
        </>
    )
}

export default Counter
```

---
title: useReducer Example
description: What is the usecase of useReducer?

---

## useReducer Example
* useState is simple and can be used to set one type of set.
* Let's say we want to set the state for the form component it has several state entries
* We can use the setState but we will require a class object for it then
* It has only one usecase i.e. deal with complex state variable.
* First, we have to define an initial state
* It has the same syntax as redux
* We have defined two things a state and a function
* Let us say I have a simple counter component, increment, decrement, +5 and -5
* So we will require 4 states
* So we will use the useReducer which will take the initial state and all the possible state changes
* go to your handler, instead of changing a different state, just pass the state to be changed in dispatch.

```javascript
import React, { useReducer } from 'react';


/***
 * useReducer 
 * * have all the state management logic of a component in one place using reducer
 * * there is no quirk of prev state 
 * * you get only one dispatch function to make all the state mutation
 * */ 
// deal with complex state variables 
function CounterUseReducer() {
    const intialState = 0;
    // all the possible state mutation
    const reducer = (state, action) => {
        switch (action) {
            case "increment":
                return state + 1;
            case "decrement":
                return state - 1;
            case "IncrementByFive":
                return state + 5;
            case "DecrementByFive":
                return state - 5;
            default:
                return state;
        }
    }
    const [count, dispatch] = useReducer(reducer, intialState);
    return (
        <>
            <div  style = {{color:"white"}}> {count} </div>
            <button onClick = {() => { dispatch("increment") }}>Increment</button>
            <button onClick = {() => { dispatch("decrement") }}>Decrement</button>
            <button onClick = {() => { dispatch("IncrementByFive") }}> +five </button>
            <button onClick = {() => { dispatch("DecrementByFive") }}> -five </button>
        </>
    )
}

export default CounterUseReducer; 
```
### Features 
* have all the state management logic of a component in one place using a reducer
* There is no quirk of prev state 
* you get only one dispatch function to make all the state mutation

---
title: Complex example of useReducer
description: Example of Complex Reducer Function

---

## Complex example of useReducer
* Let's take an example of a form
* Let's have lots of components and a reset button
* first name, last name, number, password, etc.
* Usually we will create five useState functions but let's see how can we do it using the reducer function
* First is state and second is action
* In real life we need a lot of validations
* In the previous example, we were just calling the dispatch function but now we need to pass data as well.
* We will call a dispatch function `onChange`. It takes a type and a payload i.e. action
* We will have a switch case for the firstname input and other states.
* Add the rest of the remaining code, all actions, and components
```javascript
import React, { useReducer } from 'react'

function Form() {
    let initalState = {
        firstName: "",
        lastName: "",
      
    };
    const reducer = (state, action) => {
        switch (action.type) {
            case "firstNameInput":
                return { firstName: action.payload }
            case "lastNameInput":
                return { lastName: action.payload }
            case "clear":
                return {
                    firstName: "",
                    lastName: "",
                   
                }
            default:
                return state;
        }

    }
    const [formState, dispatch] = useReducer(reducer, initalState);
    return (
        <form >
            <input type = "text" className = "firstName" value = {formState.firstName} onChange = {(e) => {
                dispatch({ type: "firstNameInput", payload: e.target.value });
            }} />
            <input type = "text" className = "lastName" value = {formState.lastName}
                onChange = {(e) => {
                    dispatch({ type: "lastNameInput", payload: e.target.value });
                }}
            />
            <button onClick = {
                
                (e) => { 
                    e.preventDefault();
                    dispatch({ type: "clear" }) }}>Reset all fileds</button>

        </form>
    )
}

export default Form
```

---
title: Memoization
description: Memoization introduction

---

# Memoization

## Definition
- Memoization is a performance optimization technique that involves caching and reusing computed results in software development.

## useMemo
- Syntax:
  ```javascript
  const memoizedValue = useMemo(() => computeValue(dep1, dep2, ...), [dep1, dep2, ...]);
  ```
- `useMemo` is a hook in React used to memoize values.
- It takes a function for value computation and an array of dependencies. The value is recalculated only when dependencies change.

## useCallback
- Syntax:
  ```javascript
  const memoizedFunction = useCallback(() => someFunction(dep1, dep2, ...), [dep1, dep2, ...]);
  ```
- `useCallback` is a React hook for memoizing functions.
- It returns a memoized version of the function, and the function reference remains constant as long as the specified dependencies don't change.
