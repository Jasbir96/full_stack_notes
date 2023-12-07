# Full Stack LLD: FE Machine coding-2: Events & Event Handling, Bubbling & capturing

---


## Agenda
* Fragments and their use cases
* Event Handling
* Mouse and Keyboard Events
* Event Bubbling and Event delegation

---


## Fragment
**[Ask the learners]**
What are fragments?
-> May have heard `<React.Fragment>`, its task is to wrap up a collection of HTML tags. Similarly, we have fragments in DOM.

Let's say we have an `ul` and we wish to append `10000` list items to it. We can use a for loop and append items in each iteration.

```javascript
        for (let i = 0; i < 10000; i ++ ) {
            let newLi = document.createElement("li");
            newLi.textContent = `I am list item ${i + 1}`;
        //    10k times -> html page
            ul.appendChild(newLi);
        }
```

The browser is very fast and sources are constant, but when we build a big application multiple elements are moving.

**[ask the learners]**
How many times I will have to change my DOM structure or the HTML page?

-> Yes, `n` number of times. Here, n = 10000. So the page updates 10,000 times.
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/343/original/upload_05d5fa9f3e622252ed0d6344b5a5a7fe.png?1695180438)

### The smarter way
What we can do is we can create a new element append all the `li` tags to this element and at the end attach this new element to the `ul`.  But for this, we will require to add a div to our `ul` which is again not desired.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/344/original/upload_8975ab6e59e4ef3e671f043c1a311588.png?1695180469)

Thus, we have another way to resolve this, we can make use of Fragments. The fragment is a temporary node with which you can attach a lot of node substructures and when you append that fragment then that fragment will disappear.

To create a fragment and append a child:
```javascript
let fragment = document.createDocumentFragment();
fragment.appendChild(newLi);
```

**So whenever you want to add a subcomponent add a fragment.**

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
    <!-- Fragment is
wrap up a collection of HTML tags -> a part of something
    <React.Fragment></React.Fragment>
    <>
    -
    -
    -
    </>
    -->
    <h1>List Item</h1>
    <ul id = "listContainer"></ul>
    <script>
        let ul = document.querySelector("#listContainer");

        // // to add li items -> ul -> 10000
        for (let i = 0; i < 10000; i ++ ) {
            let newLi = document.createElement("li");
            newLi.textContent = `I am list item ${i + 1}`;
        //    10k times -> html page
            ul.appendChild(newLi);
        }

        // smarter way of doing it 
        // a node creation
        let fragment = document.createDocumentFragment();
        for (let i = 0; i < 10000; i ++ ) {
            let newLi = document.createElement("li");
            newLi.textContent = `I am list item ${i + 1}`;
        //    appending my elements to that fragment
            fragment.appendChild(newLi);
        }
        // fragment -> ul  -> once 
        // ul.appendChild(fragment);

    </script>
</body>

</html>


<!-- 

    MC rounds : add sub-component : fragments
 -->
```

---


## EventListener
**[Ask the learners]**
What are the ways to add event Listeners?
- inline events -> For a given event you can only use it once

```javascript
let box = document.querySelector(".box");
box.onclick = function(){
    alert("I was clicked");
}
box.onclick = function () {
    alert("I am fake");
}
```

- addEventListeners -> have multiple callbacks for a given event
```javascript!
box.addEventListener("click", () => {
    console.log("I was clicked");
})
box.addEventListener("click", () => {
    console.log("I was fake");
})
```

**[Ask the learners]**
It executes the same event on every click. How can we execute it only once?

We have a parameter called `once` to be set to true and it will call the function only once.

```javascript
box.addEventListener("click", cb,{once:true});
```

---
title: Common Event Handling
description: Some of the most commonly used events.
duration: 2540
card_type: cue_card
---

## Event Handling
Different ways to interact with the webpage:
* Mouse -> hover, click mouse down, mouse up, scroll, double-click 
* Keyboard -> keyup , key-down, keypress = keydown+keyup


### Mouse Events 
Event occurs on all of these, but are visible only when an event listener is set.
```javascript
        const box = document.querySelector(".box");

        // hover  -> mouseover
        box.addEventListener("mouseover", () => {
            console.log("hover occurred");
        })
        box.addEventListener("mouseleave", () => {
            console.log("mouseleave occurred");
        })

        // click=mousedown+mouseup
        box.addEventListener("click", () => {
            console.log("click occurred");
        })
        box.addEventListener("mousedown", () => {
            console.log("mousedown occurred");
        })

        box.addEventListener("mouseup", () => {
            console.log("mouseup occurred");
        })
        box.addEventListener("dblclick", () => {
            console.log("dblclick occurred");
        })
```

### Keyboard Events

```javascript
        let input = document.querySelector("input");
        input.addEventListener("keydown", function (){
            console.log("Key down called");
        })
        input.addEventListener("keyup", function () {
            console.log("keyup called");
        })
        input.addEventListener("keypress", cb);
    
        function cb(eventObj) {
            console.log("keypress called");
            console.log("Key preseed is ",eventObj.key)
        }
```

* An object is passed to the callback function that returns what key is pressed or basically what event has occurred.

---
title: Question 1
description: Show ticket booking question.
duration: 3590
card_type: cue_card
---

## Question 1
You are given a movie booking website having a filter with few options. We are supposed to make the filter work. Each movie card is given a few attributes and an attribute `data-category` containing a string that matches the filter string.

### Steps to solve this question
1. Get the option selected by the user
2. Filter all the cards according to that value

### Step 1:

* Search on Google `select change of options in JS` and show how to browse and find a solution.
* We will use the `change` event listener for the `select` element.

```javascript
    const select = document.querySelector("select");
        const moviesCardArr = document.querySelectorAll(".movies")
        select.addEventListener("change", function () {
            filterMovies(select.value);
        })
```

* Now to filter movies we will implement the `filterMovies` function.

```javascript
        mfunction filterMovies(cGenere) {

            for (let i = 0; i < moviesCardArr.length; i ++ ) {
                let moviesCardNode = moviesCardArr[i];
                let para = moviesCardNode.querySelector("p");
                let movieGenre = para.getAttribute("data-category");
                if (cGenere != movieGenre && cGenere != "none") {
                    // moviesCardNode.style.visibility = "hidden";
                    moviesCardNode.style.display = "none";
                } else {
                    moviesCardNode.style.display = "block";
                }
            }
        }
```

* `Opacity = 0` only makes the object invisible, it still occupies the space, but `display = "none"` removes the element from the list.
* To set `opacity = 0` we can make `visibility = "hidden"`
* Intuition for filtration is first identified where to get the input from, how to navigate the DOM tree, do the changes, and return.
* If the filter matches then change `display` to `block` else set it to `none`.

---
title: Event Bubbling
description: Event bubbling is passing of message from child event listeners to parent event listeners.
duration: 5340
card_type: cue_card
---

## Event Bubbling
When an event occurs on an element, then that event travels up from itself to the document via all the ancestor elements is known as **Event Bubbling**.

In the example below, when we click on the innermost button, then innerBox, outerBox, and document all print the message on event listeners.
```htmlembedded
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8">
    <meta http-equiv = "X - UA - Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
    <style>
        div[id] {

            border: 1px solid red;
        }

        #outer_box {
            height: 10rem;
            width: 10rem;
            background-color: lightblue;
        }

        #inner_box {
            height: 50%;
            width: 50%;
            background-color: lightgreen;
            margin-top: 10px;
        }

        #button {
            margin-top: 3px;
            background-color: lightsalmon;
            width: 50%;
            margin-left: 3px;
        }
    </style>
</head>

<body>
    <h1>Event Bubbling</h1>
    <div id = "outer_box">
        <div id = "inner_box">
            <div id = "button">Click Me </div>
        </div>
    </div>

    <script>
        const outerBox = document.querySelector("#outer_box")
        const innerBox = document.querySelector("#inner_box")
        const button = document.querySelector("#button");

        button.addEventListener("click", (e) => {
            console.log("Button was clicked", e.target, "ctarget",
                e.currentTarget
            )
        })

        innerBox.addEventListener("click", (e) => {
            console.log("innerBox was clicked", e.target, "ctarget",
                e.currentTarget)
            // e.stopPropagation();
            // e.stopImmediatePropagation();
        })
        innerBox.addEventListener("click", (e) => {
            console.log("innerBox was  next handler");
        })
        outerBox.addEventListener("click", (e) => {
            console.log("outerBox was clicked", e.target, "ctarget",
                e.currentTarget)
        })
        document.addEventListener("click", (e) => {
            console.log("doc was clicked", e.target, "ctarget",
                e.currentTarget)
        })
    </script>
</body>

</html>
```
* **e.target:** the dom node on which the event has occurred
* **e.currentTarget:** the dom node on which the event listener is present

* **event.stopPropogation():** This function prevents further propagation of the current event bubbling.
* **event.stopImmediatePropogation():** If several listeners are attached to the same element for the same event type, they are called in the order in which they were added. If `stopImmediatePropagation()` is invoked during one such call, no remaining listeners will be called, either on that element or any other element.

---
title: Question 2
description: Event bubbling test question
duration: 6300
card_type: cue_card
---
## Question 2
Use a single event handler(only one element should have an event handler attached to it) and do the following:
On clicking the color name change the background color of the div.container to the same.

```htmlembedded
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8" />
    <meta http-equiv = "X - UA - Compatible" content = "IE = edge" />
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0" />
    <title>Event Delegation</title>
    <style>
        * {
            box-sizing: border-box;
        }

        .container {
            border: 1px solid;
            height: 10rem;
            width: 10rem;
        }
    </style>
</head>

<body>
    <div class = "container">
        <div class = "red">red</div>
        <div class = "blue">blue</div>
        <div class = "green">green</div>
    </div>
    
</body>

</html>
```
**Solution:**
```htmlembedded
    <script>
        // you use parent -> accept the events of children ->
        //  event delegation
        const parent = document.querySelector(".container");
        parent.addEventListener("click", function (e) {
            console.log("target", e.target)
            // console.log("currentTarget", e.currentTarget)
            const color = e.target.getAttribute("class");

            e.currentTarget.style.backgroundColor = color
        })

    </script>
```

---
title: Question 3
description: Real life example of event listeners
duration: 6750
card_type: cue_card
---

## Question 3
You're creating an online shopping website with a list of products.
Implement the Add to Cart button by only adding a single event listener to achieve it

Consider the following HTML structure:

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
    <div id = "productList">
        <div class = "product">
            <h3>Product 1</h3>
            <p>Price: $10</p>
            <button class = "add-to-cart">Add to Cart</button>
        </div>
        <div class = "product">
            <h3>Product 2</h3>
            <p>Price: $20</p>
            <button class = "add-to-cart">Add to Cart</button>
        </div>
        <div class = "product">
            <h3>Product 3</h3>
            <p>Price: $30</p>
            <button class = "add-to-cart">Add to Cart</button>
        </div>
    </div>
    
</body>

</html>
```
**Solution:**
```htmlembedded
    <script>
        const parent = document.querySelector("#productList");
        parent.addEventListener("click", addToCart);
        function addToCart(e) {
            // console.log(e.target)
            let targetElem = e.target;
            let targetClass = targetElem.getAttribute("class");
            //    filter out stray events 
            if (targetClass == "add-to-cart") {
                // allow adding
                // console.log(e.target)
                let p = e.target.parentElement.children[1];
                let price = p.innerText;
                console.log('You have added ${price} to your cart')
            } else {
                // Nothing
            }
        }


    </script>
```

