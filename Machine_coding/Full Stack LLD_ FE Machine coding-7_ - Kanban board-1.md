# Full Stack LLD: FE Machine coding-7: - Kanban board-1

---
title: Agenda of the lecture
description: What will be covered in the topic?
duration: 120
card_type: cue_card
---

## Agenda
**Topics to cover in Javascript:**

Certainly, here are the headings for the provided topics:

1. **Kanban Board**
2. **Create and Update Task**
3. **Lock and Unlock Tasks**
4. **Colors**
5. **Pop-up Dialog Box**
6. **Delete**
7. **Client-side Storage**
8. **Local Storage**
9. **Cookies**
10. **Session Storage**
11. **IndexedDB**
We will try to cover most of these topics in today's sessions and the remaining in the next.

So let's start.

---
title: Kanban Board
description: Discussion about the various functionalities with html, css and javascript code in detail.
duration: 2100
card_type: cue_card
---

## Demo of the project:
Initially showing the demonstartion of the adding task, setting the colors, unlock/lock feature, filtering based on the colors and delete feature.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/490/original/upload_6a0aba9b3dc5d4f3f227120f9966429d.png?1695620798)

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/491/original/upload_42f74c38dbea707f63b91c1643f00f00.png?1695620835)

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/492/original/upload_9fe8e2d7c92c194286c1211b553810df.png?1695620859)

Discussing about the local storage and the crud operation and also try to cover the drag-drop functionalities, accessibility.

On the highest level, mainly there are two components, to mark those will be using the two divs namely toolbox-container and maincontainer.

Inside the toolbox container first there are different tags for colors which we ca have and after that these two boxes of + and delete. Apart fro that we can those ticket also.

In toolbox container there are two container namely toolbox-priority-cont and action-btn-cont. 
* Inside the toolbox-priority-cont we are having the four divs of colors(pink, blue, purple and green).
* In the toolbox-priority-cont having the two divs of add-btn and remove-btn. Alaso adding the font from the cdn font library. 

Opening the live server and just seeing the effects of the added code till now and nothing is visible till now since no css has been added.

```htmlembedded
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8">
    <meta http-equiv = "X - UA - Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <link rel = "stylesheet" href = "https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.1.1/css/all.min.css">
    <title>Document</title>
    <link rel = "stylesheet" href = "style.css">
</head>

<body>
    <div class = "toolbox_cont">
        <div class = "toolbox-priority-cont">
            <div class = "color pink"></div>
            <div class = "color blue"></div>
            <div class = "color purple"></div>
            <div class = "color green"></div>
        </div>
        <div class = "action-btn-cont">
            <div class = "add-btn">
                <i class = "fa-solid fa-plus"></i>
            </div>
            <div class = "remove-btn">
                <i class = "fa-solid fa-trash"></i>
            </div>
        </div>
    </div>

    <div class = "main_cont">

    </div>

    <div class = "modal-cont">
        <textarea class = "textarea-cont" placeholder = "Enter Your Task"></textarea>
        <div class = "priority-color-cont">
            <div class = "priority-color pink"></div>
            <div class = "priority-color blue"></div>
            <div class = "priority-color green active"></div>
            <div class = "priority-color purple "></div>
        </div>
    </div>
    <script src = "https://cdn.jsdelivr.net/npm/short-unique-id@4.4.4/dist/short-unique-id.min.js"
        crossorigin = "anonymous"></script>
    <script src = "script.js"></script>

</body>

</html>
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/494/original/upload_e30d878377b9bd9cf474cb1301fc205a.png?1695621024)

Nothing is visible as no css has been added.

Let's start creating the css file namely style.css, and adding the code for the colors first by creating the css variables for the colors like pink, green, purple etc.

```css 
/* create css variables  */
:root {
    --pink: lightpink;
    --blue: lightblue;
    --green: lightgreen;
    --purple: #8e44ad;
    ;
    --light: white;
}
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

.pink {
    background-color: var(--pink);
}

.blue {
    background-color: var(--blue);

}

.purple {
    background-color: var(--purple);

}

.green {
    background-color: var(--green);
}

```

Now adding the css for the toolbox container starting with height, background-color and then adding the toolbox-priority-container by providing it with height, background-color. Same for the action-btn also same color and height. 

```css
/* styling starts from here */


/* ************toolbox_cont **********/
.toolbox_cont {
    height: 5rem;
    background-color: black;
    display: flex;
    align-items: center;
}
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/495/original/upload_9ef1193fd88cb439b7fb2047ac31ceed.png?1695621065)



Now adding the display: flex in the parent conatiner namely in the toolbox-container in the css code. Add margin-left in the toolbox-priority-cont and margin-left in the action-btn-cont. Also change the background-color of the action-btn-cont to the light. 

Now add the color blocks in the toolbox-priority-cont. Providing the height and width to the color block. Also add the display:space-evenly and align-items:center to the parent class.

```css
.toolbox-priority-cont {
    height: 60%;
    background-color: #393c3f;
    width: 18rem;
    margin-left: 5rem;
    display: flex;
    justify-content: space-evenly;
    align-items: center;
}

.color {
    height: 1.5rem;
    width: 3rem;
}
.action-btn-cont {
    height: 60%;
    background-color: var(--light);
    width: 10rem;
    margin-left: 5rem;
    display: flex;
    align-items: center;
    justify-content: space-evenly;
}
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/496/original/upload_b16e475253b1a6317223fc3e4575ab3b.png?1695621089)

Provide the font size to the both add-btn and remove-btn and also add the display:flex, align-item:center and justify-content:space-evenly in the action-btn-cont.

```css
.add-btn,
.remove-btn {
    font-size: 1.75rem;

}
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/496/original/upload_b16e475253b1a6317223fc3e4575ab3b.png?1695621089)


* Mainly the ticket is consist of the four components for the ticket-color, ticket-id, ticket-area, and lock-unlock icon.
* Discussing  about any feature through which we can create the div into editable and vice-versa. Actually div is nothing but the input with content-editable=false.
* Discussing about the unique ticket-id through a library. To use library mainly there are two methods namely npm-package and cdn-link. We will be using the uuid cdn link for the unique id for the ticket-id.


```htmlembedded
<div class = "main_cont">
<div class = "ticket-cont">
    <div class = "ticket-color green"> </div>
    <div class = "ticket-id">#eidut3</div>
    <div class = "ticket-area">Some Task </div>
    <div class = "lock-unlock"><i class = "fa-solid fa-lock"</i> </div>  
    </div>
</div>
```
Also corresponding css for the maincont is as follows:

```css
/* css for main container */

.main_cont {
    margin-top: 2rem;
    display: flex;
    justify-content: space-evenly;
    /* if the horizontal space is not 
    enough  move the further items to the
     next row*/
    flex-wrap: wrap;
    /* creates a gap between every elem
    row& column wise 2rem;
    */
    gap:2rem;
}
.ticket-cont {
    height: 10rem;
    width: 15rem;
    background-color: #ecf0f1;
    position: relative;
}

.ticket-color {
    height: 10%;
}

.ticket-id {
    height: 0.75rem;
    font-weight: 600;
    font-size: 0.75rem;
    padding-left: 10px;
}

.ticket-area {
    height: 7.5rem;
    padding: 5px;
    font-size: 1.25rem;
}

.lock-unlock {
    font-size: 1.25rem;
    position: absolute;
    bottom: 10px;
    right: 15px;
}
```


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/497/original/upload_8c4a88237a0200100cabe0f4d85f42a8.png?1695621308)

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/498/original/upload_d4d0298b9b80eb7e0695a3ea58dbfed7.png?1695621378)

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/499/original/upload_83d7df33d584d534b8c9195a9404182b.png?1695621417)

The CSS code snippet appears to describe styles for an element with the class `.main_cont`. Here's a breakdown of the styles applied:

1. `display: flex;`: This property turns the `.main_cont` element into a flex container, allowing its child elements to be arranged in a row or column, depending on the specified direction.

2. `justify-content: space-evenly;`: This CSS rule distributes the child elements within the flex container horizontally with equal spacing around them. The "space-evenly" value ensures that there is space before the first element and after the last element as well as between the items.

3. `flex-wrap: wrap;`: When the horizontal space is not sufficient to display all child elements in a single row, this rule causes the remaining items to wrap to the next row, creating a multi-row layout.

4. `gap: 2rem;`: This CSS property sets a gap of `2rem` between each child element, both vertically (between rows) and horizontally (between columns), providing spacing and visual separation.

That is how we create a single ticket, lets discuss the where will be adding the the functionalities of the editable text and change the color.


Lets make the corresponding changes in the lock-unlock features in class lock-unlock.
```htmlembedded
<div class = "main_cont">
<div class = "ticket-cont">
    <div class = "ticket-color green"> </div>
    <div class = "ticket-id">#eidut3</div>
    <div class = "ticket-area">Some Task </div>
    <div class = "lock-unlock"><i class = "fa-solid fa-lock-open" </i>
    </div>  
    </div>
</div>
```

this will happen:

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/500/original/upload_7c02caa736dcef4b827551a9550f10ba.png?1695621571)


Now to implement the tasks of:
* Toggle editable behaviour
* Change the color on toggle

**Dom Tree**

In the Document Object Model (DOM), the structure is represented as a tree-like structure, often referred to as the DOM tree. This tree structure consists of nodes, and each node represents an element or part of the document, such as elements, attributes, or text content.

When you query the DOM using JavaScript, you are essentially interacting with these nodes. 

For example, if you query for an element with a specific `ticket-id` attribute, you are searching for a specific node in the DOM tree, and when you manipulate that node, you are changing the corresponding part of the document.

**Toggle editable behaviour:**
```javascript
const ticketContainer = document.querySelector(".ticket-cont");
const ticketArea = ticketContainer.querySelector("ticket-area");
const lockBtn = ticketContainer.querySelector(".lock-unlock"); 
let isLocked = true;
lockBtn.addEventListener("click",handleLock);
function handleLock(){
if(isLocked == true){
ticketArea.setAttribute("contenteditable",true);
lockBtn.children[0]children[0]..classList.remove("fa-lock");
lockBtn.children[0].classList.add("fa-open-lock");

}
else{
ticketArea.setAttribute("contenteditable",flase);
lockBtn.children[0].classList.remove("fa-open-lock");
lockBtn.children[0].classList.add("fa-lock");
}
isLocked =!= isLocked;
}
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/501/original/upload_8dcb1312dcbc22749d220480e0ee6fec.png?1695621713)


**Change the color on toggle**

**Asking question** 
Any ideas how we can implement this color change functionality? Any logic?

```javascript
let colors = ["pink","blue","purple","green"];
const ticketColor.add =  document.querySelector(".ticket-color");
ticketColor.addEventListener("click", toggleColor);
function toggleColor(e){
    const cColor = e.target.ClassList[1];
    int idx = colors.indexOf(cColor);
    let nextIdx = (idx + 1) % colors.length;
    ticketColor.classList.add(colors[nextIdx]);
}
```

* colors.indexOf(cColor) finds the index of the current color class in the colors array and assigns it to the variable idx.
* let nextIdx = (idx + 1) % colors.length calculates the index of the next color in the colors array. It ensures that the index wraps around to 0 when it reaches the end of the array.

**Asking the question**

How the filter based upon the selected colors will work? just tell me the logic.
**Ans:**
So basically the color of the main container's tickets which is matching with the color of the selected toolbox container color.

Lets discuss how it can be done,

* add the eventListener on the toolboxPriorityContainer.
* Basically the e.target is the element on which the event occured and e.currentTarget is the element on which the eventlistener is added. Because the empty space also has to taken care of by comparing the colors.
* The code then checks if the color of the current ticket (cTicketColor) is not equal to the color clicked in the toolbox (cColor). If they are not equal, it sets the display style property of the current ticket element to "none," effectively hiding it. If they are equal, it sets the display property to "block," making the ticket visible.

```javascript
const tollBoxPriorityContainer =  document.querySelector(".toolbox-priority-cont");
tollBoxPriorityContainer.addEventListener("click",function()){
if(e.target = e.currentTarget){
return;
}
const currentColorelem = e.target;
const cColor = currentColorelem.classList[1];
const ticketArr = document.querySelectorAll(".ticketColor");
for(let i = 0; i < ticketArr.length; i++){
const ticketColorElem = ticketArr[i].querySelector(".ticket-color");
let cTicketColor = ticketColorElem.classList[1];
if(cTicketColor != cColor){
ticketArr[i].style.display = "none";
}
else{
ticketArr[i].style.display = "block";
}
}
}
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/502/original/upload_85ebf4df392bdde6471dc6d2f4d6e988.png?1695622007)


### Modals Area
Now we will start our discussion of adding the modals so when we click on the **+** button then the modal appears having the space for the text and the color options. As we click on the enter after choosing the color then the ticket is added with the corresponding text , ticketId and color.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/505/original/upload_3a11bd32a0a51d298b1844bf96cd03f3.png?1695622139)

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/506/original/upload_72674617abe6bf45295552e1f91dc084.png?1695622200)

```htmlembedded
<div class = "modal-cont">
    <textarea class = "textarea-cont" placeholder = "Enter Your Task"></textarea>
    <div class = "priority-color-cont">
        <div class = "priority-color pink"></div>
        <div class = "priority-color blue"></div>
        <div class = "priority-color green active"></div>
        <div class = "priority-color purple "></div>
    </div>
</div>
```

The corresponding css for the modal is as follows:

```css
/* modal css */
.modal-cont {
    /* we want it fixed to viewport */
    position: fixed;
    height: 50vh;
    width: 50vw;
    left: 25vw;
    top: 25vh;
    display: none;
    /* display: flex; */
    /* border: 2px solid grey; */

    -webkit-box-shadow: 10px 10px 40px -4px rgba(0, 0, 0, 0.75);
        -moz-box-shadow: 10px 10px 40px -4px rgba(0, 0, 0, 0.75);
        box-shadow: 10px 10px 40px -4px rgba(0, 0, 0, 0.75);
   
}
.textarea-cont {
    height: 100%;
    width: 75%;
    font-size: 2rem;
    padding: 2rem;
    border: none;
    outline: none;

    resize: none;
}
.priority-color-cont {
    background-color: black;
    height: 100%;
    width: 25%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-evenly;

    /* border-left: 2px solid grey; */
}
.priority-color {
    height: 3rem;
    width: 5rem;
}

.active {
    border: 5px solid white;
}
```


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/507/original/upload_3e8bb8396873f8e8c111a1d49f1090ec.png?1695622404)

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/508/original/upload_e5f73ae79400c2d9d902183a3017e6b7.png?1695622452)


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/509/original/upload_61fea72f891c1de82d9ca74fc7301a13.png?1695622483)


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/510/original/upload_a2b7dfb56f7a3764d60476c0d9252a57.png?1695622550)


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/511/original/upload_887fcda2a48dfb8a78e632f7d833ce47.png?1695622596)


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/512/original/upload_c452abfa78354c4980fd7f93595193ff.png?1695622628)



* The CSS defines styles for a modal container (.modal-cont) that appears to be fixed to the viewport. It's centered both horizontally and vertically and initially hidden (display: none). It has a box shadow for a visual effect.
* A textarea component (.textarea-cont) within the modal is styled to take up a certain percentage of its parent's width and height. It has specific font size, padding, and no border or outline. Resizing is disabled.
* A priority color container (.priority-color-cont) within the modal is styled with a background color, takes up a percentage of its parent's width, and is set to display its child elements in a column layout with even spacing.
* Priority color elements (.priority-color) within the container have fixed dimensions.
* The .active class appears to define a style for an active or highlighted state, where elements with this class have a white border.



### On clicking Plus (**+**)
1. Make modal ->visible
    * Enter the data
    * Event Listener on the button of modal container 
2. When you press enter
    * Set the color given by the modal and the text ->create Ticket
    * Reset teh modal



```javascript
const addBtn = document.querySelector(".add-btn");

// plus -> wala guy
addBtn.addEventListener("click", function () {
    // display's modal
    modal.style.display = "flex";
})
```
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/513/original/upload_d26868b32ec93cc8b460235fac641db0.png?1695622663)


```javascript
//  add event lisetner -> which color will be intial one
prioritySetModal.addEventListener("click", function (e) {
    // console.log("68", e.target);
    if (e.target == e.currentTarget) {
        return;
    }
    currentColor = e.target.classList[1];

    // console.log("currentcolor", currentColor);
    // remove active 
    for (let i = 0; i < priorityColorArray.length; i++) {
        priorityColorArray[i].classList.remove("active")
    }
    // add active to required element
    e.target.classList.add("active");


})
modal.addEventListener("keypress", function (e) {
    console.log("key", e.key);
    if (e.key !== "Enter") {
        return;
    }
    const content = textArea.value;
    createTicket(content, currentColor);
    // reset it's existence 
    textArea.value = "";
    currentColor = "green";
    modal.style.display = "none";

})
```
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/514/original/upload_f4a83eb62db42741be3ae788bda2dd13.png?1695622697)


Let's explain the key actions and interactions in a js code above:

1. **Initialization of Priority Colors:**
   - An event listener is added to an element with the id `prioritySetModal`.
   - This listener responds to clicks within the priority color container.

2. **Initial Color Selection:**
   - When the user clicks a color within the priority color container, the color of the clicked element is captured.
   - The previously selected color (if any) is cleared by removing the "active" class from all color elements.
   - The "active" class is then added to the clicked color element, visually indicating the currently selected color.

3. **Modal Interaction:**
   - Another event listener is added to a modal element (possibly used for creating or editing tasks).
   - This listener responds to keypress events, specifically the "Enter" key (key code).

4. **Handling "Enter" Keypress:**
   - When the user presses the "Enter" key within the modal:
     - The content entered in a textarea (possibly used for task descriptions) is captured.
     - A new task or ticket is created with the captured content and the currently selected color (previously determined in step 2).
     - The textarea is cleared.
     - The default color for the next task is set to "green."
     - The modal is hidden (`display: "none"`).

