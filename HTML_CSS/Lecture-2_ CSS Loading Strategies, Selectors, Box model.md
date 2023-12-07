# Full Stack LLD: HTML/CSS-2: CSS Loading Strategies, Selectors, Box model
## Agenda
* Demo of the Project -> in the upcoming 5 lectures
* HTML
* Structure for Webpage -> [Semantic Tags]
* CSS 
    * Selectors and combinators
    * Add CSS to the Web Page
    * Optimized way to add CSS


## Demo Project

* **Zomato-like website having** 
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/346/original/upload_ee4b1e8c95dc6913c0547f123560bd8a.png?1695532203)

* **headers and main area**
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/347/original/upload_1c6cf8d7e281f95851464847937606de.png?1695532226)

* **buttons - increases size when hovered over**
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/348/original/upload_4ac833da46c3fe90be3b8258e45a734a.png?1695532563)

* **navbar - stays on the page all the time, even when user scrolls down**
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/350/original/upload_ecd1b34285a1ea04f382bc15367e9aa6.png?1695532612)

* **cards and headings - to display features of the app**
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/351/original/upload_76782c567f615fe2f4b9c9841c5366d7.png?1695532647)

* **images - to showcase products**
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/352/original/upload_d3c6442b70c84f97c42f3127247ed19c.png?1695532685)

* **icons - used in user review**
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/353/original/upload_c7a58b11858ba488adef04904410a113.png?1695532737)

* **hoverables - All the buttons, cards when hovered they increase their size to provide better user experience.**

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/354/original/upload_8ab0f57cfa49af3908973bcb561dc592.png?1695532780)

* **responsive - The website can be displayed over any screen size**

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/356/original/upload_9d76d118fb125c0afa89ad5c2faf74b6.png?1695532819)

* **forms - to get user details like contact us section**.
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/357/original/upload_6c97757b04e48fca9319daf2ce73097f.png?1695532864)

* This will be built using HTML and CSS.
* The instructor will share the GitHub link with the students.


## First Web Page
* Platform instructor is going to use - VS Code for example.
* Boilerplate of HTML
    * Write `!` and press enter to get the boilerplate 
    * The boilerplate has head and body parts inside the `HTML` tags.

## MDN


Do you know about MDN?

* MDN is the holy book for web developers, a reference book, or documents on web development.
* It has everything in detail explained.


Which tag [MDN](https://developer.mozilla.org/en-US/) must have been used to give this heading?

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/358/original/upload_19c9b81c1fe796b6e7b80ec32e3b2909.png?1695532940)

* **Right-click on the heading and click on inspect.** 

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/360/original/upload_20e586129dd11f5482e2a989522c1085.png?1695533703)

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/361/original/upload_215e4e97cd2ab9f5b74841a8b36211e3.png?1695533737)

-> Yes, they have used `<h1></h1>` tags to give the main heading of a web page.

**[Ask the learners]**
Which tag [MDN](https://developer.mozilla.org/en-US/) must have been used to give this heading?

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/362/original/upload_0a165b3be7da53b370114e2aa21f411f.png?1695533776)

-> Yes, `<h2></h2>`. Whenever you are building a webpage, section section-wise heading should be `h2`. A web crawler like Google goes through the webpage using the structure to get important sections of the page.

-> These are the best practices. There are 6 different headings but `h1`, `h2`, and `h3` are more commonly used.

-> Let's explore the same page more.

* `<p>` tags are used for paragraphs
* `<img>` tag is used for images on the webpage. It has two commonly used properties `src` for the source of the image and `alt` as an alternative if the image is not loaded. we can give height and width 
* `<a>` tag or anchor tag is used to insert links to the other webpage.

## Vacabulory around HTML

* It stands for Hyper Text Markup Language.
* It is used to add content to our webpage (skeleton)
* Tags:
    * `<element></element>` where element is the name of the tag.
    * **Self-enclosing tags -** they don't require a closing tag.
    * **Empty tags -** When there is no content between opening and closing tags. Thus, some of these tags allow writing only the opening tags or self-enclosing tags.

### Helpful Extensions for vscode  (recommendations)
- live server
- auto rename

* Let us try to write the structure of our webpage (only HTML).
* We don't want the UI we only want the structure. 
* Turn off the styling and show the structure of the webpage. We will try to build this using HTML. [Link](https://chrome.google.com/webstore/detail/web-developer/bfbameneiokkgbdmiekhjnmfkcnldhhm)

**[Ask the learners]**
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/363/original/upload_d2e318a7784166c31e9b6ed929bc2d96.png?1695533882)
What is this section called?

-> Menu bar, header

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/364/original/upload_07a349649b0bb3413f1090bf43d8a812.png?1695533909)

-> Main container of the web page

-> The rest are the other sections 




## Semantic Tags

What do you understand by Semantic Tags?
-> Semantic in english means having a literal meaning

* Semantic tags are tags that have some logical/actual meaning are used to classify your HTML in logical sections.
* Different sections of the a usual webpage contains

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/365/original/upload_48aca32309ab79ffaa5d5635c8b118ad.png?1695533950)

* Navigate the MDN website to see different sections used in it.
    * **header**
    ![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/366/original/upload_c36567069c383982764d8a7a3445f073.png?1695534037)
    
    * **nav**
    ![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/367/original/upload_02e2d52db5fdd0986f8902073fac4f89.png?1695534062)
    
    * **article & section**
    ![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/368/original/upload_f1b404fad25f0981a30b501b4732538f.png?1695534084)

    * **figure**
    ![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/369/original/upload_ee8c7ab44a7d6748dd2a5c3d0170d2bc.png?1695534103)

    * **aside**
    ![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/370/original/upload_726cf1bb39646a173e3ff716c0515e10.png?1695534122)
    
    * **footer**
    ![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/371/original/upload_f29851ee45011ff2100002327a0688b9.png?1695534154)

## Usecase
* Better code readability
* Search Engine Optimization - The structure helps the engine to find topics and information faster and in a meaningful way.
* Adds to the accessibility : webpage will be easily udestood by the devices like screen reader 

## Index Page of the our project
**[Ask the learners]**
Can you help me write HTML code for the webpage?

* Let us create a simple html page
* Start with the `header` inside the `body`
* Initialize `nav` tags with comments
* Initialize `h1` and `anchor` tags for buttons and wrap inside these in `main` tags
* Create a list using the `ul` tag inside `nav` tags, inside each `li` tag create a link using the `a` tag with its respective values.
* Use `section` tags along with the class for the section used in the webpage.
* Lastly, define the footer for the page. 

---
## CSS 
* Cascading style sheet - styling
* CSS Rule : It has four 
    ![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/372/original/upload_7ce9f127bfc20fe42171ff790fa46b37.png?1695534184)


## Selectors

* Selectors are used to identify HTML content to which we are required to apply styling.

**[Question 1]**
1. Make all the paragraphs color red
2. Give every element with class "bggreen" background color of lightgreen
3. Make every element with id "special" -> underline

* Open the `style` tag inside the `head` section.

```html
<style>
/* element selector     */
    p{
        color: red;
    }
/*  class selector  -> give them a name that can be reused  */
    .bggreen{
        background-color: lightgreen;
    }
/* id selector   -> unique id to a HTML element  */
    #special{
        text-decoration: underline;
    }
/* Universal selector -> applies to every element     */
    *{
        font-family: sans-serif;
    }
</style>
```
**Output:**

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/373/original/upload_dc7e6fdacaa7880a7eade9cd4b068c51.png?1695534213)

**[Question 2]**

Select the element to select me 
        and make it blue
```html
<html lang = "en">

<head>
    <meta charset = "UTF - 8">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
    <style> 
    
    </style>
</head>

<body>
    <!-- Select the element select me 
        and make them blue -->
    <div class = "m1 m2">
        Select me
    </div>

    <div class = "m1">Not me</div>
    <div class = "m2">Not me either</div>
</body>

</html>
```

-> **Solution Add this to the style tags**

```html
<style>
    .m1.m2{
        color:blue;
    }
</style>
```

### Element with multiple classes
* To select elements with more than one class and apply styling `.class1.class2{}`

## Combinators and Descendants

* Combinators - combinations of a few things
* Descendants - Every child or child of a child or anyone lower in the hierarchy compared to the current elements are called its descendants.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/374/original/upload_2f40ec61959ab98b4aed319b32c7cadc.png?1695534285)

A combination of these two elements is called a descendent combinator.

**[Question 3]**
Select "I'm here. Find me" and make it blue 

```html
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8" />
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0" />
    <title>Document</title>
</head>

<body>
    <!-- Select "I'm here. find me" and make it blue  -->
    <div class = "c1">
        <div>
            <div>
                <span>
                    <div class = "c2">I'm here. find me</div>
                </span>
            </div>
        </div>
    </div>
    <span>
        <div class = "c2">Dont select me</div>
    </span>
</body>

</html>
```

**-> Solution**
```html
<style>
.c1 .c2{
    color:blue;
}
</style>
```

In such cases we are using descendant combinators, we must have a space between two classes and the parent class must be written first while styling.

**[Question 4]**
```html
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
</head>
<body>
    <!-- select "I'm the direct son" and make it blue  -->
    <div class = "c1">
        <p>
            I'm the direct son
        </p>
    </div>
    <div class = "c1">
        <span>
            <p>I'm the indirect son</p>
        </span>
    </div>
</body>

</html>
```

-> Solution
```html
<style>
.c1>p{
    color:blue
}
</style>
```

* This selector is called children combinator `.c1>p`. It is a specific/more precise version of the descendent combinator. It selects the direct child of the given parent class.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/375/original/upload_a8d6445e967ccfdb27281b8584672046.png?1695534387)

**[Question 5]**
```html
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
</head>
<body>
    <!-- Select the input element with the value
         "Select Me" and 
        make its background blue  -->
    <form>
        <input type = "button" value = "Click Me">
        <input type = "button" value = "Select me">
        <!-- <div name = "something">Hello</div> -->
    </form>
</body>

</html>
```

**-> Solution:**
```html
<style>
    /*attribute selector*/
    input[value = "select me"]{
        background-color: blue
    }
</style>
```

* To select an element having an attribute with a particular value we use attribute selectors. These are enclosed within square brackets([]).

---

## Summary

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/376/original/upload_7e14d0083629c991f87b63b1b542d0b8.png?1695534492)

## Test Question
**[Question 6]**

```html
<!DOCTYPE html>
<html lang = "en">

<head>
    <meta charset = "UTF - 8" />
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0" />
    <title>Document</title>
</head>
<body>
    <!-- Select the element with "Select me" 
        and make it blue  -->
    <div class = "some-class">
        <div id = "the-one">
            <p>Random</p>
            <div>
                <p class = "c1 c2">Random</p>
            </div>
            <p class = "c1 c2">Select me</p>
            <div class = "c1 c2">Select me</div>
        </div>
    </div>
    <div class = "some-class">
        <p class = "c1">Random</p>
        <p class = "c2">Random</p>
        <p class = "c1 c2">Random</p>
    </div>
</body>
</html>
```

**-> Solution:**
```html
<style>
 .some-class #the-one>.c1.c2{color:blue}
</style>
```

* Try all the combinations suggested by the learners.
* We are firstly taking descendent combinators and then we are specifying that it should have both the classes `c1` as well as `c2`.

---


## Different ways to load CSS in our HTML page

### External file
* Create a file called the `external.css` file.
* add in head section `<link rel="stylesheet" href="external.css">`

### Using style tag
* Adding style tags inside the head section

### Inline styling
* Applying style with the element declaration as follows
```html
<h1 style = "font-family: sans-serif">Hi i am h1</h1>
```

### User Agent Styling
* Default browser stylesheet
* 3 ways to add css to web page
    * external -> using CSS file
    * internal -> using style tag
    * inline -> to the element
* User agent stylesheet -> browser's default style.



**Note:** Recommended way is to use an external style sheet.

---
## Cascading

**Index file**
```html
<!DOCTYPE html>
<html lang = "en">
<head>
    <meta charset = "UTF - 8">
    <meta http-equiv = "X-UA-Compatible" content = "IE = edge">
    <meta name = "viewport" content = "width = device-width, initial-scale = 1.0">
    <title>Document</title>
    <!-- /external -->
   
</head>
<!-- style -->

<link rel = "stylesheet" href = "external.css">
<style>
    h1 {
        background-color: lightgray;
        color: red;
    }
</style>
<body>
    <h1 style = "font-family: sans-serif;
    color:bisque
    ">Hi i am h1</h1>
</body>
</html>

<!-- inline, external, internal -->
<!-- user-agent stylesheet -->
<!-- external -->
```

**External CSS file**
```css

h1{
    color:blue;
}
```

**[Ask the yourself]**
Q What will happen on adding color property to `h1`?

-> It will override

**[Ask the yourself]**
Q Internal properties have higher priority than external properties

-> It will depend on the order. both have the same preference. The nearest to the element is chosen.

### Priority
Inline > External/Internal > Order > User-agent

* **Cascading** means stairs. CSS looks for the most specific and most recent styles used.

---

