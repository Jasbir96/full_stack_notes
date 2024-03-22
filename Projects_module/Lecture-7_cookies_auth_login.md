
## Agenda
* cookies and it's usecase  
* Authentication , Authorization and identification 
* JWT algorithm 
* Signup 
* Login 
* Protect route 
code for the lecture : [link](https://github.com/Jasbir96/Full_stack_may_2023/tree/master/Projects_Module/code/Lec_6_cookies_jwt_auth)


## cookies and it's usecase 


## HTTP packet
* Header : metadata about the packet
* cookies -> part of header 
* Body : contains actual data that is being
## cookies
* cookie is a client side storage
* It stores data in the format `key : value` pairs . These pair should be of type string
* Server sends these cookies to the client 
* on the client side these cookies are stored and mapped to the server that has send the cookies
* For next request -> client will automatically share this cookie with the server

## using  cookies with express 
```js
const express = require("express");
const app = express();
const cookieParser = require("cookie-parser");


// home
// products
// clearCookie
app.use(cookieParser());
// -> home get thre request i will add a cookie -> share that cookie with res
app.get("/", function (req, res) {
    /****
     * To send a cookie you will need 
*           key and value both in string
*   Some optional  but important options
*       **maxAge** : after how many milli seconds cookie will expire
*       **httpOnly** : true , "now your cookie will only be accessed through server not by client
     side js  so it can't be tampered
     * 
     * **/
    res.cookie("prevpage", "home", {
        maxAge: 1000000000000,
        httpOnly: true
    })
    res.status(200).json({
        message: "thank you for the visit"
    })
})

// i will check whether the user visiting for the first or it has already visited
app.get("/product", function (req, res) {
    console.log("cookie", req.cookies)
    let messageStr = ""
    if (req.cookies.prevpage) {
        messageStr = `You have already visited ${req.cookies.prevpage}`;
    }
    res.status(200).json({
        message: `thank you for accessing product route ${messageStr}`
    })
})

// clear your cookies -> on a server
app.get("/clearCookies", function (req, res) {
    // clearing a cookie
    res.clearCookie("prevpage", { path: "/" });

    res.status(200).json({
        message: "i have cleared your cookie"
    })

})

app.listen(3000, function () {
    console.log(` server is listening to port 3000`);
})
```



## Identification vs Authentication vs Authorization 

**Identification** : Identification is the process of stating or claiming who you are. It's the initial step where a user asserts an identity, but it doesn't validate the authenticity of the claim. : 
* User perferences are solved 

* **Authentication**: is the process of verifying whether the claimed identity is valid and accurate.It ensures that the user's identity is genuine before granting access to protected resources or functionalities. 
    * login ,otp , bometric 
    * web tokens -> if that token  is secure 

* **Authorization** is the process of determining what actions or resources an authenticated user is permitted to access within a system or application.
---


## JSON web token 

* To prevent tampering
1. JSON token creation :
JSON web token is built out of three component
    * `Payload` : plain text (identifier for the user)  
    * `Algorithm`: in plain text name of the algorithm
    * `signature`: encryped text that is build using ecrypting  three texts
        * payload+ algorithm name+ secret key 
2. Secret key is only known by the server
3. validation 
         a. server takes -> payload from incoming token + algorithm from incoming token + secret from the server and builds a fresh signature 
         b. compare  signature of incoming token and signature that is build in the above step
         c.) validation fails if signature coming from the client does not match with freshely prepared signature

* To prevent client side  access -> httpOnly 

## using jwt with express 
```js
const express = require("express");
const cookieParser = require("cookie-parser");
const app = express();
app.use(cookieParser());

const jwt = require("jsonwebtoken");
const promisify = require("util").promisify;

const promisfiedJWTSign = promisify(jwt.sign);
const promisfiedJWTVerify = promisify(jwt.verify);

const payload = "1234";
const secretKey = "i am a secret";
/****send the token ***/
app.get("/sign", async function (req, res) {
    try {
        const authToken = await promisfiedJWTSign({ data: payload, }, secretKey, { algorithm: 'HS256' });
        // transport -> cookies
        res.cookie("jwt", authToken, { maxAge: 90000000, httpOnly: true });
        res.status(200).json({
            message: "signed the jwt and sending it in the cookie",
            authToken
        })

    } catch (err) {
        console.log("err", err);
        res.status(400).json({
            message: err.message,
            status: "failure"
        })
    }
})

/*************verifying those tokens********************/
app.get("/verify", async function (req, res) {
    try {
        const token = req.cookies.jwt;
        const decodedToken = await promisfiedJWTVerify(token, secretKey);
        // console.log("Token decoded", decodedToken);
        res.status(200).json({
            message: `token is decoded`,
            decodedToken
        })
    } catch (err) {
        console.log("err", err);
        res.status(400).json({
            message: err.message,
            status: "failure"
        })
    }

})


// server -> run on a port 
app.listen(3000, function () {
    console.log(` server is listening to port 3000`);
})
```



## Signup
## setup for auth 
```js
/************routes***************/
app.post("/signup", signupController);
app.post("/login", loginController);
app.get("/allowIfLoggedIn", protectRouteMiddleWare, getUserData);
```

## signup

```js
const signupController = async function (req, res) {
    try {
        // add it to the db 
        const userObject = req.body
        //   data -> req.body
        let newUser = await UserModel.create(userObject);
        // send a response 
        res.status(201).json({
            "message": "user created successfully",
            user: newUser,
            status: "success"
        })
    } catch (err) {
        console.log(err);
        res.status(500).json({
            message: err.message,
            status: "success"
        })
    }
}
```


## login
```js
const loginController = async function (req, res) {
    try {

        /***
         * 1. enable login -> tell the client that user is logged In
         *      a. email and password 
         **/

        let { email, password } = req.body;
        let user = await UserModel.findOne({ email });
        if (user) {
            let areEqual = password == user.password;
            if (areEqual) {
                // user is authenticated
                /* 2. Sending the token -> people remember them
                       * */
                // payload : id of that user 
                let token = await promisifiedJWTSign({ id: user["_id"] }, JWT_SECRET);
                console.log("sendning token");
                res.cookie("JWT", token, { maxAge: 90000000, httpOnly: true, path: "/" });
                res.status(200).json({
                    status: "success",
                    message: "user logged In"
                })
            } else {
                console.log("err", err)
                res.status(404).json({
                    status: "failure",
                    message: "email or password is incorrect"
                })
            }


        } else {
            res.status(404).json({
                status: "failure",
                message:
                    "user not found with creds"
            })
        }


    } catch (err) {
        console.error(err);
        res.status(500).json({
            status: "failure",
            message: err.message
        })
    }
}
```

## Protect Route 

```js
const protectRouteMiddleWare = async function (req, res, next) {
    try {
        let jwttoken =req.cookies.JWT;

        let decryptedToken = await promisifiedJWTVerify(jwttoken, JWT_SECRET);
        if (decryptedToken) {
            let userId = decryptedToken.id;
            // adding the userId to the req object
            req.userId = userId
            next();
        }
    } catch (err) {
        res.status(500).json({
            message: err.message,
            status: "failure"
        })
        
    }
}

```
