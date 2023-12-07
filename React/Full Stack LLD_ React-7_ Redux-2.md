---
title: Agenda of the lecture
description: What will be covered in the topic?

---

## Agenda

We will try to cover most of these topics in today's sessions and the remaining in the next.
- Principles of Redux
- Redux Dev Tools
- Async Redux- middleware



let's start.

---
title: Redux concepts
description: Discussion of  various concepts related to redux  detail.

---

## Redux Concepts

For any components we should have few things 
- first is your UI
- second is your business logic
- third is the state management

- When we use redux we offload the whole state management to that. So the todo slice that gives the intial state as well as functions to update your state. Just take the interstate give to the store, store will give it to the provider, that will hand it over that whole thing for your application . 

- Using the useSelector hook, you will get the store and whatever part of the state you want you will get that you supply your state to the element. Next to send back that same thing use the useDispatch.

```javascript
import React from 'react'
import { useSelector, useDispatch } from "react-redux";
import TodoSlice from '../../redux/TodoSlice';
const actions = TodoSlice.actions;
function TodoRedux() {
    const { value, todoList } = useSelector((store) => {
        return store.todoState;
    })
    const dispatch = useDispatch();
// Bussiness logic
    const handleChange = (e) => {
        const updatedVal = e.target.value;
        dispatch(actions.setValue(updatedVal));
    }

    const handleAddTask = () => {
        dispatch(actions.addtask(value));
    }
// UI
    return (
        <>
            <h2>Todo</h2>
            <div style = {{ display: "flex" }}>
                <div className = "inputBox">
                    <input type = "text"
                        value = {value}
                        onChange = {handleChange}
                    />
                    <button onClick = {handleAddTask}>Submit</button>
                </div>
                <div className = "list">
                    <ul>
                        {todoList.map((task, idx) => {
                            return <li key = {idx} > {task}</li>
                        })}
                    </ul>

                </div>
            </div>

        </>


    )
}

export default TodoRedux
```

---
title: Redux and slices 
description: Discussion of  various concepts related to the counter slice .

---

## Redux and Slices

Let's understand this better with the below diagram as follows:

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/625/original/upload_1de0eb1f19f119946b00fe9768538928.png?1699690087)

Your application in divided into the multple slices. And out of these slices, currently we are having the counter slice, intially we need two things name, initial state. Next we send it to the store , that gives it to the provider, next this provider wraps around the application, and if you need anything soecific from that slice initial state, then you have to use the use-selector to get the state.

---
title: Redux's Principles
description: Discussion of  various concepts related to the principle of redux in detail.

---


### Principles of Redux
- **Single source of truth** - <br> You can only have one store in whole application . We can have multiple slices.
- **State is read only** - <br> When you want to update the state, then you have to dispatch an 'action' (an object that describes what happened.)
- **Changes are made by pure reducer function** <br> To specify how to changes the state action is passed to the Reducer. 

**Pure function:**

```javascript
let y = 10;
function fn(x,y){
    y = 2 * x;
    x = y * x;
    console.log("x",x,"y",y);
}
fn(5,2);
```

**Impure function:** Will not give the same result for the same input, it will input from the outer scope.

**Pure function:** five same output for the same input. All it's variables does not changes any outer variables(produces side effects). 


**How reducer are the pure function?**

In the below function the reducer take the input state and descObj as the parameters which will generate the same output for the same input hence they are the pure functions.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/626/original/upload_a2188af1f0b1c2882776ec5688950d4f.png?1699690238)


You can install the redux-dev tool extension from the chrome as shown below:

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/627/original/upload_568f5425b4683a06ff64bc311a03ed9b.png?1699691660)



After clicking on the inspect go to the redux devtools and you can these below features:
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/628/original/upload_726dc5bc2af80bbf12dfb9677e65f82a.png?1699691689)


- **State:**<br> represent all the present state in the redux. It has multiple things like tree, chart and raw.
- **Diff:**<br> it will show the change in the state due to the last action. 
- **Action:**<br> when a action is called using the dispatch the state is changed, it demonstrates how the states changes onclicking on the various buttons like +, -  and input the value  with also the timeline jump.
- **Play Button:**<br> this plays all the changes happed in the timeline with their effects.

Lets say you are doing an operation, and you want to test your dispatch worked or not which required so many steps, like placing an order, which includes login, payment, personal infoamtion etc. so here the facility of seeing the redux and dispatch from here and can easily debug the application. 

---
title: Implementation of async redux
description: Discussion of  various concepts related to the async redux in detail.

---

## Implementation of Async Redux

**Lets implement the asynchronous redux:**

```javascript
import React, { useEffect, useState } from 'react';


function User() {
    const [user, setUser] = useState(null);
    const [error, setError] = useState(false);
    const [loading, setLoading] = useState(true);
    useEffect(function(){
        async function(){
          try {
              setLoading(true);
              const resp = await fetch("https://jsonplaceholder.typicode.com/users/1")
              const user = await resp.json();
              console.log("user",user);
              setUser(user)
          } 
            catch(err){
                setError(true);
                setLoading(false);
            }
        },
    },[]);


    const heading = <h2>User Example</h2>;

    if (loading) {
        return <> {heading}
            <h3>...Loading</h3>
        </>
    }
    //if error 
    if (error) {
        return <> {heading}
            <h3>Error occurred</h3>
        </>
    }
    return (
        <>
            {heading}
            <h4>Name: {user.name}</h4>
            <h4>Phone: {user.phone}</h4>
        </>
    )
}

export default User
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/629/original/upload_51eecc7d48cf28cbc5abdb1a06199c23.png?1699691768)


**Why do we need async function in useEffect?**
**Ans:** useEffect has two portion one is function, and the second is cleanup function , so when the useEffect takes up a function it expects that it returns an clean up function but for an async function basically return a promise.


---
title: Middleware
description: Discussion of  various concepts related to the middleware file in detail.

---

## Middleware

As in the below diagram, 

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/630/original/upload_75b22ed245153d326a2f65a9f2f9f9fd.png?1699691802)

You attach a middleware file, as the react component wants to talk to the some third part API and then get some data and then update itself but it cannot do, because it does not have the access to the states as the state can be accessed and updated in slice area and then can be stored into the store. Hence we need to have the middleware in between component and the slice. 

Middleware will have the code that will talk to the third pary API , will get the result and then will have the code to talk ot the slice. Hence the component will have to talk to the middleware then the middleware will talk to the slice.

Lets create the userMiddleWare.js, 

```javascript
// useEffect(function () {
//     (async function () {
//         try {
//             setLoading(true);
//             const resp = await fetch("https://jsonplaceholder.typicode.com/users/1")
//             const user = await resp.json();
//             console.log("user", user);
//             setLoading(false);
//             setUser(user);
//         } catch (err) {
//             setError(true);
//             setLoading(false);
//         }
//     })()
// }, []);


// dispatch is provided to this middleware
import userslice from "../UserSlice";
const action = userslice.actions;
export const fetchUserMiddleWare =  (param) => {
    return async (dispatch) => {
        // input state
        try {
            dispatch(action.userLoading());
            const resp = await fetch(`https://jsonplaceholder.typicode.com/users/${param}`);
            const user = await resp.json();
            console.log("user", user);
            dispatch(action.userData(user));
        } catch (err) {
            dispatch(action.userError());
        }
    }
}
```
It imports actions from a user slice, which is a part of the Redux store. The fetchUserMiddleWare function accepts a parameter, param, which represents the user ID to be fetched.

Inside the middleware function, it dispatches actions to signal loading, data retrieval, or errors to the Redux store. It begins by dispatching a loading action, then makes an asynchronous request to a web service to fetch user data based on the provided param.

If the request is successful, it logs the user information and dispatches an action to store the user data in the Redux store. In case of an error during the fetch operation, it dispatches an error action to indicate the failure.

Also create the userSlice.jsx,

```javascript
// to create a slice -> redux;

import { createSlice } from "@reduxjs/toolkit";
//1
const userslice = createSlice({
    name: "userslice",
    // intinal state value
    initialState: {
        user: null,
        error: false,
        loading: true,
        param: null
    },
    // functions to update your state 
    reducers: {
        userLoading: (state) => {
            state.error = false;
            state.loading = true
        },
        userError: (state) => {
            state.error = true;
            state.loading = false
        },
        userData: (state, action) => {
            state.loading = false;
            state.user = action.payload;
        },
        getParam: (state, action) => {
            state.param = action.payload;
        }
    }

});

export default userslice;
```

- This code creates a "slice" using Redux Toolkit, which is a part of the application's state.
- It defines the initial state with properties like "user," "error," "loading," and "param."
- It specifies functions called "reducers" that can update these state properties. For example, "userLoading" sets "error" to false and "loading" to true.
- These functions are used to manage and modify the state as needed in a Redux store.

In the store.js we are having the:

```javascript
import { configureStore } from '@reduxjs/toolkit';
import counterSlice from './counterSlice';
import TodoSlice from './TodoSlice';
import userslice from './UserSlice';

import thunkMiddleWare from "redux-thunk";
// 2
const store = configureStore({
    reducer: {
        counterState: counterSlice.reducer,
        todoState: TodoSlice.reducer,
        userState: userslice.reducer
    },
    middleware:[thunkMiddleWare]
})
export default store;
```
Install the library redux-thunk as shown below:

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/631/original/upload_60903376f43e6b1299e918954c9516f6.png?1699691885)

- Add the thunkMiddleWare as this library helps you to get the data.

Also make the changes in the userRedux:

```javascript
import React, { useEffect, useState } from 'react';
import { useSelector, useDispatch } from "react-redux";
import { fetchUserMiddleWare } from '../../redux/middleWare/userMiddleWare';
import userslice from '../../redux/UserSlice';
const action = userslice.actions;
function UserRedux() {
    const { loading, error, user, param } = useSelector((store) => { return store.userState });

    const [value, setValue] = useState();
    const dispatch = useDispatch();

    useEffect(function () {
        if (param != null) {
            dispatch(fetchUserMiddleWare(param));
        }
    }, [param]);
    
    const handleParam = () => {
        dispatch(action.getParam(value));
    }

    const heading = <>
        <h2>User Example</h2>
        <input type = "text"
            value = {value}
            onChange = {(e) => { setValue(e.target.value) }} />
        <button onClick = {handleParam}> send Param</button>
    </>

    if (loading) {
        return <> {heading}

            <h3>...Loading</h3>
        </>
    }
    //if error 
    if (error) {
        return <> {heading}
            <h3>Error occurred</h3>
        </>
    }
    return (
        <>
            {heading}
            <h4>Name: {user.name}</h4>
            <h4>Phone: {user.phone}</h4>
        </>
    )
}

export default UserRedux;
```

1. **State Management**:<br> Utilizes Redux to manage user-related data and state.
2. **Data Fetching**:<br> Uses `useEffect` to initiate data fetching based on the `param` value.
3. **User Input Handling**:<br> Allows users to input a value and dispatch actions.
4. **Conditional Rendering**:<br> Renders content based on loading and error states.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/632/original/upload_98d6244a336048284d238ef48d703198.png?1699691941)

---
title: Implementation using Dev Tools
description: Discussion of  various concepts related to the redux using dev tools in detail.

---

## Implementation Using Dev Tools

Now lets see the same thing on the dev tools for better understanding:

In the main.jsx:

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'
import './index.css'
import { Provider } from "react-redux";
import store from "./redux/store";
import User from './components/normalComponents/User.jsx';

ReactDOM.createRoot(document.getElementById('root')).render(
 
   
    <Provider store = {store}>
      <App />
    </Provider>
 ,
)
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/633/original/upload_8798116d25e051aa7f1fcddcbc786f61.png?1699691998)

**As all the state handling is done by the redux.**

As in the userState user is null, error is false,and loading is true.


As in the userRedux.jsx file we are having the diaptch which calls the useEffect which calls the dispatch then fetchUserMiddleWare. Then it calls the function dispatch with action.userLoading  so without calling this function there are two factors state management can be done using the reducer only so the userLoading  is sent  from the fetchUserMiddleWare  and it tell the reducer that please go to the loading state.
As we jump to next state in the timeline as follows:

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/634/original/upload_09f7b0f1b905cbca1ba96d0a0a5df1ca.png?1699692180)

In the userRedux make the changes as follows:

```javascript
import React, { useEffect, useState } from 'react';
import { useSelector, useDispatch } from "react-redux";
import { fetchUserMiddleWare } from '../../redux/middleWare/userMiddleWare';
import userslice from '../../redux/UserSlice';
const action = userslice.actions;
function UserRedux() {
    const { loading, error, user, param } = useSelector((store) => { return store.userState });

    const [value, setValue] = useState();
    const dispatch = useDispatch();

    useEffect(function () {
        if (param != null) {
            dispatch(fetchUserMiddleWare(param));
        }
    }, [param]);
    
    const handleParam = () => {
        dispatch(action.getParam(value));
    }

    const heading = <>
        <h2>User Example</h2>
        <input type = "text"
            value = {value}
            onChange = {(e) => { setValue(e.target.value) }} />
        <button onClick = {handleParam}> send Param</button>
    </>

    if (loading) {
        return <> {heading}

            <h3>...Loading</h3>
        </>
    }
    //if error 
    if (error) {
        return <> {heading}
            <h3>Error occurred</h3>
        </>
    }
    return (
        <>
            {heading}
            <h4>Name: {user.name}</h4>
            <h4>Phone: {user.phone}</h4>
        </>
    )
}

export default UserRedux;
```

1. **handleParam Function:**
   - `handleParam` is a JavaScript function that's triggered when the "send Param" button is clicked.
   - It uses the `dispatch` function to dispatch an action called `getParam` with the `value` as a parameter.
   - Essentially, it updates the Redux store with the user's input value.

2. **heading Component:**
   - `heading` is a React component created using a React fragment (`<>`).
   - It includes a heading with the text "User Example," an input field, and a button.
   - The input field's value is controlled by the `value` state, and it triggers an `onChange` event to update the `value` state as the user types.
   - The "send Param" button calls the `handleParam` function when clicked, which, in turn, updates the Redux store with the `value` provided by the user.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/635/original/upload_9df36213f8ec2bdfcafd5ec1ff963766.png?1699692261)

