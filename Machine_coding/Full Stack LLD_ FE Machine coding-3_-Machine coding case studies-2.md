# Full Stack LLD: FE Machine coding-3:-Machine coding case studies-2

---
description: What will be covered in the topic?
duration: 120
---

## Agenda

We will try to cover most of these topics in today's sessions and the remaining in the next.
* CountDown Timer - 2


It is going to be a bit challenging, advanced, but very interesting session covering topics that are asked very frequently in interviews.

So let's start.

---
title: CountDown Timer
description: Discussion of adding the functionalities of start,pause,continue and reset.

---

## CountDown Timer

Lets take a look at what have been already covered in the last class. 
Discussion of the must have features such as:
* Pass time in hours, minutes and seconds.
* Start, pause, continue.
* Validate that if the user enters any time greater 60 then it should move to the left bit.
    * Edge case Validation.
    * Negative values.
    * Hours greater than 24 hours.


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/050/468/original/upload_e688692d271f726f22c6465d22d6106f.png?1695615406)

Discussion about the basics Html already completed which covers:
* Container having a h1, below that having a timer label and below this having three h2 for the hours, minutes and seconds.
* Three boxes for the inputof the hours, minutes and seconds.
* After this having four buttons namely start, reset, pause, and continue.
* Coming about the inputs which are all numbers type.

**Asking Question:**

What can be done on the 'oninput'?
You can manage which type of entry of the data can be given to input elem.

Now lets start the discussion of the js code:

* Start with the selection of the elements such as start, reset, continue, pause for the buttons and added the event listeners foe these button.

* Next will be startBtn eventlistener, in this we have to perform the three operations mainly as:
    * Taking the input:
        * validate the input
        * transform the input
        * process transformed input
    * Change the UI

Lets start with taking input, as mins, hrs, and secs variable and also set the default values as 0 if empty otherwise they will be undefined. Parsin the inputs into integers values.

Now coming to the validation of the input, lets start with the implementation of the function `validateInputs()`, in this function check the conditions for the various valid values of hrs, min and secs.
To include the transform in the validate function, check if the secs and minutes exceed the value of the 60 check respective changes will be made in the secs, minutes, and hours.

**Suggestion of always use 'USE STRICT'.**

Now the implementation of the processing of the transformed inputs,  where the respective maximum values of the seconds, minutes and hours in the respective acceptable values and returned as objects.

Now using the function `timer()`, which process the input every one second and also change the UI. 
Updating the UI can happen in the two ways where we can use the countDownTime where hrs:min:secs will be put into the UI (or) get it from UI and by perform respective operation of condition based if sec > 0 the sec-- (or) sec == 0 then min-- (or) min == 0  && sec == 0 then hrs-- (or) if min == 0 && sec == 0 then **stop**.

Out of the calculation and the DOM manipulation, the DOM manipulation changes are the costliest one.

Lets calculate the respective values of the hours, minutes and seconds from the countdown time by performin the below operations.

```javascript
let hrs = Math.floor(countDownTime / 3600);
let mins = Math.floor((countDownTime % 3600) / 60);
let secs = countDownTime % 60;

// The if statement below should be part of the countdown logic.
// It checks if the countdown has reached zero.

if (hrs == 0 && mins == 0 && secs == 0) {
    // The following code should be inside this if block.

    if (secs > 0) {
        secs--;
        // Correct the interpolation syntax below.
        secInput.value = secs < 10 ? `0${secs}` : `${secs}`;
        return;
    }
    if (mins > 0) {
        mins--;
        minInput.value = mins < 10 ? `0${mins}` : `${mins}`;
        secInput.value = 59;
        return;
    }
    if (hrs > 0) {
        hrs--;
        minInput.value = 59;
        secInput.value = 59;
        // Correct the variable name from "hrsTnout" to "hrsTimeout".
        hrsTimeout.value = hrs < 10 ? `0${hrs}` : `${hrs}`;
        // Also, it's not clear what "hrs3:`" is supposed to represent; it should be corrected.
    }
}

```

Subsequently, based on the above conditions we can modify the result of the function to set the values in the UI.

Certainly, let's break down and explain the corrected code:

1. **Time Calculation:**
   - `hrs`, `mins`, and `secs` are variables used to calculate and represent the hours, minutes, and seconds remaining based on the `countDownTime` variable. This calculation divides the total time in seconds into hours, minutes, and seconds components.

2. **Countdown Logic:**
   - The code contains an `if` statement that checks whether the countdown timer has reached zero. This is done by checking if all three components (`hrs`, `mins`, and `secs`) are equal to zero.

3. **Countdown Handling:**
   - If the countdown timer has reached zero, the code enters the `if` block and executes the countdown handling logic.

   - Inside this block, there are three conditional checks:
     - If `secs` (seconds) is greater than zero, it decreases by one, and the `secInput` value is updated accordingly. The interpolation ensures that the displayed value is in two-digit format (e.g., "05" instead of "5").

     - If `mins` (minutes) is greater than zero, it decreases by one, and the `minInput` value is updated accordingly. Again, the interpolation ensures a two-digit format. Additionally, `secInput` is set to 59 because when the minute changes, the seconds should reset to 59.

     - If `hrs` (hours) is greater than zero, it decreases by one. Both `minInput` and `secInput` are set to 59 because when an hour changes, both minutes and seconds should reset to 59. The variable `hrsTimeout` is updated with the new hours, also in a two-digit format.

4. **Variable Corrections:**
   - The code correctly changes "hrsTnout" to "hrsTimeout," which appears to be a variable intended to hold the remaining hours.

5. **Additional Comments:**
   - There is a comment in the code explaining its purpose and suggesting where the countdown logic should be placed.

Overall, this code handles the countdown timer logic by decrementing the hours, minutes, and seconds, and updating the corresponding input fields to display the remaining time in a user-friendly format. It ensures that the displayed values are always in a two-digit format for consistency.



```javascript
resetBtn.addEventListener("click",function(){
    minInput.value = 0.0;
    secInput.value = 0.0;
    hrsInput.value = 0.0;
    
    clearInterval(counterID);
})
```

Lets talk about the resetbtn , for this we have to pass clearInterval with counterID and then will be settin the minInput, secInput, and HrsInput all as 0.

Lets talk about the pausebtn, 

**Asking about any ideas?**

Basically we have to just clear the interval, and pause the processing and there will be no change on the UI.

In the continuebtn, functionality of the timer function with the parameter as saveTimeLeft is passed. Here the savedtime will be passed with the help of timer function. 


Now when the UI is concerned, when intially start is visible on the UI at that time the pause,continue are hidden, as we click on the start, the pause will be visible on the UI, and start will be hidden similary on clicking the pause then the continue will appear on the UI.

The above functionalities can be done by adding the style.display as none in the startBtn function, pauseBtn and in the continueBtn.

**Suggestion for the Machine Coding Round:**

* DRY: do not repeat yourself
* Single responsibility principles

