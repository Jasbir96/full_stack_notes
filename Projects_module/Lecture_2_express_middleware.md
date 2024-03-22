## Agenda
* Installation : Nodejs, express , postman , mongodb compass
* Intro to express and res.json
* Middlewares in express
	* inbuilt 
	* userDefined middleWare
* Get and POST req with Express
* Template routes in express
* Error handling in express
## Setup of a Nodejs  Project with Express

 * npm init -y
 * npm i express nodemon


### Introduction to Express.js
Express.js is a minimal and flexible Node.js web application framework that provides a robust set of features for building web applications and APIs. It simplifies common tasks like routing, handling HTTP requests, and managing middleware
1. Basic Setup:
```javascript
const express = require('express');
const app = express();

```
2. Creating Routes:
Define routes for different HTTP methods and URLs:
```javascript
app.get('/hello', (req, res) => {
  const jsonResponse = {
    message: 'Hello, you accessed the GET route!',
    timestamp: Date.now()
  };

  res.json(jsonResponse);
});

```

3. Start the server
```javascript
const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

### Nodemon

Nodemon is a utility that monitors changes in your Node.js application and automatically restarts the server. It's useful during development to save time and effort when making changes to the code.

#### Using Nodemon

1. After installing Nodemon as dev dependecies  
```bash
npm install -D nodemon
```

2. **Setting Up `package.json` Scripts**: Within `package.json`, you can define a script that runs your Node.js application using `nodemon`.
```json=
`"scripts": {   "dev": "nodemon your_app_file.js" }`
```

3. **Running Your Application with Nodemon**: To start your application using `nodemon`, you'd execute the script defined in `package.json` via the terminal:

```bash
npm run dev
```


## Handling GET and POST Requests with Express

Express.js provides methods for handling different types of HTTP requests like GET, POST, PUT, DELETE, etc.

### GET Request

We've seen an example of handling a GET request earlier. To handle a GET request for a specific route:


```javascript

app.get('/hello', (req, res) => {   // Handle the GET request });
```
### POST Request

Handling a POST request involves parsing incoming data. Here's an example:


```javascript

app.post('/submit', (req, res) => { 
    const formData = req.body;   // Process the submitted data 
});
```
However, to handle POST requests with Express,  Express's built-in `express.json()`  middleware to parse incoming request bodies.



## Error Handling in Express Using Try and Catch

Express allows you to handle errors gracefully by using `try` and `catch` blocks in your route handlers or middleware.

### Example of Error Handling

```javascript

app.get('/route', async (req, res) => {   
    try {     // Code that might throw an error     
        const result = await someAsyncOperation();     
        res.json(result);   
    } catch (error) {     // Handle the error gracefully 
        res.status(500).json({ error: error.message });   
    } });
```

In this example, an asynchronous operation `someAsyncOperation` is attempted within a `try` block. If an error occurs, it's caught in the `catch` block, and a 500 status along with an error message is sent as the response.

---
title: app.use and it's uses 
description: usecase of app.use for 404 page not found and as a middleware
duration: 
card_type: cue_card
---
### 404 not route is matched

`app.use is the route handler that is called for  every request`\


**Problem Statement**: We need to create 404 /not found handler . This should by it's behaviour should only work when no other route is matched.


**Solution** : add app.use route handler as the last route handler so if none of the route matches that the last app.use will be matched and from there you can return the result .

`Request send GET /products`
```javascript=

const express = require('express');
const app = express();

// Define routes
app.get('/', (req, res) => {
  res.send('Welcome to the homepage!');
});

app.get('/about', (req, res) => {
  res.send('About page');
});

// Middleware for handling 404 - Route not found
app.use((req, res) => {
  res.status(404).send('404 - Route not found');
});

// Start the server
const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```


**Explaination** :For request  GET /products non of the routes will match and finally app.use will be matched and that respond with 404 Page not found
### user defined middleware 
User defined middleware are used in a lot of places to check if certain contition are satisifed for a particular HTTP request . 

**Signature** 
```
function (req, res, next)
```
this next method is called inside any handler function then the request is forwarded to next handler in the order . This next is often used to modify the flow of execution 

One of the usecase is to `check  for a given post request whether req.body is empty or not`.

Assuming that an `POST` request is 
to the route `\api\user` then we can create a middleware like below 

**Example**
```javascript=
app.use(function (req, res, next) {
    if (req.method == "POST") {
        // check if the body is empty or not 
        const userDetails = req.body;
        const isEmpty = Object.keys(userDetails).length == 0;
        if (isEmpty) {
            res.status(400).json({
                status: "failure",
                message: "userDetails are empty"
            })
        } else {
            next();
        }
    } else {
        next();
    }
})

app.post("/api/user", createuserHandler);
```

**Explaination**
app.use will be called for every request that is send to the backend and handler function is called 
here we are checking following things
    * if it is a post request ->if yes
    * if req.body has some data or not -> if yes
then we call next function and this forward the same request  to `app.post("/api/user", createuserHandler);`  route otherwise if request.body is  empty then we will send the response from here directly to frontend that -> `userDetails are empty` 
    



## Template Routes
Template routes in Express help organize and structure your application's endpoints, making it easier to manage and perform actions on different resources within your application.


### Example of getById Route

Assuming you have a collection of items and you want to retrieve a specific item by its ID:
#### Express Route Setup:

```javascript
// Assume you have an array of items (for demonstration purposes) 
const items = [   { id: 1, name: 'Item 1' },   { id: 2, name: 'Item 2' },   { id: 3, name: 'Item 3' } ];  
const express = require('express'); 
const app = express();
// GET route to fetch item by ID 
app.get('/items/:id', (req, res) => {  
const itemId = parseInt(req.params.id); // Extract ID from URL parameter   
const foundItem = items.find(item => item.id === itemId);    
if (!foundItem) {     
return res.status(404)
.json({ error: 'Item not found' });   
}else{
res.json(foundItem); // Respond with the found item 

}    

});  // Start the server 
const PORT = 3000; app.listen(PORT, () => {   console.log(`Server running on port ${PORT}`); });
```
#### Usage and Explanation:

- The route `/items/:id` is defined to handle GET requests where `:id` is a placeholder for the item's unique identifier.
- Inside the route handler function, `req.params.id` extracts the `id` parameter from the URL.
- The `find` method is used to search for the item with the corresponding ID within the `items` array.
- If the item is found, it's sent as a JSON response using `res.json()`. If not found, a 404 error response is sent.

#### Requesting the Route:

Assuming your server is running locally, you can make a GET request to fetch an item by its ID using tools like cURL or a web browser:

- To get the item with ID 2: `http://localhost:3000/items/2`

This URL triggers the `getById` route in your Express application, responding with the item information for the specified ID.
