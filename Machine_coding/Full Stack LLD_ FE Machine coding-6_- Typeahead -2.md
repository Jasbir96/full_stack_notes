# Full Stack LLD: FE Machine coding-6:- Typeahead -2

---
title: Agenda of the lecture
description: What will be covered in the topic?

---

## Agenda

We will try to cover most of these topics in today's sessions and the remaining in the next.
* Debouncing and Throttling
    * Implement
    * Usecase
* Abort Controller
    * Timeout a request
    * How can you manage stale request
* Cache an Http request

It is going to be a bit challenging, advanced, but very interesting session covering topics that are asked very frequently in interviews.

So let's start.

---
title: Intuition for debouncing and throttling 
description: Explain what is debouncing and throttling and their use cases.

---

# Intuition for Debouncing and Throttling

Basically on the high level both are used to apply limits

## Throttling


Throttling is a technique used in web development to limit the rate at which a particular action or event can be triggered.

For example, you want to implement throttling for a coupon code entry on a checkout page with a 2-hour limit starting from 8:30 AM. This means you want to restrict users from applying coupon codes too frequently during this 2-hour window.

In the example usage, we attempt to apply a coupon at 8:30 AM and then at 9:30 AM. The first attempt succeeds, but the second attempt shows a cooldown message because it's within the 2-hour limit.


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/482/original/upload_af2777248006a90ff715ba72d36390cc.png?1695619345)

This is a basic example of throttling coupon code applications to prevent abuse or frequent applications within a specific time frame. You can adapt and expand upon this concept as needed for your specific use case. Here time represent the interval.

**Algorithm:**
* Only the first call is registered and acted upon.
* All the subsequent function calls are **ignored**.

**Use Cases:**
* Apply a coupon
* Scroll event
* Resize

## Debouncing

Debouncing is a useful technique when implementing features like autocomplete in a search bar, particularly when dealing with user input events like typing. It helps reduce unnecessary requests to a server or processing of input, improving the performance and user experience. Let's explain how debouncing is used for autocomplete in a Google search bar:

**1. User Input Event:**
   - When a user starts typing in the Google search bar, the `input` event is triggered with each keystroke. This event is used to detect user input and initiate autocomplete suggestions.

**2. Debouncing Function:**
   - A debounce function is employed to control the timing of autocomplete suggestions. This function is responsible for delaying the execution of a search query until the user has finished typing, rather than immediately responding to every keystroke.

   - The debounce function introduces a delay, usually measured in milliseconds, before executing a search query. This delay allows users to type several characters before any autocomplete suggestions are shown.
   - A timer is set when the `input` event occurs. If the user continues typing, the timer is reset. This effectively restarts the countdown to executing the search query.

   - Once the user stops typing or pauses for a specified duration (the debounce delay), the search query is executed.
   - If the user types quickly, the debounce function ensures that only one search query is triggered after the user stops typing, rather than firing multiple queries for each keystroke.

In this example, the `debounce` function is used to wrap with a 10-millisecond delay. The `input` is attached to the search input, and the `performSearch` function is called only after a 10-millisecond pause in user input, effectively throttling the search requests. This ensures that autocomplete suggestions are fetched only when the user has paused typing as in searching of the word 'ROBERT'.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/483/original/upload_18e16495e25d7dc0dca5e79fc52f9cb9.png?1695619425)

**Throttling for the first call and debouncing for the latest result.**


---
title: Code for throttling and debouncing 
description: Explain and implement the codes for the throttling and debouncing.

---

## Throttle function
Let's explain the throttle function:

```javascript

let productValue = 100;
function apply20Coupon() {
    console.log("New price", productValue - (productValue * 0.2));
}
function throttle(fn, interval = 500) {
    let shouldWait = false;
    console.log("1 returning fn");
    return function (...args) {
        //prevent it also 
        if (shouldWait == true) {
            console.log(" 3 I have been called before");
            return;
        }
        fn(...args);
        shouldWait = true;
        // console.log(" 2Called fn first time");
        // console.log(" 2should Wait", shouldWait);
        setTimeout(() => {
            shouldWait = false;
        }, interval)
    }
}






const throttledApplyCoupon = throttle(apply20Coupon, 3000);
console.log("```````````````````");
throttledApplyCoupon();  //1
console.log("```````````````````");
throttledApplyCoupon();  //2


setTimeout(() => {
    console.log("at t=2000");
    throttledApplyCoupon(); //3
}, 2000);

setTimeout(() => {
    console.log(" at t=5000");
    throttledApplyCoupon(); //4
},5000);
```


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/484/original/upload_eb25bf825e4545ec03b40a85c490e31a.png?1695619471)


Here's an explanation of how the corrected throttle function works:

1. `throttle` is a higher-order function that takes two parameters: `fn` (the function you want to throttle) and `interval` (the minimum time interval between consecutive executions of the throttled function).

2. Inside the `throttle` function, there's a `shouldWait` flag initialized as `false`. This flag is used to control whether the throttled function should execute or not.

3. The `return function (...args) { ... }` block represents a closure that returns a new function. This new function is the throttled version of the original function (`fn`).

4. Inside this new function:
   - It checks if `shouldWait` is `true`. If it is, it immediately returns without executing the throttled function, preventing multiple calls within the interval.
   - If `shouldWait` is `false`, it calls the original function `fn` with the provided arguments.
   - After calling the original function, it sets `shouldWait` to `true` to indicate that the throttled function was just executed.
   - It logs that the function was called for the first time and the value of `shouldWait`.

5. To ensure that the throttled function becomes available for execution again after the specified `interval`, a `setTimeout` is used. It sets a timeout to reset `shouldWait` to `false` after the `interval` has passed. This allows the function to be called again once the interval elapses.


## Debounce function

Let's explain the debounce function:

```javascript
function debounce(fn, delay = 500) {
    let timeoutID = null;
    return function (...args) {
        if (timeoutID) {
            // reseting the timer 
            console.log("reseting the timers");
            clearTimeout(timeoutID)
        }

        timeoutID = setTimeout(() => {
            fn(...args);
        }, delay)
    }
}

function printHello() {
    console.log("hello");
}
const debouncedPrintHello = debounce(printHello, 2000);
debouncedPrintHello();


setTimeout(() => {
    debouncedPrintHello();
    setTimeout(() => {
        debouncedPrintHello();
    }, 1000);
}, 1000)
```

Now, let's explain the code step by step:

1. The `debounce` function takes two parameters:
   - `fn`: The function you want to debounce.
   - `delay`: The time in milliseconds to wait before executing `fn` after the last call. By default, it's set to 500 milliseconds.

2. Inside the `debounce` function, there's a `timeoutID` variable initialized as `null`. This variable is used to keep track of the setTimeout timer.

3. The `return function (...args) { ... }` block represents a closure that returns a new function. This new function is the debounced version of the original function (`fn`).

4. Inside this new function:
   - It checks if `timeoutID` is truthy (i.e., there's an existing timeout).
   - If there's an existing timeout, it clears that timeout using `clearTimeout`. This step cancels the previously scheduled execution of the function.
   - Then, it sets a new timeout using `setTimeout` that will execute the function (`fn`) after the specified `delay` milliseconds.
   - The `...args` spread operator allows passing any arguments to the debounced function.

5. The example function `printHello` is used as an example function to be debounced.

6. `debouncedPrintHello` is created by calling `debounce(printHello, 2000)`, which means that `printHello` can be called at most once every 2000 milliseconds (2 seconds).

7. The `debouncedPrintHello` function is initially called, and it logs "hello" after a 2-second delay.

8. After another 2 seconds, the `debouncedPrintHello` function is called again within the `setTimeout`, which resets the debounce timer. This demonstrates how debouncing works to delay the execution of a function until a specified delay has passed since the last call.

---
title: Abort controller code example 
description: Show the use of the `AbortController` and the `AbortSignal` interfaces.
---

## Abort controller code example

The code demonstrates the use of the `AbortController` and the `AbortSignal` interfaces in JavaScript for aborting fetch requests. Let's break down the code and explain how the `AbortController` is used:

1. **Create an `AbortController` instance**:
   ```javascript
   const abortController = new AbortController();
   ```
   This line creates a new `AbortController` object named `abortController`. The `AbortController` object allows you to control and potentially abort one or more asynchronous operations, such as fetch requests.

2. **Attach the signal to a fetch request**:
   ```javascript
   const responsePromise = fetch("https://restcountries.com/v3.1/name/india", {
     signal: abortController.signal
   });
   ```
   Here, a fetch request is made to the URL "https://restcountries.com/v3.1/name/india," and the `signal` option is set to the `signal` property of the `abortController`. This signal links the fetch request to the abort controller, allowing you to abort the request if needed.

3. **Sending the request**:
   ```javascript
   console.log("Request is sent");
   ```
   This line simply logs a message indicating that the fetch request has been sent.

4. **Handling the fetch response**:
   ```javascript
   const response = await responsePromise;
   const ans = await response.json();
   ```
   These lines handle the response of the fetch request. The response is awaited, and the JSON data is extracted from it. This part of the code is responsible for processing the fetch response once it's received.

5. **Aborting the request**:
   ```javascript
   abortController.abort();
   ```
   This line aborts the fetch request by calling the `abort()` method on the `abortController`. This is useful if you want to cancel an ongoing fetch request, for example, in response to user input or specific conditions.

6. **Error handling**:
   ```javascript
   } catch (err) {
     if (err.name == "AbortError") {
       console.log("Fetch request was aborted.");
       return err.name;
     } else {
       console.error("Fetch error:", error);
     }
   }
   ```
   
   ![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/485/original/upload_bbf1c55766bfd525703291c9115ccc56.png?1695619582)

   In this code block, error handling is implemented using a try-catch block. If the fetch request is aborted, it logs a message and returns the error name "AbortError." If any other error occurs during the fetch, it is caught and logged as a "Fetch error."

---
title: Request with timeout
description: Will be explaining to add a timeout to a fetch request.

---


## Request with Timeout

To add a timeout to a fetch request using the `AbortController`, you can utilize the `AbortSignal` in combination with the `setTimeout` function. This allows you to specify a maximum time duration for the request to complete, and if it exceeds that duration, you can abort it. Here's how you can modify the code to include timeout options:

```javascript
// timout after 2 seconds
// inbuilt API 
// 1 create a instance from AbortController
const abortcontroller = new AbortController();
(async function () {
    // fetch -> url , options
    try {
        // 2 attach signal to a fetch request 
        const responsePromise = fetch("https://restcountries.com/v3.1/name/ind",
            { signal: abortcontroller.signal });
        console.log("request is send");
        // 3 call abort 
        setTimeout(() => {
            // 2second
            try{
                console.log(" calling abort")
                abortcontroller.abort();
            }catch(err){
                if (err.name == "AbortError") {
                    // Request was aborted
                    console.log('Fetch request was aborted.');
                    return err.name;
                } else {
                    console.error("Fetch error:", error);
                }
            }
        }, 2000);
        const response = await responsePromise;
        const ans = await response.json();
        console.log("ans", ans);

    } catch (err) {
       
    }
})();

//  implement -> request timeout -> response -> 2sec -> abort it 
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/486/original/upload_0083125c3aefb81b5ddbe4f788b563b6.png?1695620160)


Explanation:

1. We define a `timeoutDuration` variable, which specifies the maximum time duration in milliseconds for the request to complete. In this example, it's set to 5000 milliseconds (5 seconds).

2. Inside the fetch request, we use the `AbortController`'s `signal` property to link it to the abort controller.

3. We set a timeout using `setTimeout`, which will trigger if the request takes longer to complete than the specified `timeoutDuration`.

4. In the timeout callback function, we abort the request by calling `abortController.abort()`. We also log a message indicating that the request timed out.

5. In the asynchronous function, we use a try-catch block to handle the fetch request. If the request succeeds, we clear the timeout using `clearTimeout(timeoutId)` because it completed before the timeout triggered.

6. If the request fails or is aborted (due to a timeout), we handle the errors accordingly in the catch block.

By adding this timeout mechanism, you can ensure that the fetch request won't take longer than the specified duration, helping to prevent long delays in your application due to slow or unresponsive servers.

---
title: Cancelling older requests using abort controller
description: Explore and explain the code for cancelling older requests using abort controller.

---

## Cancelling Older Requests Using Abort Controller
Cancelling older requests using the `AbortController` methodology is a valuable technique in scenarios where you want to ensure that only the most recent request is processed, and any older, pending requests are aborted. This can be particularly useful in situations where user interactions trigger multiple asynchronous operations, such as fetching data, and you want to prioritize the most recent user request while saving resources and preventing unnecessary work.

Let's break down the process of adding request cancellation using the `AbortController` and provide a detailed explanation in approximately 1000 words.

### Understanding the Need for Request Cancellation

Imagine a web application where users can search for products, and each search triggers a network request to fetch relevant product data. Users can type quickly or change their search terms frequently. Without request cancellation, all pending requests would be sent to the server, even if they're outdated due to new user input. This can lead to several issues:

1. **Wasted Resources:**<br> Sending multiple outdated requests consumes bandwidth, server resources, and processing power, impacting server performance and increasing latency.

2. **Inconsistent UI:**<br> Displaying results from older requests can lead to an inconsistent user interface, where data from previous searches briefly appears before being replaced by the most recent results.

3. **User Frustration:**<br> Slow or irrelevant results can frustrate users, as they may have to wait for the outdated requests to complete before seeing the desired data.

To address these issues, you can implement request cancellation using the `AbortController`. This technique ensures that only the most recent request is sent to the server, while older requests are explicitly cancelled, saving resources and providing a smoother user experience.

### Implementing Request Cancellation with AbortController

To achieve request cancellation, follow these steps:

1. **Create an AbortController for Each Request:**
   - For every asynchronous operation that can be cancelled (e.g., a network request), create a new `AbortController` instance.

   ```javascript
   const abortController = new AbortController();
   ```

2. **Attach the AbortSignal to the Fetch Request:**
   - When making a network request (e.g., using the `fetch` API), attach the `AbortSignal` from the `AbortController` to the request's `signal` option. This signal is used to link the request to the controller.

   ```javascript
   const responsePromise = fetch("https://api.example.com/search", {
     signal: abortController.signal
   });
   ```

3. **Canceling Older Requests:**
   - Before starting a new request, check if there's an existing request associated with the same `AbortController`.
   - If an older request exists, abort it using `abortController.abort()`. This ensures that only the most recent request is processed, and older ones are cancelled.

   ```javascript
   // Cancel the previous request before starting a new one
   if (abortController) {
     abortController.abort();
   }
   ```

4. **Handling Cancellation Errors:**
   - When a request is aborted, it can throw an `AbortError`. Handle this error gracefully to prevent application crashes or undesired behavior.

   ```javascript
   try {
     const response = await responsePromise;
     // Handle response data
   } catch (err) {
     if (err.name === "AbortError") {
       // Request was cancelled, handle it as needed
     } else {
       // Handle other errors
     }
   }
   ```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/487/original/upload_488bf9096caffed776cd0461a17506d2.png?1695620292)

In this scenario, request cancellation ensures that only the results corresponding to the user's latest input are displayed. It avoids displaying potentially outdated or irrelevant search results from previous queries, creating a more user-friendly and responsive search feature.

Request cancellation using the `AbortController` methodology is a powerful technique for managing asynchronous operations in web applications. It allows you to prioritize the most recent user interactions, conserve resources, and provide a more responsive user experience. 

---
title: Caching API request
description: Explore the designe to implement caching for API requests.

---

## Caching API Request

The `cachedFetch` function is designed to implement caching for API requests. It caches the responses of API requests and returns cached data if the request is made again within a specified expiration time (`expirey`). If the request is made after the cache has expired, it fetches the data from the API and caches it for future use. This function helps reduce redundant API calls and improve the performance of your application. Let's break down how this `cachedFetch` function works:

```javascript
/***
 * Problem statement -> cachedFetch(timer);
 * */


function cachedFetch(expirey) {
    // Cached data storage
    const cache = {};
    return async function cachedFetch(url) {
        // Check if data is available in cache
        if (cache[url]) {
            const cachedData = cache[url];
            const cacheTime = cachedData.timestamp;
            const currentTime = new Date().getTime();
            if (currentTime - cacheTime < expirey * 60 * 1000) {
                return cachedData.data;
            }
        }

        // Data not in cache or cache expired, fetch and cache
        try {
            const response = await fetch(url);
            const data = await response.json();

            // Store data in cache
            cache[url] = {
                data,
                timestamp: new Date().getTime()
            };

            return data;
        } catch (error) {
            console.error('Fetch error:', error);
            throw error;
        }
    };
}

const cachedFetch = createCachedFetch(10000);
// Example usage
const apiUrl = 'https://api.example.com/data';

cachedFetch(apiUrl)
    .then(data => {
        // Handle the fetched data
        console.log(data);
    })
    .catch(error => {
        console.error('Error:', error);
    });
```

By using this `cachedFetch` function, you can efficiently cache API responses and minimize unnecessary API calls, resulting in improved performance and reduced server load. The cache expiration time (`expirey`) allows you to control how long the data remains cached before being refreshed from the API.

 