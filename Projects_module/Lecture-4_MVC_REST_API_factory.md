

## Agenda
* Restful Design
* CRUD factory
* MVC architecture explanation
* Converting to MVC
	* Mounting 
	* Creating routes
	* Creating controllers
	* Chaining handlers
code for the lecture : [link](https://github.com/Jasbir96/Full_stack_may_2023/tree/master/Projects_Module/code/Lecture_3_Rest_MVC)


## User Model
Schema and Model wrt to user should be kept in different file 
`userModel.js`
```js
const mongoose = require("mongoose");

// our schema for users for a ecommerce website

const userSchemaRules = {

name: {

type: String,

required: true,

},

email: {

type: String,

required: true,

unique: true,

},

password: {
type: String,
required: true,
minlength: 8,

}
,

confirmPassword: {

type: String,

required: true,

minlength: 8,

// validate property

validate: function () {

return this.password == this.confirmPassword

}

},

createdAt: {

type: Date,

default: Date.now()

}

}

// schema-> structure and validation

const userSchema = new mongoose.Schema(userSchemaRules);

// this model -> will have queries

const UserModel = mongoose.model("userModel", userSchema);

module.exports=UserModel;
```


## refactoring handlers
**Problem Statement**: If we use function statement then those statements are hoisted and because they are hoisted by mistake function can overwritten 

`api.js`
```js
app.get("/api/user", getAllUserHandler);
async function getAllUserHandler(req, res) {
    try {
        console.log("I am inside  get method");

        const userDataStore = await UserModel.find();
        if (userDataStore.length == 0) {
            throw new Error("No users are present")
        }
        res.status(200).json({
            status: "success",
            message: userDataStore
        })
    } catch (err) {
        res.status(404).json({
            status: "failure",
            message: err.message
        })
    }

}

```
**Solution** : convert function statement into function expression . If we use function expression so they needs to created before the use and because we are using const declaration so function definition can't be overwritten 

`api.js`

```js
const getAllUserHandler=async function (req, res) {
    try {
        console.log("I am inside  get method");

        const userDataStore = await UserModel.find();
        if (userDataStore.length == 0) {
            throw new Error("No users are present")
        }
        res.status(200).json({
            status: "success",
            message: userDataStore
        })
    } catch (err) {
        res.status(404).json({
            status: "failure",
            message: err.message
        })
    }

}
app.get("/api/user", getAllUserHandler);
```
 Note:  Repeat the same for all the handler functions 


## chaining of middleware , refactoring checkInput and deleteById
In express we chain middleware in a route so and behaviour remains the same . This chaining feature helps to improve code readability 
## Check input middleWare 
### Initially 
```js
app.use(express.json());
app.use(function (req, res, next) {
    // console.log("36",req.method);
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

app.get("/api/user", getAllUserHandler);
app.post("/api/user", createuserHandler);
app.get("/api/user/:userId", getUserById);
```

### finally  
```js
const checkInput=(req, res, next) => {
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
}
app.use(express.json());


app.get("/api/user", getAllUserHandler);
app.post("/api/user", checkInput,createuserHandler);
app.get("/api/user/:userId", getUserById);
```


**Note**: test your code after this 

## DeleteByid

```js
const deleteUserById=async function (req, res) {

let { elementId } = req.params;

try {

let element = await UserModel.findByIdAndDelete(elementId);

res.status(200).json({

status: "successfull element deleted",

message: element

});

} catch (err) {

console.error(err);

res.status(500).json({

status: "failure",

message: err.message

  

});

}

}

app.delete("/api/user/:userId", deleteUserById);
```




When we are working on a backend API as it is a very complex piece of software so to make your code more readble and scalable to write we follow rest principle to create an API
## Key Principles of RESTful APIs

1. **Client-Server Separation of **:
- Clear separation between client and server responsibilities.
- Clients handle the user interface, while servers manage data storage and processing.

2. **Resource-Based Routing**:
- Data is exposed as resources (e.g., `/users`, `/products`).
- Each resource is uniquely identified by a route.

**Note**  Resources on which routes should be decided
for ex in and Ecommerce Website we can have 
* Users
* Products
* Reviews
* Booking
* Returns

3. **Actions are done by HTTP method**
- `GET`: Retrieve a resource.
- `POST`: Create a new resource.
- `PATCH`: Update an existing resource.
- `DELETE`: Delete a resource.

Example of routes 
Instead of `app.get("/getUser")` it should be  `app.get("/user")``
Instead of `app.get("/reviewforIphone14")` it should be  `app.get("/review/iphone14)``
  
4. **Statelessness**:
- Each request from the client to the server must contain all the necessary information for the server to fulfil it .

- The server doesn’t store any client state between requests. Every request is independent.simplifying scalability and reducing coupling.

  Instead of `app.get("/nextPage")` it should be  `app.get("/5")`
### Benefits
* **Scalability**: REST APIs are scalable and can handle a large number of clients.
* **Simplicity**: Easy to understand and use due to the use of standard HTTP methods and URLs.
* **Independence** : tech stack of client/server


## Adding Product Model and handler functions
## creating Product Model
`ProductModel.js`
```js
const mongoose = require("mongoose");

// ecommerce -> Amazon

const productSchemaRules = {

name: {

type: String,

// error handling

required: [true, "kindly pass the name"],

unique: [true, "product name should be unique"],

maxlength: [40, "Your product length is more than 40 characters"],

},

price: {

type: String,

required: [true, "kindly pass the price"],

validate: {

validator: function () {

return this.price > 0;

},

message: "price can't be negatives"

}

},

categories: {

type: String,

required: true

},

productImages: {

type: [String]

},

averageRating: Number,

discount: {

type: Number,

validate: {

validator: function () {

return this.discount < this.price;

},

message: "Discount must be less than actual price",

},

},

}

// schema-> structure and validation

const productSchema = new mongoose.Schema(productSchemaRules);

// this model -> will have queries

const ProductModel = mongoose.model("productModel", productSchema);

module.exports = ProductModel;

```


## Adding routes and createProductHandler

Now if we follow our rest principles then the route should start from `/api/products`. These will be the routes and handler associated to Products
### Creating routes 
In `api.js`
```js
app.get("/api/product", checkInput,getAllProductHandler);
app.post("/api/product", createProductHandler);
app.get("/api/product/:productId", getProductById);
app.delete("api/product/:productId", deleteProductById);
```

### Creating handler function 

Create all four route handlers just by copying and pasting handler function of user and replacing `UserModel` with `PlanModel`



## issue with having separate crud method for all the resources
**Problem 1**  : we are doing same operation of crud for both user and product for that we are creating different handler function and the only difference is of model. Clearly we are not following dry principle  as  more resources will be added so this problem will grow 

**Solution** : `Factory Design Pattern` will help us create common factory function for each of the CRUD operation that will serve  all the resources


**Problem 2** : we are writing all the code is same file so we need to make it more structured  
**Solution** : We should use `MVC architecture` to organise our code  



## creating factory function
`factory.js`

```js
const createFactory = (ElementModel) => {

return async function (req, res) {

try {

const elementDetails = req.body;

// adding element to the file

const element = await ElementModel.create(elementDetails);

  

res.status(200).json({

status: "successfull",

message: `added the element `,

element: element

})

} catch (err) {

res.status(500).json({

status: "failure",

message: err.message

})

}

  

}

}

  

const getAllFactory = (ElementModel) => {

return async function (req, res) {

try {

console.log("I am inside get method");

const elementDataStore = await ElementModel.find();

if (elementDataStore.length == 0) {

throw new Error("No elements are present")

}

res.status(200).json({

status: "success",

message: elementDataStore

})

} catch (err) {

res.status(404).json({

status: "failure",

message: err.message

})

}

  

}

}

  

const getByIdFactory = (ElementModel) => {

return async function (req, res) {

try {

const elementId = req.params.elementId;

const elementDetails = await ElementModel.findById(elementId);

  

if (elementDetails == "no element found") {

throw new Error(`element with ${elementId} not found`);

} else {

res.status(200).json({

status: "successfull",

message: elementDetails

})

}

} catch (err) {

res.status(404).json({

status: "failure",

message: err.message

})

}

}

}

const deleteByIdFactory = (ElementModel) => {

return async function (req, res) {

let { elementId } = req.params;

try {

let element = await elementModel.findByIdAndDelete(elementId);

res.status(200).json({

status: "successfull element deleted",

message: element

});

} catch (err) {

console.error(err);

res.status(500).json({

status: "failure",

message: err.message

  

});

}

}

}

  
module.exports =

{

createFactory,

getAllFactory,

getByIdFactory,

deleteByIdFactory
}

```


## usage factory function 
`api.js`
```js
// Product Crud
const createProductHandler = createFactory(ProductModel);
const getAllProductHandler = getAllFactory(ProductModel);
const getProductById = getByIdFactory(ProductModel);
const deleteProductById = deleteByIdFactory(ProductModel);

// User Crud
const getAllUserHandler = getAllFactory(UserModel);
const createuserHandler = createFactory(UserModel);
const getUserById = getByIdFactory(UserModel);
const deleteUserById = deleteByIdFactory(UserModel);

```

## explanation of MVC architecture 

MVC is an abbreviation for model-view-controller. Here is what each of those elements means:

- **Model**: It is the backend containing all of the data logic. It represents the business layer of the application. The model is the core section of this architectural pattern and only contains application information. It has no instructions for displaying the data to the user. It is unaffected by the user interface. It is in charge of the logic and application rules.
- **View**: It forms the graphical user interface (GUI) and represents the presentation layer of the application. It is used to visualize the data that the model contains.`In our case react is going to handle it`
- **Controller**: It is the application's brain that controls how data flows. It works on both the model and the view. It is used to manage the application flow, i.e., data flow in the model object, as well as to update the view whenever data changes. 
`Note`: all the handler function belong here 
![mvc](https://hackmd.io/_uploads/HymP5icCa.png)

## mounting the router
we can delegate traffic using Router where a router represents part of a route 
in our case 
* `ProductRouter` represents `/api/product`
* `UserRouter` represents `/api/user`

```js
const express = require("express");
const ProductRouter = express.Router();
const UserRouter = express.Router();
app.use("/api/user", UserRouter);
app.use("/api/product", ProductRouter);

//User handlers
UserRouter.get("/", getAllUserHandler);
// chaining
UserRouter.post("/", checkInput, createuserHandler);
UserRouter.get("/:userId", getUserById);
UserRouter.delete("/:userId", deleteUserById);

// Product handlers
ProductRouter.post("/", checkInput, createProductHandler);
ProductRouter.get("/", getAllProductHandler);
ProductRouter.get("/:productId", getProductById);
ProductRouter.delete("/:productId", deleteProductById);
```

## code refactoring  

### Directory Structure
![](https://hackmd.io/_uploads/rJtiji9R6.png)
### Code refractoring 
* Controller
	* Move all the handler function of `user` to `userController`
	*  Move all the handler function of `product` to `productController`
* Router
	*  Move all the routes of  `user` to `userRouter`.
	* Move all the routes of  `product` to `productRouter`.
* Model
	*  Move `userModel` and `productModel` files to model folder