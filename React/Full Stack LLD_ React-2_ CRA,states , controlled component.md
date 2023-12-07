# Full Stack LLD: React-2: CRA,states , controlled component

---
title: Agenda of the lecture
description: What will be covered in the topic?

---

## Agenda


We will try to cover most of these topics in today's sessions and the remaining in the next.
- Setup dev env using vite
- State and dynamic UI
- Forms in React: controlled components
- Thought process to create a react components
- Lifting the state up


let's start.

---
title: useState  case-1 Counter
description: explaining useState with counter example

---

## useState  case-1 Counter

Let's understand how we can create a dynamic application:
So the easiest example for that is a simple counter component. Intially we are not using any Create-React-App because you should be clear with the concept first.

What are the use cases of react(algorithm), react-dom(library actually changes the UI) and Bable (transpiler converts JSX to JS).


So we are building the counter which consist of two two buttons like +, - and the counter in between as shown below.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/528/original/upload_74967057b5fb9e6075fe281955ae0b99.png?1699637012)


- So while developing anything in the react first step includes Create static UI.
- Second is always add all the required event listener
- Third is to add the sate. 


```jsx
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
    <!-- algorithm to efficiently change your UI -->
    <script src = "https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
    <!-- React DOM  -> uses react to update the DOM-->
    <script src = "https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
    <!-- transpiler -> JSX -> into javascript -->
    <script src = "https://unpkg.com/babel-standalone@6/babel.min.js"></script>
</head>

<body>

    <div id = "root"></div>
    <script type = "text/babel">
        function Counter(){
        //useState ->initial value
        const [count,setCount] = React.useState(0);
        //const count=arr[0];
        //call this fn given by useState->
        //const updateCount=arr[1];
        //const count=10;
        
        const increment = () => {
            //tell react -> update count by 1
            console.log("Incremented");
            //setCount(1000);
            setCount(count + 1);
        }
        const decrement = () => {
            //tell react -> update count by -1
            console.log("Decremented");
            setCount(count - 1);
        }
        console.log("rendered");
        return <div>
        <button onClick = {increment}> + </button>
        <p>{count}</p>
        <button onClick = {decrement}> - </button>
        </div>
        }
        ReactDOM.render(<Counter></Counter>, document.getElementById("root"));
    </script>

</body>

</html>
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/530/original/upload_6cf147f641c589e19ef8ba1552466dca.png?1699637277)

- The code defines a React component called "Counter," responsible for managing and displaying a count.
- React's `useState` hook is used to manage the state of the "count" variable. It initializes "count" to 0 and provides a function "setCount" for updating it.
- The component includes two functions, "increment" and "decrement," which are triggered when the respective buttons are clicked. They are responsible for updating the "count" state.
- The `render` method of the "Counter" component returns JSX elements, including buttons for incrementing and decrementing the count, and a paragraph displaying the current count value.
- The last line of code uses `ReactDOM.render` to render the "Counter" component inside the HTML element with the ID "root."
- When the setCount gets called first it upadtes the count state variable and then call the component again called as rerendering.
- `arr[0]` is state variable and the `arr[1]` is the function to update state variable.


---
title: useState case-2 multiple counters
description: showing the power of components with useState

---

## useState case-2 Multiple Counters

Now what should be done to implement the increment and decrement function?
Simply use the `setCount(count+1)` to increment and `setCount(count-1)` to decrement.

What changes will we make in the above code implement the multiple counter as shown below:

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/531/original/upload_5a86ac1a9a0b3b6f220de2d1201c8dca.png?1699637361)


```jsx
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
    <!-- algorithm to efficiently change your UI -->
    <script src = "https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
    <!-- React DOM  -> uses react to update the DOM-->
    <script src = "https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
    <!-- transpiler -> JSX -> into javascript -->
    <script src = "https://unpkg.com/babel-standalone@6/babel.min.js"></script>
    <style>
        .counter{
            margin-top:2rem;
            border-bottom:2px solid gray;
            padding-bottom:1rem;
        }
    </style>
</head>

<body>

    <div id = "root"></div>
    <script type = "text/babel">
        function Counter(){
        const [count,setCount] = React.useState(0);
        const increment = () => {
            setCount(count + 1);
        }
        const decrement = () => {
            setCount(count + 1);
        }
        console.log("rendered");
        return <div>
        <button onClick = {increment}> + </button>
        <p>{count}</p>
        <button onClick = {decrement}> - </button>
        </div>
        }
        
        fucntion ParrentCounter(){
            return <div>
                <Counter/>
                <Counter/>                
                <Counter/>
        </div>
        }
        ReactDOM.render(<ParrentCounter></ParrentCounter>, document.getElementById("root"));
    </script>

</body>

</html>
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/532/original/upload_35b7ced644f94a97e95e28c1851d5a4d.png?1699637472)

- The "increment" and "decrement" functions in the "Counter" component update the count state when the respective buttons are clicked.
- The "ParentCounter" component is defined to render multiple instances of the "Counter" component, creating a parent-child relationship.
- `ReactDOM.render` is used to render the "ParentCounter" component inside the HTML element with the ID "root." This sets up the initial rendering of the application.

The code creates a React application that consists of a "Counter" component for counting and a "ParentCounter" component that renders multiple "Counter" components. Users can interact with each "Counter" independently to increment and decrement the count value. The application is rendered inside an HTML element with the ID "root."

---
title: useState case-3 counter with custom start 
description: how props and states can be used to give custome behaviour to components.

---

## useState case-3 Counter with Custom Start

**How we can send the send the custom start values for the counters?**

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/533/original/upload_382ed2a67dac4c0e3d34917d4fc6f865.png?1699637521)

We can simply pass the Props, 

```jsx
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
    <!-- algorithm to efficiently change your UI -->
    <script src = "https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
    <!-- React DOM  -> uses react to update the DOM-->
    <script src = "https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
    <!-- transpiler -> JSX -> into javascript -->
    <script src = "https://unpkg.com/babel-standalone@6/babel.min.js"></script>
    <style>
        .counter{
            margin-top:2rem;
            border-bottom:2px solid gray;
            padding-bottom:1rem;
        }
    </style>
</head>

<body>

    <div id = "root"></div>
    <script type = "text/babel">
        function Counter(props){
        const {index,value} = props;
        const [count,setCount] = React.useState(value);
        const increment = () => {
            setCount(count + 1);
        }
        const decrement = () => {
            setCount(count + 1);
        }
        console.log("rendered");
        return <div>
        <h2>Counter Number:{index} </h2>
        <button onClick = {increment}> + </button>
        <p>{count}</p>
        <button onClick = {decrement}> - </button>
        </div>
        }
        
        fucntion ParrentCounter(){
            return <div>
                <Counter index = {1} value = {2}/>
                <Counter index = {2} value = {3}/>       
                <Counter index = {3} value = {4}/>
        </div>
        }
        ReactDOM.render(<ParrentCounter></ParrentCounter>, document.getElementById("root"));
    </script>

</body>

</html>
```

- You can create the complex component by using the multiple independent component.
- usecase of state wrt to a component: to make it dynamic
- Props: to pass the data to the children component. Majorly the data specific to that component. 

---
title: controlled component
description: using inputs to show what controlled components are 

---

## Controlled Component

Now lets see how we will implement the below mentioned functionalities also:

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/536/original/upload_4fd1aec46579e39c12ccbb3ca41d5e1a.png?1699638261)

```jsx
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
    <!-- algorithm to efficiently change your UI -->
    <script src = "https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
    <!-- React DOM  -> uses react to update the DOM-->
    <script src = "https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
    <!-- transpiler -> JSX -> into javascript -->
    <script src = "https://unpkg.com/babel-standalone@6/babel.min.js"></script>
    <style>
        .set {
            border-bottom:1px solid;
            padding-bottom:1rem;
        }
    </style>
</head>

<body>

    <div id = "root"></div>
    <script type = "text/babel">
        function InputCounter(props){
        const {index,value} = props;
        const count,setCount] = React.useState(value);

        const increment = () => {
            setCount(count + 1);
        }
        const decrement = () => {
            setCount(count + 1);
        }
        console.log("rendered");
        
        return <div>
        <div class = "set"> 
            <input type = "text"/>
            <button>Set</button>
        </div>
        <h2>Counter Number:{index} </h2>
        <button onClick = {increment}> + </button>
        <p>{count}</p>
        <button onClick = {decrement}> - </button>
        </div>
        }
        ReactDOM.render(<InputCounter></InputCounter>, document.getElementById("root"));
    </script>

</body>

</html>
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/537/original/upload_47f0b9076ff1e66b17607cc20f44d0d9.png?1699638551)

Whenever you have any data that changes on your UI you should not be using like this, as we not write anything around the input. In react it is always a good practise that your UI should be change using your state variable. It means that count should be changed using the useState variable, this input also is achnging without the will of the react that is not a good practise.

So we can use the below recommended changes in the above code as follows:


```jsx
<body>

    <div id = "root"></div>
    <script type = "text/babel">
        function InputCounter(props){
        const {index,value} = props;
        const count,setCount] = React.useState(value);
        
        const [value,setValue] = React.useState("");
        
        const increment = () => {
            setCount(count + 1);
        }
        const decrement = () => {
            setCount(count + 1);
        }
        const handleValue = {event} => {
        console.log("e.target.val", event.target.value);
            setValue(event.target.value);
        }
        console.log("rendered");
        
        return <div>
        <div class = "set"> 
            <input type = "text" value = {value} onChange = {handleValue}/>
            <button>Set</button>
        </div>
        <h2>Counter Number:{index} </h2>
        <button onClick = {increment}> + </button>
        <p>{count}</p>
        <button onClick = {decrement}> - </button>
        </div>
        }
        ReactDOM.render(<InputCounter></InputCounter>, document.getElementById("root"));
    </script>

</body>

</html>
```



This is way how you should handle the incoming input value in react:
* Start with input tag.
* Create a state variable. 
* Go with onChange and handleValue.


* When we talk about html elements like inputs its UI manipulation is handled by html only.
* When we are controlling the UI change of elements like input using state then these components are known as control components.

---
title: Build Tools (Vite)
description: react project setup using vite and explaining the react project structure 

---

## Build Tools (Vite)

Generally we use the HTML + CDN links + Live server.But in the production we can't work like this so here are the some tools provided like:

1. Build Tools
    1.Linters
    2.Hot Reload
    3.Build App

We can use Create-React_App or the Vite. But here we will be going ahead with using the` vite`.

To set up the vite follow the below commands:
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/546/original/upload_f59c710a13b46f219ee16b2edb3bb257.png?1699639074)

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/547/original/upload_09eef56f60db21284a5d59202048ffca.png?1699639097)


**Thought**: Was the react was developed for the above simple application of counters, can't you build these things using the normal html,css and javascript. No, there are bigger websites like linkedin, facebook problems were there that is why the react was introduced. So you cannot depend on these html, css beacause they are not even ideal for creating bigger application.

React is used in the bigger application and even a small component requires a lot of moving things  so beacause of this you need a proper structure. Basically tools like Vite is a complete package which provides a lot of features and the main feature includes structure in which everything works.

Before going ahead lets explore the structure of Vite which includes following:
1. **Package.json:**<br> which include react, react-dom, and other libraries.
2. **Node_Modules:**<br> various libraries and tools like bable, eslint, nodelib etc are present.
3. **index.html:**<br> in this the div is present ad rollup is done in this.
4. **Src Folder:**<br> all the code would be written in the src folder.
5. In the main.jsx the ReactDom.render is called by passing the component.
6. In the app.jsx the actual code is written.


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/552/original/upload_f7556fa3322939b6c03fbd467dafb472.png?1699639150)

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/555/original/upload_ffa124d02dff6e40ba67e96c28746912.png?1699639270)

---
title: Basic TODO application in React
description: Discussion of  various concepts related to the todo application in react in detail.

---

## Basic TODO Application in React

Now lets start creating the TO DO application:

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/557/original/upload_ed009d4550e361bf5c9455e52ffcf5b8.png?1699639337)

In the above area the input box which consist of text are for the input and a button. In the below part there are the added tasks. so mainly there are two components in this:

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/558/original/upload_c15225a3aa7d2b6c2ca7e94fdebb3a70.png?1699639436)

Now lets start with the App.jsx file as below:

A **React fragment** is a lightweight syntax feature in React that allows you to group multiple elements or components without adding extra nodes to the HTML output. It's primarily used when you want to return adjacent JSX elements without a wrapping container.


We will be following the main steps as follows:
1. Create static HTML
2. Add Eventlistener
3. Identify dynamic data (task and list of tasks)
4. Divide them intp independent component
5. For common state-> lift that state to parent
6. Pass the handler function and state as props.

```javascript
// rfce
import React, { useState } from 'react';
import InputBox from './inputBox';
import List from './List';

function Todo() {
    const [tasksArr, setTasks] = useState([]);
    const addTask = (inputValue) => {
        const newTask = inputValue;
        // we will never mutate  a state variable on our own 
        let newTaskArr = [...tasksArr, newTask];
        setTasks(newTaskArr);
    }
    return (
        // react Fragments
        <>
            <InputBox addTask = {addTask}></InputBox>
            <List tasksArr = {tasksArr}
                handleDelete = {handleDelete}
            ></List>
        </>
    )
}

export default Todo
```
In the inputbox.jsx we have:

```javascript
import React,{useState} from 'react'

function InputBox(props) {
    const [inputValue, setValue] = useState("");
    const handleInput = (e) => {
        setValue(e.target.value);
    }
    const addTaskChild = () => {
        props.addTask(inputValue);
        setValue("");
    }
    // we have provide acces to taskArr -> task ARR
    return (
        <div className = "inputbox">
            <input type = "text" value = {inputValue}
                onChange = {handleInput} />

            <button onClick = {addTaskChild}>Add Task</button>
        </div>
    )
}

export default InputBox;
```

In the List.jsx we have:

```javascript
import React from 'react';

function List(props) {
    const { tasksArr ,
        handleDelete
    } = props;
    return (
        <div className = "list">
            {/* list */}
            <ul>
                {tasksArr.map((task, idx) => {
                    return <li
                        onClick = {
                    () => {handleDelete(idx) }
                        }
                        key = {idx} > {task}</li>
                })}
            </ul>
        </div>
    )
}

export default List
```


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/560/original/upload_83e2f0587b460a02ad1f4f3d5c8da38c.png?1699639541)


- Inside the `Todo` component, there's a state variable `tasksArr` and a function `setTasks` to manage an array of tasks.
- The `addTask` function is called when a new task is added. It takes an `inputValue` as a parameter and creates a new task with it. It avoids mutating the state variable directly by creating a new array `newTaskArr` using the spread operator. The new array is set as the state using `setTasks`.
- It uses a React Fragment `<>` to wrap the JSX elements without introducing extra HTML nodes.
- Inside the fragment, it renders two child components:
     - `InputBox` component for adding tasks, passing the `addTask` function as a prop.
     - `List` component for displaying the list of tasks and handling deletions, passing the `tasksArr` and `handleDelete` function as props.

In the "Todo" component, two props are used:

1. **addTask**:<br> This prop is passed to the "InputBox" component and allows users to add new tasks. It's a function that takes the input value of the task and updates the list of tasks.

2. **tasksArr and handleDelete**:<br> These props are passed to the "List" component. "tasksArr" is an array that holds the list of tasks, and "handleDelete" is a function for removing tasks. "List" uses these props to display the tasks and provide the functionality to delete them.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/561/original/upload_e309f5183906703fcbed940f33d72348.png?1699639583)

- Common state lifted upto the parent.


---
title: Refactoring our Code
description: Coding with components in Reach

---

## Refactoring our Code
* Create a folder named Components and inside it create another folder TODO. Always create a components folder.
* Install an extension ES7
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/563/original/upload_25ea71cdc92240180ad8643c873f324b.png?1699640034)
* Create 3 files 
    * Todo.jsx
    * List.jsx
    * inputBox.jsx
* Always start the component with UPPERCASE.
* Just do `rfce + TAB` and a basic outline will be written.
**Todo.jsx**
```jsx
// rfce
import React, { useState } from 'react';
import InputBox from './inputBox';
import List from './List';

function Todo() {
    const [tasksArr, setTasks] = useState([]);
    const addTask = (inputValue) => {
        const newTask = inputValue;
        //We will never mutate  a state variable on our own 
        let newTaskArr = [...tasksArr, newTask];
        setTasks(newTaskArr);
    }

    return (
        // react Fragments
        <>
            <InputBox addTask = {addTask}></InputBox>
            <List tasksArr = {tasksArr}></List>
        </>
    )
}

export default Todo
```
**List.jsx**
```jsx
import React from 'react';

function List(props) {
    const { tasksArr ,
        handleDelete
    } = props;
    return (
        <div className = "list">
            {/* list */}
            <ul>
                {tasksArr.map(task) => {
                    return <li>{task}</li>
                })}
            </ul>
        </div>
    )
}

export default List
```
**InputBox.jsx**
```jsx
import React,{useState} from 'react'

function InputBox(props) {
    const [inputValue, setValue] = useState("");
    const handleInput = (e) => {
        setValue(e.target.value);
    }
    const addTaskChild = () => {
        props.addTask(inputValue);
        setValue("");
    }
    //We have provided access to taskArr -> task ARR
    return (
        <div className = "inputbox">
            <input type = "text" value = {inputValue}
                onChange = {handleInput} />

            <button onClick = {addTaskChild}>Add Task</button>
        </div>
    )
}

export default InputBox;
```

**App.jsx**

```jsx
import { useState } from 'react'
import Todo from './components/Todo/Todo';
function App() {
    return (
        <Todo></Todo>
    )
}
export default App;
```
**Output:**
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/566/original/upload_0bef09d0d84c74f3d2b5e4060dd57071.png?1699640145)

### Code explanation
* We are using React Use states in `InputBox.jsx`
* In `List.jsx` we are just passing the props
* `Todo.jsx` is importing everything
* To automatically do import press `ctrl + space`
* In `App.jsx`, just a file to put a high-level component.
* Run the code on the website

---
title: Delete Component
description: Adding a feature to delete a task 

---

## Delete Component 
* **Delete Feature:**<br> Delete feature deletes an element from the list. As the user clicks on any of the list item it should be deleted from the UI as well as from the list.
* **Delete Feature Logic:**<br> We can have an event listener on the list items. As the user clicks on the element, the listner should call a funciton `handleDelete`.
* This constant `handleDelete` funciton that will take the index of the elemenet that is clicked and delete the element from the TODO List.
* This is parent to child and then child to parent relation.
* On click we have to call `handleDelete` in the List element.

**Gist:**
* To lift the common state
    * When you want to send the data to the child pass the array
    * When you want the child to talk to the parent pass the function.

### Delete Feature Implementation
* We will use const `filterTasks` and inside it `filter` function to filter out the index passed. We are comparing this index with each element in the list. If the index doesn't match then only we add it to the filtered list and we set this list to the initial list. Thus, the element with the matching index is removed.

**Todo.jsx**
```jsx
// rfce
import React, { useState } from 'react';
import InputBox from './inputBox';
import List from './List';

function Todo() {
    const [tasksArr, setTasks] = useState([]);
    const addTask = (inputValue) => {
        const newTask = inputValue;
        //We will never mutate  a state variable on our own 
        let newTaskArr = [...tasksArr, newTask];
        setTasks(newTaskArr);
    }
    const handleDelete = (idx) => {
        // console.log("handle Delete",idx);
        const filteredTasks = tasksArr.filter((task, cIdx) => {
            return cIdx != idx;
        })
        setTasks(filteredTasks);
    }

    return (
        // react Fragments
        <>
            <InputBox addTask = {addTask}></InputBox>
            <List tasksArr = {tasksArr}
                handleDelete = {handleDelete}
            ></List>
        </>
    )
}

export default Todo
```

**List.jsx**

```jsx
import React from 'react';

function List(props) {
    const { tasksArr ,
        handleDelete
    } = props;
    return (
        <div className = "list">
            {/* list */}
            <ul>
                {tasksArr.map((task, idx) => {
                    return <li
                        onClick = {
                    () => {handleDelete(idx) }
                        }
                        key = {idx}>{task}</li>
                })}
            </ul>
        </div>
    )
}

export default List
```

**InputBox.jsx**

```jsx
import React,{useState} from 'react'

function InputBox(props) {
    const [inputValue, setValue] = useState("");
    const handleInput = (e) => {
        setValue(e.target.value);
    }
    const addTaskChild = () => {
        props.addTask(inputValue);
        setValue("");
    }
    //We have provided access to taskArr -> task ARR
    return (
        <div className = "inputbox">
            <input type = "text" value = {inputValue}
                onChange = {handleInput} />

            <button onClick = {addTaskChild}>Add Task</button>
        </div>
    )
}

export default InputBox;
```

---
title: Todo App review
description: What we have built in this project?

---

## Project overview
* We will be creating a To Do list as follows:
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/570/original/upload_d571a9bac2a1a47c663ea6438b84969d.png?1699640437)
* As we type in the text box and click on add a new item is added to the list.
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/571/original/upload_22823293a7ca5f46420a144286706b84.png?1699640491)
* The prototype can be as follows
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/572/original/upload_75f6ca8a4b1fb072d3c2aa5f985ee63f.png?1699640539)
* So we have an app, this app component contains an input box and a list.
* Dynamic part
    * Input
    * task list
* When we type something, the input is constantly changing.
* We can have a state variable for the input box. 
* This input then goes to the App, and then only it can be sent to the task list. It is like a tree structure.
* This procedure is called **lifting the state up.**

**[Ask the learners]**
when does a component re-render?
-> State change
-> Re-render:- component and all its children will re-render.

* When App will changes, the input box as well as the list will re-render.
* Input box cannot directly to the list.
* It first needs to forward it to the app and then the app can transfer it to the list.
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/573/original/upload_e1642a4297d905681f7379ace8ffdc53.png?1699640568)
* A task list is required by both the children of the App. So it should be lifted up i.e. kept at the top.
* As soon as the parent changes, all its children are re-rendered.
* Parent want to talk to children - we have to pass the data in **props**.
* All the components are functions in React.
* Relate it to call-back functions
    * the bigger function calls the smaller function
    * When a task is done it calls it again
* Here, we are using the same concept. For children to parent, we pass a function to children - children talk to the parent by calling the same function.
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/575/original/upload_8428b9167c43a205ee7c6a3079cbca5e.png?1699640604)
* In the case of the input box we pass a function `addTask()` as a parameter from a parent.
* This is received by the input box in props and when the button is clicked, we use this same function to add the item to the list.

