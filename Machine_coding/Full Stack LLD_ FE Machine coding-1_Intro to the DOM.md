# Full Stack LLD: FE Machine coding-1:Intro to the DOM

---


## Agenda
* Overview of the browser
* DOM
* CRUD in Document
* Node and its properties
* NodeList and HTML Collection
* Adding Event listeners


## Overview of Browser

**[Ask the learners]**
What do you understand by browser?
-> Software application that helps us to access information on the web and present it in a human-understandable format.

* Open google.com and go to `view page source`. This is what the browser gets as input and the Google window is what is something it presents. The browser converts that into a human-readable form.
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/342/original/upload_f4d1bf907b440b8c752743cb010d79c6.png?1695161179)

**[Ask the learners]**
What are the different components/features that make up a browser?
1. cookies, local storage, session storage -> storage
2. Address bar, button, tabs -> User Interface
3. JS engine, 
4. Rendering Engine -> convert code -> UI
5. Networking 
6. history 
7. Browser security model
8. Dev Tools


**[Ask the learners]**
How browser allow to access these features developer -> developing a UI app?

* WEB APIs: is the interface to access all the components or features 
* API (Application Programming Interface): API is an object that represents a feature. 
* Go to inspect and console and search for `document`, **[ask the learners]** What is **document**?
-> **document** is an object that refers to the entire webpage. `document.querySelector('element')` selects any object from the webpage.
* The developer can interact with API using its exposed props and methods.
* You can use JS to interact with all these API
* Web APIS Examples: console, DOM API, Web storage API (local storage, cookies, session storage), Fetch API.
* console is also an API, it allows us to print output using `console.log("message")`.

**Note:**  These all web API and JS from your browser environment.

### Summary
* Browser is a software that gets the information and converts it to UI
* It is made up of multiple components. Every component has its task.
* APIs are objects that represent these features. We interact with the browser using APIs. APIs have properties and methods.

**First Principle is**: To implement any feature -> You have to use an API given by Browser and that API is always an object -> method and props and we will use JS to do it.

---

## DOM API
**[ask the learners]**
What is DOM?

* It is an API that represents an HTML page. 
* Full form: Document Object Model
* **Document -** file.html
* **Object -** Object representation of the element/ tag
* **Model -** Structure -> takes the file, converts it into objects, and creates a structure.
* A tree structure is used to represent the DOM structure.
* In the browser's console, type `document.all`. It returns the list of elements and its child elements.


```htmlembedded
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8">
    <meta http-equiv = "X - UA - Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
</head>


<body>



    <h1 class = "hifine"> I am h1</h1>
    <h1> I am another</h1>
    <h2> I am h1</h2>
    <h3> I am h1</h3>
    <p class = "para">Lorem ipsum dolor sit amet consectetur adipisicing elit. Amet inventore adipisci dignissimos
        voluptatibus ad
        debitis vel natus nobis? Doloribus fugit vitae veritatis esse! Cumque odio quasi sapiente quisquam est! Odio!
    </p>
    <p class = "para">Lorem ipsum dolor sit amet consectetur adipisicing elit. Amet inventore adipisci dignissimos
        voluptatibus ad
        debitis vel natus nobis? Doloribus fugit vitae veritatis esse! Cumque odio quasi sapiente quisquam est! Odio!
    </p>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Amet inventore adipisci dignissimos voluptatibus ad
        debitis vel natus nobis? Doloribus fugit vitae veritatis esse! Cumque odio quasi sapiente quisquam est! Odio!
    </p>

    <input type = "text">
    <input type = "password" id="special">
    <input type = "text">
    <button>click</button>


    <script>

        /****read**************************/
        // returns first element ->using css selectors 
        // const h1 = document.querySelector("h1");
        // console.log(h1);
        // let inputPass = document
        //     .querySelector("input[type = 'password']")
        //     console.log(inputPass);
        // returns all the  element ->using css selectors 
        const h1Arr = document.querySelectorAll("h1");
        console.log(h1Arr);
        // indexing
        // console.log(h1Arr[0]);
        // console.log(h1Arr[1]);
        // // hof -> forEach
        // h1Arr.forEach((elem) => {
        //     console.log(elem.textContent);
        // })
        // Note: You get a nodeList from querySelectorAll 
        // node list has few features similar to an array-> length, foreach
        //But it  is not an array

        const allelems = document.getElementsByClassName("para");
        // console.log(allelems);
        // console.log(allelems[0]);
        // console.log(allelems[1]);


        /***
         * 
         * When you are doing a read operation for an element -> nodelist, HTML collection 
        and both are array-like but not array
         * */


        //  convert nodelist/HTMLcollection Array

        const newH1Arr = Array.from(h1Arr);
        console.log(newH1Arr);

    </script>
</body>

</html>
```

* How to select elements from the given webpage using a console? Select the H1 and password field.
```javascript
const h1 = document.querySelector("h1");
console.log(h1);
let inputPass = document.querySelector("input[type='password']");
```
* `document.querySelector("h1")` - selects the first element of the matching type.
* `document.querySelectorAll("h1")` - selects all the elements of the matching type in the form of an array.
* Show that it exhibits features of an array using indexing for each loop.
* It returns a **node list** which has few features similar to an array-> length, foreach
* but it is **not an array**.

* `document.getElementById()` - another way of getting an element with the help of its ID.
* `const allelems = document.getElementsByClassName("para");` We can also use ClassName. It **returns an HTMLCollection list** and not a single element.

**[ask the learners]**
Why do they use `ClassName` and not `class` for the class of the HTML elements?
-> We have a keyword class in JavaScript as well as in React, to distinguish them from this we are using `ClassName`.

* **HTMLCollection** - It allows indexing but not for each functionality.
* When you are doing a read operation for an element -> nodelist, HTML collection and both are array-like but not an array
* **Array.from()** method facilitates the conversion of NodeList and HTMLCollection to Array as follows:

```javascript
const newH1 = Array.from(h1List)
```

---

## Reading Attribute

**[ask the learners]**
```javascript
let div = document.querySelector("div");
console.log("innerHtml",div.innerHTML);
```
What will be the output?
-> All the three divs.

```javascript
// visible text 
console.log("innerText", div.innerText);
console.log("````````````````````");
        //  all the text inside an element-> hidden
console.log("text content",div.textContent);
console.log(div);
```
Both of them returned the same output. 
**[ask the learners]**
So what's the difference, why do we have two different properties?

-> Hide one of the `div` and you'll see that `innerText` won't show the hidden `div`. Whereas, `textContent` will show all the content even if it's hidden.

```htmlembedded
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8">
    <meta http-equiv = "X - UA - Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
</head>

<body>
    <h1 class = "hifine"> I am h1</h1>
    <h1> I am another</h1>
    <h2> I am h1</h2>
    <h3> I am h1</h3>
    <div id = "unique">
        <p class = "para">Lorem ipsum dolor sit amet consectetur adipisicing elit. Amet inventore adipisci dignissimos
            voluptatibus ad
            debitis vel natus nobis? Doloribus fugit vitae veritatis esse! Cumque odio quasi sapiente quisquam est!
            Odio!
        </p>
        <p class = "para" style = "display: none;">Lorem ipsum dolor sit amet consectetur adipisicing elit. Amet inventore
            adipisci dignissimos
            voluptatibus ad
            debitis vel natus nobis? Doloribus fugit vitae veritatis esse! Cumque odio quasi sapiente quisquam est!
            Odio!
        </p>
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Amet inventore adipisci dignissimos voluptatibus ad
            debitis vel natus nobis? Doloribus fugit vitae veritatis esse! Cumque odio quasi sapiente quisquam est!
            Odio!
        </p>
    </div>

    <input type = "text">
    <input type = "password" id = "special">
    <input type = "text">
    <button>click</button>
    <script>

        /**************content****************/
        let div = document.querySelector("div");
        // console.log("innerHtml",div.innerHTML);
        // visible text 
        // console.log("innerText", div.innerText);
        // console.log("````````````````````");
        // //  all the text inside an element-> hidden
        // console.log("textcontent",div.textContent);
        // console.log(div);

        /***************styling**************************/
        div.style.backgroundColor = "lightyellow";
        div.style.color = "red";
        /******************attribute****************************/
        // const idName = div.getAttribute("id");
        // console.log(idName);d
        const input = document.getElementById("special");
        const type = input.getAttribute("type");
        input.setAttribute("type", "email");
        console.log(type);
    </script>

</body>

</html>
```

* Go through the console and show the list of all the attributes and properties associated with elements of DOM. 


There is an attribute called `style` which allows us to add styling as follows:
```javascript
div.style.backgroundColor = "lightyellow";
div.style.color = "red";
```

* **getAttribute()** method returns the value of the attribute specified for that particular element.

```javascript
const type = input.getAttribute("type");
```

This returns the value assigned to the `type` attribute of the `input` element.

* **setAttribute()** method updates the value of the attribute specified for that particular element.
```javascript
const type = input.getAttribute("type");
input.setAttribute("type", "email");
console.log(type);
```
This sets the attribute `type` to `email`.

---

## CRUD Operations

### Creating and Reading 
**[ask the learners]**
Let's say we are given a pair of `ul` tags and we want to add a list item `make coffee` inside it using a script. How can we do this?

-> The three steps are:
* Create 
* put attributes
* Add to the tree

```htmlembedded
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8">
    <meta http-equiv = "X - UA - Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
</head>

<body>
    <h1>I am list</h1>
    <ul>


    </ul>

    <script>
        //Your task to create a making coffee
        // create element
        let li = document.createElement("li");
        //  add attributes+ content

        li.textContent = "make coffee";

        let ul = document.querySelector("ul");
        
        ul.appendChild(li);



    </script>

</body>

</html>
```

The above code first creates an element li with the help of `document.createElement`. We set the text as `make coffee`. We can also add more attributes to it. Next, we select the `ul` using `querySelector` and `appendChild` which we just created.

### Insertion 

**[ask the learners]**
```htmlembedded
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8" />
    <meta http-equiv = "X - UA - Compatible" content = "IE = edge" />
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0" />
    <title>Document</title>
</head>

<body>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
        <li>6</li>
        <li>8</li>
        <li>9</li>
        <li>10</li>
    </ul>
</body>
```

We have the above list, the `7` is missing in the list. We want to add it with the help of the list.

-> Solution
```htmlembedded
    <script>
        // Q fix the list by making it go from 1 to 10. 
        //  creation
        let li = document.createElement("li");
        //  adding attributes+content
        li.textContent = "7";
        // selecting element
        let allList = document.querySelectorAll("li");
        let ul = document.querySelector("ul");
        ul.insertBefore(li,allList[6]);
    
//  adding

    </script>
```

Steps to update a list:
* Create
* select parent 
* insert

We will first create an element. Next, we will select the parent and the element before which we want to insert the current element. Lastly, we will use the `insertBefore()` method and update the list.

**Note:** We are not required to remember all these methods, but we should just know how to use them.

---


## Update an attribute
**[ask the learners]**
How can we convert all the paras into green?
```htmlembedded
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8" />
    <meta http-equiv = "X - UA - Compatible" content = "IE = edge" />
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0" />
    <title>Document</title>
</head>

<body>
    <p>lorem</p>
    <p>lorem</p>
    <p>lorem</p>
    <p>lorem</p>
    <p>lorem</p>
    <p>lorem</p>
    <p>lorem</p>
    <p>lorem</p>
    <p>lorem</p>
    <p>lorem</p>
    <h1>Hi i am h1 </h1>
</body>
```

-> Solution
```javascript
let allps = document.querySelectorAll("p");
for(let i = 0;i < allps.length ;i ++ ){
    allps[i].style.color = "green"
}
```

To **delete** the h1 from the above page we will simply use the `remove()` method.

```htmlembedded
    <script>
        const h1 = document.querySelector("h1");
        h1.remove();
    </script>
```

---

## Event Listeners
**[ask the learners]**
What do you understand by event listeners?
-> Event listeners are the procedures that wait for a particular event to occur like a mouse click, keyboard press, etc. When the event occurs this procedures follow the step of instructions.

**[ask the learners]**
Are these callbacks synchronous or asynchronous?
-> These event listeners need APIs and thus are asynchronous.

```htmlembedded
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8">
    <meta http-equiv = "X - UA - Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
</head>

<body>
    <button>click Me </button>
    <input type = "text">
    <ul></ul>
    <script>
        // setup
        const button = document.querySelector("button");
        const ul = document.querySelector("ul");
        const input = document.querySelector("input");
        // console.log("before");
        // listening for input
        button.addEventListener("click", cb);
        function cb() {
            // when you get the input
            console.log("Between");
            // create
            if (input.value) {
                const li = document.createElement("li");
                const value = input.value;
                li.innerText = value;
                ul.appendChild(li);
                input.value = "";
            }


        }
        console.log("after");

    </script>
</body>

</html>
```
* In the above code on the click of a button a list element is added to the `ul`.
* Once we know how we can use event listeners, we will next take user input and the user text will be added to the list.
* Lastly, we will set `input = ""` which will clear the input box for a better user experience.

Refer to [Event Reference](https://developer.mozilla.org/en-US/docs/Web/Events) for all the event-based functions. You need not know all of these but you must know to implement these using reference or Google.