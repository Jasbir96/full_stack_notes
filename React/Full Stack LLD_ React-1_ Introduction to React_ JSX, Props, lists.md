# Full Stack LLD: React-1: Introduction to React: JSX, Props, lists

---
title: Agenda of the lecture
description: What will be covered in the topic?

---

## Agenda

We will try to cover most of these topics in today's sessions and the remaining in the next.
* What is React & it's advantages
* Setting up react using CDN links
* components
* JSX -> Javascript and XML
* Props
* Component composition
* Rendering Lists

It is going to be a bit challenging, advanced, but a  very interesting session covering topics that are asked very frequently in interviews.

So let's start.

---
title: Challenges in modern frontend
description: Discussion of  challanges faced in the modern frontend in detail.

---

## Challenges in Modern Frontend

**Asking Question**
What are the Challenges faced in the modern frontend?
**Ans:**
* Code Maintainability
* Single Page Application
* Lot of functionalities

**Lot of feature in our WebApp**
- Hence the DOM manipulation is costly.

As we open linkedin we see a lot of functionalities such as shown below like home, mynetwork,jobs, messaging, me etc.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/489/original/upload_a8b0bac0a165c827242385575fafe404.png?1699630915)
 
**Single Page Application (SPA)**
* That does not reload even if the URL is changed
* It is done by having single html file which utilizes the History API.
*  the cross team communication, and hot reload. This done using the Create-React-App ad Vite.

**Asking Question**
**Q:** Then why there is so much name of react?
**Ans:** We have tools like document API(least performant), where the react can effeciently help to do  the DOM manipulation.

---
title: What is React
description: Discussion of react in detail.

---

## What is React?

**React:** has the algorithm which efficiently manipulate the UI.

**Types of UI:**

|         |              |
|:-------:|:------------:|
| Desktop |   Electron   |
| Mobile  | React Native |
|   Web   |  React DOM   |
|   VR    |   React VR   |

Algorithm used is Reconciler that is why react is called as library.

---
title: Hello World in react 
description: Discussion of basic application in react in detail.

---

## Hello World in React 

Lets start creating the first application in react as the below code in the index.html.

* Just pasting the links for the algorithm to efficently change your UI and react dom.
* The whole application lives inside the root.
* Single page application (SPA) build using the javascript code.
* ReactDom render the component which is a function that return the html
* To print the hello React, using the doucument.getelementbyId by passing the root and inside that you want to put the hello component that is function which gets called and HelloReact gets printed.


```jsx
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge"> 
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
    <!-- algorithm to efficiently manipulate your UI -->
    <script src = "https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
    
    <!-- React DOM  -> uses react to update the DOM-->
    <script src = "https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
    
     <!-- transpiler -> JSX -> into javascript -->
    <script src = "https://unpkg.com/babel-standalone@6/babel.min.js"></script>
</head>
    

<body>
    <!-- the whole application lives inside-->
    <div id = "root"></div>

    <!-- single Page applcation : JS -->
    <script type = "text/babel">
       
        function HELLO(obj) {
            return <h1>Hello React </h1>;
        }

        ReactDOM.render(<HELLO></HELLO>, document.getElementById("root"));
    </script>
</body>

</html>
```

This just prints nothing on the browser as shown below:

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/497/original/upload_72f4116cfcee76aa78c5781b70fa258b.png?1699633345)


As in the above function Hello which is returning something which is neither html and not the javascript. Hence we use the Babble transpiler to convert the code of the one language to another. Babble is a javascript compiler to one language to another here it is JSX to JS.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/498/original/upload_670d3d99b94b2f2f48eb61a521409241.png?1699633373)



---
title: Bare bone React example 
description: Discussion of basic application in react in detail.

---

## Bare Bone React Example

If we take another example where without using JSX format to return the html unless we just use the simple JavaScript code with basic string manipulation that also print the same result on the browser as shown below:

```jsx
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
    <script src = "./lib/react.js"></script>
    <script src = "./lib/reactDOM.js"></script>
</head>

<body>
    <div id = "root"></div>
    <script>

        function HELLO() {
            let name = "React";
            let age = 30
            return `<h1>Hello  ${name} Thanks Babel and you are ${age<18 ?"underage":"eligble"}</h1>`;
        }

ReactDOM.render(HELLO, document.getElementById("root"));
    </script>

</body>

</html>
```


```jsx
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
    <!-- algorithm to efficiently manipulate your UI -->
    <script src = "https://unpkg.com/react@18/umd/react.development.js" crossorigin></script>
    
    <!-- React DOM  -> uses react to update the DOM-->
    <script src = "https://unpkg.com/react-dom@18/umd/react-dom.development.js" crossorigin></script>
    
    <!-- transpiler -> JSX -> into javascript -->
    <script src = "https://unpkg.com/babel-standalone@6/babel.min.js"></script>
</head>

<body>
    <!-- the whole application lives inside-->
    <div id = "root"></div>

    <!-- single Page applcation : JS -->
    <script type = "text/babel">
        // -> component : a function that returns HTML
        function HELLO(obj) {
            let {age,occupation} = obj;
            let name = "Jasbir";

            return <h1>Hello {name} Thanks Babel :) is {age} old and works as {occupation} </h1>;
        }
        // a method in REACTDOM -> puts the retruned HTML from componente in root element
        ReactDOM.render(<HELLO age = {35} occupation = {"mentor and SDE "}></HELLO>, document.getElementById("root"));
    </script>
</body>

</html>
```

The reactDom.js file and the react.js file in the index.html is consist of the:

```javascript
let ReactDOM = {};
function render(component, root) {
    let OptimizedDOM = react(component);
    console.log("Rendering to DOM");
    // it puts into the root element
    root.innerHTML = OptimizedDOM;
}
ReactDOM.render = render;
```

```javascript
function react(component) {
    // calling that fn
    let dom = component();
    console.log("....optimizing changes");
    return dom;
}
```

1. component: Components are independent and reusable bits of code. 
2. They serve the same purpose as JavaScript functions, 
3. but work in isolation and return HTML
4. you create bigger components using smaller component

---
title: JSX
description: Discussion of basic detail about JSX.

---

## JSX

JSON: Represent data in in JS fromat.
```javascript
{
name:Jasbir,
age:26
}
```

XML: Represent data in HTML

```javascript
<name>Jasbir</name>
<age>26</age>
```

In a typical web development workflow, JSX (a JavaScript extension used in libraries like React) is transformed into valid JavaScript, which is then utilized to render HTML elements. The resulting HTML can subsequently be converted into XML if needed for specific use cases or data interchange.

---
title: Props
description: Discussion of basic detail about Props.

---

## Props

(Properties passed to a component) Passing the parameters that will be as objects.

```javascript
<script type = "text/babel">
        function HELLO(obj) {
            let {age,occupation} = obj;
            let name = "Jasbir";

            return <h1>Hello {name} Thanks Babel :) is {age} old and works as {occupation} </h1>;
        }
        ReactDOM.render(<HELLO age = {35} occupation = {"mentor and SDE "}></HELLO>, document.getElementById("root"));
</script>
```
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/514/original/upload_3a5c83aa759fedde3fa4ddb2fa3ce43e.png?1699635530)

---
title: Component Composition
description: Discussion of basic details about the component composition in detail.

---

## Component Composition

If we are having the multiple components, then what will happen as shown below like in the HELLOPARENT component returning the HELLO component.

This code creates a single-page application using React and Babel. It defines two components, `HELLO` and `BYE`, which display messages. The `HELLOPARENT` component combines them. Finally, it renders the `HELLOPARENT` component into the HTML element with the ID "root."

```jsx
<body>
    <!-- the whole application lives inside-->
    <div id = "root"></div>

    <!-- single Page applcation : JS -->
    <script type = "text/babel">
        // -> component : a function that returns HTML
        function HELLO(prop) {
            let {name,age} = prop
            return <h1>Hello {name} Thanks Babel is {age} old </h1>;
        }
        function BYE(){
            return <p>BYE Component</p>
        }

        function HELLOPARENT() {
            return <div>
                <HELLO name = {"Rohan"} age = {10}></HELLO>
                <HELLO name = {"Rajneesh"} age = {30}></HELLO>
                <HELLO name = "Krishna" age = {40}></HELLO>
                <HELLO ></HELLO>
                <BYE></BYE>
                </div>

        }
        ReactDOM.render(<HELLOPARENT></HELLOPARENT>, document.getElementById("root"));
    </script>
</body>
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/515/original/upload_4c0071f1727e30cb02ea4e927d07de0c.png?1699635619)


- Create -> parent component from children component 
- Significance -> you can break a problem into multiple smaller parts
- A method in REACTDOM -> puts the returned HTML from component in root element

Like in the linkendin example shown below we can distribute the bigger block of info into smaller components like one for pic, for the three dots, for the information etc.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/516/original/upload_42b7a589753bea34470b27f115f6835a.png?1699635748)


---
title: Rendering list from Data
description: Discussion of basic introduction of using the list in the react in detail.

---

## Rendering List from Data

Intially the list items are fetched using the simple for loop from the list and gets rendered on the browser.

```jsx
<div id = "root"></div>
    <script type = "text/babel">
        let statinoary = ["Pen", "Pencil", "Eraser", "Ruler"];
        
        // console.log(liItemsArr)
        
        function List() {
            return<div>
                <h2>List Items</h2>
                <ul>{statinoary.map((item) => {
                    return <li>{item}</li>
                })}</ul>
            </div>
        }

        ReactDOM.render(<List></List>, document.getElementById("root"));
    </script>
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/517/original/upload_0b90af9c88d82aaa1b7535a2cb1650cd.png?1699635846)

When you want to print a list it expects an array of string and that string code should be valid html, so when you map you get the valid html in the form of an array and then react internally spread that and then put it as li element.

---
title: Key Prop
description: Key Props help identify each item uniquely.

---

## Key Prop

**[Ask the learners]**
Do you see any mistake here?

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/518/original/upload_93a7ee3cb2e710a3513898f5e5c65fb1.png?1699635897)

-> Every child in a list should have a key prop. A unique value for each list item.
-> A key ensures that none of the component is rendered multiple times. 
-> It also helps to identify each item uniquely.
-> Purpose of react is to optimize DOM.
-> Browser is very bad a doing DOM manipulation.
-> React makes it easier for the browser to manipulate DOM structure.

```jsx
<div id = "root"></div>
    <script type = "text/babel">
        let statinoary = ["Pen", "Pencil", "Eraser", "Ruler"];
        
        // console.log(liItemsArr)
        
        function List() {
            return<div>
                <h2>List Items</h2>
                <ul>{statinoary.map((item,idx) => {
                    return <li key = {idx}>{item}</li>
                })}</ul>
            </div>
        }

        ReactDOM.render(<List></List>, document.getElementById("root"));
    </script>
```
