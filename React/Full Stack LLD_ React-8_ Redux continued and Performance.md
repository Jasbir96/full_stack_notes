# Full Stack LLD: React-8: Redux continued and Performance
---
title: Agenda of the lecture
description: What will be covered in the topic?

---

## Agenda

We will try to cover most of these topics in today's sessions and the remaining in the next.

- Include Redux - Cart feature
    - UI
    - Redux State management
- Lazyloading / code splitting


It is going to be a bit challenging, advanced, but very interesting session covering topics that are asked very frequently in interviews.

So let's start.

---
title: Review of the code and writing css
description: Discussion of  various concepts related to the counter and todo application in react in detail.

---

## Review of the Code

First of all we need to add the cart feature and counter + and - with number of product you added and want to add that part only.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/636/original/upload_ea813c75c8cd9604d94d15117f9db997.png?1699692872)

Lets quickly write the css for the UI for that:

In the App.jsx:

```javascript
import { useState } from 'react'
import reactLogo from './assets/react.svg'
import viteLogo from '/vite.svg'
import './App.css'
import NavBar from './components/NavBar'
import { Routes, Route, Navigate } from "react-router-dom";
import PageNotFound from './pages/PageNotFound'
import Home from './pages/Home';
import ProductDetails from './pages/ProductDetails';
import Cart from "./pages/Cart";
import User from './pages/User';
import PaginationProvider from './contexts/PaginationContext'

function App() {
  return (
    <PaginationProvider>
      <NavBar></NavBar>
      <Routes>
        <Route path = "/" element = {<Home></Home>}> </Route>
        <Route path = "/cart" element = {<Cart></Cart>}></Route>
        <Route path = "/product/:id" element = {<ProductDetails></ProductDetails>}> </Route>
        <Route path = "/user" element = {<User></User>}> </Route>
        <Route path = "/home" element = {<Navigate to = "/"></Navigate>}></Route>
        <Route path = "*" element = {<PageNotFound></PageNotFound>}> </Route>
      </Routes>
    </PaginationProvider>

  )
}

export default App
```

In the app.jsx we have added the following:
* The code sets up client-side routing using React Router's Routes and Route components.
* It defines various routes for different pages, including the home page, cart page, product details page, user page, and a "page not found" route.
* Each route specifies a path and the component to render when that path is accessed. For example, the /cart path renders the Cart component.

---
title: Implementation of cart
description: Discussion of  various concepts related to the cart for shopping details in react in detail.

---

## Implementation of Cart

Go to the materialUI to get the cart icon component,

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/637/original/upload_b3a4495922ae1fe408a4af12d0e0aba6.png?1699693063)

We need the cart icon in the navBar.jsx:

```javascript
import React from 'react'
import { Link } from 'react-router-dom';
import ShoppingCartIcon from '@mui/icons-material/ShoppingCart';
import { useSelector } from "react-redux";
function NavBar() {
  const quantity  = useSelector((store) => { return store.cartReducer.cartQuantity })
  return (
    <div className = 'navbar'>
      <Link to = "/">Home </Link>
      <Link to = "/user">Users</Link>
      <Link to = "/cart">
        <div className = "cart_container">
          <ShoppingCartIcon></ShoppingCartIcon>
          <div className = "cart_quantity">{quantity}</div>
        </div>

      </Link>
    </div>
  )
}

export default NavBar
```
- The "Cart" link also displays a shopping cart icon and the quantity of items in the cart, which is obtained from the Redux store. The quantity is displayed within a div with the class "cart_quantity."
- Inside the NavBar component, it uses the useSelector hook to access the Redux store. Specifically, it retrieves the cartQuantity from the cartReducer in the store.

Now also add the css for that as shown below:

```css
.cart_container {
  position: relative;
}

.cart_quantity {
  position: absolute;
  bottom: 0px;
  right: -10px;
  color: #3395b3;
  font-size: 0.9rem;
}
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/638/original/upload_55d0295fcdc0d468c0337e1c37914ffb.png?1699693108)

---
title: Buttons for the products
description: Discussion of increment and decrement buttons for the count of products in cart in detail.

---

## Buttons for the Products

Now we will bw adding the buttons heres for that firstly go to the materialUI and search for the plus button as shown below:

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/639/original/upload_5ba1e309c582bbdf0b4dd36e24474e71.png?1699693146)

Next to that we have to go to the home page because  holds everything like Categories Component, pagination, main, and the productList which render the products on the page

Now go to the productList.jsx and get the icons of the buttons of the plus and minus.

In the productList.jsx we need to add the following:

```javascript
import React from 'react';
import AddBoxIcon from '@mui/icons-material/AddBox';
import IndeterminateCheckBoxIcon from '@mui/icons-material/IndeterminateCheckBox';
import { useDispatch, useSelector } from "react-redux";
import { action } from '../redux/slices/cartSlice';

function ProductList(props) {
    const { productList } = props;
    const cartProducts = useSelector((store) => { return store.cartReducer.cartProducts })
    const dispatch = useDispatch();
    const handleAddProduct = (product) => {
        dispatch(action.addToCart(product));
    }

    const handleDeleteProduct = (product) => {
        dispatch(action.deleteFromCart(product));
    }

    return (
        <>
            {productList == null ? <h3 > Loading...</h3> :
                productList.map((product) => {
                    return (<div className = "product" key = {product.id}>
                        <img src = {product.image}
                            className = 'product_image' />
                        <div className = "product_meta">
                            <p className = "product_title">{product.title}</p>
                            <p className = 'Price'>$ {product.price}</p>
                        </div>
                        <div className = "add_to_cart_container">
                            <AddBoxIcon
                                onClick = {() => { handleAddProduct(product) }}
                            ></AddBoxIcon>
                            <div className = "currentCartCount">{<PrintCount cartProducts = {cartProducts} id = {product.id}></PrintCount>}</div>
                            <IndeterminateCheckBoxIcon
                                onClick = {() => { handleDeleteProduct(product) }}
                            >
                            </IndeterminateCheckBoxIcon>
                        </div>
                    </div>
                    )
                })}
        </>
    )
}
function PrintCount(props) {
    const { cartProducts, id } = props;
    let quanitity = 0;
    for (let i = 0; i < cartProducts.length; i++) {
        if (cartProducts[i].id == id) {
            quanitity = cartProducts[i].indQuantity
        }
    }
    return (<>
        {quanitity}
    </>)

}

export default ProductList;
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/640/original/upload_b929bd2bac8c187873f8f41f7da54bba.png?1699693265)

Also give the proper css to make it in horizontal direction:

```css
.add_to_cart_container {
  display: flex;
  justify-content: center;
}

.cart_product_wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
}
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/645/original/upload_5aba40a4394e1d80c15a99b81f44a72d.png?1699695784)

---
title: Add products to cart
description: Discussion of addition of the products in cart in detail.

---

## Add to Cart

* add to cart , remove cart -> given to a product list ->
    * remain the same even of the page change
    * filtering, sorting, searching
* You have show the cart with product quantity
* You have show the added product on carts page

In the Cart.jsx:

```javascript
import React from 'react';
import ProductList from '../components/ProductList';
import { useSelector } from "react-redux";
function Cart() {
  const productList = useSelector((store) => { return store.cartReducer.cartProducts })
  return (
    <>
      <h1>Cart</h1>
      <h2>Add to Product List</h2>
      <div className = "cart_product_wrapper">

        <ProductList productList = {productList}></ProductList>
      </div>
    </>

  )
}

export default Cart
```

- It imports the necessary modules and components, `ProductList` for displaying a list of products, and `useSelector` from Redux for accessing state.
- Inside the `Cart` component, it uses the `useSelector` hook to access the Redux store. Specifically, it retrieves the `cartProducts` from the `cartReducer` in the store, which represents the products in the shopping cart.
- It also includes a subheading, "Add to Product List."
- The `ProductList` component is used to render the list of products in the shopping cart, and it receives `productList` as a prop.


**Now lets organise our code a little bit:**

- For this lets create the page folder which will only consist the component that will pertain to a web page.


we want to have the single state of number of the products that should be added to your cart 

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/647/original/upload_a7a7af22719ba89ac5de799430e4be39.png?1699696188)

---
title: Integrate the redux
description: Discussion of itegration of the redux into application and slice creation in detail.

---

## Integrate the Redux

Lets itegrate the redux into your application and create the slice and then jump to all these things:

**To install redux, open terminal and type the command:**

```javascript
npm i react-redux @reduxjs/toolkit
```

Then create a folder redux in the src folder, and then create few files slices and stores.

In the cartSlice in the slices folder do the below needful things:
```javascript
// to create a slice -> redux;

import { createSlice } from "@reduxjs/toolkit";
//1
const cartSlice = createSlice({
    name: "countername",
    initialState: {
        cartQuantity: 0,
        // array of object -> [{details or th product, individal quantity},]
        cartProducts: []
    },
    // all the update logic 
    reducers: {
        addToCart: (state, action) => {
            state.cartQuantity++;
            const productToBeAdded = action.payload;
            const requiredProduct = state.cartProducts
                .find((cProduct) => { return cProduct.id == productToBeAdded.id });
            if (requiredProduct == undefined) {
                //quanityt
                productToBeAdded.indQuantity = 1
                state.cartProducts.push(productToBeAdded)
            } else {
                // already present
                requiredProduct.indQuantity++;
            }
        },

        deleteFromCart: (state, action) => {
            
            const productToBeAdded = action.payload;
            const productIdx = state.cartProducts
                .findIndex((cProduct) => { return cProduct.id == productToBeAdded.id });
            if (productIdx == -1) {

            } else {
                
                let product = state.cartProducts[productIdx];
                if (product.indQuantity == 0) {
                    state.cartProducts.splice(productIdx, 0);
                } else {
                    state.cartProducts[productIdx].indQuantity--;
                    state.cartQuantity--;
                }
            }
        }
    }
});

export const action = cartSlice.actions;
export default cartSlice;
```

**In the store.js:**

```javascript
import { configureStore } from '@reduxjs/toolkit';
import cartSlice from './slices/cartSlice';

import thunkMiddleWare from "redux-thunk";
// 2
const store = configureStore({
    reducer: {
        cartReducer: cartSlice.reducer
    },
    middleware: [thunkMiddleWare]
})
export default store;
```

In the main.jsx also add the provider to the whole application and pass the store in that as shown below:

```javascript
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'
import './index.css'
import { Provider } from 'react-redux';
import store from "./redux/store";
import { BrowserRouter } from "react-router-dom";
// import Routing from './poc/Routing.jsx';
// import Context from './poc/Context.jsx';
// import ThemeManger from './poc/context/themes/ThemeManger.jsx';
ReactDOM.createRoot(
  document.getElementById('root'))
  .render(
    <Provider store = {store}>
      <BrowserRouter>
        <App />
        {/* <Routing></Routing> */}
        {/* <Context></Context> */}
        {/* <ThemeManger></ThemeManger> */}
      </BrowserRouter>
    </Provider>
    ,
  )
```

Now also update the navBar.jsx for the redux to provide the cart related item in the navbar as follows:

```javascript
import React from 'react'
import { Link } from 'react-router-dom';
import ShoppingCartIcon from '@mui/icons-material/ShoppingCart';
import { useSelector } from "react-redux";
function NavBar() {
  const quantity  = useSelector((store) => { return store.cartReducer.cartQuantity })
  return (
    <div className = 'navbar'>
      <Link to = "/">Home </Link>
      <Link to = "/user">Users</Link>
      <Link to = "/cart">
        <div className = "cart_container">
          <ShoppingCartIcon></ShoppingCartIcon>
          <div className = "cart_quantity">{quantity}</div>
        </div>

      </Link>
    </div>
  )
}

export default NavBar
```

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/648/original/upload_e2290cb2356482c350661798d9fba471.png?1699696418)


Now lets go in the homepage where productList which render, add and remove products 

```javascript
import React from 'react';
import AddBoxIcon from '@mui/icons-material/AddBox';
import IndeterminateCheckBoxIcon from '@mui/icons-material/IndeterminateCheckBox';
import { useDispatch, useSelector } from "react-redux";
import { action } from '../redux/slices/cartSlice';

function ProductList(props) {
    const { productList } = props;
    const cartProducts = useSelector((store) => { return store.cartReducer.cartProducts })
    const dispatch = useDispatch();
    const handleAddProduct = (product) => {
        dispatch(action.addToCart(product));
    }

    const handleDeleteProduct = (product) => {
        dispatch(action.deleteFromCart(product));
    }

    return (
        <>
            {productList == null ? <h3 > Loading...</h3> :
                productList.map((product) => {
                    return (<div className = "product" key = {product.id}>
                        <img src = {product.image}
                            className = 'product_image' />
                        <div className = "product_meta">
                            <p className = "product_title">{product.title}</p>
                            <p className = 'Price'>$ {product.price}</p>
                        </div>
                        <div className = "add_to_cart_container">
                            <AddBoxIcon
                                onClick = {() => { handleAddProduct(product) }}
                            ></AddBoxIcon>
                            <div className = "currentCartCount">{<PrintCount cartProducts = {cartProducts} id = {product.id}></PrintCount>}</div>
                            <IndeterminateCheckBoxIcon
                                onClick = {() => { handleDeleteProduct(product) }}
                            >
                            </IndeterminateCheckBoxIcon>
                        </div>
                    </div>
                    )
                })}
        </>
    )
}
function PrintCount(props) {
    const { cartProducts, id } = props;
    let quanitity = 0;
    for (let i = 0; i < cartProducts.length; i++) {
        if (cartProducts[i].id == id) {
            quanitity = cartProducts[i].indQuantity
        }
    }
    return (<>
        {quanitity}
    </>)

}

export default ProductList;
```

- It receives `props` as an argument, which includes the `productList` prop. This prop represents an array of products to be displayed in the list.
- The component uses `useSelector` to access the Redux store, specifically the `cartProducts` from the `cartReducer`.
- It also uses `useDispatch` to obtain the `dispatch` function for dispatching Redux actions.
- The component defines two functions, `handleAddProduct` and `handleDeleteProduct`, which use the `dispatch` function to trigger Redux actions for adding and deleting products from the cart.
- It conditionally renders the list of products based on the `productList` prop. If `productList` is `null`, it displays a "Loading..." message; otherwise, it maps through the products and renders each one.
- For each product in the `productList`, it displays the product's image, title, and price.
- It provides an "Add to Cart" button (represented by an icon) to allow users to add the product to the cart.
- The current quantity of the product in the cart is displayed using the `PrintCount` component.
- It also provides a "Remove from Cart" button (represented by an icon) to allow users to remove the product from the cart.
- Each product in the map has a unique `key` assigned based on its `id`. This helps React efficiently update and render the list.

---
title: Immer library and reducer
description: Discussion of immer library and reducer in the application in detail.

---

## Immer Library and Reducer

**Immer Library:** is a tiny package that allows you to work with immutable state in a more convenient way.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/650/original/upload_f0dc9f49aadd9ee96d432efbac119704.png?1699696958)


In the cartSlice also provide the functionalities of the reducer of the addToCart and `deleteFromCart` as follows in the `cartSlice.jsx`:

```javascript
// to create a slice -> redux;

import { createSlice } from "@reduxjs/toolkit";
//1
const cartSlice = createSlice({
    name: "countername",
    initialState: {
        cartQuantity: 0,
        // array of object -> [{details or th product, individal quantity},]
        cartProducts: []
    },
    // all the update logic 
    reducers: {
        addToCart: (state, action) => {
            state.cartQuantity++;
            const productToBeAdded = action.payload;
            const requiredProduct = state.cartProducts
                .find((cProduct) => { return cProduct.id == productToBeAdded.id });
            if (requiredProduct == undefined) {
                //quanityt
                productToBeAdded.indQuantity = 1
                state.cartProducts.push(productToBeAdded)
            } else {
                // already present
                requiredProduct.indQuantity++;
            }
        },

        deleteFromCart: (state, action) => {
            
            const productToBeAdded = action.payload;
            const productIdx = state.cartProducts
                .findIndex((cProduct) => { return cProduct.id == productToBeAdded.id });
            if (productIdx == -1) {

            } else {
                
                let product = state.cartProducts[productIdx];
                if (product.indQuantity == 0) {
                    state.cartProducts.splice(productIdx, 0);
                } else {
                    state.cartProducts[productIdx].indQuantity--;
                    state.cartQuantity--;
                }
            }
        }
    }
});

export const action = cartSlice.actions;
export default cartSlice;
```


- `createSlice` includes the initial state, which consists of two properties:
     - **cartQuantity:** Represents the total quantity of items in the cart.
     - **cartProducts:** An array of objects that store details about the products in the cart, including individual quantities.
- The slice defines a `addToCart` reducer, which is responsible for increasing the `cartQuantity` and adding a product to the `cartProducts` array. If the product is not already in the cart, it is added with an individual quantity of 1. If it's already present, the individual quantity is incremented.
- The `deleteFromCart` reducer handles decreasing the `cartQuantity` and removing a product from the `cartProducts` array. If the product is not found in the cart, nothing happens. If the product is found and its individual quantity becomes zero, it is removed from the cart. Otherwise, the individual quantity is decremented.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/651/original/upload_17a8cc237aa2690168553e64e1e65dbd.png?1699697019)


Also make the changes in productList.jsx for the handling AddProduct and DeleteProduct as follows:

```javascript
import React from 'react';
import AddBoxIcon from '@mui/icons-material/AddBox';
import IndeterminateCheckBoxIcon from '@mui/icons-material/IndeterminateCheckBox';
import { useDispatch, useSelector } from "react-redux";
import { action } from '../redux/slices/cartSlice';

function ProductList(props) {
    const { productList } = props;
    const cartProducts = useSelector((store) => { return store.cartReducer.cartProducts })
    const dispatch = useDispatch();
    const handleAddProduct = (product) => {
        dispatch(action.addToCart(product));
    }

    const handleDeleteProduct = (product) => {
        dispatch(action.deleteFromCart(product));
    }

    return (
        <>
            {productList == null ? <h3 > Loading...</h3> :
                productList.map((product) => {
                    return (<div className = "product" key = {product.id}>
                        <img src = {product.image}
                            className = 'product_image' />
                        <div className = "product_meta">
                            <p className = "product_title">{product.title}</p>
                            <p className = 'Price'>$ {product.price}</p>
                        </div>
                        <div className = "add_to_cart_container">
                            <AddBoxIcon
                                onClick = {() => { handleAddProduct(product) }}
                            ></AddBoxIcon>
                            <div className = "currentCartCount">{<PrintCount cartProducts = {cartProducts} id = {product.id}></PrintCount>}</div>
                            <IndeterminateCheckBoxIcon
                                onClick = {() => { handleDeleteProduct(product) }}
                            >
                            </IndeterminateCheckBoxIcon>
                        </div>
                    </div>
                    )
                })}
        </>
    )
}
function PrintCount(props) {
    const { cartProducts, id } = props;
    let quanitity = 0;
    for (let i = 0; i < cartProducts.length; i++) {
        if (cartProducts[i].id == id) {
            quanitity = cartProducts[i].indQuantity
        }
    }
    return (<>
        {quanitity}
    </>)

}

export default ProductList;
```

**ProductList Component:**

2. **Redux State Access:** 
   - It uses `useSelector` to access the Redux store, specifically the `cartProducts` state from the `cartReducer`, which represents the products in the shopping cart.

3. **Dispatching Actions:** 
   - It uses `useDispatch` to obtain the `dispatch` function for dispatching Redux actions.
   - `handleAddProduct` and `handleDeleteProduct` functions are defined to dispatch actions when adding or deleting products from the cart.

4. **Product Rendering:** 
   - It maps through the `productList` and renders each product.
   - For each product, it displays the product's image, title, and price.

5. **Add to Cart and Delete from Cart:** 
   - Each product is associated with an "Add to Cart" button (represented by the `AddBoxIcon` component) and a "Delete from Cart" button (represented by the `IndeterminateCheckBoxIcon` component).
   - When the "Add to Cart" button is clicked, the `handleAddProduct` function is called to add the product to the cart.
   - When the "Delete from Cart" button is clicked, the `handleDeleteProduct` function is called to remove the product from the cart.

**PrintCount Component:**

6. **Functional Component:** 
   - `PrintCount` is a separate functional component used to display the quantity of a specific product in the cart.

7. **Props:** 
   - It receives `cartProducts` (the list of products in the cart) and the `id` of the product for which the quantity is to be displayed.

8. **Count Calculation:** 
   - It calculates the quantity of the specified product by iterating through `cartProducts` and matching the product's `id`. The quantity is then displayed.
   
![](https://hackmd.io/_uploads/HJh8v61Gp.png)

---
title: Render the products in Cart
description: Discussion of render the products in the carts page in detail.

---
**Now task is to render the products in the carts page, so lets go to the cart.jsx as shown below:**

```javascript!
import React from 'react';
import ProductList from '../components/ProductList';
import { useSelector } from "react-redux";
function Cart() {
  const productList = useSelector((store) => { return store.cartReducer.cartProducts })
  return (
    <>
      <h1>Cart</h1>
      <h2>Add to Product List</h2>
      <div className="cart_product_wrapper">

        <ProductList productList={productList}></ProductList>
      </div>
    </>

  )
}

export default Cart
```


- It uses `useSelector` to access the Redux store. Specifically, it retrieves the `cartProducts` state from the `cartReducer`, which represents the products in the shopping cart.
- It stores the list of products from the cart in the `productList` variable.
- It displays a heading with "Cart" to indicate the cart page. It also includes a secondary heading with "Add to Product List."
- It renders the product list by using the `ProductList` component and passing the `productList` as a prop. This component is responsible for displaying the products in the cart.
- It uses the `ProductList` component to render the list of products in the cart. The `ProductList` component handles the display of individual products and their quantities in the cart.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/652/original/upload_14d9fa19636bebff18f2bdce0b710e29.png?1699697165)

---
title: Performance and Lazy loading
description: Discussion of performance of  the large projects in detail.

---

## Performance and Lazy loading

take an example of makemytrip website which is build on the react
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/653/original/upload_d6e6d68e70d50ec3f8dadfeadc80e6ee.png?1699697212)


### Large Projects
* There will 1000s of components
* Redux


### Peformance
* Split the code unto multiple sections
* Two ways to load the code
    * load that code when it is required -> button is clicked
    * **Lazy Loading** : is when you depriortize certain part of react code that not important.

In create-react-app there is bundle code but the in the vite the code splitting is done automatically.

In the app.js:

```javascript
import './App.css';
import { useState, lazy, Suspense, } from "react";
import Navbar from './component/Navbar';
import Home from './pages/Home';

import { Route, Routes } from "react-router-dom";


function App() {
   const [visibility, setVisibility] = useState("none");
  const [posts, setPosts] = useState([]);
  const handleClick = () => {
    // dynmaic import
    import("./posts.js").then((module) => {
      // console.log(module);
      setPosts(module.default);
    })
     if (visibility == "none") {
       setVisibility("block");
       // dynmaic import

     } else {
       setVisibility("none");
     }
     // console.log("handleClick");


  }

  return (
    <>
      { <h1>I am App</h1>
      <button onClick = {handleClick}> Add Image</button>
      <ul >{
        posts.map((post, idx) => {
          return <p key = {idx}>{JSON.stringify(post)}</p>
        })
          
      }</ul> }

  );
}

export default App;
```
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/654/original/upload_0ac6add54c0e0fae24358d6f40a72926.png?1699697251)

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/655/original/upload_a5c1fd744425bebc197e45bfa82a2acf.png?1699697276)


- The `handleClick` function is called when a button is clicked. It uses dynamic import to asynchronously load a module from a file named "posts.js." When the module is loaded, the posts are set in the `posts` state.
- It toggles the visibility of an element by changing the `visibility` state from "none" to "block" and vice versa.
- The list of posts is rendered conditionally inside a `<ul>` element. It checks if there are posts in the state before rendering.
- If there are posts in the state, it maps through the `posts` array and displays each post as a stringified JSON object inside a `<p>` element.

---
title: Component Wise importing and code splitting
description: Discussion of lazy loading and the code splitting techniques for component wise importing in detail.

---

## Component Wise Importing and Code Splitting

Now lets do it on the basis of the importing the component wise as follows:

Lazy loading and code splitting help improve the performance of the application by loading components only when they are needed. This is similar to how MakeMyTrip's website loads different sections of the site as the user navigates.

Overall, this code implements lazy loading to optimize the loading of components, which is a common strategy for improving the performance of web applications, especially when dealing with larger applications like MakeMyTrip.

```javascript
import './App.css';
import { useState, lazy, Suspense, } from "react";
import Navbar from './component/Navbar';
import Home from './pages/Home';

import { Route, Routes } from "react-router-dom";


// import About from './pages/About';
// import Products from './pages/Product';
const About = lazy(() => { return import("./pages/About") });
const Products = lazy(() => { return  import("./pages/Product") });

function App() {
   const [visibility, setVisibility] = useState("none");
  const [posts, setPosts] = useState([]);
  const handleClick = () => {
    // dynmaic import
    import("./posts.js").then((module) => {
      // console.log(module);
      setPosts(module.default);
    })
     if (visibility == "none") {
       setVisibility("block");
       // dynmaic import

     } else {
       setVisibility("none");
     }
     // console.log("handleClick");


  }

  return (
    <>
      { <h1>I am App</h1>
      <button onClick = {handleClick}> Add Image</button>
      <ul >{
        posts.map((post, idx) => {
          return <p key = {idx}>{JSON.stringify(post)}</p>
        })
        
      <Suspense fallback = {<h2>...Loading</h2>}>
        <Navbar></Navbar>
        <Routes>
          <Route path = "/" element = {<Home />}></Route>
          <Route path = "/about" element = {<About />}></Route>
          <Route path = '/products' element = {<Products />}></Route>
        </Routes>
      </Suspense>

    </>       
      }</ul> }

  );
}

export default App;
```


- The code uses the `lazy` function from React to dynamically import components. In this case, it imports the "About" and "Products" components using code splitting.
- The `handleClick` function is called when a button is clicked. It uses dynamic import to asynchronously load a module from a file named "posts.js."
- When the module is loaded, the posts are set in the `posts` state.
- Components are conditionally rendered inside a `<Suspense>` component. The `fallback` prop displays a loading message while the components are being loaded.
- The code uses the `Routes` and `Route` components from React Router for routing. The main route is defined for the home page, and additional routes are defined for the "About" and "Products" pages.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/656/original/upload_884d588d618343cb2e0d0d1d98087dc7.png?1699697376)


![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/657/original/upload_15df7661db900fb76153f62d6578bdb5.png?1699697402)

