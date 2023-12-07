# Full Stack LLD: React-3: React Router and hooks

---
title: Agenda of the lecture
description: What will be covered in the topic?

---

## Agenda
* Review of Todo App
* React Devtools
* Useffect and 6 cases with API '
* Making request to third Party API
* Application of useEffect
* Understanding React Router
* Setting up routes and links in a React Application
* Redirects , private route, 404 Page not found


---
title: React Developer Tools
description: An Extension for VS Code

---

## React Developer Tools

* Install an extension in Chrome called React Developer Tools
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/584/original/upload_83a7ed1288c098ee4edf85b2f966e846.png?1699676844)
* `main.jsx` -> `App.jsx`
* `App.jsx` -> `Todo.jsx`
* `Todo.jsx` -> `list.jsx`
* `Todo.jsx` -> `inputBox.jsx`

Go to Components in inspect and compare it with the code
* **App.jsx**
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/585/original/upload_efafcc710bac4ba6151362f5ab3aae31.png?1699676982)
This has props, rendered by and source
* **Todo.jsx**
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/586/original/upload_e9806d763f8c4f3291f80e818023f455.png?1699677015)
* **InputBox.jsx**
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/587/original/upload_a6a26b66e2eea68d98e04067451b9dd7.png?1699677051)
* **List.jsx**
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/588/original/upload_bf9cdb4ec78250b51185f9b87a4b0bea.png?1699677087)

---
title: Intuition of useEffect
description: Building an intuition for why we need useEffect

---

## Intuition of useEffect

* Open YouTube open Inspect and go to the network, make it 3G slow and show the placeholder screen in Youtube.
![image.png](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/589/original/upload_ccd3037acdb7fec6ad9707d7ceeb9366.png?1699677149)
* So initially this screen is rendered, in the mean time content is fetched from the API and UI is formed and displayed.

## Application

### UI is Rendered for A Real-Life App in These Three Steps 

* We have to do an initial render -> loader/placeholder appears
* Parallelly we  make the request 
* Replace the initial placeholder with actual data 

In the Todolist, the state changes when an event occurs. In the above scenario, there is no user interaction. 

### React Must Provide a Feature to Just Call the Function at Different Stages of An Application 
flag = false
data = [];

**Render the UI (Loading):**
Initially, display a loading state or some user interface elements to indicate to the user that the application is loading.

**Make a Request:**
After rendering the initial UI, fetch data from API. This request is typically done asynchronously to avoid blocking the user interface.

**Get the Data (Data State):**
After request completion, you will receive data from the API. Update the UI with the data replacing the loading state or placeholders with the actual data.

**Re-render:**
After updating the UI with the retrieved data, the user interface may need to re-render to reflect these changes. This could involve refreshing a specific part of the UI, such as a data table or a list of items.

---
title: Intro to useEffects
description: UseEffects examples and introduction

---

## Intro to UseEffects
* We will create a file **GetData** as follows:
* Always put some initial value in `useState`
* Let us create an empty component and check if `data == null`. If it's null then we render Placeholder.
* As soon as the data is loaded we render the data
* `useEffect` takes a function and let's say `fetchData` that takes 
* remove the `StrictMode` from `main.jsx` and render this `getData()` in the `App.jsx`.

```jsx
import React, { useState, useEffect } from 'react'

function GetData() {
    const [data, setData] = useState(null);
    // hook -> given react 
    useEffect(
        () => {
            console.log("useEffect Ran");
        }
        , []);
    console.log("Render");

    return (
        <>
            {data == null ?
                <h2>Placeholder loading the data....</h2> : <h2>Data arrived</h2>
            }
        </>
    )
}

export default GetData
```

**Output:**

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/590/original/upload_f77996a7076beeb7bbf4f82ae7d41a10.png?1699677495)

**Strict Mode will give the following output**

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/591/original/upload_9b6eb4ccf2bec330376b7ba14d33ca4c.png?1699677571)

React has a feature that runs useEffect twice just to make sure everything is loaded.

### GetData
* JSON Placeholder API is a simple API that you can request user details
* `fetch` is our inbuild function
* This will give the user data
* Always wrap the code inside a function when you get this error. we will talk about this errro in today's lecture why it happens

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/592/original/upload_0819358d085dc3a48c76a95efd875960.png?1699677609)

```jsx
import React, { useState, useEffect } from 'react'

function GetData() {
    const [data, setData] = useState(null);
//    http -> call -> update the state -> rerender 
    function fn() {
        async function fetchData() {
            console.log("useEffect ran ");
            const response = await fetch("https://jsonplaceholder.typicode.com/users/1");
            const user = await response.json();
            console.log(data);
            setData(user);
        }
        fetchData();
    }
    // hook -> given react 
    useEffect(fn, []);
    console.log("Render");

    return (
        <>
            {data == null ?
                <h2>Placeholder loading the data....</h2> : <>
                    <h2>Name : {data.name}</h2>
                    <h2>Email : {data.email}</h2>
                    <h2>username : {data.username}</h2>
                </>
            }
        </>
    )
}

export default GetData
```

---
title: 6 use-cases of useEffect
description: covering all the types of the useEffects

---

## 6 use-cases of useEffect

* When we are loading the application we have to show something to the user and at the same time send the request to the backend to get the data and show UI.

### Demo of useEffect with the Todo App

* useEffect contains a state and an optional empty array
* To represent a task we are using a component called `task`.

**Note**: useffect is called twice in Strict mode in development mode 

### First useEffect
* **useEffect -** <br> with an empty array the callback is only called once after the first render
```jsx
import React, { useState, useEffect } from 'react';

function UseEffectExamples() {
    const [value, setValue] = useState("");
    const [taskList, setTaskList] = useState([]);
    const setTask = function () {
        // /
        let newTaskList = [...taskList];
        newTaskList.push({
            id: Date.now(),
            task: value
        })
        setTaskList(newTaskList);
        setValue("");
    }
    const removeTask = function (id) {
        let restOftasks = taskList.filter(function (taskObject) {
            return taskObject.id != id;
        })
        setTaskList(restOftasks);
    }
    function firstCb() {
        console.log("first useEffect");
    }
    // 1st version -> only it's cb fn only once after the first render
    useEffect(firstCb, []);
    console.log("render");
    return (
        <>
            <div>
                {/* input */}
                <input type = "text" placeholder = "Input Task" value = {value}
                    onChange = {(e) => { setValue(e.target.value) }}></input>
                <button onClick = {setTask}>Add Task </button>
            </div>

            {/* list  */}
            {taskList.map((taskObj) => {
                return (
                    <Task key = {taskObj.id} id = {taskObj.id} task = {taskObj.task}
                        removeTask = {removeTask}></Task>)
            })}
        </>
    )
}  
```


### Second useEffect

* useEffect is called after every render
```jsx
import React, { useState, useEffect } from 'react';

function UseEffectExamples() {
    const [value, setValue] = useState("");
    const [taskList, setTaskList] = useState([]);
    const setTask = function () {
        // /
        let newTaskList = [...taskList];
        newTaskList.push({
            id: Date.now(),
            task: value
        })
        setTaskList(newTaskList);
        setValue("");
    }
    const removeTask = function (id) {
        let restOftasks = taskList.filter(function (taskObject) {
            return taskObject.id != id;
        })
        setTaskList(restOftasks);
    }
    function secondCb() {
        console.log("second useEffect");
    }
    // 2nd version -> it's cb fn is called after every render and re-render
    useEffect(secondCb);
    console.log("render");
    return (
        <>
            <div>
                {/* input */}
                <input type = "text" placeholder = "Input Task" value = {value}
                    onChange = {(e) => { setValue(e.target.value) }}></input>
                <button onClick = {setTask}>Add Task </button>
            </div>

            {/* list  */}
            {taskList.map((taskObj) => {
                return (
                    <Task key = {taskObj.id} id = {taskObj.id} task = {taskObj.task}
                        removeTask = {removeTask}></Task>)
            })}
        </>
    )
}  
```


### Third useEffect

* useEffect is called after the first render and after that  when element changes its value in the dependency array
```jsx
import React, { useState, useEffect } from 'react';

function UseEffectExamples() {
    const [value, setValue] = useState("");
    const [taskList, setTaskList] = useState([]);
    const setTask = function () {
        // /
        let newTaskList = [...taskList];
        newTaskList.push({
            id: Date.now(),
            task: value
        })
        setTaskList(newTaskList);
        setValue("");
    }
    const removeTask = function (id) {
        let restOftasks = taskList.filter(function (taskObject) {
            return taskObject.id != id;
        })
        setTaskList(restOftasks);
    }
    function firstCb() {
        console.log("first useEffect");
    }
    // 3rd version -> it's cb fn is called after render and after the element changes its value 
    // inside  the dependent array
    useEffect(thirdCb, [taskList]);
    console.log("render");
    return (
        <>
            <div>
                {/* input */}
                <input type = "text" placeholder = "Input Task" value = {value}
                    onChange = {(e) => { setValue(e.target.value) }}></input>
                <button onClick = {setTask}>Add Task </button>
            </div>

            {/* list  */}
            {taskList.map((taskObj) => {
                return (
                    <Task key = {taskObj.id} id = {taskObj.id} task = {taskObj.task}
                        removeTask = {removeTask}></Task>)
            })}
        </>
    )
}  
```


---
title: useEffectCleanUp
description: What is cleanup in useEffect and used in various types of useEffects?

---

## useEffectCleanUp

**[Ask the learners]**
* When is cleanup called with the third useEffect?
* `CleanUp` is called right before the third `useEffect` call.
* `CleanUp` without dependency array - again it is called before calling the second `useEffect`.
* So, basically `cleanUp` function is always called before the next `useEffect` call.
* `cleanUp` is never called if there is no next `useEffect`.
* Also, `cleanUp` is never called for the first `useEffect` call.

### CleanUp for First useEffect

**[Ask the learners]**
For the first useEffect, when will the cleanUp be called?
-> As someone has once occupied the room but nobody else is going to use it, `cleanUp` is only called when the building is destroyed.
-> In our case when the task is destroyed only it will be called.
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/592/original/upload_0819358d085dc3a48c76a95efd875960.png?1699677609)
-> The cleanup function is executed when the effect is cleaned up, usually when the component is removed from the DOM.


### CleanUp for Second useEffect

-> Cleanup is always called before the next useEffect Call or when the component is destroyed.
-> This is similar to the first case, but it's like someone cleaning a room before someone else comes in. If someone uses the room, they clean it up before the next person uses it. So, it's like cleaning a room before the next person arrives.
-> But if there is no next person no cleanUp is requried.

```jsx
function secondCb() {
        console.log("second useEffect");
        return function () {
            console.log("cleanup for useffect without dependency array");
```

### CleanUp for Third useEffect

-> If there are any other useEffect calls then cleanup will be called before them.
-> Imagine multiple rooms in a building. If someone cleans a room and then moves to the next one, they clean up before moving to the next room. So, cleanup happens room by room, making sure each room is tidy before moving to the next.

```jsx
function thirdCb() {
        console.log("third useEffect");
        return function () {
            console.log("cleanup for useffect with TaskList Dependency");
        }
}
```

**Complete Code:**

```jsx
import React, { useState, useEffect } from 'react';

function UseEffectCleanup() {
    const [value, setValue] = useState("");
    const [taskList, setTaskList] = useState([]);
    const setTask = function () {
        // /
        let newTaskList = [...taskList];
        newTaskList.push({
            id: Date.now(),
            task: value
        })
        setTaskList(newTaskList);
        setValue("");
    }
    const removeTask = function (id) {
        let restOftasks = taskList.filter(function (taskObject) {
            return taskObject.id != id;
        })
        setTaskList(restOftasks);
    }

    function firstCb() {
        console.log("first useEffect");
    }
    function secondCb() {
        console.log("second useEffect");
        return function () {
            console.log("cleanup for useffect without dependency array");
        }
    }
    function thirdCb() {
        console.log("third useEffect");
        return function () {
            console.log("cleanup for useffect with TaskList Dependency");
        }
    }
    // 1st version -> only it's cb fn only once after the first render
    // useEffect(firstCb, []);
    /**
     * 2nd version -> its cb fn is called after every render and re-render
     * cleanup fn: it is called just before the next useEFFECT call
    */

    useEffect(thirdCb);
    /**
     * 3rd version -> 
     * its cb fn is called after render and after the element changes its value inside  the dependent array
    * cleanup fn: it is called just before the next useEFFECT call
     * */
    // useEffect(thirdCb, [taskList]);
    console.log("render");

    return (
        <>
            <div>
                {/* input */}
                <input type = "text" placeholder = "Input Task" value = {value}
                    onChange = {(e) => { setValue(e.target.value) }}></input>
                <button onClick = {setTask}>Add Task </button>
            </div>

            {/* list  */}
            {taskList.map((taskObj) => {
                return (
                    <Task key = {taskObj.id} id = {taskObj.id} task = {taskObj.task}
                        removeTask = {removeTask}></Task>)
            })}
        </>
    )
}
function Task(props) {
    let { id, task, removeTask } = props;
    function firstCb() {
        console.log("first useEffect");
        return function () {
            console.log("Cleanup called for empty dependency");
        }
    }
    useEffect(firstCb, []);
    return (
        <li
            onClick={() => {
                removeTask(id)
            }}
        >
            {task}
        </li>
    )
}
export default UseEffectCleanup;
```
### Summing Up
* UseEffect -> to be called after render
1. cb is called once in the lifetime -> useffect(fn,[])
*      cleanup -> after component is removed from UI
*      use-case : on-page first Load data fetching
2. cb is called n number of times in the lifetime -> useEffect (fn);
*      cleanup -> before next Useffect call
*      usecase : autosave 5sec 
3. cb is called if the dependency updates the number of times in the lifetime -> useEffect (fn,[dp1,dep2])
*     cleanup -> before next Useffect call
*     use case :  Resize aware component

**[Ask the learners]**


`Q Why were we getting errors in the async function?`
Ans:  to understand it first we will need to know what an async function returns so an async function returns a `promise` but useEffect needs `function` as a return value so it can use it as a cleanup function that's why we are getting this error 

---
title: Application of useEffect
description: Explaining the use case of the useEffect with an example

---
## Resize aware component
* Let us say we are building the resize-aware component that tracks the current resize values and just prints them.
* we cannot have an eventListner after every render
* We want it only when the screen resizes
* At that point in time we can add a dependency array and update the event listeners.
```jsx=
import React, { useState, useEffect } from 'react';

const ResizeAwareComponent = () => {
    const [windowWidth, setWindowWidth] = useState(window.innerWidth);
    // Set up a window resize event listener
    useEffect(() => {
        const handleResize = () => {
            setWindowWidth(window.innerWidth);
        };
        console.log("Listener added");
        window.addEventListener('resize', handleResize);
        return () => {
            console.log(" cleanup Listener added");
            window.removeEventListener('resize', handleResize);
        };
    });

    return (
        <div>
            <h1>Resize Aware Component</h1>
            <p>Window width: {windowWidth}px</p>
        </div>
    );
};

export default ResizeAwareComponent;
```
* So we have a useEffect and a function handleResize. 
* every time a screen resizes, we add an eventListener for that.
* EventListner cannot be added outside the useEffect as it is not desired to be called after every render
* Also, every useEffect adds a new EventListner.
* What we can do is use cleanUp to remove the old EventListner and add a new one.

---
title: Intution for Frontent Routing 
description: The client Server requests a response in the form of React Bundles

---

## Req res cycle for a react app 
* Let us discuss the client server once again
* The browser sends the request for the page `linkedin.com`. The server returns a `React Bundled file`.
**Note**  Show the live LinkedIn website and visit different parts of the website to show how it works or changes
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/593/original/upload_07d2f2bf20afee593ee6172896d07768.png?1699678960)

### supposed behaviour of SPA

* For this kind of web apps 
-> The URL changes but the app doesn't reload
-> You need to have different routes for different page
* For the end user 
-> It should look like an app
-> load time should be small
* Let us say the browser is sending a request for the `Job` so it fetches the latest React bundle. But here exists a problem
* When we request data from the backend there are always two components UI and data.
* Initial Request for UI and Bundle: When the user accesses the application, the browser sends a request for the initial UI and a minimal bundle of the application.
* Initial UI and Placeholder/Loader: The application loads an initial UI, which may include placeholders or loaders for various components that are not immediately visible. These placeholders help create a better user experience by giving the impression that the application is responsive.
* User Interaction: When the user interacts with the application, such as clicking a button or navigating to a specific page, the application checks whether the necessary UI components for that action are already loaded.
* Conditional Data Request: If the UI components for the requested action are already loaded, the application only makes a request to the backend for the data needed for that action, rather than fetching the entire UI and bundle again. Initial few pages are loaded to avoid unnessary loading again and again on the user side.

![Request for webpage.png](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/594/original/upload_9e183e0bc1a9400de4dced059e963817.png?1699679084)


* Initially we will fetch a few UIs of the pages and when the user clicks on any buttons, it will simply request data if the page UI already exists.
* If you don't optimize it up to a level, the bundle size can be very big.

---
title: React Router DOM Setup 
description: Adding react router to a react app and sample code 

---

## React Router DOM

* Create a new project and navigate inside it.
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/595/original/upload_aa4f31b760917d6b72428d89834be016.png?1699679122)
* install module react-router-dom using `npm i react-router-dom`
* import BrowserRouter from 'react-router-dom' in the `main.jsx` or the page to the lowermost component.
* Wrap `<App />` inside `<BrowserRouter></BrowserRouter>`

```jsx
    <React.StrictMode>
      <BrowserRouter>
        <App />
      </BrowserRouter>
    </React.StrictMode>
```
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/596/original/upload_98ec82dca5689e31add65762abba19ad.png?1699679221)

* Create a folder named `poc` in `src` and create a file `Routing.jsx` inside it.
* Inside `Routing.jsx` create a navbar with `home`, `about`, and `listing`.
```jsx
function About() {
    return (
        <h2>About Page</h2>
    )
}
function Home() {
    return <h3>I am Home Page</h3>
}
function Listing() {
    return <h3>I am Listing Page</h3>
}
```
* Create Routes for different pages in the same file.
* `import {Routes, Route}` from 'react-router-dom'
* Use Route to specify paths. It takes two props `path` to define and whenever it has that path it renders the items mentioned inside the `element` prop.
* **Routes** is used to combine multiple `Route`. And Inside `Routes` only `Route` can be called.
```jsx
<Routes>
                <Route path = "/home/" element = {<Home></Home>}></Route>
                <Route path = "/about/*" element = {<About></About>}> </Route>
                <Route path = "/listing" element = {<Listing></Listing>}></Route>
    </Routes>
```

* There is a wildcard matching if the path is given as `path = "*"` which matches everything. The order of placing wildcard won't affect its working. It will always try to match the specific path first.
* We are adding Page Not Found using the wildcard.
```jsx
<Route path = "*" element = {<PageNotFound></PageNotFound>}> </Route>
                {/* path -> /* -. wild card  */}
```

```jsx
function PageNotFound() {
    return <h3>Page Not found</h3>;
}
```

---
title: Link
description: Using the Link in React Router DOM

---

## Link

* There is another tag called `Link` in "react-router-dom". It takes a prop `to="/Home"` where we can give some path and it will change the URL and the Page accordingly.
* On clicking these buttons, the page won't reload. The content of the page changes but the page doesn't reload.

```jsx
            <nav>
                <ul>
                    <li>
                        <Link to = "/home" >Home Page </Link>
                    </li>
                    <li><Link to = "/about">About</Link></li>
                    <li><Link to = "/listing">Listing</Link></li>
                </ul>
            </nav>
```
---
title: Template Routes
description: How to do dynamic routing in React

---

## Template Routes/ Dynamic Routes
* Let's say we are rendering user routes
* So based on the `id` of the user the path as well as the page is defined.
```jsx
<Route path = "/users/:id" element = {<Users isAdmin = {true}></Users>}> </Route>
```
* The hook called `usePrams()` provided by React Router DOM returns an object whatever template route you have given.
* This will return the path given after `../users/`.
* We are using props to define if the user isAdmin
```jsx
function Users(props) {
    console.log(props.isAdmin);
    let params = useParams();
    const userID = param.id;
    console.log("param", param);
    
    return <h3>I am a user component</h3>
}
```

## Application of Template route
To understsna it better let's take a realife usecase 
* Fake Store API - we want to make a simple get request for the users.
* Show demo to the learners how users and id returns the data in Fake Store API
**[Ask the learners]**
If the route is given how are you going to get the data and represent it in this HTML?
-> `useEffect` with an empty list
* Before rendering we will just check if the user data is not null then we will print the user data else we will provide some placeholder like `loading...`.
```jsx
function Users(props) {
    // console.log(props.isAdmin);
    let params = useParams();
    let [user, setUser] = useState(null);
    console.log("46", params)
    // https://fakestoreapi.com/users/2
    useEffect(() => {
        (async function () {
            const resp = await fetch(`https://fakestoreapi.com/users/${params.id}`)
            const userData = await resp.json();
            console.log(userData);
            setUser(userData);
        })()
    }, [])
    return <>
        {user == null ? <h3>...loading</h3> : <>
            <h4>User Name: {user.username}</h4>
            <h3> Name: {user.name.firstname + " " + user.name.lastname}</h3>
            <h4> Phone: {user.phone}</h4>
        </>}
    </>

}
```
* These are called template routes or dynamic routes. Everything written like `:abc` can be derived with the help of `params.abc`, where `let params = useParams()`.

---
title: Redirecting Routes
description: Using the Navigate tag in Routes

---

## Redirecting Routes
* Let's say we want to redirect the user to another path when some specific path is given as input.
* `Navigate` component inside React Router DOM helps us achieve this.
```jsx
<Route path = "/abc" element = {<Navigate to = "/home"></Navigate>}></Route>
```

So in the above example if the user uses url `/abc` he/she will be redirected to the home page.

---
title: Nested Routes
description: Using nested routes in the About section to navigate through the company and founders.

---


## Nested Routes
these are the routes that are nested inside a components that is in itself rendered using an outer route . Let's tak an example for the same
* In the About sections let us have some components rendered using Route  
* Let us have company and founder components
```jsx

<Routes>
                <Route path = "/home/" element = {<Home></Home>}></Route>
                <Route path = "/about/*" element = {<About></About>}> </Route>
                <Route path = "/listing" element = {<Listing></Listing>}><Route>
</Routes>

function About() {
    return (
        <>
            <h2>About Page</h2>
            <Routes>
                <Route path = "company" element = {<Company />}> </Route>
                <Route path = "founders" element = {<Founder></Founder>}> </Route>
            </Routes>
        </>
    )
}
function Company() {
    return <h4> We are  a good firm</h4>
}
function Founder() {
    return <h4> We are Nice People </h4>
}
```
* So the path becomes `/about/company` or `/about/founder`
* If we have `/about/someroute` where the someroute doesn't match then it will still be in about only same happens for `/about`



**Routing.jsx**

```jsx
import React, { useEffect, useState } from 'react'
import { Routes, Route, Link, useParams, Navigate } from "react-router-dom";
function Routing() {
    return (
        <div style = {{
            textAlign: 'center',
            marginLeft: "50vw"
        }}>
            <h2>Routing Example</h2>
            <nav>
                <ul>
                    <li>
                        <Link to = "/home" >Home Page </Link>
                    </li>
                    <li><Link to = "/about">About</Link></li>
                    <li><Link to = "/listing">Listing</Link></li>
                </ul>
            </nav>
            <Routes>
                <Route path = "/home/" element = {<Home></Home>}></Route>
                <Route path = "/about/*" element = {<About></About>}> </Route>
                <Route path = "/listing" element = {<Listing></Listing>}></Route>
                <Route path = "/abc" element = {<Navigate to="/home"></Navigate>}></Route>
                {/* template routes -> dynamic routes  */}
                <Route path = "/users/:id" element = {<Users isAdmin = {true}></Users>}> </Route>
                <Route path = "*" element = {<PageNotFound></PageNotFound>}> </Route>
                {/* path -> /* -. wild card  */}
            </Routes>
        </div>
       
    )
}

function About() {
    return (
        <>
            <h2>About Page</h2>
            <Routes>
                <Route path = "company" element = {<Company />}> </Route>
                <Route path = "founders" element = {<Founder></Founder>}> </Route>
            </Routes>
        </>
    )
}
function Company() {
    return <h4> We are  a good firm</h4>
}
function Founder() {
    return <h4> We are Nice People </h4>
}

function Users(props) {
    // console.log(props.isAdmin);
    let params = useParams();
    let [user, setUser] = useState(null);
    console.log("46", params)
    // https://fakestoreapi.com/users/2
    useEffect(() => {
        (async function () {
            const resp = await fetch(`https://fakestoreapi.com/users/${params.id}`)
            const userData = await resp.json();
            console.log(userData);
            setUser(userData);
        })()
    }, [])
    return <>
        {user == null ? <h3>...loading</h3> : <>
            <h4>User Name: {user.username}</h4>
            <h3> Name: {user.name.firstname + " " + user.name.lastname}</h3>
            <h4> Phone: {user.phone}</h4>
        </>}
    </>

}

function Home() {
    return <h3>I am Home Page</h3>
}
function Listing() {
    return <h3>I am Listing Page</h3>
}

function PageNotFound() {
    return <h3>Page Not found</h3>;
}

export default Routing
```


**Note**
* All the component should have their own file. It's just for explanatory purposes that we have defined everything under the same file. 
* Use best practices and define a new file for each component.
