## HTML/CSS:  Units, Specificity, Positioning

## Agenda
 
* Units 
    * absolute units 
    * relative units
* Inheritance, Specificity, cascading 

* Positioning
    - Relative
    - Absolute
    - Fixed (Z-index)
    - Sticky


## Units
* Units can make your design or break your design.
* Absolute -> px
* Relative -> percentage, rem, em,vh, vw

### Pixels
* Is a very simple and straightforward unit. It is default and absolute. It remains the same.

### rem
* 1 rem is equal to the font size of the html tag.
* So on modifying the font size the size of rem also increases.
* The use case of this is where all the other elements depend on the other elements.

### vh and vw
* viewport height and viewport width i.e. current height and width of the browser window respectively.
* Grid in Excel uses vh

### Percentage
**[Ask the learners]**

What is 10% of 500
It remains the same.
* The percentage depends on the parent.
*  Never use percentage without parent element. 
* Whatever percentage we give it will refer to its parent.


### em
* It is related to the current font size. 1 em = current fontszie.
* If we don't have font size for our current element then it goes to its parent and in the worst case it will go to html i.e. 16px.
//Recap everything after the break

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
    .px,
    .rem,
    .vh-vw,

    .em {
        margin-bottom: 20px;
        background-color: lightgreen;
    }

    /* **************demo starts from here*************/
    .px {
        /* absolute */
        font-size: 20px;

    }

    /* 1rem  -> font-size of html tag -> usually 16px*/
    html {
        font-size: 10px;
    }

    .pa {
        font-size: 20px;
    }

    /* relative to current font size */
    .em {
        /* font-size: 20px; */
        /* 1em -> 10px */
        height: 10em;
        width: 10em;
        background-color: lightpink;
    }

    .rem {
        /* height:20rem; */
        /* width:30rem; */
        /* font-size  */
        font-size: 2rem;
    }



    /* viewport height ->browser window height
    where 100vh -> current height of browser window
    */
    .vh-vw {
        /* 20% of browser screen height  */
        height: 20vh;
        /* 50% of browser screen width */
        width: 50vw;
        font-size: 2rem;
    }

    /* percentage -> it's value depend upon parent */
    .gp {
        height: 200px;
        width: 200px;
        background-color: bisque;
    }

    .parent {
        height:50%;
        width:50%;
        background-color: lightcoral;
    }

    .child {
        background-color: lightblue;
        height: 50%;
        width: 50%;
    }
</style>

<body>
    <div class = "px">Pixel unit</div>
    <div class = "rem">REM Unit</div>
    <div class = "pa">

        <div class = "em">I am em</div>
    </div>

    <div class = "vh-vw">VH - VW unit</div>
    <div class = "gp">
<div class="parent">
    <div class="child"> percentage </div>
</div>
    </div>
    
</body>
```



## Inheritance
* We have a class Body having a property `color : red`.
* Another class div extends the body having property `font-family : lato`.
* Another class span extends div has properties `padding : 10px` and `color : green`.

**[Ask the Learners]**
* How many properties span class will have?
* Adding a property color to span will change the parent color?
*  Inheritance works the same way we have inheritance in programming languages.

## Specificity
* Let's say we have a h1 tag having id, class, and element defining the same property color of an element. 
* id -> blue
* class -> red
* element -> green
* But one element can only have one color at a time. **[Ask the learners]** So what color will the element have?
* The element will have a blue color color. 
* As CSS sees conflicting styles -> it applies the most specific style. Element -> Class -> ID
* **[Ask the learners]** Now I want to change the color property of the element but without giving it in **id**, how can I do this?
* 
```html
p.myclass{
    color: new_color;
}
```
* In this way we are being more specific than the paragraph element having this class. But if we define id then the property provided in id will apply anyway.
* **Inline styling has even higher priority than id.**
* Lastly if `!important` is added to anything then the element will follow it.
* The order of specificity is as follows:
* inherited -> user agent -> element -> class -> id -> inline -> `!important`
* User-agent is something by default like anchor tag as underline.

```html
<!DOCTYPE html>
<html>

<head>
    <title>Example Stylesheet</title>
    <style>

        #my-id {
            color: violet;
        }

        .parent {
            color: lightgoldenrodyellow 
        }

                .my-class {
                    color: blue ;
                }

        p {
            color: red ;
        }

        p.my-class {
            color: gray;
        }
        p#my-id {
            color: aqua
        }

         p#my-id {
            color: rgb(225, 0, 255);
        }
      
        
    </style>
</head>

<body>
    <div class = "parent">

        <p class = "my-class" id="my-id" >This is a paragraph</p>
    </div>
</body>

</html>
```


# Normal flow

In the normal flow of HTML elements on a web page, the browser renders content from **top to bottom** and **left to right**. Elements are placed in a way that naturally follows the document's structure.

* **Inline Elements**:
  - Inline elements are displayed on the same line as their preceding content.
  - Examples include spans, anchors (`<a>`), and emphasis (`<em>`) tags.

* **Block Elements**:
  - Block elements start on a new line, pushing subsequent content down.
  - Examples include headings (`<h1>` - `<h6>`), paragraphs (`<p>`), and divs (`<div>`).

* **Inline-Block Elements**:
  - Inline-block elements are displayed on the same line, but they also maintain the block-level properties.
  - They allow for elements to share a line while still respecting margins and padding.
  - Useful for creating layouts where elements need to be inline but retain block behavior.

**Code**:

```htmlmixed
<h2> Hello from h2 </h2>
<!-- Block level element -->
<div class = "block"> Hello, How are you </div>
<div class = "block"> Hello, How are you </div>

<!-- Inline element -->
<span class = "inline"> I am inline </span>
<span class = "inline"> I am inline </span>

<!-- Inline block -->
<div class="inline_block"> I am inline block </div>
```

**Code flow**:

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/438/original/upload_fd2b4df6578a1bf378a67941fb5929f6.png?1695579017)


- **h2** represents the `<h2>` tag
- **div** represents the block elements
- **green box** represents inline elements
- **blue box** represents inline block

> Origin of a page is the **top-left** corner of the web page

The normal flow provides the foundation for the layout of a web page, establishing the initial positioning of elements based on their display type. Understanding these basic concepts is crucial for building more complex layouts using CSS positioning techniques.

---
# Positioning

## Relative

Relative positioning allows elements to be shifted from their original position while still occupying space in the normal flow.

* Without `top`, `bottom`, `left`, `right` properties:
  - The element stays at the same position it would be in the normal flow.

* With `top`, `bottom`, `left`, `right` properties:
  - `top`: Pushes the element down from its original position.
  - `left`: Pushes the element right from its original position.
  - `right`: Pushes the element left from its original position.
  - `bottom`: Pushes the element up from its original position.

- Relative positioning retains the element's original space in the flow and shifts it from there.

## Absolute

Absolute positioning takes an element out of the normal flow and positions it relative to its closest positioned ancestor or the entire page.

* Without `top`, `bottom` properties: 
  - The element remains at the same position, but other elements will take its place.

* With `top`, `bottom`, `left`, `right` properties:
  - Positioned relative to its containing block (closest positioned ancestor).
  - If no positioned ancestor, it's positioned relative to the page.
  - `top`, `left`, `right`, and `bottom` determine the exact position.

- Absolute positioning removes the element from the normal flow and adjusts its position based on the specified properties.

**Code**:

```html
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8">
    <meta http-equiv = "X - UA - Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width=device-width, initial-scale = 1.0">
    <title>Absolute relative</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: sans-serif;
        }

        h2 {
            margin-bottom: 1rem;
            ;
            text-align: center;
        }

        /* template starts from here */
        .parent {
            margin-left: 1rem;
            background-color: rgb(146, 85, 202);
            width: 90vw;
            /* any position prop -> parent */
            position: absolute;
        }

        .box {
            width: 10rem;
            height: 10rem;
            margin-bottom: 1rem;
            border: dashed;
        }

        .box-1 {
            background-color: lightblue;
        }

        .box-2 {
            background-color: lightcoral;
            /*  you are relative you original pos */
            /* 1. relative -> your browser still treates like
            you are occuping your org space
            */
            /* position: relative; */
            /* 2. it pushes you down from your orginal pos*/
            /* top: 5rem; */
            /*  it pushes you right from your orginal pos*/
            /* 2. it pushes you up from your orginal pos*/
            /* bottom: 5rem; */
            /*  it pushes you left from your orginal pos*/
            /* right: 5rem; */
            /* left: 10rem; */
        }

        .box-3 {
            background-color: lightgreen;
            /* by default -> browser will remove you from current page 
            next elem to you will occupy your place . You will placed above the later elem
            */
            /* position: absolute; */
            /* opacity: 0; */
            position: absolute;
            /* left:20rem; */
            /* by default -> t  it will take values w.r.t -> page origin */
            top: 5rem;
            /* left from top left*/
            left: 5rem;
            /* be pushed up from page bottom  */
            /* bottom: 30px; */
            /* be pushed up from page bottom right  */
            /* right: 30px; */
        }

        .box-4 {
            background-color: lightsalmon;
        }
    </style>
</head>

<body>
    <h2>Position - Absolute, Relative </h2>

    <div class = "parent">
        <div class = "box box-1">1</div>
        <div class = "box box-2">2</div>
        <div class = "box box-3">3</div>
        <div class = "box box-4">4</div>

    </div>
</body>

</html>

<!-- orginal pos -> top left corner of the elem -->
<!-- # Relative 
* Browser treats the element is if it still occupies it's original pos
* Without top, bottom, left, right : element stays at org pos
    * top : pushes element down from it's original pos
    * left : pushed element right from it's original pos
    * right : pushes element left from it's original pos
    * bottom : pushes element up from it's original pos -->

<!-- # Absolute
* Browser removes it from the current flow and next element to it will take it's place
* top, left, right, bottom
* Default
* top and left -> top left corner
* bottom -> bottom part of the page
* right -> right part of the page -->
```

## Fixed

Fixed positioning removes an element from the normal flow and positions it relative to the viewport.

* Without `top`, `bottom` properties:
  - The element remains at the same position, but other elements will take its place.

* With `top`, `left` properties:
  - Element becomes fixed in its original position on the screen.
  - Remains visible while scrolling.

- Fixed positioning is great for creating elements that stay in a fixed location while the user scrolls. Remember to provide a width to avoid unexpected resizing.

> **Note 1**: The positioning properties (`relative`, `absolute`, and `fixed`) allow you to manipulate element positions within your layout. Understanding their behavior and usage is crucial for creating sophisticated and responsive designs.

> **Note 2**: These positioning properties are used to take elements out of the normal flow, and the `top`, `left`, `bottom`, and `right` values determine how elements are moved when positioning is applied. Keep in mind that `top` takes precedence over `bottom`, and `left` takes precedence over `right`. For example, when `top` is applied, `bottom` is ignored, and when `left` is applied, `right` is ignored.

**Use Case**:

Here's a use case table that demonstrates the positioning properties in various scenarios:

| **Position Property** |       **Use Case**       |
|:---------------------:|:------------------------:|
|       Relative        |      Image Caption       |
|       Absolute        |         Tooltip          |
|         Fixed         |          Navbar          |
|        Sticky         | Calendar (Month, Mobile) |


**Code**:

```html
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Fixed value</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: sans-serif;
        }

        h2 {
            text-align: center;
            margin-bottom: 1rem;
        }

        .parent {
            margin-left: 1rem;
            width: 70vh;
            background-color: rgb(146, 85, 202);
           
        }

        .box {
            width: 10rem;
            height: 10rem;
            margin-bottom: 1rem;
            border: dashed;
        }

        .box-1 {
            background-color: lightblue;
        }

        .box-2 {

            background-color: lightcoral;
        }

        .box-3 {
            background-color: lightgreen;
        }

        .box-4 {
            background-color: lightsalmon;
        }

        /* template starts from here */
        li {
            color: white;
        }

        /* display ->  */
        nav {
            background-color: black;
            /* it is fixed on the
            view port  -> like absolute next elem is going
            take your place */
            /* position: fixed; */
            /* always give width*/
            width: 100%;
    /* takes top and left from origin of the page */
            top: 0px;
            left: 30px;
        }
    </style>
</head>

<body>
    <h2>Position:Fixed</h2>
    <nav>
        <ul>
            <li>Home</li>
            <li>About</li>
            <li>Signup</li>
        </ul>
    </nav>
    <div class = "parent">
       
        <div class = "box box-1">1</div>
        <div class = "box box-2">2</div>
        <div class = "box box-3">3</div>
        <div class = "box box-4">4</div>
    </div>
    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Quidem rerum cumque necessitatibus tempora nihil sit
        placeat quisquam voluptatibus, sunt voluptates architecto in nisi porro atque reiciendis distinctio esse. Minima
        ad ratione, praesentium sunt doloribus dolore eum quo voluptatem corrupti culpa, obcaecati tempora quos. Fugit
        sed reprehenderit culpa illum, corrupti aliquam vero adipisci dolorum! Asperiores laborum, ullam omnis tenetur
        temporibus nihil cumque, possimus ut tempora voluptas doloribus, id ipsam? Error rem adipisci nostrum natus
        aspernatur expedita eos numquam inventore aliquid molestiae neque distinctio, architecto, eveniet voluptatum
        nisi iste eligendi ut repellat officia maxime at, praesentium deserunt aliquam facere. Aspernatur beatae quam
        accusantium minus fuga totam! Quis, earum unde tempore, fugit ipsa in laboriosam vel alias iste voluptatibus
        fugiat rerum voluptatum odit. A error neque maxime sequi veniam impedit cupiditate saepe voluptatem dolor quis
        exercitationem perferendis, incidunt atque beatae. Vel dolore asperiores perspiciatis quaerat possimus eligendi
        molestiae dignissimos libero ut repellendus officia exercitationem placeat natus fugiat nesciunt, consequatur
        temporibus sed dolores officiis? Deleniti itaque, exercitationem fuga odit repudiandae necessitatibus quaerat
        hic corrupti fugiat. Reprehenderit illo, sunt sed quibusdam delectus adipisci nobis ad corrupti blanditiis,
        error sapiente. Nesciunt vitae ipsa at quia eveniet commodi dolores iusto aliquid aut eum perspiciatis
        necessitatibus quidem minus labore, in quae quos quaerat? Harum ex accusamus ullam dolorum, ea aliquid
        voluptatum distinctio, ad nam odit reprehenderit voluptates? Aperiam asperiores repellat ratione iusto quis.
        Ipsa repudiandae neque repellendus commodi fugit consectetur molestiae. Cupiditate, id? Cum quam aperiam at vel
        labore pariatur neque voluptatibus quae ipsam inventore ipsa beatae libero suscipit nihil perferendis corporis
        quidem rerum, similique doloribus et eum quis commodi? Fugiat officia explicabo voluptatibus excepturi sapiente
        quisquam tempore voluptatum molestiae ipsum asperiores praesentium placeat eos odit ullam obcaecati odio
        corrupti eveniet, perferendis reiciendis error officiis atque temporibus harum hic! Vel mollitia voluptatem ex
        nulla beatae. Magni, vel sunt.</p>
</body>

</html>



<!-- /* it is fixed on the
view port -> like absolute next elem is going
take your place */ -->
<!-- /* always give width*/ -->
    <!-- /* takes top and left from origin of the page */ -->
```

### Z-index 

The `z-index` property is used to control the stacking order of positioned elements along the z-axis, which determines which element appears on top of others when they overlap.

* The default value of `z-index` is `auto`, which works in a similar manner to the cascading order of elements in the document.
* Elements with a higher `z-index` value will appear on top of elements with lower values.
* **Stacking Order**: The order in which elements are positioned on the z-axis is known as stacking order.
* If multiple elements have the same `z-index`, the one that appears later in the HTML structure will be on top.
* Increasing the `z-index` value increases an element's priority in the stacking order.

>**Note**: Be cautious when using high `z-index` values, as they can lead to unintended visual results and make the code harder to manage.

* **Stacking Context**: Each positioned element creates its own stacking context. If a parent element has a higher `z-index` value, its children will also inherit the same `z-index`, effectively forming a stacking context within that parent.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/439/original/upload_a81866a49980672a17a90cff4331f77f.png?1695579170)


#### Stacking Content

**Code**:

```html
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Stacking Context</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: sans-serif;
        }
        .parent {
            margin-left: 1rem;
            background-color: rgb(146, 85, 202);

        }
        .box {
            width: 10rem;
            height: 10rem;
            margin-bottom: 1rem;
            border: dashed;
        }


        
        /* template starts from here  */
        /*  */
        .box-1 {
            background-color: lightblue;
            position: relative;
            top:10rem;
            z-index:3;
        }
        .box-2 {
            background-color: lightcoral;
            position: relative;
            z-index:4;
        }
        .box-3 {
            background-color: lightgreen;
        }
        .box-4 {
            background-color: lightsalmon;
        }
    </style>
</head>

<body>
    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Ullam, autem assumenda! Quis quae labore quo?
        Blanditiis dicta consectetur quas optio dolor. Eligendi hic eveniet sunt nesciunt sequi unde nobis! At?</p>
    <div class = "parent">
        <div class = "box box-1">1</div>
        <div class = "box box-2">2</div>
        <div class = "box box-3">3</div>
        <div class = "box box-4">4</div>
    </div>
</body>
</html>

<!-- 1.positioned elemns are placed closer to the user -->
<!-- 2. the elem on which position prop is applied later will on top if both the elem are postioned -->
<!-- z-index : higher the value closer it is to the screen -->
```

#### Parent Content

**Code**:

```html
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Stacking Context</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: sans-serif;
        }

        .parent {
            margin-left: 1rem;
            background-color: rgb(146, 85, 202);

        }

        .parent2 {
            margin-left: 1rem;
            background-color: rgb(218, 235, 163);

        }

        .box {
            width: 10rem;
            height: 10rem;
            margin-bottom: 1rem;
            border: dashed;
        }

        .box-1 {
            background-color: lightblue;
        }

        .box-2 {
            background-color: lightcoral;
        }

        .box-3 {
            background-color: lightgreen;
        }

        .box-4 {
            background-color: lightsalmon;
        }
        /* template starts from here */
        .box-2 {
            position: fixed;
            z-index: 4;
        }
        .parent2>.box-2{
            background-color: gray;
            position: relative;
            z-index:-1;
        }
        .parent2{
            position: relative;
            z-index:5;
        }
    </style>
</head>

<body>
    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Ullam, autem assumenda! Quis quae labore quo?
        Blanditiis dicta consectetur quas optio dolor. Eligendi hic eveniet sunt nesciunt sequi unde nobis! At?</p>
   
    <div class = "parent">
        <div class = "box box-1">1</div>
        <div class = "box box-2">2</div>
        <div class = "box box-3">3</div>
        <div class = "box box-4">4</div>
    </div>

    <div class = "parent2">
        <div class = "box box-1"> Parent2 1</div>
        <div class = "box box-2">Parent2 2</div>
        <div class = "box box-3">Parent2 3</div>
        <div class = "box box-4">Parent2 4</div>
    </div>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Necessitatibus dolore, quas hic neque eaque aliquam
        fugiat velit, odio quam autem culpa sequi sunt? Provident tempora laboriosam rerum earum, a magni, accusantium
        illo sint delectus dolores quam possimus debitis vel eos asperiores in aperiam quia architecto ullam reiciendis
        commodi aliquam cumque ad ipsum! Quod, libero sint! Dicta sed, earum iure cum, dolorem rerum commodi illum quasi
        harum debitis asperiores. Cum voluptatibus delectus officiis neque, maxime, necessitatibus culpa enim eligendi
        iusto incidunt aperiam ducimus corporis. Inventore assumenda explicabo quis praesentium molestiae ut dolor omnis
        eius ullam a, neque, sint quos hic reprehenderit vitae cum eum quidem. Eos repellat, commodi, suscipit eius
        dolorem asperiores iste unde nobis obcaecati at porro nihil. Nostrum vitae doloremque magni praesentium atque,
        sed perferendis consequuntur architecto dolorum! Impedit provident labore aut ex molestiae. Voluptatibus ducimus
        quibusdam necessitatibus? Aliquid natus fugiat, reprehenderit impedit exercitationem doloribus ipsam. Architecto
        repellendus quaerat minima eveniet aperiam, in sequi nam nihil velit libero placeat ad incidunt dolorem, dolor
        minus exercitationem. Aperiam hic incidunt provident, sed eum rerum voluptates dolore repellat. Dolores
        inventore dicta laborum porro ratione perspiciatis tempora ducimus, architecto quasi, error fugit quod, omnis
        possimus doloremque impedit! Nisi, dolorum consectetur voluptatem libero adipisci id ea veniam quis facere
        molestiae atque, blanditiis laborum debitis cumque at quos provident dignissimos quod ipsa voluptatum. Omnis
        culpa voluptatibus deserunt itaque ratione dolor, mollitia ipsum nulla quasi qui doloremque aut error veritatis.
        Esse soluta ipsam sequi expedita dolores fugit, corrupti maiores, ullam pariatur dignissimos ratione a nostrum
        voluptate natus iusto! Quaerat consectetur corporis quam non. Incidunt, sit? Quas, quis odit nihil nemo,
        adipisci quaerat aut, perspiciatis sequi distinctio impedit repellat non porro? Unde in veritatis hic, dolorem
        ipsam ullam temporibus assumenda quo porro aliquam modi esse ad nesciunt quaerat voluptas possimus.
        Exercitationem accusamus velit consequatur ipsa itaque rerum!</p>
</body>

</html>


<!-- 
Rules :
1. Positioned element is always above statically placed elements
2. if both the elements are positioned then the element 
that is positioned later is placed above.
3. higher the z-index higher the value 
4. your z-index depend upon your parent
 -->
```


## Sticky

Sticky positioning is a hybrid between fixed and absolute positioning. It combines elements of both, providing unique behavior based on the scrolling context.

> **Note**: Sticky positioning requires the `top` property to be set. This property defines an offset from the top edge of the element.

* The sticky element remains fixed within its parent as long as the parent is within the visible area of the page.
* If the parent element goes out of view due to scrolling, the sticky element is released from its fixed position and resumes normal flow.
* Sticky positioning is commonly used to create elements that remain visible while scrolling, such as headers or navigation menus.
* It's important to note that the behavior of sticky positioning depends on the scrolling behavior of the parent element. Once the parent element is scrolled out of view, the sticky element returns to its normal position in the flow.

Understanding the behavior and application of sticky positioning allows you to create elements that maintain visibility during scrolling, enhancing the user experience of your website.

**Code**:

```html
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Sticky</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: sans-serif;
        }

        h2 {
            margin-bottom: 1rem;
            ;
            text-align: center;
        }

        .parent {
            margin-left: 1rem;
            background-color: rgb(146, 85, 202);
            width: 90vw;
        }

        .box {
            width: 10rem;
            height: 10rem;
            margin-bottom: 1rem;
            border: dashed;
        }

        .box-1 {
            background-color: lightblue;
        }

        .box-2 {
            background-color: lightcoral;

        }

        .box-3 {
            background-color: lightgreen;
        }

        .box-4 {
            background-color: lightsalmon;
        } 
        /* template starts from here */
       
        .box-2{
            position: sticky;
            /* your element and screen top has 
            this much distance */
            top:20px;
            /*  2. if your elem is in a parent
            the as soon as your parent leaves the screen
            you are also removed
            */
        }
    </style>
</head>

<body>
    <h2>Sticky </h2>
        <div class = "box box-2">2</div>
<p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Nemo sapiente, vitae deleniti quos illo nam ab
    voluptatum recusandae molestias voluptatem libero tempore porro iure minus, officiis placeat enim vero maiores
    aperiam. Obcaecati adipisci quaerat molestiae id voluptatum? Maxime aut ab ullam esse quos molestias natus
    aliquam odit provident rerum. Voluptatem accusantium alias rem, cupiditate suscipit pariatur sint commodi
    temporibus necessitatibus, magni tenetur doloremque. Placeat perferendis quam pariatur dolor tempora minus eos
    voluptatem enim quae atque eaque fugit eius, labore aliquam fuga exercitationem, assumenda veritatis quo ipsa
    saepe ducimus consectetur quos illo sint. Veniam, dignissimos sequi sint facilis reiciendis minima. Quas
    praesentium ab ratione perspiciatis nostrum voluptatum dolorum nisi in omnis reprehenderit recusandae nam
    dolorem qui, esse nihil hic adipisci dolor facere voluptas? Voluptatum necessitatibus facilis praesentium, quam
    voluptas magnam quisquam vitae odit inventore, laboriosam, eaque ullam ipsam. Rem, a hic iusto quidem veritatis
    at. Provident, nisi neque assumenda sunt nihil quidem nam iusto officiis modi cum distinctio ratione veniam
    facere minus laborum, architecto voluptate. Quas rerum voluptatibus ipsa harum incidunt odit sunt accusantium
    quisquam, corrupti nihil, illum earum vitae iusto odio tenetur amet culpa quis? Voluptatibus dolores sit aliquid
    numquam, officiis dolorem temporibus autem doloremque eveniet mollitia rerum rem illum incidunt nemo eligendi
    quasi sed optio quidem aut assumenda veritatis tenetur corrupti quisquam nostrum! Expedita hic debitis
    dignissimos inventore ducimus repellendus aliquam quibusdam mollitia doloremque totam, vitae alias officia, in
    amet laboriosam id, distinctio consectetur. Possimus eligendi deleniti fugiat! Adipisci error, aspernatur eum
    sint earum animi molestias aut nisi optio iusto modi tenetur, ipsa natus quis omnis quibusdam eveniet odit.
    Provident mollitia quod qui. Laborum culpa, ipsa similique, voluptates laboriosam quia totam ad vero in autem,
    rem commodi! Incidunt velit architecto esse id repudiandae et numquam ducimus voluptatum dignissimos doloribus
    iste voluptatem unde quisquam sunt totam optio, corporis ratione voluptas!</p>
    <div class = "parent">
        <div class = "box box-1">1</div>
        <div class = "box box-2">2</div>
        <div class = "box box-3">3</div>
        <div class = "box box-4">4</div>

    </div>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Nemo sapiente, vitae deleniti quos illo nam ab
        voluptatum recusandae molestias voluptatem libero tempore porro iure minus, officiis placeat enim vero maiores
        aperiam. Obcaecati adipisci quaerat molestiae id voluptatum? Maxime aut ab ullam esse quos molestias natus
        aliquam odit provident rerum. Voluptatem accusantium alias rem, cupiditate suscipit pariatur sint commodi
        temporibus necessitatibus, magni tenetur doloremque. Placeat perferendis quam pariatur dolor tempora minus eos
        voluptatem enim quae atque eaque fugit eius, labore aliquam fuga exercitationem, assumenda veritatis quo ipsa
        saepe ducimus consectetur quos illo sint. Veniam, dignissimos sequi sint facilis reiciendis minima. Quas
        praesentium ab ratione perspiciatis nostrum voluptatum dolorum nisi in omnis reprehenderit recusandae nam
        dolorem qui, esse nihil hic adipisci dolor facere voluptas? Voluptatum necessitatibus facilis praesentium, quam
        voluptas magnam quisquam vitae odit inventore, laboriosam, eaque ullam ipsam. Rem, a hic iusto quidem veritatis
        at. Provident, nisi neque assumenda sunt nihil quidem nam iusto officiis modi cum distinctio ratione veniam
        facere minus laborum, architecto voluptate. Quas rerum voluptatibus ipsa harum incidunt odit sunt accusantium
        quisquam, corrupti nihil, illum earum vitae iusto odio tenetur amet culpa quis? Voluptatibus dolores sit aliquid
        numquam, officiis dolorem temporibus autem doloremque eveniet mollitia rerum rem illum incidunt nemo eligendi
        quasi sed optio quidem aut assumenda veritatis tenetur corrupti quisquam nostrum! Expedita hic debitis
        dignissimos inventore ducimus repellendus aliquam quibusdam mollitia doloremque totam, vitae alias officia, in
        amet laboriosam id, distinctio consectetur. Possimus eligendi deleniti fugiat! Adipisci error, aspernatur eum
        sint earum animi molestias aut nisi optio iusto modi tenetur, ipsa natus quis omnis quibusdam eveniet odit.
        Provident mollitia quod qui. Laborum culpa, ipsa similique, voluptates laboriosam quia totam ad vero in autem,
        rem commodi! Incidunt velit architecto esse id repudiandae et numquam ducimus voluptatum dignissimos doloribus
        iste voluptatem unde quisquam sunt totam optio, corporis ratione voluptas!</p>
        <div class = "parent">
            <div class = "box box-1">1</div>
            <div class = "box box-2">2</div>
            <div class = "box box-3">3</div>
            <div class = "box box-4">4</div>
        
        </div>
        <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Nemo sapiente, vitae deleniti quos illo nam ab
            voluptatum recusandae molestias voluptatem libero tempore porro iure minus, officiis placeat enim vero maiores
            aperiam. Obcaecati adipisci quaerat molestiae id voluptatum? Maxime aut ab ullam esse quos molestias natus
            aliquam odit provident rerum. Voluptatem accusantium alias rem, cupiditate suscipit pariatur sint commodi
            temporibus necessitatibus, magni tenetur doloremque. Placeat perferendis quam pariatur dolor tempora minus eos
            voluptatem enim quae atque eaque fugit eius, labore aliquam fuga exercitationem, assumenda veritatis quo ipsa
            saepe ducimus consectetur quos illo sint. Veniam, dignissimos sequi sint facilis reiciendis minima. Quas
            praesentium ab ratione perspiciatis nostrum voluptatum dolorum nisi in omnis reprehenderit recusandae nam
            dolorem qui, esse nihil hic adipisci dolor facere voluptas? Voluptatum necessitatibus facilis praesentium, quam
            voluptas magnam quisquam vitae odit inventore, laboriosam, eaque ullam ipsam. Rem, a hic iusto quidem veritatis
            at. Provident, nisi neque assumenda sunt nihil quidem nam iusto officiis modi cum distinctio ratione veniam
            facere minus laborum, architecto voluptate. Quas rerum voluptatibus ipsa harum incidunt odit sunt accusantium
            quisquam, corrupti nihil, illum earum vitae iusto odio tenetur amet culpa quis? Voluptatibus dolores sit aliquid
            numquam, officiis dolorem temporibus autem doloremque eveniet mollitia rerum rem illum incidunt nemo eligendi
            quasi sed optio quidem aut assumenda veritatis tenetur corrupti quisquam nostrum! Expedita hic debitis
            dignissimos inventore ducimus repellendus aliquam quibusdam mollitia doloremque totam, vitae alias officia, in
            amet laboriosam id, distinctio consectetur. Possimus eligendi deleniti fugiat! Adipisci error, aspernatur eum
            sint earum animi molestias aut nisi optio iusto modi tenetur, ipsa natus quis omnis quibusdam eveniet odit.
            Provident mollitia quod qui. Laborum culpa, ipsa similique, voluptates laboriosam quia totam ad vero in autem,
            rem commodi! Incidunt velit architecto esse id repudiandae et numquam ducimus voluptatum dignissimos doloribus
            iste voluptatem unde quisquam sunt totam optio, corporis ratione voluptas!</p>
</body>

</html>
```

