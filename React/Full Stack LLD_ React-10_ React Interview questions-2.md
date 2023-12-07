# Full Stack LLD: React-10: React Interview questions-2

---
title: Agenda
description: 

---

# Agenda

- Testing in frontend
- React unit testing 
- Class based components


---
title: Testing
description: Testing definition

---


# Testing
## Definition : 
    your code matches the required specification

- Testing is the process of evaluating and validating a React application's functionality to ensure it meets the desired quality and performance standards.


## Types of Testing 
### Way to you test
* Manual testing 
* Automated testing

### Areas of testing

- **Unit Testing:**
   - *Description:* Verifying the smallest units (components) of the app in isolation to ensure individual functionality.
- **Functional Testing/Integration Testing:**
   - *Description:* Assessing how multiple components interact to achieve specific functions within the application.
- **End-to-End Testing:**
   - *Description:* Evaluating the entire application workflow to ensure alignment with specified requirements.
- **Stress Testing:**
   - *Description:* Assessing how well the system performs under extreme conditions, identifying breaking points.
- **Performance Testing:**
   - *Description:* Measuring responsiveness and efficiency to ensure smooth user experience under normal conditions.
- **Security Testing:**
   - *Description:* Identifying vulnerabilities and weaknesses to ensure protection against security threats.

## React: unit testing  
## Tech:(create-react-app) 
* JEST 
    * test runner: it finds and executes all the tests
    * It also provides you feature of describe, test, and expect
    * snapshot testing
    *  https://jestjs.io/

* React testing library
        * emulates Rendering
        * find an element on that emulated UI
        * fire event




### What is required to test a react component???

- **Snapshot Test:**
   - **Explanation:** A snapshot test captures the current state of a React component's rendered output. It takes a snapshot of the component's markup and compares it to the stored snapshot. If there are unintended changes, the test fails, highlighting potential issues with the component's visual representation.
   - **Use Case:** Verifying that the rendered output of a component remains consistent over time, helping detect unexpected changes.
- **Initial State:**
   - **Explanation:** Testing the initial state of a React component involves ensuring that, upon rendering, the component initializes its state as expected. This is crucial for validating that the component starts in the correct state before any user interactions or data updates occur.
   - **Use Case:** Confirming that a form component begins with empty input fields or that a counter component starts with an initial count of zero.
- **Update to That Initial State:**
   - **Explanation:** This aspect of testing involves simulating user interactions or external events that trigger updates to the component's state. It ensures that the component responds correctly to changes and updates its state as intended.
   - **Use Case:** Testing that a button click updates the state of a counter component or that user input correctly modifies the state of a form component.

### When to write a unit test case (Recommendation)
* only those components which critical/complex

## TDD: Test-driven Development (red-green)
`Usecase`: requirements are stable
* First write all the test cases -> write the component
* refractor all your test cases -> optimize your component

---
title: React unit testing
description: Explanation of testing in react

---


# React Unit testing

## Usage

-> To test individual components in isolation.
-> You have to test the component's functions, logic, and rendering.
-> Tools to be used: Jest and RTL(React Testing Library)

## Jest 

It's a testing framework provided by Facebook. -> It's a test runner and provides the assertions, mocking, snapshot testing, and code coverage as well.

## Setting up Jest



1. Visit Jest documentation: [Jest Docs](https://jestjs.io/)

2. Install the required Babel dependencies for transpiling your code:

   ```
   npm install --save-dev @babel/core @babel/preset-env @babel/preset-react
   ```

3. Create a configuration file either as `.babelrc` or `babel.config.js` and specify the presets:

   ```json
   {
      "presets": ["@babel/preset-env", "@babel/preset-react"]
   }
   ```

4. Create a Jest configuration file `jest.config.js`:

   ```javascript
   export default {
       transform: {
           "^.+\\.(js|jsx|ts|tsx)$": "babel-jest",
       },
   };
   ```

5. Install Jest, Jest's DOM environment for running tests in a browser-like environment, and React Test Renderer:

   ```
   npm i -D jest jest-environment-jsdom react-test-renderer
   ```

6. Update your Jest configuration in `jest.config.js` to specify the test environment as 'jsdom':

   ```javascript
   export default {
       testEnvironment: 'jsdom',
       transform: {
           "^.+\\.(js|jsx)$": "babel-jest",
       },
   };
   ```

Setting up React Testing Library (RTL):

1. Explore the RTL documentation: [RTL Docs](https://testing-library.com/docs/)

2. Install the RTL package:

   ```
   npm install @testing-library/react
   ```

3. Update your test script in the `package.json` file to run Jest on your source files. Add or modify the "test" script like this:

   ```json
   "scripts": {
       "test": "jest src"
   }
   ```

4. Run your test cases using the following command:

   ```
   npm run test
   ```

This setup will allow you to write and run tests for your React components using Jest and React Testing Library, ensuring a user-centric testing approach.


---
title: Class-based components
description: Class-based components description
duration: 3000
card_type: cue_card
---


# Class-based components

## class-based component
* The initial state is defined inside the constructor
*  you this is only available inside the life cycle methods
* this.setState to update the

## Stages of a react  component

- **Mounting (Creation):** The phase in the React component lifecycle where the component is created and inserted into the DOM.
- **Updation:** The phase in the React component lifecycle where a component re-renders due to changes in props or state. 
- **Unmount (Deletion):** The phase in the React component lifecycle where the component is removed from the DOM.

### Life cycle methods -> class based component


- **`constructor`:** Used to define the initial state of a component.
- **`render`:** Invoked to render the output of the component.
- **Runs only once after the first render:**
  - **Class Component:** Executes `componentDidMount()` once after the component's initial render.
  - **Functional Component:** Utilizes `useEffect(cb, [])` to run the effect callback after the initial render.
- **Runs only when the dependency array changes:**
  - **Class Component:** Combines `componentDidMount` with `componentDidUpdate` to respond to changes in the dependency array.
  - **Functional Component:** Utilizes `useEffect(cb, [dep1, dep2])` to run the effect callback when dependencies change.
- **Runs on unmount:**
  - **Class Component:** Uses `componentWillUnmount` to perform cleanup when the component is about to unmount.
  - **Functional Component:** Employs `useEffect(cb, [])` with a clean-up function for actions to be taken when the component unmounts.


