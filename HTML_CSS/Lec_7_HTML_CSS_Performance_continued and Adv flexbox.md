
* Previous  session's  review 
	* How browser loads a webpage
* HTTP : resource  hints
	* Preload
	* Prefetch 
	* Pre-connect
* HTML performance features
	* Async 
	* Defer
* Advanced Flexbox
	* flex-direction
	* align self
	* flex-grow and shrink
* Intuition for flex-direction 
 
## How a webpage is loaded - Revision?

**1. Navigation**
    - DNS lookup
    - TCP Connection
    - SSL Handshake
    - HTTP Request

**2. Parsing**
    - Initially we get HTML document. Browser Main thread performs the whole parsing process.
    -  **Preload-scanner :** It works parallel to main thread and make request to for any image src, css link and script file you encounter *only responsibility : to load the file nothing else*
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/035/original/upload_a09f3ba713cf97cd93671efcb8f4f118.png?1696311833)

**3. Rendering**
    - DOM and CSSOM to print the output on the screen
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/036/original/upload_eac2ded8bcc811f99920652211fe5dfc.png?1696311857)

---

## Async and Defer

**Asking Question**
What should be the output of the following code?

```htmlmixed
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF-8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
</head>
<!-- js -> parsed and executed -->
<body>
    <h1>Hi How are You</h1>
    <h2>Hi How are You</h2>
    <h3>Hi How are You</h3>
    <h4>Hi How are You</h4>
    <h5>Hi How are You</h5>
    <script src = "external.js"></script>
</body>

</html>
```
**Ans:**
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/037/original/upload_7fe756420a366fd0d429a09210f2f24b.png?1696311929)

There is lag in loading the html because the script file gets parsed and executed.

There are two ways to solve these problems:
* defer
* async

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/052/038/original/upload_39ca87993c8bec5f717feefbca7932c0.png?1696311984)

**Defer:**
* Sends parallel request to load js file without blocking the parsing and excutes the js file after dom content is loaded on the screen.
* If there are multiple files with defer so they will executed in the same manner.
**Async :**
* sends parallel request to load js file without blocking the main thread but as soon as file arrives it pause the parsing.
* No gurantee of the order

## Resource Hinting
* Used to give the browser extra information that it can't infer from the document


There are three main types of resource hints:

**Preload** – load content that's required for the intial render
 - **usecase :** getting an image, font ,script that is required for the intial render
 - **example**
```html
 <link rel = "preload" href = "/public/home.js" as = "script">
```

**Prefetch** - load content that may be needed to render the next page
 - **usecase :** Prefetched resources might be needed when the user navigates to the next page, or after the user starts interacting with the page. So loading them before the user starts the navigation will make the page load faster for them.
 - **example:**
```html
 <link rel = "prefetch" href = "/public/app.08343a72.js" as = "script">
```

**Preconnect** - establish a server connection without loading a specific resource.
 - example:
```html
 <link rel = "preconnect" href = "https://storage.googleapis.com">
```


Reference Article : https://www.debugbear.com/blog/resource-hints-rel-preload-prefetch-preconnect

## Advanced Flexbox
### Flex Direction

- **Definition:** The `flex-direction` property in CSS is used to specify the direction in which the flex items are placed within their flex container. It establishes the primary axis of the flex layout.

- **Values:**
  - `row`: The default value. Flex items are placed horizontally in a row.
  - `row-reverse`: Flex items are placed horizontally in a reversed order.
  - `column`: Flex items are placed vertically in a column.
  - `column-reverse`: Flex items are placed vertically in a reversed order.

**Code**:

```html
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8">
    <meta http-equiv = "X - UA - Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Flexbox</title>
</head>
<style>
    * {
        box-sizing: border-box;
    }


    .parent1 {
        height: 30rem;
        width: 60vw;
        background-color: rgb(176, 141, 211);
    }


    .child1 {
        color: rgb(179, 40, 40);
        background-color: lightcoral;
        font-weight: bold;
        border: dashed;
        font-family: sans-serif;
    }

    /******************template starts from here ************/

    .child1 {
        width: 9rem;
        height: 6rem;
        max-height: 8rem;
    }
    .parent1 {
        display: flex;
        /* flex direction determine you primary axis */
        flex-direction: column;
        /* now justify content  apply on primary axis */
        justify-content: space-evenly;
        /* on horizontal */
        align-items: center;
       
    }
</style>

<body>
    <!-- ******************************************** -->
    <h3>Directions</h3>
    <div class = "parent1">
        <div class = "child1 one">I am child special
        </div>
        <div class = "child1 two ">I am child</div>
        <div class = "child1 three"> I am child</div>
    </div>
    <br>
    <!-- ******************************************** -->
   




</body>

</html>
```

### Flex Shrink

- **Definition:** The `flex-shrink` property in CSS is used to determine how flex items shrink when there's not enough space available along the main axis.

- **Value:**
  - `<number>`: The factor by which the flex item will shrink relative to other flex items. A higher value means the item will shrink more when space is limited.

### Flex Grow

- **Definition:** The `flex-grow` property in CSS is used to determine how flex items grow to occupy available space along the main axis when there's extra space in the container.

- **Value:**
  - `<number>`: The factor by which the flex item will grow relative to other flex items. A higher value means the item will grow more to occupy extra space.

**Code**:

```html
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8">
    <meta http-equiv = "X - UA - Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Flexbox</title>
</head>
<style>
    * {
        box-sizing: border-box;
    }

    .parent1 {
        height: 30rem;
        width: 60vw;
        background-color: rgb(176, 141, 211);
    }

    .child1 {
        color: rgb(179, 40, 40);
        background-color: lightcoral;
        font-weight: bold;
        border: dashed;
        font-family: sans-serif;
    }

    /******************template starts from here ************/

    .child1 {
        width: 10rem;
        height: 6rem;

    }

    .parent1 {
        display: flex;
        flex-direction: row;
        flex-wrap:wrap;
    }

    .one {
        /* does not effect your element before there is enough space  */
        /* flex-grow:1; */
        background-color: lightgreen;
        flex-shrink:2;
        
    }

    .two {
        /* flex-grow:2; */
        flex-shrink:1;
        background-color: lightpink;
    }

    .three {
        /* flex-grow: 2; */
        background-color: lightblue;
    }
</style>

<body>
    <!-- ******************************************** -->
    <!-- flex-grow and shrink -->
    <h3>Grow and shrink</h3>
    <div class = "parent1">
        <div class = "child1 one">I am child special
        </div>
        <div class = "child1 two ">I am child</div>
        <div class = "child1 three"> I am child</div>
    </div>
    <br>


</body>

</html>
```

### Align Self

- **Definition:** The `align-self` property in CSS is used to align a single flex item along the cross axis, overriding the alignment set by the `align-items` property on the flex container.

- **Values:**
  - `auto`: Uses the value of `align-items` on the parent container.
  - `flex-start`: The item is aligned at the start of the cross axis.
  - `flex-end`: The item is aligned at the end of the cross axis.
  - `center`: The item is centered along the cross axis.
  - `baseline`: The item is aligned based on its baseline.
  - `stretch`: The item is stretched to fill the container along the cross axis.

**Code**:

```html
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8">
    <meta http-equiv = "X - UA - Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Flexbox</title>
</head>
<style>
    * {
        box-sizing: border-box;
    }


    .parent1 {
        height: 30rem;
        width: 30rem;
        background-color: rgb(176, 141, 211);
    }


    .child1 {
        color: rgb(179, 40, 40);
        background-color: lightcoral;
        font-weight: bold;
        border: dashed;
        font-family: sans-serif;
    }

    /******************template starts from here ************/
    .child1 {
        width: 9rem;
        height: 6rem;
        max-height: 8rem;
    }

    .parent1 {
        display: flex;
        flex-direction: column;
        justify-content: space-evenly;
        align-items: center;
    }
    .three {
        /* move an element in the direction of align items */
        align-self: flex-start;
    }
</style>

<body>
    <!-- ******************************************** -->
    <h3>Directions</h3>
    <div class = "parent1">
        <div class = "child1 one">I am child special
        </div>
        <div class = "child1 two ">I am child</div>
        <div class = "child1 three"> I am child</div>
    </div>
    <br>
    <!-- ******************************************** -->

</body>

</html>
```


