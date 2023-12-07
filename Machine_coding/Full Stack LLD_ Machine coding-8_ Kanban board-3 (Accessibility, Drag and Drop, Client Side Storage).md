# Full Stack LLD: Accessibility, Drag and Drop, Client Side Storage

---
title: Agenda of the lecture
description: What will be covered in the topic?
---

## Agenda
* Drag and Drop Functionality
* Accessibility
    * Align the page readers
    * keyboard navigation

**Note:** Make our application of drag and drop worthy

---
title: Building UI for finished and pending 
description: Building UI for the two sections of the project finished and pending.
 
---

## Building UI for finished and pending 

* Present an overview of the project as an importantlication of drag and drop functionality.
*  We can create notes inside the pending section and these notes are dragable to the finished section. 

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/376/original/upload_09535d4cfdb8cda05565ff00518daf95.png?1695183893)


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/377/original/upload_dd1415996b29f922806764280ff85689.png?1695183933)

* We have the HTML structure prepared as per the above wireframe to save time.
* We will start with the CSS part.

* Start with giving the `width` property to all the containers.
* For heading containers we will give `display: flex` and `text-align: center` and some margin.

**Main Container:**
```css
.main_cont{
    width: 100vw;
}
```

**Heading Container:**
```css
.heading_cont{
    width: 100%;
    display: flex;
    text-align: center;
    margin-top:2rem; 
}
```

**Pending Header:**
```css
.task_heading--pending{
    width: 70%;
}
```

**Finished Header:**
```css
.task_heading--finished{
    width: 30%;
}
```

* Next we will have wrapper containers, pending, and finished containers.

**Wrapper Container:**
```css
.container_wrapper{
    width: 100%;
    display: flex;
}
```

**Pending Container:**
```css
.pending-cont{
    width: 70%;
    display: flex;
    justify-content: space-evenly;
    flex-wrap: wrap;
    flex-direction: row;
    gap: 2rem;
    border-right: 2px solid gray;
}
```

**Finished Container:**
```css
.finished-cont{
    width: 30%;
    display: flex;
    flex-direction: column;
    gap: 2rem;
    justify-content: space-evenly;
    align-items: center;
}
```
* We are using flex to align the finished container in the form of a stack.

### Flex and Justify
**Default:**
* By default justify-content works horizontally and align items work vertically.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/378/original/upload_4d8282e75cdf0f743110b61de8483c73.png?1695184006)

**Changing direction to column**
* Justify content will work vertically and align will work on horizontal content.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/379/original/upload_d5aad990cbd1ab366e632e925270b06a.png?1695184032)

---
title: Drag and Drop Example
description: 
---

## Drag and Drop

### HTML
```htmlembedded
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Drag and Drop Example</title>
    <style>
.container {
    display: inline-block;
    border: 1px solid #ccc;
    width: 200px;
    margin: 10px;
    padding: 10px;
}

.box  {
    height:30px;
    padding: 5px;
    margin: 5px;
    background-color: #f4f4f4;
    border: 1px solid #ddd;
    cursor: grab;
    
}
#container1>.box{
    background-color: lightcoral;
}
#container2>.box{
    background-color: lightgreen;
}
    </style>
</head>

<body>
    <h1>Drag and Drop Example</h1>

    <div class = "container" id = "container1">
        <!-- 1st -> add an attribute draggable=true -->
        <div class = "box" draggable = "true">Box 1</div>
        <div class = "box" draggable = "true">Box 2</div>
        <div class = "box" draggable = "true">Box 3</div>
    </div>

    <div class = "container" id = "container2">
        <div class = "box" draggable = "true">Box A</div>
        <div class = "box" draggable = "true">Box B</div>
    </div>

    <script src="drag_drop.js"></script>
</body>

</html>
```

To implement drag and drop we need to add an attribute `draggable=true`.

### JavaScript
```javascript
// console.log("Js attached");
const containers = document.querySelectorAll('.container');
let draggedBox = null;
containers.forEach((container) => {
    // info whenever drag behaviour starts 
    container.addEventListener("dragstart", (event) => {
        console.log("Drag is started on ", container);
        draggedBox = event.target;
        event.target.style.opacity = "0.5";
    })
    // when you are dragging over a drop point, only triggered when you are in draggable area  
    container.addEventListener("dragover", (event) => {
        event.preventDefault();
        // console.log("Dragging is going on ", container);
    })
    // when you either leave the draggable container/press esc
    container.addEventListener("dragend", (event) => {
        console.log("Dragging is finished", container);
        event.target.style.opacity = "1";

    })
    // drop represent -> when you drop a draggable element in a drop zone
    container.addEventListener("drop", (event) => {
        console.log("Drop happened");
        if (draggedBox) {
            container.appendChild(draggedBox);
        }
    })

})
```
1. The `draggable="true"` attribute is added to each element to make them draggable.
2. The `dragstart` event is triggered when the user starts dragging a task. It is only called once
3. The `dragover` event is used to indicate that a draggable element is being dragged over a valid drop target. It is called continuously.
4. The `drop` event is triggered when the user drops a draggable element onto a drop target.
5. The `dragend` event is triggered when the drag operation ends. it is when the whole iteration is finished

### Difference between dragend and drop
**Cronological order:**
* drag started
* drop happened
* drag end


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/380/original/upload_1a78a26d5f636e359cde23ba39672b92.png?1695184196)

* `dropend` doesn't care about where you dropped, it simply triggers when you are holding a draggable element and you drop it in any region.
* The `dragend` only triggers when we drop it in a draggable area.

### Moving element from one box to another
* We will have a variable `draggedbox` and store the box which is dragged.
* **[Ask the learners]**
When will you know that we have dropped in the valid area? Using `drop` or `dragend`?
-> Exactly, drop.

* We will append the child to the container it is dropped in.
* To avoid confusion while dragging (just to be sure which element we dragged), we can slightly change the opacity of the box.

---
title: Adding Drag and Drop to Kanban box
description: 

---

### Adding Drag and Drop to the Kanban box

* **[Ask the learners]** 
What should be the first step to make them draggable?

-> making `draggable=true`

* making the `cursor: grab` for ticket.

```javascript
// console.log("Js attached");
const containers = document.querySelectorAll('.container');
let draggedBox = null;
containers.forEach((container) => {
    // info whenever drag behaviour starts 
    container.addEventListener("dragstart", (event) => {
        console.log("Drag is started on ", container);
        draggedBox = event.target;
        event.target.style.opacity = "0.5";
    })
    // when you are dragging over a drop point, only triggered when you are in draggable area  
    container.addEventListener("dragover", (event) => {
        event.preventDefault();
        // console.log("Dragging is going on ", container);
    })
    // when you either leave the draggable container/press esc
    container.addEventListener("dragend", (event) => {
        console.log("Dragging is finished", container);
        event.target.style.opacity = "1";

    })
    // drop represent -> when you drop a draggable element in a drop zone
    container.addEventListener("drop", (event) => {
        

        console.log("Drop happened");
        if (draggedBox) {
            container.appendChild(draggedBox);

            // identify your parent
            const isPending = container.classList[0] == "pending-cont" ? true: false;
            // dragged box -> ticket ->id
            const cId = draggedBox.querySelector(".ticket-id").innerText.split("#")[1];
            // `#1234` -> ['',1234]
            console.log("cId", cId);
            //Compare that ID with every object 
            // console.log(allTickets);
            let ticketObj = allTickets.find((ticketObject) => {
                return ticketObject.id == cId;
            })
            // console.log("cTicketObjet", ticketObj)
            ticketObj.isPending = isPending;
            // function statement
            updateLocalStorage();

        }
    })

})
```

* The same code of drag and drop example is copied here by changing the class and adding a draggable attribute.

**[Ask the learners]**
* Whenever we are dragging and dropping the card from pending to finished all the functionalities are the same, we can change the color, lock and unlock it, etc. How can we store them differently?

-> We can store all the tasks in an array. Along with everything else, we can add an extra flag `isPending` for whether it is pending or finished.

**[Ask the learners]**
Where you will write the code?

-> In development there are two things, identify the algorithm and identify where to apply that.
-> In the `drag_drop.js`  file we will change the state. Rest changes must be made in the `script.js` file.

**When the ticket is dropped:**
* Go to the container and check if it is a pending container or not.
* Get the ticket id and find the required ticket from the array using the `ticketId`.
* Lastly set the `isPending` variable for this `ticketObj`  and update local storage.

There were two problems: 
1. Drag and drop just changed the UI.
2. We were just rendering in the pending part
    * We added local storage
        * add property isPending
        * Always update 
        * When you rerender it should work

How we pulled it off:
* When I get a ticket and it is not from local storage we are adding a property `isPending`.
* Next, we go to drag and drop -> wherever the ticket is dragged and dropped we are changing the `isPending` property. UI part will be updated by drag and drop.
* We will go to the parent element where the ticket is dropped to find out whether it is pending or finished.
* Next we will get the ID of the ticket. Using the id we will search for the ticket object from the localstorage.
* Then `ticket.isPending` is set. And update these changes.
* While populatingUI, pass an extra parameter attribute `isPending` while we are creating the ticket.
* Once the ticket is created, if it is from local storage and `isPending == false` then we will append it in `finishedContainer` else we will append it in `pendingContainer`.

---
title: Accessibility
description: To make your application accessible to people with special abilities

---

## Accessibility 
* If your website works well on Keyboards and Screen reader
* People with sight disabilities

## Screen reader Friendly


### Semantic Tag
Like **Header**, **Footer**, etc. tags must be used for the page so that it is easier for the screen reader.


### ARIA -> attributes
*  **aria-label** : Use this attribute to provide a concise label for an element that may not have a visible label. It helps screen reader users understand the purpose of the element.
* **aria-live: polite**, **assertive** Use this attribute to indicate that an element's content should be announced by screen readers as it changes dynamically. This is useful for displaying real-time updates.
* **aria-hidden Attribute:** Use this attribute to indicate that an element should be hidden from screen readers. It's often used to hide decorative or redundant

### role : 
* It is an attribute that is added to a div to explain its attributes

## Keyboard Navigation 
*  Tab and escape should work 
   *  we should be able to move(focus) across element using tab -> in an intuitive 
   * When you click on escape if you have a modal/POPUPS then it should be hidden

https://docs.google.com/document/d/1Gn2ib5YhhpcFUiWGAUbCpg0ZPh3m_wSA-9IolGMjkIE/edit#heading=h.hteovoit9b96