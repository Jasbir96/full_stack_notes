
## Agenda

- Introducing env file 
- Data storage in mongodb
- Connecting to Mongodb atlas
- Intro to Mongoose ORM : Model ,schema 
- CRUD with DB

code for the lecture : [link](https://github.com/Jasbir96/Full_stack_may_2023/tree/master/Projects_Module/code/Lecture_3_Rest_MVC)
---
title: adding env file in our project
description: 
duration: 780
card_type: cue_card
---
## Use case of env variables
**Problem Statement** :In every Project there are certain values that uniquely identifies a project for Example: DB Password , Port , any API keys. Also these  information can be sensitive and should not be shared to outside world directly so to prevent this to happen 

**Solution**: 
* create a file with the name `.env`  at top most level of your project  . Where you  store all these sensitive  information   in `KEY=VALUE` format where by convention keys should be in upper case 

```env

PORT = 3000
DB_PASSWORD = RoK2jkO2LFU6fpXc
DB_USER = Jasbir
```
* add that .env file to `.gitignore`  so that file could not be uploaded to GitHub
* install dot.env package and follow the below step to access all the env variables 
```javascript
const dotenv = require("dotenv");
dotenv.config();
const { PORT, DB_PASSWORD, DB_USER } = process.env;
```



---
title: Usecase of  DB in a server
description: 
duration: 780
card_type: cue_card
---
## usecase of DB
### Req-Res Cycle of A Simple Request  
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/057/782/original/upload_eb74e29082819ee7fd16de4fbcbf7427.png?1700828085)

Usually when a user visits a website ->to get some data. It makes the http request by clicking on a button or by doing some other action . A request will be send to a backend server and then the server will search the content related to the request and return the resp.

**Problem** :  

In a production level server there is a lot of data that is stored for example Amazon has 2 million plus products and store data and all the assets in the same server is not possible . 
**Solution** : First two steps that everyone takes solve it are
* Having a special software whose task is work with data efficiently . Also can provide features related to data like validation , access management and other features specific to a particular use-case : for this we can use **Data base management system : DBMS**. [To know more](https://byjus.com/commerce/concept-and-features-of-dbms/) 
* Now to further scale them we can have a different server just for the DBMS.
so in today's lecture we will  see how to perform these two steps 

**Now for starters our setup will look like this** 
 ![Pasted image 20231120190010](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/057/783/original/upload_850e0fd00a33aa2fd62fc50b946e1957.png?1700828195)

---
title: Intro to Mongodb 
description:
duration: 670
card_type: cue_card
---

 MongoDB is a database system that is used to store data that resembles JSON format  . For someone new to backend development, understanding its core concepts like BSON, collections, and documents is essential.
 
 ### Core Concepts:

1. **BSON (Binary JSON):** <br> MongoDB stores data in BSON format. Think of BSON as a way to represent data in a binary-encoded form, much like how JSON represents data in a text-based format. It's used by MongoDB to efficiently store and retrieve data.
    
2. **Collections:** <br> Collections are like containers or folders in which MongoDB stores related documents. You can think of them as similar to tables in a traditional SQL database, but with a more flexible structure.
    
3. **Documents:** <br> Documents are the basic unit of data in MongoDB. They're similar to rows or records in a table, but unlike SQL databases where each row follows a fixed schema, MongoDB documents can have varying structures within the same collection.

### Real-World Example:

Let's consider a scenario where you're building an e-commerce platform. You have a MongoDB database to store product information.

#### Collection: Products

This collection stores information about various products available on your platform.

#### Documents: Product Information

Each document represents a specific product and can have different fields based on the product type. For instance:


```json
{
  "_id": ObjectId("61e65529b6fc4670e05a1c7a"),
  "name": "Smartphone",
  "brand": "XYZ",
  "price": 699,
  "specs": {
    "display": "6.5 inches",
    "storage": "128GB",
    "camera": "Quad-camera setup"
  }
}

```

Here, the Products collection holds documents representing different types of products. This document represents a smartphone with its name, brand, price, and specifications.


```jsonld
{
  "_id": ObjectId("61e65545b6fc4670e05a1c7b"),
  "name": "Laptop",
  "brand": "ABC",
  "price": 1299,
  "specs": {
    "display": "15.6 inches",
    "storage": "512GB SSD",
    "RAM": "16GB",
    "processor": "Intel Core i7"
  }
}

```


---
title: Getting Deployed DB link for express  
description: 
duration: 780
card_type: cue_card
---

- **Express and MongoDB:** <br> Express handles the incoming HTTP requests and responses. It interfaces with MongoDB to perform operations like reading, writing, updating, and deleting data from the database.
    
## Steps to get DB Link from Mongodb Atlas
* Signup to mongodb.com 
![Pasted image 20231123171056](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/057/784/original/upload_86111677dfec257e36b4bcc2a93fcfba.png?1700828387)
* follow the steps and create a cluster
* once cluster is created you will see this dashboard screen
![Pasted image 20231123171116](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/057/785/original/upload_a4cca7c2d80be2271a8fcef57804970d.png?1700828423)
* Usually server connecting with DB server has satatic IP and for security reasons this network access should only be given to creatin Ip address but for develop enviornment we can allow it to acces from anywhere. In `network access select allow access from anywhere`   
![Pasted image 20231123171247](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/057/786/original/upload_d43a548b812518332d9f455bf82c9b73.png?1700828507)
* Create a DB user , create it's password
![Pasted image 20231123171321](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/057/787/original/upload_ac12efc36c17feff1cc26beef9712d87.png?1700828572)
* Select Role as atlas admin
![Pasted image 20231123171346](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/057/788/original/upload_a9fcfa3177af770c1842ec11545420e5.png?1700828623)
* Go to Home Page and select connect . Choose Driver for node and copy the link
![Pasted image 20231123171402](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/057/789/original/upload_0bc2f97a718b86e0ed145b606c754fc0.png?1700828682)

This DB link will help our express server to connect with mongodb Database and details like  password and username should not be exposed

---
title: connecting MongoDB database with Express server  
description:
duration: 780
card_type: cue_card
---
## Significance of Mongoose

Mongoose acts as an intermediary between your Express server and MongoDB. It helps define data models, schemas, and provides methods for interacting with MongoDB. It simplifies the process of querying and manipulating data in MongoDB from your Express application.

```jsonld
// driver 
const dbURL = `mongodb+srv://${DB_USER}:${DB_PASSWORD}@cluster0.drcvhxp.mongodb.net/?retryWrites = true&w = majority`;
// once 
mongoose.connect(dbURL)
    .then(function (connection) {
        console.log("connected to db",)
    }).catch(err => console.log(err))
```

---
title: creating schema and model
description:
duration: 780
card_type: cue_card
---

## Important Terms of Mongoose
In Mongoose, schemas and models play crucial roles in defining and working with data in MongoDB.
### Schema:

- **What it is:** <br> A schema in Mongoose is a blueprint that defines the structure of your data.
- **Significance:**
    - **Structure Definition:** <br> It specifies the fields, types, default values, validations, and other constraints for documents in a collection.
    - **Enforcement:** <br> Schemas help enforce a consistent structure for your data. They define what fields exist, their types, and any rules for their content.
    - **Validation:** <br> You can define validations for fields, ensuring that data conforms to specific criteria before it's saved to the database.

### Model:

**What it is:**

A model in Mongoose is represent actual data . All the entries that follow the schema will be added to a model

## Creating user Schema
```javascript
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

```


### Creating Model with Schema
```javascript
// schema-> structure and validation 
const userSchema = new mongoose.Schema(userSchemaRules);

// this model -> will have queries 
const UserModel = mongoose.model("userModel", userSchema);
```
---
title: Doing crud with DB   
description:
duration: 680
card_type: cue_card
---


### Create User
```javascript
async function createuserHandler(req, res) {
    try {
        const userDetails = req.body;
        // adding user to the file 
        const user = await UserModel.create(userDetails);

        res.status(200).json({
            status: "successfull",
            message: `added  the user `,
            user: user
        })
    } catch (err) {
        res.status(500).json({
            status: "failure",
            message: err.message
        })
    }
}
```
### GetAll User

```javascript
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

### Get User

```javascript
async function getUserById(req, res) {
    try {
        const userId = req.params.userId;
        const userDetails = await UserModel.findById(userId);

        if (userDetails == "no user found") {
            throw new Error(`user with ${userId} not found`);
        } else {
            res.status(200).json({
                status: "successfull",
                message: userDetails
            })
        }
    } catch (err) {
        res.status(404).json({
            status: "failure",
            message: err.message
        })
    }
}
```





