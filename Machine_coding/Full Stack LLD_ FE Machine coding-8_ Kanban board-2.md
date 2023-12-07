# Full Stack LLD: FE Machine coding-8: Kanban board-2

---
title: Agenda of the lecture
description: What will be covered in the topic?
duration: 120
card_type: cue_card
---

## Agenda
**Topics to cover in Javascript:**

* Creating a dynamic ticket by adding  feature to that like:
    * color change
    * update
    * delete
    * lock/unlock.
* Implement local storage.

We will try to cover most of these topics in today's sessions and the remaining in the next.


So let's start.

---
title: Create Ticket UI
description: Discussion about the creating the ticket UI  in detail.
duration: 2100
card_type: cue_card
---

## Create Ticket UI

Now for showing and hiding the modal, just double click to show the every element by adding the event listener for the same. For the add button you just firstly go to the priority and displays the modal and coming to the keypress firstly the enter key for hiding it. 

Removing and adding the active ticket functionalities are added to the click event-listener in the priorityColorArray classlist.


Now in the modal eventlistener on click on the enter after inserting the text, for creating the ticket we are having the three parts first the ribbon, then the unique id, then the ticket-text area.

``` javascript
// create ticket fn
function createTicket(content, currentColor,cId, isPending) {
    console.log("ticket with ", content, "and color", currentColor)
    // create a static UI with an Id
    if (content == "")
        return;
    const id = cId||uid();

    const ticketContainer = document.createElement("div");
    ticketContainer.setAttribute("class", "ticket-cont");
    ticketContainer.setAttribute("draggable","true");
    ticketContainer.innerHTML = `<div class = "ticket-cont">
            <div class = "ticket-color ${currentColor}"></div>
            <div class = "ticket-id">#${id}</div>
            <div class = "ticket-area">${content}</div>
            <div class = "lock-unlock">
                <i class = "fa-solid fa-lock"></i>
            </div>
        </div>`;

    /************************************************/

    const ticketColorElem = ticketContainer.querySelector(".ticket-color")
    AddcolorChangeListeners(ticketColorElem,id);

    const ticketArea = ticketContainer.querySelector(".ticket-area")
    const lockBtn = ticketContainer.querySelector(".lock-unlock")
    AddLocknUnlock(ticketArea, lockBtn,id)
    deleteListeners(ticketContainer,id);
    let ticketObj = {
        id:id,
        content:content,
        color:currentColor,
        isPending:true
    }
allTickets.push(ticketObj);
    updateLocalStorage();
}
```


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/515/original/upload_8a330081943725bd7e3cf13beeb043f9.png?1695623281)

So we have to create these parts as follows:
* Create a Static UI with an Id for this will be using the fragments and add that to the main container but here taking some shortcuts we can just append the main-cont by querselecting that.
* To the ticket container we will be passing a div and also pass the ticket-cont so that all childs are linked.
* After that just adding the html element if all the required ticket-id, ticket-color, ticket-area and the unlock/lock button. Pass the color and the content which was received in the function.
* Now appending the ticket-container to the main container. Also add the functionalities of the unique id as discussed in the last class which can be done in the two ways either by the npm module or the cdn link. Paste the id part in the function also for the unique id.


---
title: Color change, Content edit and Delete ticket
description: Discussion about the Color change, Content edit and Delete ticket functions in detail.
duration: 2100
card_type: cue_card
---

## Color Change, Content Edit and Delete Ticket

Now here for the newly added tickets the functionalities of the color change and the lock/unlock does not work, lets start with that functionalities.

### handlePriority function

The `handlePriority` function appears to be part of a larger application that manages tickets and their priority colors. Let's break down the function step by step with explanations for each part:

1. **Function Signature**: 

   ```javascript
   function handlePriority(ticketCont, id) {
      // Function body goes here
   }
   ```

   This function takes two parameters:
   - `ticketCont`: This parameter represents a container element that likely holds a ticket, and the function will perform operations on elements within this container.
   - `id`: This parameter appears to be an identifier for a specific ticket.

2. **Getting the Ticket Color Element**:

   ```javascript
   let ticketColor = ticketCont.querySelector(".ticket-color");
   ```

   Here, the function uses the `querySelector` method to find an element with the class name "ticket-color" within the `ticketCont` container. This element is likely responsible for displaying the color of the ticket's priority.

3. **Adding a Click Event Listener**:

   ```javascript
   ticketColor.addEventListener("click", function() {
      // Event handling logic goes here
   });
   ```

   This code adds a click event listener to the `ticketColor` element. When the user clicks on the priority color of the ticket, this listener will execute the event handling logic defined in the callback function.

4. **Getting the Current Color**:

   ```javascript
   let currentColor = ticketColor.classList[1];
   ```

   This line retrieves the current color of the ticket by accessing the second class from the `classList` of the `ticketColor` element. It assumes that the color is represented as a class.

5. **Finding the Current Color Index**:

   ```javascript
   let currentColorIndex = color.findIndex(function(col) {
      return currentColor == col;
   });
   ```

   This code determines the index of the current color within a predefined `color` array. It uses the `findIndex` method to search for a color in the array that matches the current color.

6. **Calculating the Next Color Index**:

   ```javascript
   let nextColorIndex = (currentColorIndex + 1) % color.length;
   ```

   Here, the function calculates the index of the next color in a circular fashion. It ensures that if the current color is the last color in the `color` array, it wraps around to the first color.

7. **Getting the Next Color**:

   ```javascript
   let nextColor = color[nextColorIndex];
   ```

   This line retrieves the actual color (from the `color` array) that corresponds to the `nextColorIndex`.

8. **Updating the UI and Data**:

   ```javascript
   ticketColor.classList.remove(currentColor);
   ticketColor.classList.add(nextColor);
   let ticketIdx = getTicketIdx(id);
   ticketArr[ticketIdx].color = nextColor;
   ```

   - It removes the current color class from the `ticketColor` element and adds the next color class.
   - It identifies the index of the ticket in the `ticketArr` array using the provided `id`.
   - It updates the color of the ticket in the `ticketArr`.

9. **Logging the Updated Ticket Array**:

   ```javascript
   console.log(ticketArr);
   ```

   Finally, the function logs the updated `ticketArr` to the console, which likely represents the collection of all ticket data.

In summary, the `handlePriority` function is designed to handle the change of priority color for a specific ticket when the user clicks on its color indicator. It accomplishes this by updating the UI and data model, ensuring that the color changes in a circular fashion. The code assumes that the `color` array contains all possible color options for ticket priorities, and it uses this array to determine the next color.

### handleLockUnlock Function
The `handleLockUnlock` function is designed to handle the locking and unlocking of a specific task associated with a ticket. Let's break down the function step by step with explanations for each part:

1. **Function Signature**:

   ```javascript
   function handleLockUnlock(ticketCont, id) {
      // Function body goes here
   }
   ```

   This function takes two parameters:
   - `ticketCont`: This parameter represents a container element that likely holds a ticket, and the function will perform operations on elements within this container.
   - `id`: This parameter appears to be an identifier for a specific ticket.

2. **Getting the Lock/Unlock Button and Task Area Elements**:

   ```javascript
   let lockUnlockBtn = ticketCont.querySelector(".lock-unlock i");
   let taskArea = ticketCont.querySelector(".task-area");
   ```

   These lines find two elements within the `ticketCont` container:
   - **lockUnlockBtn:** This element likely represents a button that allows the user to lock or unlock the associated task.
   - **taskArea:** This element is likely a content area where the task details are displayed and can be edited.

3. **Adding a Click Event Listener to the Lock/Unlock Button**:

   ```javascript
   lockUnlockBtn.addEventListener('click', function() {
      // Event handling logic goes here
   });
   ```

   This code adds a click event listener to the `lockUnlockBtn` element. When the user clicks this button, the listener will execute the event handling logic defined in the callback function.

4. **Locking and Unlocking Logic**:

   ```javascript
   if (lockUnlockBtn.classList.contains('fa-lock')) {
      lockUnlockBtn.classList.remove("fa-lock");
      lockUnlockBtn.classList.add("fa-lock-open");
      taskArea.setAttribute("contenteditable", "true");
   } else {
      lockUnlockBtn.classList.remove("fa-lock-open");
      lockUnlockBtn.classList.add("fa-lock");
      taskArea.setAttribute("contenteditable", "false");
   }
   ```

   Inside the click event listener, this code checks the current class of the `lockUnlockBtn` element. If it contains the class "fa-lock," it means the task is currently locked, and the code changes it to unlocked by updating the classes and setting the `contenteditable` attribute of the `taskArea` to "true." If the button contains the class "fa-lock-open," it means the task is currently unlocked, and the code changes it to locked.

6. **Logging the Updated Ticket Array**:

   ```javascript
   console.log(ticketArr);
   ```

   As a final step, the function logs the updated `ticketArr` to the console, which likely represents the collection of all ticket data.

In summary, the `handleLockUnlock` function allows the user to lock or unlock a task associated with a ticket by toggling the lock/unlock button. When the task is locked, it becomes read-only, and when unlocked, it becomes editable. The function updates the UI and data model accordingly and logs the updated data to the console.

### handleDelete Function

The `handleDelete` function is designed to handle the deletion of a specific ticket when the user clicks on its container. Let's break down the function step by step with explanations for each part:

1. **Function Signature**:

   ```javascript
   function handleDelete(ticketCont, id) {
      // Function body goes here
   }
   ```

   This function takes two parameters:
   - `ticketCont`: This parameter represents a container element that holds a ticket, and the function will perform operations on this container element.
   - `id`: This parameter appears to be an identifier for a specific ticket.

2. **Adding a Click Event Listener to the Ticket Container**:

   ```javascript
   ticketCont.addEventListener("click", function() {
      // Event handling logic goes here
   });
   ```

   This code adds a click event listener to the `ticketCont` element. When the user clicks on the ticket container, the listener will execute the event handling logic defined in the callback function.

3. **Checking a `removeFlag` Condition**:

   ```javascript
   if (removeFlag) {
      // Deletion logic
   }
   ```

   Inside the click event listener, the code checks a condition represented by the `removeFlag` variable. It appears that this condition controls whether the ticket can be deleted or not. If `removeFlag` is `true`, the ticket can be deleted; otherwise, it cannot.

4. **Deleting the Ticket**:

   ```javascript
   ticketCont.remove();
   ```

   If the `removeFlag` condition is met, this line of code removes the `ticketCont` element from the DOM, effectively deleting the ticket container and its contents from the user interface.

5. **Updating the Ticket Array and Local Storage**:

   ```javascript
   let idx = getTicketIdx(id);
   ticketArr.splice(idx, 1);
   ```

   After removing the ticket from the DOM, the code identifies the index of the ticket in the `ticketArr` array using the provided `id`. It then uses the `splice` method to remove one element from the array at that specific index, effectively deleting the ticket data from the array.

6. **Logging the Updated Ticket Array**:

   ```javascript
   console.log(ticketArr);
   ```

   As a final step, the function logs the updated `ticketArr` to the console, which likely represents the collection of all ticket data.


Summarizing all three codes:

``` javascript
//  helper fns of create Ticket
function AddcolorChangeListeners(ticketColorElem,currentId) {
    ticketColorElem.addEventListener("click", toggleColor);
    function toggleColor(e) {
        const cColor = e.target.classList[1];
        let idx = colors.indexOf(cColor);
        let nextIdx = (idx + 1) % colors.length;
        e.target.classList.remove(cColor);
        e.target.classList.add(colors[nextIdx]);
        // set color change in local storage
     let ticketObj = allTickets.find((ticketObject) => {
         return ticketObject.id == currentId;
        })
        // console.log("Hello", ticketidx);
        ticketObj.color = cColor;
        updateLocalStorage();
    }
}

function AddLocknUnlock(ticketArea, lockBtn, currentId) {
    lockBtn.addEventListener("click", handleLock);
    function handleLock() {
        let isLocked = lockBtn.children[0].classList.contains("fa-lock");
        if (isLocked == true) {
            ticketArea.setAttribute("contenteditable", "true");
            lockBtn.children[0].classList.remove("fa-lock")
            lockBtn.children[0].classList.add("fa-lock-open")
        } else {
            ticketArea.setAttribute("contenteditable", "false")
            lockBtn.children[0].classList.remove("fa-lock-open");
            lockBtn.children[0].classList.add("fa-lock");
        }
        let ticketObj = allTickets.find((ticketObject) => {
            return ticketObject.id == currentId;
        })
        ticketObj.content = ticketArea.textContent;
        updateLocalStorage();
        
    }
}

function deleteListeners(ticketContainer,currentId) {
    ticketContainer.addEventListener("click", function () {
        if (deleteFlag) {
            ticketContainer.remove();
            const restofTickets = allTickets.filter((ticketObject) => {
                return ticketObject.id != currentId;
            })
            allTickets = restofTickets;
            updateLocalStorage();

        }
    })

}
```

In summary, the `handleDelete` function allows the user to delete a specific ticket by clicking on its container. It checks a condition (`removeFlag`) to determine if deletion is allowed, and if so, it removes the ticket container from the DOM, updates the data model, logs the updated data, and presumably updates local storage to persist the changes.


---
title: Intution behind the local storage
description: Discussion about the various characteristics of local storage in detail.
duration: 2100
card_type: cue_card
---

## Intution Behind the Local Storage

 Local storage is a client-side storage mechanism, and it doesn't directly interact with a server or share data across multiple users. Let's explore the theoretical explanation of using local storage in the context of a client-server architecture, taking into account the facts you've mentioned:

1. **Client-Server Architecture**:
   
   In a client-server architecture, there are two primary components: the client, which is typically a user's device (e.g., a web browser), and the server, which hosts the application or website. The client communicates with the server to request and receive data. However, local storage is a feature exclusive to the client-side, meaning it's only accessible and relevant within the user's browser on their device.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/516/original/upload_e61955570a77d68359c07bcee1416e4d.png?1695623533) 

   Local storage is a web storage mechanism provided by modern web browsers. It allows web applications to store small amounts of data on the user's device. This data is stored in the browser and persists even after the user closes the browser or navigates away from the website.
   
   Local storage is known for its lightning speed because it's a part of the user's browser. Reading and writing data from/to local storage is much faster compared to making network requests to a server. 

The URL of "scaler.com" is likely the website's domain. Local storage is scoped to a specific domain. Data stored in local storage for "scaler.com" will only be accessible when a user visits web pages under that domain.

Different websites have their own isolated local storage, so data stored on one website's domain is not accessible by websites with different domains.


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/517/original/upload_c636927dbba2630d177f17e0c8db76e8.png?1695623560<br>)


   Local storage is not designed for sharing data among multiple users. Each user's local storage is unique to their own device and is not accessible or shared with other users. Data stored in local storage is private to the user and cannot be seen or modified by other users, even if they visit the same website.

Local storage is a client-side web storage mechanism provided by modern web browsers. Its major characteristics include:

- **Persistence**:<br> Data stored in local storage persists across browser sessions and even when the user closes the browser. 
- **Storage Limit**:<br> Local storage has a size limit, which varies from browser to browser but is typically around 5-10 megabytes per domain. 
- **Key-Value Pairs**:<br> Local storage is essentially a collection of key-value pairs, where each key is a unique identifier for the data, and the value is the data itself.
- **Usecases** :
    - **Caching**:<br> Local storage is commonly used for caching data that can be retrieved quickly without making additional server requests, improving the performance and responsiveness of web applications.
    - **User Preferences**:<br> It's used to store user preferences and settings, allowing users to customize their experience on a website.



---
title: The local storage code example
description: Discussion about the example code for the local storage in detail.
duration: 2100
card_type: cue_card
---

## Local Storage Code Example

Here's a detailed explanation of the code:

```htmlembedded
<!DOCTYPE html>
<html>
<head>
    <title>Local Storage Example</title>
</head>
<body>
    <h2>Local Storage Example</h2>

    <ul>
        <li>I am a list Item</li>
        <li>I am a list Item</li>
        <li>I am a List Item</li>
        <li>I am a list Item</li>
    </ul>

    <button id = "read">Read</button>
    <button id = "remove">Remove</button>
    <button id = "add">Add</button>

    <script>
        // Retrieve references to the HTML elements we want to interact with.
        const readBtn = document.querySelector("#read");
        const removeBtn = document.querySelector("#remove");
        const addBtn = document.querySelector("#add");
        const allLis = document.querySelectorAll("li");

        // Event listener for the "Read" button.
        readBtn.addEventListener("click", function () {
            // Create an array to store the text content of list items.
            const locaData = [];

            // Iterate through all list items and collect their text content.
            for (let i = 0; i < allLis.length; i++) {
                const textInside = allLis[i].innerText;
                locaData.push(textInside);
            }

            // Store the collected data in local storage as a JSON string.
            localStorage.setItem("listData", JSON.stringify(locaData));
        });

        // Event listener for the "Remove" button.
        removeBtn.addEventListener("click", function () {
            // Remove the data associated with the "listData" key from local storage.
            localStorage.removeItem("listData");
        });

        // Event listener for the "Add" button.
        addBtn.addEventListener("click", function () {
            // Retrieve data from local storage associated with the "listData" key.
            const storedData = localStorage.getItem("listData");

            // If there's data in local storage, parse it and add it as new list items.
            if (storedData) {
                // Parse the stored JSON data back into an array.
                const parsedData = JSON.parse(storedData);

                // Iterate through the parsed data and create new list items.
                parsedData.forEach(function (text) {
                    const li = document.createElement("li");
                    li.innerText = text;
                    // Append the new list item to the existing unordered list.
                    document.querySelector("ul").appendChild(li);
                });
            }
        });
    </script>
</body>
</html>
```


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/518/original/upload_3d69e26c9fd708fd4d6f94924d526502.png?1695623688)



---
title: Adding the local storage to the project
description: Discussion about the various usage and the functionalities of the local storage in project in detail.
duration: 2100
card_type: cue_card
---

## Adding the Local Storage to The Project

In the project, local storage is used to store and retrieve data related to tickets. Here's an explanation of how local storage is utilized within each function:

```javascript
/**
 * 1. Toggle editable behaviour
 * 2. change the color on click
 * */
/*****************Given ticket selection****************************/




const toolBoxPriorityContainer = document.querySelector(".toolbox-priority-cont");

const addBtn = document.querySelector(".add-btn");
const deleteBtn = document.querySelector(".remove-btn");

const modal = document.querySelector(".modal-cont");
const prioritySetModal = modal.querySelector(".priority-color-cont");
const textArea = modal.querySelector(".textarea-cont");
const priorityColorArray = modal.querySelectorAll(".priority-color");


const pendingContainer = document.querySelector(".pending-cont");
const finishedContainer = document.querySelector(".finished-cont");



let colors = ["pink", "blue", "purple", "green"];
let currentColor = "green";
const uid = new ShortUniqueId();
let deleteFlag = false;
let allTickets = localStorage.getItem("localTickets") || [];
let isFromLocalStorage = false;
if(typeof allTickets == "string"){
    allTickets = JSON.parse(allTickets);
    populateUI();
}




function populateUI(){
    isFromLocalStorage = true;
    for(let i = 0; i < allTickets.length; i++){
      let ticketObj =  allTickets[i];
        createTicket(ticketObj.content, ticketObj.color,ticketObj.id, ticketObj.isPending);
    }

    isFromLocalStorage = false;
}

/************change color /filter ticket****************************************/
toolBoxPriorityContainer.addEventListener("click", function (e) {
    if (e.target == e.currentTarget) {
        return;
    }
    const curentColorelem = e.target;
    const cColor = curentColorelem.classList[1];
    const ticketArr = document.querySelectorAll(".ticket-cont");
    for (let i = 0; i < ticketArr.length; i++) {
        const ticketColorElem = ticketArr[i].querySelector(".ticket-color");
        let cTicketsColor = ticketColorElem.classList[1];
        if (cTicketsColor !== cColor) {
            ticketArr[i].style.display = "none";
        } else {
            ticketArr[i].style.display = "block";
        }
    }


})
toolBoxPriorityContainer.addEventListener("dblclick", function (e) {
    if (e.target == e.currentTarget) {
        return;
    }
    const ticketArr = document.querySelectorAll(".ticket-cont");
    for (let i = 0; i < ticketArr.length; i++) {
        ticketArr[i].style.display = "block";
    }
})
/******************** modal and ticket creation********************************/

// plus -> wala guy
addBtn.addEventListener("click", function () {
    // display's modal
    modal.style.display = "flex";
})
deleteBtn.addEventListener("click",() => {
    deleteBtn.style.color = "red";
   deleteFlag =! deleteFlag;
})

// 1. make modal -> visible 
// enter the data
// event listener on the button of modal 
// 2. when you press enter  
// set  the color given by  modal and the text -> create ticket
// reset Modal 
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



modal.addEventListener("keydown", function (e) {
    console.log("key", e.key);
    if (e.key == "Escape"){
        textArea.value = "";
        currentColor = "green";
        modal.style.display = "none";
        return;
    }
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
// create ticket fn
function createTicket(content, currentColor,cId, isPending) {
    console.log("ticket with ", content, "and color", currentColor)
    // create a static UI with an Id
    if (content == "")
        return;
    const id = cId||uid();

    const ticketContainer = document.createElement("div");
    ticketContainer.setAttribute("class", "ticket-cont");
    ticketContainer.setAttribute("draggable","true");
    ticketContainer.innerHTML = `<div class = "ticket-cont">
            <div class = "ticket-color ${currentColor}"></div>
            <div class = "ticket-id">#${id}</div>
            <div class = "ticket-area">${content}</div>
            <div class = "lock-unlock">
                <i class = "fa-solid fa-lock"></i>
            </div>
        </div>`;

    if (isFromLocalStorage && isPending == false){
        finishedContainer.appendChild(ticketContainer);
        }else{
            pendingContainer.appendChild(ticketContainer);

        }

    /************************************************/

    const ticketColorElem = ticketContainer.querySelector(".ticket-color")
    AddcolorChangeListeners(ticketColorElem,id);

    const ticketArea = ticketContainer.querySelector(".ticket-area")
    const lockBtn = ticketContainer.querySelector(".lock-unlock")
    AddLocknUnlock(ticketArea, lockBtn,id)
    deleteListeners(ticketContainer,id);

    if (isFromLocalStorage)
        return;
    /********save it to localStorage********/ 
    
    let ticketObj = {
        id:id,
        content:content,
        color:currentColor,
        isPending:true
    }
allTickets.push(ticketObj);
    updateLocalStorage();
}
//  helper fns of create Ticket
function AddcolorChangeListeners(ticketColorElem,currentId) {
    ticketColorElem.addEventListener("click", toggleColor);
    function toggleColor(e) {
        const cColor = e.target.classList[1];
        let idx = colors.indexOf(cColor);
        let nextIdx = (idx + 1) % colors.length;
        e.target.classList.remove(cColor);
        e.target.classList.add(colors[nextIdx]);
        // set color change in local storage
     let ticketObj = allTickets.find((ticketObject) => {
         return ticketObject.id == currentId;
        })
        // console.log("Hello", ticketidx);
        ticketObj.color = cColor;
        updateLocalStorage();
    }
}

function AddLocknUnlock(ticketArea, lockBtn, currentId) {
    lockBtn.addEventListener("click", handleLock);
    function handleLock() {
        let isLocked = lockBtn.children[0].classList.contains("fa-lock");
        if (isLocked == true) {
            ticketArea.setAttribute("contenteditable", "true");
            lockBtn.children[0].classList.remove("fa-lock")
            lockBtn.children[0].classList.add("fa-lock-open")
        } else {
            ticketArea.setAttribute("contenteditable", "false")
            lockBtn.children[0].classList.remove("fa-lock-open");
            lockBtn.children[0].classList.add("fa-lock");
        }
        let ticketObj = allTickets.find((ticketObject) => {
            return ticketObject.id == currentId;
        })
        ticketObj.content = ticketArea.textContent;
        updateLocalStorage();
        
    }
}

function deleteListeners(ticketContainer,currentId) {
    ticketContainer.addEventListener("click", function () {
        if (deleteFlag) {
            ticketContainer.remove();
            const restofTickets = allTickets.filter((ticketObject) => {
                return ticketObject.id != currentId;
            })
            allTickets = restofTickets;
            updateLocalStorage();

        }
    })

}

function updateLocalStorage(){
    localStorage.setItem(  "localTickets" ,JSON.stringify( allTickets));
}

/*********************************************/ 




// function toggleColor(e) {
//     const cColor = e.target.classList[1];
//     let idx = colors.indexOf(cColor);
//     let nextIdx = (idx + 1) % colors.length;
//     ticketColor.classList.remove(cColor);
//     ticketColor.classList.add(colors[nextIdx]);
// }


```
