# Full Stack LLD: FE Machine coding-3: - Machine coding case studies



## Agenda
* Learning about frontend machine coding round.
* Create a star rating component.
* Create a countdown timer Part-1.



> Ask the students if they are familiar with machine coding rounds. Also ask if they have appeared for one in the past.

### Background
* To test a candidate for DSA, OA related to DSA is conducted.
* For frontend or backend interview, it is important to test the knowledge of the candidate. The first round can be the OA round where the skills like JS, DSA can be tested.
* Because of the nature of the job, the candidates cannot be tested in just 1hr of the interview.
* Flipkart started the machine coding round trend where they started asking the code for small reusable UI component. 
* They test your approach when you write the code,in such a manner where the code is maintainable, extendable, secured and readable. 

**Key points**
* The main aim of machine coding rounds is to test the practical aspects of knowledge that how good of a developer you are. 
* Many renowned companies like Google, Amazon, Flipkart, Walmart conduct it.




> Start by showing a glimpse of the component that we are creating. 
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/346/original/upload_9e087c44cc8c1b0dd4b82b92137953a2.png?1695181209)


### Requirement
Begin by asking questions to the interviewer for must asked features and good to have features.

#### Must have features
* Rating from 1 to 5
* when clicked on it, rating should appear
* Hover effect

#### Good to have features
* Emojis instead of numbers
* slider

### Approach
> Ask the students what should be the approach to solve the problem

* You have to listen to 3 events-
    * **Click**
        * Give the rating
        * Update star upto that level
        * Update the rating count also
    * **mouseover**
        * Change stars to that level to yellow
    * **mouseleave**
        * Stars Should turn back to gray
> Also explain the edge case for every problem you solve. Here the edge case is on mouseleave stars should turn back to gray

### Steps

#### HTML code for Dummy UI

* We first create one `h1` tag, 5 divs for a star. To display a star, we can either use the HTML code for characters or some font library symbol. Here, we are using the HTML CODE. The code will be wrapped the the container class as we want to center the content.
* We will create a `h2` for the rating count and `span` for the number.

```htmlembedded
<!DOCTYPE html>
<html lang = "en">
<head>
    <meta charset = "UTF - 8">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Star Rating Component</title>
    <link rel = "stylesheet" href = "reset.css">
    <link rel = "stylesheet" href = "style.css">
</head>
<body>
    <div class = "container">
        <h1>Star Rating Component</h1>
        <div id = "star_container">
            <div class = "star" idx = "1">&#9733;</div>
            <div class = "star" idx = "2">&#9733;</div>
            <div class = "star" idx = "3">&#9733;</div>
            <div class = "star" idx = "4">&#9733;</div>
            <div class = "star" idx = "5">&#9733;</div>
        </div>
        <h2>Rating count: <span id = "count">0</span></h2>
    </div>
    <script src = "script.js"></script>
</body>
</html>
```

The webpage till now looks like this-

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/347/original/upload_5f22b9665228080de14de58b475b839e.png?1695181344)

#### CSS
> Tell the students that we will not be writing the CSS code for the basic `reset.css` file. . We will copy it and focus on the rest of the part as CSS is easy.

We will create a file `reset.css` which is used to define the properties.

```css
html {
    box-sizing: border-box;
}

*,
*::before,
*::after {
    box-sizing: inherit;
}
html,
body {
    margin: 0;
}
a {
    text-decoration: none;
}

li {
    list-style: none;
}

.disabled {
    cursor: not-allowed;
}
```
In the above code, we have converted each element for box sizing, styled the anchor and list, the disabled class for hover.

**style.css**

* First we will center the contents inside the `container` class. 

```css
.container {
    display: grid;
    place-items: center;
}
```
* Now we will align the stars in a single horizontal line. A gap will also be provided between the stars.

```css
.star_container {
    display: flex;
    justify-content: center;
    gap: 1rem;
}
```

* We will increase the size of the stars.
```css
.star {
    font-size: 2rem;
    color:gray; 
}
```

The webpage till now looks like this-

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/348/original/upload_082b8bb5447e11251ececcba1c913d82.png?1695181384)

#### JavaScript
>Ask the students to check which star was clicked, should an event listener be added for the `star_container` or the `star`. Both the methods can be used.

Before proceeding, add an attribute to each star for easy identification. The attribute `idx` will be added which is for each index.

**Approach 1**
Use query selector for all the stars and iterate using for loop. Add click event listener after it is clicked, changes are reflected. 

```javascript
let allStars = document.querySelectorAll(".star");
for (let i = 0; i < allStars.length; i ++ ) {
   event listeners are async -> it's handler fn -> 5 copies 
   allStars[i].addEventListener("click", function () {
        console.log("I am clicked");
    })
}
```
The drawback for this approach is that when the number of stars increase, the event listeners will increase automatically. The event listeners are asynchronous their handler functions will have the number of copies equal to the event listeners. This occupies a lot of memory.

**Approach 2**
We will add an event listener to the parent container. Memory is saved because of bubbling. But we need to handle the edge case- process the events coming from the stars.

```javascript
const starContainer = document.querySelector("#star_container");
starContainer.addEventListener("click", function (e) {
    // console.log(e.target);
    let elem = e.target;
    let cClass = elem.getAttribute("class");
    if (cClass != "star")
        return;
    // console.log(elem);
    let cidx = elem.getAttribute("idx");
    fillStars(pidx);
})
```  

We will write the function fillStar. This will update the count.

```javascript
const countSpan = document.querySelector("#count");
function fillStars(idx) {
    // update the count
    countSpan.textContent = idx;
}
```
We will also add a class yellow to our CSS so that whenever we hover or make a rating, it will turn yellow.

```css
.yellow{
    color:yellow
}
```

The code has to be written in JS so that when we hover on the stars it has to turn yellow and when we are not hovering, it has to turn gray. Similar is when we click the star and reset the star.

```javascript
const starsArr = document.querySelectorAll(".star");
function fillStars(idx) {
    // update the count
    countSpan.textContent = idx;
    // reset all the stars
    for (let i = 0; i < starsArr.length; i ++ ) {
        // classList -> all the classes on an elmem
        starsArr[i].classList.remove("yellow");
    }
    // update the color to cidx 
    for (let i = 0; i < idx; i ++ ) {
        // classList -> all the classes on an elmem
        starsArr[i].classList.add("yellow");
    }
}

```

The classList function is used to select all the classes on an element.  
The above code does not work when the stars have to be reset.
> Ask why is this problem? Refer to the image below to understand the problem.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/349/original/upload_3875a357ad44033d355578fceffbe9c7.png?1695181451)

It is because, when we click on the stars then another class called yellow gets added to the classes. The getAttribute method does not work in this case. We will use the hasAttribute method on our attribute- idx.

The updated code for the event bubble is-

```javascript
// event bubbble
starContainer.addEventListener("click", function (e) {
    // console.log(e.target);
    let elem = e.target;
    let isrequired = elem.hasAttribute("idx");
    if (!isrequired)
        return;
    // console.log(elem);
    pidx = elem.getAttribute("idx");
    fillStars(pidx);
})
```

The next part is to write the mouseover and mouseleave code. 

```javascript
starContainer.addEventListener("mouseover", function (e) {
    let elem = e.target;
    let isrequired = elem.hasAttribute("idx");
    if (!isrequired)
        return;
    let cidx = elem.getAttribute("idx");
    changeStars(cidx);
})
```

We will move the reset all stars and update colour to cidx part to another function called `changeStars`. 

```javascript
function fillStars(idx) {
    // update the count
    countSpan.textContent = idx;
    changeStars(idx);
}
function changeStars(idx) {
    // reset all the stars
    for (let i = 0; i < starsArr.length; i ++ ) {
        // classList -> all the classes on an elmem
        starsArr[i].classList.remove("yellow");
    }
    // update the color to cidx 
    for (let i = 0; i < idx; i ++ ) {
        // classList -> all the classes on an elmem
        starsArr[i].classList.add("yellow");
    }
}
```
Now when we hover and then remove, the yellow colour should set to default. 

```javascript
starContainer.addEventListener("mouseleave", function (e) {
    // we need to reset
    changeStars(pidx);
})
```
There is a slight difference between the `click` event and `mouseover` event. In the `click` event, we will call the `fillStars` function for the star index we have clicked. It will fill the stars with yellow till the star where we have clicked. In the `mouseover` event, we will call the `changeStars` function for the star index we have clicked. The `changeStars` function will display the colour yellow till the time we hover. 

The full code for JS is as follows-

```javascript
const starContainer = document.querySelector("#star_container");
const countSpan = document.querySelector("#count");
const starsArr = document.querySelectorAll(".star");
let pidx = 0;

/****
 * saved memory -> bubbling
 * Edge case -> only process the event coming from the start
 * 
 * */


// event bubbble
starContainer.addEventListener("click", function (e) {
    // console.log(e.target);
    let elem = e.target;
    let isrequired = elem.hasAttribute("idx");
    if ( ! isrequired)
        return;
    // console.log(elem);
    pidx = elem.getAttribute("idx");
    fillStars(pidx);
})

starContainer.addEventListener("mouseover", function (e) {
    let elem = e.target;
    let isrequired = elem.hasAttribute("idx");
    if ( ! isrequired)
        return;
    let cidx = elem.getAttribute("idx");
    changeStars(cidx);
})

starContainer.addEventListener("mouseleave", function (e) {
    // whe need to reste
    changeStars(pidx);
})

function fillStars(idx) {
    // update the count
    countSpan.textContent = idx;
    changeStars(idx);
}
function changeStars(idx) {
    // reset all the stars
    for (let i = 0; i < starsArr.length; i ++ ) {
        // classList -> all the classes on an elmem
        starsArr[i].classList.remove("yellow");
    }
    // update the color to cidx 
    for (let i = 0; i < idx; i ++ ) {
        // classList -> all the classes on an elmem
        starsArr[i].classList.add("yellow");
    }
}
```

Our web page now works as expected. 



> Start by showing a glimpse of the component that we are creating. 
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/350/original/upload_51b4020ecfa59e0029a812c1baed0c79.png?1695181571)

A countdown timer is to be created where we will have to enter the hours, minutes and seconds manually. There will be two buttons- Start and Reset. When the countdown timer is started, the start button should change to reset button. We will also add the feature when the user enters seconds more than 60, those extra seconds will be converted to minutes and added to the minutes part. 
### Requirement
Begin by asking questions to the interviewer for must asked features and good to have features.

#### Must have features
* pass time in hours , minute and second
* play and pause 
* validation so that if user enters any time greater 60 then it should move to left bit
    * edge cases -> validation 

For example- if the input is-
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/351/original/upload_295a1a0120b388f38d2441fac1ab52af.png?1695181595)

It will be converted to the given time when the start button is clicked.
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/352/original/upload_f2042735691af240dbb71a580dfd2a68.png?1695181625)

#### Good to have features
* reset option

### Approach
> Ask the students what should be the approach to solve the problem

* Inputs : 
    * test case : 25: 30
    * -> html: input:number
        mins = 25
        input : 30
        
* Start Button
> Ask the student what should be the approach for start button.

1. countdown -> 1sec  -> setInterval -> 
                    a. time -> decrement
                    b. show the text change 
2. start -> pause

### Steps

#### HTML code

We will create 3 divs for the following- 
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/353/original/upload_edd267a7968315fa5f87a94a9c73d9f2.png?1695181654)


```htmlembedded
<!DOCTYPE html>
<html lang = "en"> 

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X - UA - Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
    <link rel = "stylesheet" href = "../reset.css">
    <link rel = "stylesheet" href = "style.css">
</head>

<body>

    <div class = "container">
        <h1>Count Down Timer</h1>
        <div class = "timer__label">
            <h2 class = "timer__label--hrs">Hours</h2>
            <h2 class = "timer__label--mins">Minutes</h2>
            <h2 class = "timer__label--sec">Second</h2>
        </div>

        <div class = "timer__inputs">
            <input type = "number" maxlength="2" oninput = "this.value = this.value.slice(0,this.maxLength)" id = "hr"
                placeholder="00">
            <span>:</span>
            <input type = "number" maxlength = "2" oninput = "this.value = this.value.slice(0,this.maxLength)" id = "min"
                placeholder = "00">
            <span>:</span>
            <input type = "number" maxlength = "2" oninput = "this.value = this.value.slice(0,this.maxLength)" id = "sec"
                placeholder = "00">
        </div>
        <div class = "container__btns">
            <button class = "btn start" id = "start">Start</button>
            <button class = "btn start" id = "reset">Reset</button>
            <button class = "btn start" id = "pause">pause</button>
            <button class = "btn start" id = "continue">continue</button>

        </div>
    </div>
    <script src = "script.js"></script>
</body>

</html>
```

For `timer__inputs`, we set the maxlength as 2 because we want 2 digit inputs. If the input entered is greater than 2, the value is sliced till 2nd place. Remember, the `maxlength` property is written in lower case in HTML, but in JS, it follows camel case- `maxLength`. 

Adding CSS to the code-
```htmlembedded
.timer__label {
    display: flex;
}
```

#### JavaScript

We will add event listeners in our code. 

The events for the start button, minutes, hours, and seconds input will be created. For the click event, we will get the value of hrs, mins and secs in integer format. 

We will convert the time in seconds. The timer function is a recursive function. A setTimeOut function is created 
```javascript
const startBtn = document.getElementById("start");
const minInput = document.getElementById("min");
const hrsInput = document.getElementById("hr");
const secInput = document.getElementById("sec");
let prevTimer = 0;
let timerId;

startBtn.addEventListener("click", function () {
    // get the values 
    let hrs = hrsInput.value || 0;
    let mins = minInput.value || 0;
    let sec = secInput.value || 0;
    // find out the time in second

    timeInSeconds = parseInt(hrs) * 3600 + parseInt(mins) * 60 + parseInt(sec);
    // console.log(timeInSeconds)
    timer(timeInSeconds);
})

function timer(timeInSeconds) {
//    -> timer automatically goes to zero
    if (timeInSeconds == 0)
        return;
    //  depending upon recursion 
    timerId = setTimeout(() => {
        console.log("Timer clocked", timeInSeconds);
        timeInSeconds--;
        timer(timeInSeconds);
    }, 1000)
}

resetBtn.addEventListener("click", function () {
    prevInsecs = 0;
    clearTimeout(timerId);
    // clear the text 
})

```

For the implementation of pause and continue we will add an event for the same.

```javascript
const continueBtn = document.getElementById("continue");
const pauseBtn = document.getElementById("pause");
let prevInsecs;
let timeInSeconds;

pauseBtn.addEventListener("click", function () {
    prevInsecs = timeInSeconds
    clearTimeout(timerId);
})

continueBtn.addEventListener("click", function () {
    timer(prevInsecs);

})
```
