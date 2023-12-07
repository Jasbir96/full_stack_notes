# Full Stack LLD: FE Machine coding-6:- Typeahead part-1


---
title: Agenda of the lecture
description: What will be covered in the topic?
duration: 120
card_type: cue_card
---

## Agenda


We will try to cover most of these topics in today's sessions and the remaining in the next.
* AutoComplete/Typeahead
* Make API request (Fetch API)
* Debouncing / Throttling applications
* Removing the state request


It is going to be a bit challenging, advanced, but very interesting session covering topics that are asked very frequently in interviews.

So let's start.

---
title: Typeahead Demo, Scoping the problem
description: Discussion of Typeahead Demo, Scoping the problem in detail.
duration: 2100
card_type: cue_card
---

## Typeahead Demo

In the autocomplete when we type the character 'h' in the search space , then the dropdown list of words containing those letters shown up.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/472/original/upload_aae1cc679eb4c847a6adad4dd0e4febc.png?1695616458)


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/473/original/upload_38eae68bbe203dc5b0de0287adb45d6b.png?1695616524)

Let's start with the implementation of the same.


Initially will be starting by creating a *div* named as "autocomplete" in the html code which will have h2 heading of "Auto Complete" and the input type text of class "search_input".  It will also have the ul of id "suggestion_box".

Then open the html code in the browser and see the changes of the above code added.
```htmlembedded
<body>
    <div class = "autocomplete">
        <h2>Auto complete</h2>
        <input type = "text" class = "search_input" id = "search_input">
        <ul id = "suggestion_box" class = "suggestion_box"></ul>
    </div>
    <script src = "fetchData.js" type = "module"></script>
    <script src = "main.js" type = "module"></script>
</body>
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/474/original/upload_0e43c50a4e0c1d640ef4e963e57e75e5.png?1695616655)

Now will be adding a bit of css to the above code. 

Before this lets quickly discuss the problem statement quickly that is create Autocomplete/ Typeahead.

**MUST HAVE**

* Some of them includes, which must have on typing inside the search box, it shoud suggest related terms. 
* Then using the given api link make the request and get the data. 
* Also user should have the options to select one of the options. 

**GOOD TO HAVE**

* Search should be performant enough. 
* It should avoid unnecessary network calls.
* It should persist previously fetched data.

**SCOPING**

Lets in place of the api endpoint, an array of the related item is provided (explain the same with the example of array of the fruits, where the fn("ba") so it should return us the banana).

* So either the inerviewer provides us with the endpoint link or to use the some local data.
* Is there any design
* min number of character after which the first search should happen.
* should there be any defaults.

---
title: Styling the typeahead 
description: Discussion of css inclued in the styling of the typeahead.
duration: 2100
card_type: cue_card
---

## Styling the Typeahead

Now lets add the css by making the style.css file.

* Copying and pasting some standard style of the css for the above css file. 
* Now add the classes of .autocomplete, .search_input, and .suggestion_box in the css file and start adding the styling to those classes.
* In the autocomplete provide the margin, border, margin, and text-align.
* In the search_input, provide the width, height, margin-bottom and border-radius.
* In suggestion_box, provide the max-height, overflow-y, border, margin, width, and display as none. In the visible class add display as block.

``` css
html {
    box-sizing: border-box;
    font-family: sans-serif;
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


/*  */
.autocomplete {
    border:2px solid gray;
    
    width:30rem;
    margin: 0 auto;
    text-align: center;
}

.search_input {
    width:90%;
    height:3rem;
    border-radius: 0.25rem;
    margin-bottom: 2rem;
}

.suggestion_box {
    margin: 2rem auto;
    width:80%;
    max-height:15rem;
    overflow-y: scroll;
    border:2px solid black;
    display: none;
}
/* i want to show suggestion when there is some text  */
.visible{
    display: block;
}
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/475/original/upload_2b83d270ed7cc1dbe2c15f96388f1a3c.png?1695617016)

---
title: Using fetch API to make network request 
description: Discussion of using fetch API to make network request .
duration: 2100
card_type: cue_card
---

## Using fetch API to Make Network Request
Now create a file named as fetchdata.js and add the code of fetching the data from the given link.

* Start by fetch the url to make the http request which is a promised based function. 
* Then, in the response returning the response in the json form. Also checking the respone in by consoling then we get to know that the response mainly have the header and body. Header contains the metadata(data about data) and body which contains the data.
* The route should be complete and user should be authorised ot make the request.
* In the case of the error then consoling that particular error on the console.

``` javascript
fetch(`https://restcountries.com/v3.1/name/`)
    .then(
        function (response) {
            // response -> data(body)+ metadata(header)
            console.log("response", response)
            return response.json();
        }
    ).then((data) => {
        console.log("data", data);
    }).catch(err => {
        console.log("in catch");
        console.log("hello", err);
    })

```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/476/original/upload_c056a46a92cf1d0add6dddf6374b6047.png?1695617070)


In the provided code, an asynchronous function named `getCountries` is defined, which accepts a `keyword` parameter. Here are the key points of the code:

1. Within a try block, an HTTP GET request is made to the REST API endpoint using the `fetch` function. The `await` keyword is used to ensure that the function's execution pauses until the promise returned by `fetch` resolves, guaranteeing that the response is received before proceeding.

2. The HTTP response body is then parsed as JSON using the `json` method. Again, the `await` keyword is employed to wait for the JSON parsing to complete.

3. A conditional statement checks if the HTTP response status code is 404 (Page Not Found) using `rawResponse.status`. If the status is 404, the code logs a "Page not found" message to the console and returns an empty array, signifying that no data was found.

4. If the response status is not 404, a "Data found" message is logged to the console, indicating a successful response.

5. The parsed JSON data is returned as an object if the response status is not 404.

6. In the event of an exception, such as a network error or an error during JSON parsing, the catch block is executed, and the error is logged to the console using `console.log("err", err)`.

``` javascript
async function getCountries(keyword) {
    try {
        // http response
        const rawResponse = await fetch(`https://restcountries.com/v3.1/name/${keyword}`);
        const response = await rawResponse.json();
        // console.log("data",data);
        console.log(rawResponse.status);
        if (rawResponse.status == 404) {
            console.log("Page not found");
            return [];
        } else {
            console.log("Data found");;
        }
        return response;
    } catch (err) {
        console.log("err", err);
    }

}

export default getCountries;
```


---
title: logic to consume data from api on user input
description: Discussion of logic to consume data from api on user input.
duration: 2100
card_type: cue_card
---

## Logic to Consume Data from Api on User Input

Now lets have the main.js file where we will be having the imported code from the './fetchData.js' 

``` javascript
import getCountries from "./fetchData.js";

// getCountries("india").then(function (res) {
//     console.log(res)
// })
const inputBox = document.getElementById("search_input");
const suggestionBox = document.getElementById("suggestion_box");

const handleSearch = async (keyword) => {
    const countriesArr = await getCountries(keyword);
    const countryNameArr = countriesArr.map((country) => country.name.common);
    return countryNameArr;

}
const handleSuggestions = async (e) => {
    console.log(e.target.value);
    const keyword = e.target.value;
    const countryNameArr = await handleSearch(keyword);
    populateSuggestionBox(countryNameArr);
}
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/477/original/upload_f718c3d0df1998aa9285b86b5a575e82.png?1695617142)

To make our code modular, we are exporting the getCountries code from the fetchdata .js and importing in the main.js.

1. `const inputBox = document.getElementById("search_input");`: This line selects an HTML element with the ID "search_input" and assigns it to the variable `inputBox`. Typically, this would be an input field where users can enter search queries.

2. `const suggestionBox = document.getElementById("suggestion_box");`: This line selects an HTML element with the ID "suggestion_box" and assigns it to the variable `suggestionBox`. This element is likely to be used to display search suggestions.

3. `const handleSearch = async (keyword) => {`: This code defines an asynchronous function called `handleSearch` that takes a `keyword` parameter. It appears to be responsible for performing a search operation using the `getCountries` function and then populating the suggestion box.

4. `const ans = await getCountries(keyword);`: This line calls the `getCountries` function with the provided `keyword` and awaits its result. The result is assigned to the variable `ans`, which is not currently used in the code.

5. `populateSuggestionBox();`: This line attempts to call a `populateSuggestionBox` function, but it appears to be missing in the provided code. You should define this function separately to populate the suggestion box with search results.

6. `const handleSuggestions = (e) => {`: This code defines an event handler function called `handleSuggestions`. It takes an event (`e`) as its parameter and is likely intended to respond to input changes in the `inputBox`.

7. `console.log(e.target.value);`: This line logs the current value of the input element that triggered the event. It provides a way to check the user's input in the console.

8. `const keyword = e.target.value;`: This line captures the user's input from the event object and assigns it to a `keyword` variable.

9. `handleSearch(keyword);`: This line calls the `handleSearch` function with the `keyword` obtained from the input event. This is likely intended to trigger a search operation when the user types in the input field.

10. `inputBox.addEventListener("input", handleSuggestions);`: This line adds an event listener to the `inputBox` element, specifically listening for the "input" event. When the user types in the input field, the `handleSuggestions` function is executed, which, in turn, triggers the search operation.

In summary, this code sets up event handling for an input field ("search_input") where users can enter search queries. When users input text, it triggers a search operation (`handleSearch`) and displays suggestions in a suggestion box (`suggestion_box`). The code relies on the `getCountries` function to perform the actual search and is designed to be responsive to user input events. However, it assumes the existence of a `populateSuggestionBox` function, which should be defined separately to display search results in the suggestion box.

---
title: Edge case stale responses
description: Discussion of Edge case like stale responses.
duration: 2100
card_type: cue_card
---

## Edge Case Stale Responses

Just see on thing that we have got the 3 things firts for the h, ha and then for ham. 
**Asking the Question:** 
Generally where should the result 105 should belongs to h,ha,or ham?

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/479/original/upload_84cf3e1d972d6bf9968cf5354bfb714c.png?1695618326)

lets consider an example of client making a request to the server. As we all know that making a request is an asynchronous process. Now, request is made on h , the server will search for the h in the database. But if at the client side "ham" is typed then the request is made for the h,ha,ham are all made to the server at the same time. So it is possible that the request which is made before the response will be send later. 

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/480/original/upload_fb949b52c8d5801a287255c3cd584a38.png?1695618372)


**Asking Question:**

So is there any use case of this third result on this keyword?
It will never be a good use case. Because request are asynchronous hence order is not strict so we can have the wrong data at the wrong time.

So while building this type of systems , if we have  received the response from any of the request then should negate all other results.


---
title: Populating data in suggestion box
description: Discussion of populating data in suggestion box.
duration: 2100
card_type: cue_card
---

## Populating Data in Suggestion Box

Here's an explanation of the functionalities of adding the function populateSuggestionBox are as follows :

``` javascript
const populateSuggestionBox = (countryNameArr) => {
    if (countryNameArr.length) {
        suggestionBox.classList.add("visible");
    } else {
        suggestionBox.classList.remove("visible");
    }
    // before showing any result -> reste you suggestion box
    suggestionBox.innerHTML = "";

    const fragment = document.createDocumentFragment();
    // add all the lis to that fragment
    countryNameArr.forEach((countryName) => {
        const li = document.createElement("li");
        li.innerText = countryName;
        fragment.appendChild(li);
    })
    suggestionBox.appendChild(fragment);

}
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/481/original/upload_205677f7f5265c4da3306afdf39d2a8a.png?1695618469)

1. `const populateSuggestionBox = (countryNameArr) => {`: This code defines a function named `populateSuggestionBox`, which is responsible for populating the suggestion box based on an array of country names (`countryNameArr`) passed as a parameter.

2. `if (countryNameArr.length) {`: This conditional statement checks if the `countryNameArr` array has elements, indicating that there are suggestions to display.

3. `suggestionBox.classList.add("visible");`: If there are suggestions to display, this line adds the "visible" CSS class to the `suggestionBox` element, making it visible to the user.

4. `} else {`: If there are no suggestions to display (i.e., the array is empty), the code inside this block is executed.

5. `suggestionBox.classList.remove("visible");`: In this case, the "visible" CSS class is removed from the `suggestionBox` element, hiding it from the user.

6. `// before showing any result -> reset your suggestion box`: This comment suggests that the suggestion box should be cleared/reset before displaying new suggestions. However, the code to achieve this is missing in the provided snippet.

7. `const fragment = document.createDocumentFragment();`: This line creates a document fragment called `fragment`, which is an efficient way to append multiple DOM elements without causing unnecessary reflows or repaints.

8. `countryNameArr.forEach((countryName) => {`: The code enters a loop that iterates through each country name in the `countryNameArr` array.

9. `const li = document.createElement("li");`: Inside the loop, a new list item (`<li>`) element is created for each country name.

10. `li.innerText = countryName;`: The text content of the list item (`<li>`) is set to the current `countryName` value.

11. `fragment.appendChild(li);`: Each list item is appended to the `fragment`, which efficiently accumulates all the list items.

12. `suggestionBox.appendChild(fragment);`: After the loop, the entire `fragment`, containing all the list items, is appended to the `suggestionBox`, updating the suggestion box with the new suggestions.

Now, regarding the additional code you mentioned, it appears to be incomplete and contains a typo (`console 1ogle target value)`). If you have additional code or a specific explanation you'd like me to provide for it, please provide the corrected code or specify the context, and I'd be happy to assist further.



---
title: Debouncing
description: Discussion of concept of debouncing.
duration: 2100
card_type: cue_card
---

## Debouncing

As we are taking the input by making the API calls. Example user starts an action hello and time is taken while typing that particular characters. Debouncing says that you have typed the input that you want to give , then only operations start happening.

If the interval between the two subsequent key press is more than the delay then only it will be called again.

Introducing debouncing in your code can help control the rate at which a function is executed, especially when it's triggered by frequent or rapid events like user input. Let's break down the debouncing code provided and explain it clearly:

```javascript
function debounce(fn, delay = 1000) {
    // intial 
    let timerId;
    return function (...args) {
        //grumpy 
        if (timerId) {
            console.log("I am reseting you now wait again from the start")
            clearTimeout(timerId)
        }
        // they want to call the fn after a delay
        timerId = setTimeout(function () {
            fn(...args);
        }, delay)
    }
}

inputBox.addEventListener("input", debounce(handleSuggestions));

```

Here's what's happening step by step:

1. `debounce(fn, delay)`: This is a higher-order function that takes two parameters:
   - `fn`: The function that you want to debounce (in your case, `myFN`).
   - `delay`: The delay in milliseconds (defaulting to 500 milliseconds if not provided).

2. Inside `debounce`, a `timerId` variable is declared. This variable will hold the identifier of the timer that will be set to delay the execution of the provided function (`fn`).

3. The function returns another function (a closure) that accepts any number of arguments (`...args`). This inner function will be used to debounce the original function `fn`.

4. Inside the inner function:
   - It checks if a timer with the identifier `timerId` is already set (i.e., if the function is being called again before the delay has elapsed). If so, it logs a message indicating that the timer is being reset, and it clears the existing timer using `clearTimeout(timerId)`. This action effectively cancels the previous call to `fn`.
   - After clearing the timer (if it exists), it sets a new timer using `setTimeout`. This new timer will execute the provided function `fn(...args)` after the specified delay (`delay`).

5. `myFN` is a sample function that you want to debounce. When you call `debounce(myFN, 1000)`, it creates a new debounced function named `dbFN` that will only call `myFN` if there's a 1000-millisecond gap between subsequent calls to `dbFN`.

In practical terms, this debouncing mechanism ensures that if you rapidly invoke `dbFN`, it will only execute `myFN` once, after a delay of 1000 milliseconds from the last invocation. This is useful in scenarios like handling user input, where you want to delay execution until the user has finished typing or interacting with an input field, improving performance and responsiveness.

