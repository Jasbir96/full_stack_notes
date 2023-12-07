# Full Stack LLD: HTML/CSS-4: Flexbox ,Fluid design,Positioning

## Today's Content
-  Flex box 
	* practical introduction
	* justify content
	* align items
	* flex-wrap
	* gap
* Adding fluid design  to webpage
* Responsive design
# Flexbox

Flexbox (short for Flexible Box Layout) is a CSS layout model designed to provide an efficient and predictable way to arrange and align elements within a container. It introduces a new way of distributing space and controlling the alignment of items, making it easier to create responsive and flexible layouts.

* **Container and Items:**
  - To create a flex container, set the `display` property to `flex` or `inline-flex` on the parent element.
  - The direct children of a flex container become flex items.
  
* **Main Axis and Cross Axis:**
  - Flexbox layout is based on two main axes: the main axis and the cross axis.
  - The main axis is defined by the `flex-direction` property (row, row-reverse, column, column-reverse).
  - The cross axis is perpendicular to the main axis.
  
* **Justify Content (Main Axis):**
  - Determines how flex items are distributed along the main axis.
  - Options include `flex-start`, `flex-end`, `center`, `space-between`, `space-around`, and `space-evenly`.

* **Align Items (Cross Axis):**
  - Controls how flex items are aligned along the cross axis.
  - Options include `flex-start`, `flex-end`, `center`, `baseline`, and `stretch`.

* **Flex Items:**
  - Each flex item can have its own individual properties, like `flex-grow`, `flex-shrink`, and `flex-basis`, which determine how they grow or shrink within the container.

* **Flex Wrap:**
  - By default, flex items all fit on a single line. With `flex-wrap`, you can control whether they wrap onto multiple lines.

* **Align Content (Lines in Multiline Layouts):**
  - Used to align multiple lines of flex items when they wrap onto multiple lines.
  - Similar alignment options to `justify-content`.

* **Nested Flex Containers:**
  - Flex containers can also be nested to create more complex layouts with different levels of flexibility.

Flexbox simplifies the process of creating dynamic layouts that adapt to different screen sizes and orientations, while reducing the need for complex positioning and float techniques.

**Code**:

```html
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Flexbox</title>
</head>
<style>
    * {
        box-sizing: border-box;
    }

    .parent {
        height: 30rem;
        width: 32rem;
        background-color: rebeccapurple;
    }

    .child {
        color: rgb(179, 40, 40);
        font-size: 2rem;
        background-color: lightcoral;
        font-weight: bold;
        border: 1px solid green;
        font-family: sans-serif;
    }

    /* template starts from here */
    .parent {
        /* parent */
        display: flex;
        /* vertically */
        /* align-items: stretch; */
        /* move you vertically to top of the parent */
        /* align-items: flex-start; */
        /* move you vertically to bottom of the parent */
        /* align-items: flex-end; */
        /* align-items: baseline; */

        /* if i put some height */
        /* vertically -> all props will work 
        but baseline will not be strech
        */
    }

    /* justify content 
    is used to move content horizonatllay
    */
    .parent {
        /* justify-content: flex-start; */
        /* push you horizontally end to to px */
        /* justify-content: flex-end;
        justify-content: center;
        justify-content: space-evenly;
        justify-content: space-between; */
        /* justify-content: space-around; */
    }

    .parent {
        /* it moves the all the element in subseqent rows */
        flex-wrap: wrap;
        justify-content: center;
        /* it is used to put spacing b/w elements  */
        gap:10px;
    }

    .child {
        width: 14rem;
        height: 10rem;
    }
</style>

<body>
    <h2>Flexbox Example</h2>
    <div class = "parent">
        <div class = "child one">I am child special
            and i am being
        </div>
        <div class = "child two ">I am child</div>
        <div class = "child three"> I am child</div>
    </div>
</body>

</html>

<!-- default -> display : flex -->
<!-- children -> aligned one after another -->
<!--  the will take whole height and whole width
of the parent
-->
```

# Responsive Design

## Media Query


**Definition:** Media queries in CSS are a powerful tool used for creating responsive designs. They allow you to apply different styles based on various conditions, such as screen width, height, device orientation, and more.

**Syntax:**

```css
@media mediaType and (condition) {
    /* CSS rules to apply when condition is met */
}
```

- **Usage:** Media queries are commonly used to adapt a web page's layout and design to different screen sizes and devices, making the design more user-friendly across various platforms.

**Common Media Query Conditions:**
- **width:** `max-width`, `min-width`
- **height:** `max-height`, `min-height`
- device-width, `device-height`
- **orientation:** `portrait`, `landscape`
- aspect-ratio, `device-aspect-ratio`
- **resolution:** `min-resolution`, `max-resolution`

**Benefits:**
- **Responsive Design:** Media queries enable you to create layouts optimized for different devices and screen sizes.
- **Improved User Experience:** Pages are more usable and visually appealing on various devices.
- **Single Codebase:** A single HTML/CSS codebase can adapt to different devices, reducing development effort.

**Considerations:**
- **Mobile-First Approach:** Start with the styles for smaller screens and use media queries to enhance larger screens.
- **Test Across Devices:** Test your responsive design on various devices and browsers to ensure consistency.

**Code**:

- **index.html**:

```html
<!DOCTYPE html>
<html lang = "en">
<head>
    <meta charset = "UTF - 8">
    <meta http-equiv = "X - UA - Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
    <link rel = "stylesheet" href = "style.css">
</head>
<style>
    h1{
        text-align: center;
        font-family: sans-serif;
    }
    body{
        height:100vh;
    }
</style>
<body>
    <h1>Responsive Design</h1>
</body>

</html>
```

- **style.css**:

```css
/* 
screen -> print, potrait ,landscape
 
*/

/* max -> 1100->1400px */
/* desktop first */
/* mobile first -> min-width */
@media only screen and (max-width:1400px) {
    body {
        background-color: lightblue;
    }
}

/* 0-> 1100 */
@media only screen and (max-width:1100px) {
    body {
        background-color: lightgreen;
    }
}

/* 0-> 900 */
@media only screen and (max-width:900px) {
    body {
        background-color: lightsalmon;
    }
}



/* mobile first -> easier  */
/* desktop / mobile -> userbase*/

/* media queies -> see your content  */
```

Sure, here are the sections for "Meta Tags" and "Responsive Image Tag" in the requested format:
