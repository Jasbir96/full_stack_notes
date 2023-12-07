# Full Stack LLD: FE Machine coding-5: HTTP and network Optimization
---
title: Agenda
description: Previous class brief and what we will discuss in this lecture

---

In this lecture we will discuss the following topics:
- Bishop on Chessboard
- HTTP and its features (protocol, client, server, request method, response, features of HTTP Request)

> Let's start the lecture!

---
title: Bishop Chessboard
description: How to create a UI for a chessboard (8 * 8 square)
---

- A chessboard contains 8*8 square boxes in white and black color.
- First, create an HTML file and a CSS file.
- Coming to the HTML file.

## Question:
Make a chessboard and highlight all the possible paths for a bishop. It will consist of:
1. 8*8 = 64 boxes
2. n*n boxes


### Steps:
- Create a heading in the class "heading" with text **Chessboard** inside the `h1` tag
- Then we need to table using the `tr` tag for 8 columns.

Then we will use the CSS part to style these boxes and arrange them into a chessboard. Go to the CSS file.

- Copy the CSS file as previous lecture and then start adding the following CSS to it.

```htmlembedded
.heading{
    text-align: center;
}

.table{
    margin: 0 auto;
}
```
After table class, add CSS for boxes:

```htmlembedded
.box{
    background-color : lightblue;
    width: rem;
    height: 5rem;
    border: 1px solid gray;
}
```
- Then use the **cellspacing** in the HTML file for the boxes and put the value 0. That will merge the boxes with **0** spacing between them.
```htmlembedded
 <table id = "table" class = "table" cellspacing = "0">
```

- To style the table, create classes for every **tr** and then use the following CSS;
```css
.white{
    background-color: white;
}

.black{
    background-color: black;
}
```
FINAL

```htmlembedded
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X - UA - Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
    <link rel = "stylesheet" href = "style.css">
</head>

<body>
    <!-- 
        Chess board : Highlight all the possible path for a bishop
                ## variations
                1. 8*8 -> 64
                2. n*n -> 
     -->
    <h1 class = "heading">Chessboard</h1>
    <table id = "table" class = "table" cellspacing = "0">
    </table>
    <script src = "chessBoard.js"></script>
</body>

</html>
```


#### Now let us build this using the JS.
- Create a separate JavaScript file.
- In JS, we will create a function that will create the whole chessboard. 
- Add a **eventListener** to load the function each time a box gets created.
- Use for loop to do this.
- Then create a `**tr** = document.createElement`` and inside it put the **tr**.
 
```javascript
// init -> fn -> build whole chessboard
window.addEventListener("load", function () {
    let table = document.querySelector("#table");
    // chess grid creation  
    for (let ri = 0; ri < 8; ri ++ ) {
        let tr = document.createElement("tr");
        let white = ri % 2 == 0 ? true : false;
        // cell loop
        for (let ci = 0; ci < 8; ci ++ ) {
            let cell = document.createElement("td");
            cell.setAttribute("class", `box ${white == true ? "white" : "black"}`);
            // cell.innerText = `${ri}-${ci}`;
            cell.setAttribute("data-index", `${ri}-${ci}`)
            tr.appendChild(cell);
            white = ! white;
        }
        table.appendChild(tr);
    }

```

This will basically create the box and it will look like this:


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/371/original/upload_f23444380e70e6a2bc1fbbfab19d8257.png?1695183271)


- Now we have the basic building block for the chessboard.
- We need to decide the boxes to be white and black alternatively.
- To make the first box cell as white and then black use this code:

```javascript
let white = ri % 2 == 0 ? true : false;
```
For every iteration you need to choose white =! white, then make it black.

```javascript
cell.setAttribute("class", `box ${white == true ? "white" : "black"}`);
```
The output will look like this:


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/372/original/upload_40138b1cae6c6c267b3eb103827b2f9a.png?1695183294)




####  Now we need to identify the hovering over the cell
- To identify which on which should we need to apply eventListener.
- Use the `data index` to identify each cell individually.  

```javascript
  table.addEventListener("mouseover", function (e) {
        // console.log(e.target);
        console.log(e.target.dataset.index);
        let dataIndex = e.target.dataset.index;
        let [cRi, cCi] = e.target.dataset.index.split("-").map((idx) => idx);
        //Remove color from every  box;
        for (let i = 0; i < boxesArr.length; i ++ ) {
            boxesArr[i].classList.remove("yellow");
```

- Here we are creating an event listener for the mouseover event on the chessboard cells.
- This is done by adding an event listener to the table object that has been created in this line of code.
- table.addEventListener`("mouseover", function (e) {}`.
- The next line of code logs out what e is, which is a reference to the element that was clicked on and its dataset index number, which can be found in e's target property.
- console.log(e); // console log out what **e** is The next line of code gets rid of all yellow boxes from every box array by using classList and removing "**yellow**" from each item's class list: for `(let i = 0; i < boxesArr.length; i++) {boxesArr[i].classList .remove("yellow");}`
- The code removes the yellow color from every box in the table.



#### Identifying the possible path when hovering over a cell of the chessboard.

- First, we need to guess all the possible directions in which the bishop can go from that particular cell/
- Next thing we need storage in which all these tasks will be stored.

```javascript
 let storage = {};
        storage[dataIndex] = true;

        console.log("35", storage);
        findTopLeft(cRi, cCi, storage)
        findTopRight(cRi, cCi, storage)
        findBottomLeft(cRi, cCi, storage)
        findBottomRight(cRi, cCi, storage);

        // color wherever -> dataIndex;
        for (let i = 0; i < boxesArr.length; i ++ ) {
            let cDataIndex = boxesArr[i].dataset.index;
            if(storage[cDataIndex] == true){
                // color it 
                boxesArr[i].classList.add("yellow");
            }
```
- Inside the storage, we will use data index **true** for each cell where the mouse is hovering. then make the lop for coloring the possible path cells.
- Then we need to create four functions findTopLeft(), findTopRight(), findBottomLeft(), and findBottomRight()  as shown below:
- And we will also pass the **storage** inside the function.


```javascript
function findTopLeft(cRi, cCi, storage) {
        cRi -- ;
        cCi -- ;
        while (cRi >= 0 && cCi >= 0) {
            let dataIndex = `${cRi} - ${cCi}`;
            storage[dataIndex] = true;
            cRi -- ;
            cCi -- ;
        }

    }

    function findTopRight(cRi, cCi, storage) {
        cRi -- ;
        cCi ++ ;
        while (cRi >= 0 && cCi <= 7) {
            let dataIndex = `${cRi}-${cCi}`;
            storage[dataIndex] = true;
            cRi -- ;
            cCi ++ ;
        }

    }

    function findBottomLeft(cRi, cCi, storage) {
        cRi ++ ;
        cCi -- ;
        while (cRi <= 7 && cCi >= 0) {
            let dataIndex = `${cRi} - ${cCi}`;
            storage[dataIndex] = true;
            cRi ++ ;
            cCi -- ;
        }

    }
    function findBottomRight(cRi, cCi, storage) {
        cRi ++ ;
        cCi ++ ;
        while (cRi <= 7 && cCi <= 7) {
            let dataIndex = `${cRi} - ${cCi}`;
            storage[dataIndex] = true;
            cRi ++ ;
            cCi ++ ;
        }

    }
```

- These four functions will help to identify where we need to highlight the path and will highlight the **possible  path** with **yellow** color.
- Brief explanation for all the four functions:

1. `findTopLeft()`: This JavaScript function, findTopLeft, starts from a given grid position (**cRi**, **cCi**) and moves diagonally up and to the left while staying within the grid boundaries (up to row 0 and leftmost column 0). It marks each visited position by setting it to true in a storage object. This process continues until the function reaches the grid boundaries.


2. `findTopRight()`: This JavaScript function, findTopRight, starts from a given grid position (**cRi**, **cCi**) and moves diagonally up and to the right while staying within the grid boundaries (up to row 0 and rightmost column 7). It marks each visited position by setting it to true in a storage object. This process continues until the function reaches the grid boundaries. 

3. `findBottomLeft()`: The function findBottomLeft starts from a specified grid position (**cRi**, **cCi**) and moves diagonally down and to the left within the grid boundaries. It marks each visited position as true in a storage object and continues until it reaches the bottom-left corner of the grid (**row 7 and column 0**).


4. `findBottomRight()`: The function findBottomRight starts at a given grid position (**cRi**, **cCi**) and moves diagonally down and to the right within the grid boundaries. It marks each visited position in a storage object as true and continues until it reaches the bottom-right corner of the grid (row 7 and column 7).




### Final code:
The final javascript code will look like this: 
- https://github.com/Jasbir96/Full_stack_may_2023/blob/master/Frontend_MC/Lecture_5_FE_MC__HTTP/1_bishop/chessBoard.js#L34

> Go to this GitHub link to see the source code.

---
title: HTTP Protocol
description: What are HTTP Protocol and how the communication happens over the internet
---
Now, we will discuss what are the HTTP protocols.

## HTTP Protocol
- HTTP stands for HyperText Transfer Protocol.
- It is a set of rules for transferring data from one computer to another using the internet.


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/373/original/upload_c3d88d5f03fa70a52197466b30b32e4a.png?1695183465)


There are two things. One is the Server and the other is the Client. The exchange of data happens between them.

### HTTP Request
The HTTP Request looks like this.


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/374/original/upload_7ba95412ad830fceece45c875bea265b.png?1695183519)


The parts of the HTTP request are:

1. Method: It describes the specific type of work that you want to do.
2. Path: It is basically the directory for the Host.
3. Version of the Protocol: It starts from (0.9 to 3.0) and there are many variations of this.
4. Host: It is the homepage of the URL that you request over the internet.

### HTTP Response
The HTTP response looks like this.


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/375/original/upload_b48d4148b9ca83054b003861521de03e.png?1695183564)


The parts of the HTTP Response are:

1. Version of the protocol: The version that you are currently using.
2. Status message: It basically tells us whether the task is done or not.
3. Status code: What kind of task is being done?
 

- The **HTTP Packet** consists of two pats that are **Header** and **Body**.
- The **header** contains the metadata of the request.


Now, let us see the HTTP methods or Request methods.

### HTTP Methods:

1. **GET**: To get the content from the server.
2. **POST**: To add some content to the server.
3. **PATCH**: To update some data on the server.
4. **PUT**: It is used to replace the content with new content.
5. **OPTIONS**: It helps to get you all the communication options or methods.
6. **CONNECT**: Used to pre-connect to the server or for network optimization. Mostly used in the HTTP case to form a TCP connection.


### Response code:
- It ranges from 100 - 500.
- It basically indicates whether specific HTTP requests have been successfully completed.
- There are many response codes that refer to some specific information.
> Refer to the internet (**Wikipedia**) to see and read about the various response codes in HTTP.


### HTTP Status Code
- An HTTP status code is a message a website's server sends to the browser to indicate whether or not that request can be fulfilled.
> Refer to the internet (**Wikipedia**) to see and read about the various response HTTP Status codes.

## Features of the HTTP protocols
The two most basic and important principles of the HTTP are:
1. **HTTP is stateless**. It means the server and user can not remember the previous request of the client.
2. **HTTP have headers**. You can use this header to add features related to cookies, securities, caching etc.


> That is all about today's lecture. Happy learning!