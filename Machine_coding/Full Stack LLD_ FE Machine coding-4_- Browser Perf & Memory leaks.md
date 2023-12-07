# Full Stack LLD: FE Machine coding-4:- Browser Perf & Memory leaks

---

description: Things that will be covered in today's session
---

# Agenda

Today we will discuss:- 
- Nested Comment box
- JS performance 
- Memory leaks

## GitHub Link for today's session:

https://github.com/Jasbir96/Full_stack_may_2023/tree/master/Frontend_MC/Lec_5_FE_MC_browser_perf_n_mem_leaks/2_Memory_leaks

Let us start first with the nested comment box and how to implement it.

---
title: Nested comment box
description: What is a nested comment box problem description
---


# Definition

It is basically a series of comments nested together or multiple replies, or we can say that replies for the replies. 
- For real-life example:
1. You can comment on a social media post, and then there are more people who can reply to your reply.
2. Reddit threads

- You can also use the nested comments in some cases like when you will be provided with JSON data and you have to perform file system operations (displaying file navigation).

- In the demo shown below, you can see the nested comments:


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/358/original/upload_3ab3b820037a68132e446d138f89a5f5.png?1695182483)


Now, let us see a problem related to the nested comment box.

---
title: Nested comment box
description: Writing the code 

---

## Problem statement
Input box: produces a comment box and that comment can have the option to reply. And every reply can have multiple replies.

## Solution
- For this problem, first, we will create a basic HTML file, and inside the body tag, we will create a **div class** "container". 
- And inside the div class, again create a div class "comment-container". Again one more div class "comment-card". 
- Inside this add the message in **h3** text for example, **Good Morning, How are you ?**
- Then add a div class for "reply".

The solution will look like this:


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/359/original/upload_bb86f8f3e79365f643cf582eb97eaa86.png?1695182511)


Then we also need a submit button to add the replies. You can add input type and button by writing the code as shown below:


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/360/original/upload_3c222d9282ade982efc73e5004118ecd.png?1695182534)


> In this example, we have a basic comment that is taken as input and displayed. There is no involvement of the backend in this example.

Now, let us add some CSS to style the content.

- For CSS, add a CSS file "style.css" as shown in the lecture video, and link to the HTML file using a link tag.

- Adding CSS to the comment box:

```htmlembedded
.container{
    padding: 1rem 3rem;
}

.comment-container{
    padding-left: 1rem;
    padding-top: 1rem;
    padding-bottom: 0;
    border-left: 1px solid black;
}

.comment-text{
    margin-bottom: 0.5rem;
}

.comment-card{}

.reply{
    color: red;
    padding-left: 1rem;
    font-weight: 600;
    margin-bottom: 1rem;

}

```
Add a comment reply container by adding the code shown below:


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/363/original/upload_7049a24ce239aaeea6aa39987e7c6d24.png?1695182581)


- Then add CSS for this:

```htmlembedded
.comment-reply-container{
    margin-left: 4rem;
}
```

That is how you can add **nested comment boxes** and style them.

> Ask the students about the difference between the **comment** or **reply** in this example.
> **Ans**:- No, the structure and the action are the same here for comment and reply.
 

- You can also add some replies as shown below to perform more variations of comments and replies.


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/365/original/upload_ff1ec0f2d4f1855157dbfb2b05edc3d0.png?1695182622)


- Add the CSS for the comment container in the stylesheet.css using "**not selector**"
```htmlembedded
.comment-container: not(:first-child){
    margin-left: 4rem;
}
```

## Note:
1. Every comment --> reply is its children
2. Every reply --> sub-reply is also its children
3. Everyone has the same structure



## Event listeners
Now, let us add some JS to the content. Here you will ass the **EventListeners** for the elements. 

> Ask the students where we need to add the Eventlisteners because, on every comment and reply, there will be sub-reply and submit buttons.
> Ans:- On the container not on the button.

- Create a separate JS file "script.js" as shown below.


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/366/original/upload_e3820a39c7abb76aa77cae2fb4dc4bb0.png?1695182657)


Inside the file, write the javascript code for EventListener as shown below:


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/369/original/upload_408fed92c2e2498496abb85e3c641c73.png?1695182685)



> Discuss **DOM** and **Fragmentation**


- Now, what will happen if someone clicks on the submit? For this, we will use **if-else**.
- For **if any reply is there**, add the code as shown below. 
- There are two functions created to understand clearly. - These two functions are `replyInput(e)` and `createreplyInput(e)`.  
- And also add a **else-if**. It is not necessary in that case but it will make things more clear. 
- In that else part, you will create a **reply** button, at the time you click **submit**.
Now, 
- The **comment container** should be added to a respective place. For this, understand and explain the structure of code.
- Talking about the nesting part, add the comment contain to the right place.

> Ask students about what is that eventListener "**e.target**" in the function createComment part.
> Ans:- It is when you create a comment, you are submitting, that is how it is related.

To add the comment-container, use the replace JavaScript.
```javascript
   commentCard.replaceChild(commentContainer, commentReplyBox);
```
- An input field and a submit button are present in the HTML. When the button is clicked, the **createComment** function is called with the event object (event) as an argument. The function creates a comment container, retrieves the input value, and replaces the **comment reply** box with the **comment container**.



## Final Code

And the final code will be like this:

```javascript
const container = document.querySelector(".container");

// bubbling 
container.addEventListener("click", function (e) {
    const targetElem = e.target;
    const isreply = targetElem.classList.contains("reply");
    // -> Create a input and a button
    if (isreply) {
        //  < !-- < div class="comment-reply-container" >
        //     <input type="text" placeholder="write your comment">
        //         <button class="btn-submit">submit</button>
        //     </> 
        // -->

        createReplyInput(e)
    } else {
        const isSubmit = targetElem.classList.contains("btn-submit");
        // create the reply 
        if (isSubmit == false)
            return;
        createComment(e);
    }
})


function createComment(e) {
    // 
    console.log(e.target);
    const commentContainer = document.createElement("div");
    commentContainer.setAttribute("class", "comment-container")
    const input = e.target.parentNode.children[0];
    // console.log(input.value)
    commentContainer.innerHTML = ` <div class="comment-card">
            <h3 class="comment_text">${input.value} ? </h3><div class="reply">Reply</div>
        </div>`;
    const commentReplyBox = e.target.parentNode;
    const commentCard =commentReplyBox.parentNode;
    commentCard.replaceChild(commentContainer, commentReplyBox);
}







function createReplyInput(e) {
    const fragment = document.createDocumentFragment();
    const replyContainer = document.createElement("div");
    const input = document.createElement("input");
    const button = document.createElement("button");

    replyContainer.setAttribute("class", "comment-reply-container");

    input.setAttribute("type", "text");
    input.setAttribute("placeholder", "write your comment");
    button.setAttribute("class", "btn-submit");
    button.textContent = "submit";

    replyContainer.appendChild(input);
    replyContainer.appendChild(button);
    fragment.appendChild(replyContainer);
    console.log(e.target.parentNode)

    e.target.parentNode.appendChild(fragment);

}
```

> Briefing that part of the lecture:

- First, we have comment and every comment box have a new comment section.
- And then, when you click reply you get a box to reply. - And submitting it with again create a new reply box.


---
title: Memory Leaks
description: What are memory leaks and how to identify memory leaks

---

## Definition:
A memory leak occurs when programmers create a memory in a heap and forget to delete it. The consequence of the memory leak is that it reduces the performance of the computer by reducing the amount of available memory. 

## Things to remember for memory leaks:
- 1. **Global scope pollution**

Create a html file and add the code as shown below:
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
    <script>
        "use strict";
        // problem of scope pollution 
        b = 15;
        function abc() {
            console.log(b);
            c = "Hi people";
            b ++
        }
        abc();
        console.log(" c is ",c);
        b ++
        console.log(b);
    </script>
</body>

</html>
```
> It is always suggested to use the **strict mode**.


Now, create another file `2_closure.html` to understand closure.

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
    <h1>Closure example</h1>
    <script>

        let theThing = null;
        let replaceThing = function () {
            console.log("something");
            let originalThing = theThing;
            let unused = function () {
                if (originalThing) {
                    console.log("hi");
                };
            }
            thething = {
                longStr: new Array(10000000),
                someMethod: function () {
                    console.log("Bi");
                }

            }

        }




    
        setInterval(() => {
            replaceThing()
        }, 1000);

    </script>
</body>

</html>
```
So, what is happening there, let us understand.
- We are running **theThing()** function again and again.
- If you go to the **inspect** option of the output, then go to the **memory** section.
- Here, you can see there is memory consumption increases for each iteration.

#### Reason is that:
- The function forms a closure to get access to **theThing**.
- It is originally null but it gets replaced in the further part of the code.
- Each time you call the replace function, it gets attached to the **unused** function.
- This unused function is not running but it also forms a closure.
- Basically, you are using a memory space every time the function gets iterated. And it increases the memory many times.


let us take another example. Make a new file for **out of DOM** ref:

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
    <p>
        <h1>In am h1</h1>
    </p>

    <script>
        let a = document.querySelector("h1");
        let p=document.querySelector("p");
        console.log(a);
        // please remove the node from the tree  
       a.remove();
    //    to remove it from memory -> 
    a = null;
        console.log("a", a);
    </script>
</body>

</html>
```

When you use the **remove() function**, it removes from the DOM only but not from the memory space. You can still access the elements.

> That's all for today's session.

