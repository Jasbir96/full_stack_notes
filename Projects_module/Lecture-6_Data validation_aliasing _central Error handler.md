

## Agenda
*  Real world usecase for  pagination,  searching, sorting, filtering 
- Pagination Implementation
- MongoDB operators   
- Filtering of data
-  Alias routes like top 5 products
code for the lecture : [link](https://github.com/Jasbir96/Full_stack_may_2023/tree/master/Projects_Module/code/Lec_5_aliasing_queryParams)


## real world use case of pagination searching and sorting
Let's delve into how filtering, searching, sorting, and pagination might function on a platform like Amazon, and how APIs facilitate these functionalities, enabling various platforms to interact with the data.

* **Filtering**: Consider a scenario where a user on Amazon wishes to filter products. They might want to view laptops with specific RAM size, processor type, or price range. The API would handle these filters in the request:
```bash
GET /products?category=laptops&ram=8gb&processor=i7&price_min=500&price_max=1000

```

* **Searching**: Users often search for specific items or categories. For instance, a user might want to find "wireless headphones" on Amazon. The API manages this search key by using the search parameter, the API fetches products with titles, descriptions, or relevant fields containing "wireless headphones," presenting these results to the user.
```bash
GET /products?search=wireless+headphones

```

* **Pagination**: Amazon, with its vast product range, employs pagination for user convenience. Users prefer browsing through pages instead of overwhelming product lists. For instance, displaying 30 items per page:
```bash
GET /products?category=books&page=3&limit=30

```

This API request fetches the third page of book products, showing 30 items per page.

These functionalities are managed by the API, which communicates with the database to filter, search, sort, and paginate data. Various platforms, such as web applications, mobile apps, or other services, can utilize these API endpoints to access consistent and tailored data from Amazon, ensuring a uniform experience across different devices and applications.


## Pagination and sort  implementation
## sort implementation 

* we will use `sort` method available on find  method  to sort our data 
`poc\api.js`

```js
async function getAllProductHandler(req, res) {

try {

let query = req.query;

// getting all the keys

let selectQuery = query.select;

let sortQuery = query.sort;

let myQuery = query.myQuery;



let queryResPromise = ProductModel.find();

// // logic making query

if (sortQuery) {

// "price inc"

let order = sortQuery.split(" ")[1];

let sortParam = sortQuery.split(" ")[0];

// console.log("order",order,"sortParam",sortParam);

// applying this logic for inc and dec

if (order == "inc") {

// it by default sorts in inc order

console.log("Sorted increasing order", sortParam)

queryResPromise = queryResPromise.sort(sortParam);

} else {

// if you pass negative then it will sort in desc

queryResPromise = queryResPromise.sort(-sortParam);
}

}

// selection of the fields

if (selectQuery) {

queryPromise = queryResPromise.select(selectQuery);

}

// when find and sort both are done

const result = await queryResPromise;

console.log("Result", result);

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

## pagination implementation 

```js

async function getAllProductHandler(req, res) {

try {

let query = req.query;

// getting all the keys

let selectQuery = query.select;

let sortQuery = query.sort;

let myQuery = query.myQuery;

let page = query.page;

let limit = query.limit;

let queryResPromise = ProductModel.find();

// // logic making query

if (sortQuery) {

// "price inc"

let order = sortQuery.split(" ")[1];

let sortParam = sortQuery.split(" ")[0];

// console.log("order",order,"sortParam",sortParam);

// applying this logic for inc and dec

if (order == "inc") {

// it by default sorts in inc order

console.log("Sorted increasing order", sortParam)

queryResPromise = queryResPromise.sort(sortParam);

} else {

// if you pass negative then it will sort in desc

queryResPromise = queryResPromise.sort(-sortParam);
}

}

// selection of the fields

if (selectQuery) {

queryPromise = queryResPromise.select(selectQuery);

}

/// limit() -> no of products that could on a given page

// .skip() -> starting idx;

// /***************************Pagination**************************/

// if page number is passed

page = page || 1;

limit = limit || 2;

const elementsToSkip = (page - 1) * limit;

// console.log(elementsToSkip)

queryResPromise = queryResPromise.skip(elementsToSkip).limit(limit);

// when find and sort both are done

const result = await queryResPromise;

console.log("Result", result);

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



## Mongodb Query Operators 

In MongoDB, query operators provide powerful ways to perform specific operations while retrieving or manipulating data from the database. 

### Comparison Operators:


1. **`$eq`**: Matches values that are equal to a specified value.
    
    ```json
{ age: { $eq: 25 } }
```
    
2. **`$ne`**: Matches all values that are not equal to a specified value.
    
```json
{ status: { $ne: "archived" } }
```
    
3. **`$gt`, `$gte`, `$lt`, `$lte`**: Greater than, greater than or equal to, less than, and less than or equal to, respectively.
    
```json
{ quantity: { $gt: 100 } } { price: { $lte: 50 } }
```
    
4. **`$in`**: Matches any of the values specified in an collection.
    ```json
    { category: { $in: ["electronics", "clothing"] } }
```
    
5. **`$nin`**: Matches none of the values specified in an collection.
    
  ```json
  { status: { $nin: ["expired", "sold"] } }

```
### Logical Operators:

1. **`$and`**: Joins query clauses with a logical AND.
    
 ```json
{ $and: [ { price: { $gte: 50 } }, { price: { $lte: 100 } } ] }
```
    
    
2. **`$or`**: Joins query clauses with a logical OR.
    
  ```json
{ $or: [ { quantity: { $lt: 10 } }, { status: "backordered" } ] }
```
        
3. **`$not`**: Inverts the effect of a query expression and returns documents that do not match the query expression.
    
    ```json
{ age: { $not: { $gte: 18 } } } // Matches documents where age is less than 18
```
    
4. **`$nor`**: Joins query clauses with a logical NOR and selects the documents that fail all the specified conditions.
    
   ```json
{ $nor: [ { price: { $lt: 20 } }, { in_stock: false } ] }
```
    


## Filtering of data
 **transformQueryHelper**

```js
function transformQueryHelper(myQuery) {

const parseQuery = JSON.parse(myQuery);

const queryOperators = {

gt: '$gt',

gte: '$gte',

lt: '$lt',

lte: '$lte',

// Add more operators as needed

};

for (let key in parseQuery) {

  

let internalObject = parseQuery[key];

if (typeof internalObject == "object") {

// in the inner obj -> operators

for (let innerKey in internalObject) {

if (queryOperators[innerKey]) {

internalObject[`$${innerKey}`] = internalObject[innerKey];

delete internalObject[innerKey];

}

}

}

  

}

return parseQuery;

}
```

## filter implementation

```js
async function getAllProductHandler(req, res) {

try {

let query = req.query;

// getting all the keys

let selectQuery = query.select;

let sortQuery = query.sort;

let myQuery = query.myQuery;

let page = query.page;

let limit = query.limit;

  

// we have to make a find the list of data -> JSON object

// remove the $ from the string -> parse -> you gt

const transformedQuery = transformQueryHelper(myQuery);

console.log("47 transformedQuery", transformedQuery);

let queryResPromise = ProductModel.find(transformedQuery);

// // logic making query

if (sortQuery) {

// "price inc"

let order = sortQuery.split(" ")[1];

let sortParam = sortQuery.split(" ")[0];

// console.log("order",order,"sortParam",sortParam);

// applying this logic for inc and dec

if (order == "inc") {

// it by default sorts in inc order

console.log("Sorted increasing order", sortParam)

queryResPromise = queryResPromise.sort(sortParam);

  

} else {

// if you pass negative then it will sort in desc

queryResPromise = queryResPromise.sort(-sortParam);

}

}

// selection of the fields

if (selectQuery) {

queryPromise = queryResPromise.select(selectQuery);

}

// limit() -> no of products that could on a given page

// .skip() -> starting idx;

// /***************************Pagination**************************/

// if page number is passed

page = page || 1;

limit = limit || 2;

const elementsToSkip = (page - 1) * limit;

// console.log(elementsToSkip)

queryResPromise = queryResPromise.skip(elementsToSkip).limit(limit);

// when find and sort both are done

const result = await queryResPromise;

console.log("Result", result);

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

## Aliasing of routes
**Route aliasing** : involves creating alternative or shorter routes that point to the same resource or endpoint. It allows defining multiple routes to access the same functionality or content without duplicating the entire route logic.

### Example :

```js
app.get("/api/bigbillionProducts", getTop4Products, getAllProductHandler);

async function getTop4Products(req, res, next) {

    // stock_quantity -> 30
    // rating >4.6
    // limit -> 4 
    req.query.myQuery = {
        stock_quanitity: { $lt: 30 },
        rating: { $gt: 4.6 },
        limit: 4
    }
    // all the middlewares share the same req,res obect 
    next()
}
```

### Explanation : 
here we are using `getTop4Products` middleware  and `bigbillionProducts` route to alias a req with these query params 

```js
// stock_quantity -> 30
    // rating >4.6
    // limit -> 4 
```