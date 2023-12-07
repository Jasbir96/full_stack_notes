

---

# Agenda
* Understand the Box Model, the basis of CSS
    * Analogies 
    * dev-tools
* Overflow and height/width
* Display
    * inline
    * block
    * inline-block
* margin collapse
*  Project -> Header section
    - background properties



## Box Model
**[Ask the learners]**
What do you understand by the Box model?

* Represent every element using a box(squares and rectangles) is called **Box Model**.
* Story building - let us say you become a billionaire one day by winning a lottery and you start by buying land.
* You will start by fencing, The house is going to have a garden surrounding it, and your area outside the house which is owned by the government.
* Here (**x --> y in CSS** can be read as x represents y in CSS)
    - House --> Element
    - Garden --> Padding
    - Outside area --> margin
    - Fencing --> Border
    *  Land -> Border + House + Garden
    *  Eleemnt in CSS -> border + content + padding

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/314/original/upload_095dab78edc1b99deb57c0aeafa00f27.png?1695152071)

We will start with box_border_html.html
```html
<!DOCTYPE html>
 <html lang = "en">
 <head>
     <meta charset = "UTF - 8">
     <meta http-equiv = "X - UA - Compatible" content = "IE = edge">
     <meta name = "viewport" content = "width = device - width, initial-scale = 1.0">
    <title>Document</title>
 <style>
     .box{
         /* content */
         height:100px;
         width:100px;
         background-color: lightblue;
     }
 </style>
 </head>
 <body>
     <div class = "box"></div>
 </body>
 </html>
```

Run this in Browser right-click on the box on the screen and select inspect. Now we will have two areas first is html part and the other is css part. We have certain more things but we will go to the box we have created.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/315/original/upload_7ed8ca196d3a0f559088f56f75194e25.png?1695152164)

Add border `border: 5px solid red`.

**[Ask the learners]**
* If I add the margin of 100px to the top, will the size of the box increase?
--> No margin is something outside the box, it's not inside the box so the size remains as it is.
* If I add padding of 100px it the size of the box change? If yes how?
--> Yes, the size of the box will change. The box becomes larger and a padding of 100px is added to all the sides. If we wish to add padding only on one side we can use `padding-top: 100px`.

Add border, padding, and margin to this box. 

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/316/original/upload_f5794e99a10aade43ceefe71db023ffc.png?1695152210)

```html
<style>
    .box{
        /* content         */
        height: 100px;
        width: 100px;
        background-color: light-blue;
        border: 5px solid red;
        /* area outside my element         */
        margin-top: 100px;
        padding-top: 100px;
        padding-bottom: 100px;
    }
</style>
```

**Height of the element is** -> border bottom + border top + padding top + height of content
**Width of the element is** -> horizontal border + horizontal padding + width of the content
**Element** -> Content + Padding + Border



### Box Model 2
Another method to write the above code is using box-sizing. Look at the code and output given here.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/049/317/original/upload_fa616c42916018385b544bca02050f31.png?1695152234)

```html
<!DOCTYPE html>
 <html lang = "en">
 <head>
     <meta charset = "UTF - 8">
     <meta http-equiv = "X - UA - Compatible" content = "IE = edge">
     <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
 <style>
    .box{
        /* content         */
        height: 100px;
        width: 100px;
        background-color: lightblue;
        border: 5px solid red;
        /* area outside my element         */
        margin-top: 100px;
        padding-top: 20px;
	padding-bottom: 20px;
    }
    .box_sizing{
	box-sizing: border-box;
    }

</style>
 </head>
 <body>
     <div class = "box"></div>
     <div class = "box box_sizing"></div>
 </body>
 </html>
```

**[Ask the learners]**
* Can you spot the difference? What has happened over here?

--> On adding an extra property `box-sizing: border-box;`, the size of my box is reduced. This implies that something is subtracted from by border box.

### Calculating Height and Width of Element

1. **Content Box =** content height + verticle border + vertical padding.
2. **Border Box =** content height - verticle border - vertical padding.

The same is true for the **width**.

Adding 20px on left and right:

```html
    .box{
        /* content         */
        height: 100px;
        width: 100px;
        background-color: lightblue;
        border: 5px solid red;
        /* area outside my element         */
        margin-top: 100px;
        padding-top: 20px;
	    padding-bottom: 20px;
        padding-right: 20px;
        padding-left: 20px;
    }
```

Then again the padding width is subtracted from our box.

## Summing Up
All these variables padding margin borders are initialized with the creation of the object. We changed it as per our requirement.
* **Element** --> Content + Padding + Border
* **Element Height** 
	* default way --> height (user-defined) + padding-top +  border-top + padding-bottom + border-bottom
	* border box --> height (element) - padding-top - padding-bottom - border-top - border-bottom
**Note:** Simiarly width will also be calculated.

## Use case
* We usually use borderbox. Why?
* Because these boxes contain some content. We define some height and width i.e. dimensions. Now the designer has some specifications and we want the element to be in the same dimensions. 

---
## Overflow
* There must be a user-defined height else there won't be overflow
* The height of the content > height of the element
* Ways to handle overflow:
	* **visible** -> The content is visible outside the borders (it is the default)
	* **hidden** -> The extra content is hidden
		* The hidden part can be extracted using javascript and add scrollbars but if the clip is applied then Javascript will not be able to add a scrollbar
	* **scroll** -> scrollbar is added even if the content  length < element length, element is scrollable both ways horizontal as well as vertical
	* **Note:** By default scroll bars are off in MACs.
	* **auto **-> It adds a scrollbar only if it is required

```html
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width=device-width, initial-scale = 1.0">
    <title>Document</title>


    <style>
        h1 {
            text-align: center;
            font-family: sans-serif;
            color: darkorange
        }

        /* templates starts from here */
        .box {
            height: 100px;
            width: 200px;
            border: dashed;
            background-color: lightgreen;
            /* default */
            /* overflow: visible; */
            overflow: hidden; 
            /* it creates scroll bar on both the sides */
            /* overflow: scroll; */
            /* showing scroll bar where it is required */
            overflow :auto;
        }
    </style>

</head>


<body>
    <h1>Overflow</h1>
    <!-- defined height -> block  -->
    <div class = "box">
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Repellat quaerat id quos officiis facilis, eligendi
            sed laborum magni omnis
            modi! Accusantium est animi quibusdam tempore similique repellat deserunt pariatur
            soluta! Aliquam voluptas ipsa nesciunt commodi eius repellendus aperiam doloribus facere sunt voluptatibus
            !</p>
    </div>
</body>

</html>
```


## Display

* **Block** 
	* width -> as much as possible (the entire width of the screen)
	* next element -> Start on the next lime
	* Example -> div, p, semantic tags
	* Usage -> to create blocks and lines
* **Inline**
	* width -> length of the content
	* next element -> nex to it
	* example -> a, span
	* usage -> used to style a part of the sentence
* **Inline Block**
	* width -> same as inline
	*  next element -> next to it (same as inline)
	* We can give height and width to it
	* usage -> buttons

If there is a block-level element and we have an inline element below it, until we give top padding, it going to stay below the block-level element, but as soon as we give it padding-top, it will overlap with the element above it. 

If we have a block element above an inline-block element, the inline-block will move below rather than disturbing the block-level element.

Inline blocks are used in cases where padding is not required like highlighting/styling a part of the sentence.

```html
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
</head>
<style>
    .block,
    .inline,
    .inline_block {
        font-weight: bold;
        font-family: sans-serif;
    }


    /* template starts from here */
    .block {
        background-color: lightblue;
        width: 200px;
        height: 200px;
        border: 2px solid red;

        padding: 20px;
        /* h-margin */
        margin-top: 20px;
        /* v-margin  */
        margin-left: 20px;


    }

    .inline {
        /* width ,height , margin -vertical */
        width: 200px;
        height: 200px;
        margin-top: 100px;
        background-color: rgb(177, 177, 69);
        border: 2px solid red;
        padding-left: 30px;
        padding-bottom: 200px;
        margin-left: 30px;
        padding-top: 200px;

    }

    .inline_block {
        display: inline-block;
        background-color: lightcoral;
        width: 200px;
        height: 200px;
        border: 2px solid red;
        margin-bottom: 20px;
        padding-top: 300px;
    }
</style>

<body>
    <!-- block level element  -->
    <h2>hello from h2</h2>
    <div class = "block">Hello Hi how are you</div>
    <div class = "block">Hello Hi how are you</div>
    <!-- inline element -> style a part of a line -->
    <span class = "inline">I am inline</span>
    <span class = "inline">I am inline</span>
    <!--  inline block -->
    <div class = "inline_block">I am inline block</div>
</body>

</html>
```

---

## Margin Collapse
* If two elements have a certain margin between them then the total margin will be greater than their respective margin.
* Take the example of COVID-19 times when we were supposed to keep 6 inches of distance. Now person 1 is keeping a distance on the right. If person 2 on the right will also keep 6 6-inch distance is not desired. So in between both only 6 inches of distance.
* Margin collapse works only for **vertial margin**.

---


**[Ask the Learners]**
* How many properties span class will have?
* Adding a property color to span will change the parent color?
* --> Inheritance works the same way we have inheritance in real life.
## Project: backgroundimage pseudo selectors
* Bring back code from lecture 2 on semantic tags.

### Step 1 - Bringing the image to the background
* **[Ask the learners]** What should be the height of the header? Based on previous learnings?
* -> The height of the background equals the height of the content.
* **[Ask the learners]** For background height what unit should I use
* -> The height of the section is 70% always so we will be using `70vh`.
* `background-size: cover` -> The background width has to be 100% always and the height of the parent.
* `background-positon: center` -> The pizza in the image always remains in the center. 
* Along with the image we can pass linear gradient as follows with reduced opacity -> `background-image: linear-gradient(rgba(0,0,0,0.534),rgba(0,0,0,0.673)`,url("img.jpg");
