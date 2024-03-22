

## Agenda
* common error in express
* Improving our productModel
* pre and post hook in Mongoose 
- Query params in Express
- select ,sort  -> Products
code for the lecture : [link](https://github.com/Jasbir96/Full_stack_may_2023/tree/master/Projects_Module/code/Lecture_4_Mongodb_queries_searching_sorting_pagination)


##  Common Error in express 
![debug.png](https://hackmd.io/_uploads/By1Ge2cR6.png)

**Error Text** : the error `Error: <Route> requires a callback function but got a [Object Undefined] 

**Reason** : The route is not able to access our handler function there can be multiple reasons either function is not defined or it could not have been properly exported
 

## improving schema of productModel
Adding four fields  
* brand 
* description
* stock_quantity 
```js
const newProductSchemaRules = {

/*
original fields
*/ 

brand: {
type: String,
required: [true, "Please Enter The brand name"],
},


categories: {

type: [String],

required: true,

},

description: {
type: String,
required: [true, "kindly add desc"],
maxlength: [2000, "description can't be bigger then 2000 characters"]
},

stock_quantity: {
type: String,
required: [true, "You should enter stock of the product should be atleast 0"],
validate: function () {
return this.stock_quantity >= 0;
},
message: "stock_quantity should can't be negative "
},
}
```


## pre and post hooks in mongodb
**Problem Statement** we have a categories field to validate that only valid categories are added  we can use enum but usually categories can be dynamic as depending upon business need they can be modifies .

**Solution** : pre and post hooks there are the methods that are given by mongoose so that we can do certain operation on data before saving a document in db or after getting data from DB

for our use case we will be using pre save hook to validate valid categories 


```js
const productSchema = new mongoose.Schema(newProductSchemaRules);

let validCategories = ['Electronics', "Audio", 'Clothing', 'Accessories',"Shoes"]
  

productSchema.pre("save", function (next) {
const product = this;
const invalidCategoriesArr = product.categories
.filter(category => {
return !validCategories.includes(category)
})

if (invalidCategoriesArr.length > 0) {
const err = new Error(`product from ${invalidCategoriesArr[0]} categories are not being accepted right now`);
return next(err);
} else {
next();
}

  

})
```

## query params demo and it's use case 
### Explaination 
Query parameters are a way to pass information to a web server as part of a URL. They come after the "?" in a URL and consist of key-value pairs separated by "&". For example:


```curl
https://www.example.com/search?q=dogs&category=pets
```


In this URL:
- `https://www.example.com/search` is the base URL.
- `?` indicates the start of the query parameters.
- `q=dogs` is a query parameter with key `q` and value `dogs`.
- `&` separates multiple query parameters.
- `category=pets` is another query parameter with key `category` and value `pets`.
Query parameters are used for various purposes:
1. **Filtering and Searching**: They're commonly used in search functionalities where parameters like keywords, filters, or categories are passed to refine search results.
2. **Tracking and Analytics**: Query parameters can carry tracking information, allowing websites to monitor the effectiveness of marketing campaigns or user interactions
## Demonstrating  query params
```curl
localhost:3000/api/product?sort=price%20inc&select=name%20description%20price&
```

```js
async function getAllProductHandler(req, res) {

try {

let query = req.query;

let selectQuery = query.select;

let sortQuery = query.sort
 console.log("selectParam", selectParam);
 console.log("sortParam", sortParam);

res.status(200).json({
message: result,
status: "success"

})

} catch (err) {

console.log(err)

res.status(500).json({

message: err.message,

status: "failure"

})

}

}
```




## writing search , sort and select feature
This code fetches product data based on query parameters from incoming requests. It grabs parameters like what to select and how to sort from the URL.

It dives into the database (`ProductModel`) to find products, applying sorting if a sorting query is provided (ascending or descending order). If a selection query is there (like 'name' or 'price'), it only picks those fields.

Once it's done querying the database, it sends back a success response with the fetched data in a JSON format. If something goes wrong, it catches the error, logs it, and sends a failure response.

```js

async function getAllProductHandler(req, res) {

try {

// are done on the level of DB

// -> find all the data

// -> sort

// -> select

  

let query = req.query;

let selectQuery = query.select;

let sortQuery = query.sort

// console.log("selectParam", selectParam);

// console.log("sortParam", sortParam);

// make a find query -> searching for the product

let queryResPromise = ProductModel.find()

// sort the entries

if (sortQuery) {

// "price inc"

let order = sortQuery.split(" ")[1];

let sortParam = sortQuery.split(" ")[0];

// console.log("order",order,"sortParam",sortParam);

// applying this logic for inc and dec

if (order == "inc") {

queryResPromise = queryResPromise.sort(sortParam);

} else {

queryResPromise = queryResPromise.sort(-sortParam);

}

}

if (selectQuery) {

queryPromise = queryResPromise.select(selectQuery);

}

// when find and sort both are done

const result = await queryResPromise;

  

res.status(200).json({

message: result,

status: "success"

})

} catch (err) {
console.log(err)
res.status(500).json({
message: err.message,
status: "failure"
})
}
// sorting -> increarsing

// selecting -> (name,price)

}
```


