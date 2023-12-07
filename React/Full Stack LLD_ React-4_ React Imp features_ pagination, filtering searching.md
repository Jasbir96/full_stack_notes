# Full Stack LLD: React-4: React Imp features: pagination, filtering searching

---
title: Agenda of the lecture
description: What will be covered in the topic?

---

## Agenda
* Wirframe and routing in Ecommerce Project
* Searching(filtering)
* Sorting
* Group By
* Refractoring the code




---
title: E-commerce WebApp wireframe and routing setup
description: Wireframe of Webapp and routing for webapp

---


## E-commerce WebApp

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/597/original/upload_d8a036ffa40a59fae78e4e3397128f4a.png?1699680485)

**[Ask the learners]**
If I am creating a very simple E-commerce website what are different routes we can have?
-> Product, orders, profile, etc

create pages 
-> Home.jsx
-> Product.jsx
-> PageNotFound.jsx
-> ProductDetails.jsx
-> Profile.jsx
-> Navbar.jsx

### Navbar.jsx
* import NavBar
* import Routes and Route

### App.jsx
* here Import Navbar.jsx and define routes for various pages
* For products have use dynamic routes with the `id` param

```jsx
import { useState } from 'react'
import reactLogo from './assets/react.svg'
import viteLogo from '/vite.svg'
import './App.css'
import NavBar from './components/NavBar'
import { Routes, Route, Navigate } from "react-router-dom";
import PageNotFound from './components/PageNotFound'
import Home from './components/Home';
import Product from './components/Product'
import ProductDetails from './components/ProductDetails'
function App() {
  return (
    <>
      <NavBar></NavBar>
      <Routes>
        <Route path = "/" element = {<Home></Home>}> </Route>
        <Route path = "/product" element = {<Product></Product>}></Route>
        <Route path = "/product/:id" element = {<ProductDetails></ProductDetails>}> </Route>
        <Route path = "/home" element = {<Navigate to = "/"></Navigate>}></Route>
        <Route path = "*" element = {<PageNotFound></PageNotFound>}> </Route>
      </Routes>
    </>

  )
}

export default App
```

### main.jsx
* **just add an App to it**
```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'
import './index.css'
import { BrowserRouter } from "react-router-dom";
import Routing from './poc/Routing.jsx';
ReactDOM.createRoot(
  document.getElementById('root'))
  .render(
    <React.StrictMode>
      <BrowserRouter>
        <App />
      </BrowserRouter>
    </React.StrictMode>,
  )
```

### Home.jsx
* Let us have a search input on the homepage that listens to onchange
* Next, let us use the API of products on Fake Store API.
* We are using the same code we used previously for users so we can copy and make some changes.
* When we are using an array in useState it is a good practice to give some initial value like empty array or null.
* We will also use loading placeholder and then we will display the data when loaded.
* If the `products.length == 0` then we will display `loading...` else we will display products.
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/598/original/upload_435dc9e6d6cc38fd70417e2e11cb82e9.png?1699680720)
* We will only keep important things from this
* Pasting some css to the App.css
```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
}

.nav_wrapper {
   background-color: black;
  height: 7rem;
}

.search_input {
  height: 2rem;
  width: 15rem;
}


.product_wrapper {
  margin-top: 3rem;
  margin-left: 2rem;
  margin-right: 2rem;
  display: flex;
  flex-wrap: wrap;
  gap: 2rem;
  justify-content: space-evenly;

}

.product {
  border: 1px solid gray;
  padding: 1rem;
  text-align: center;
  height: 18rem;
  width: 20rem;
  overflow-y: auto;
}

.product_image {
  height: 200px;
  aspect-ratio: 3/4;
  text-align: center;
}
```

* We are getting an array of 20 elements, what we are doing is mapping each of them. In the API they have provided a link that returns an image.

```jsx
import React, { useState, useEffect } from 'react'

function Home() {
    const [searchTerm, setSearchTerm] = useState("");
    const [products, setProducts] = useState(null);
    useEffect(() => {
        (async function () {
            const resp = await fetch(`https://fakestoreapi.com/products`)
            const productData = await resp.json();

            setProducts(productData);
        })()
    }, [])


    return (
        <>
            <header className = "nav_wrapper">
                <input
                    className = 'search_input'
                    type = "text"
                    value = {searchTerm}
                    onChange = {(e) => { setSearchTerm(e.target.value) }} />
            </header>

            <main className = "product_wrapper">
                {/* products will be there */}
                {products.length == 0 ? <h3> Loading...</h3> :
                    products.map((product) => {
                        return (<div className = "product">
                            <img src = {product.image} alt = ""
                                className = 'product_image' />
                            <div className = "product_meta">
                                <p className = "product_title">{product.title}</p>
                                <p className = 'Price'>$ {product.price}</p>
                            </div>
                        </div>
                        )
                    })}
            </main>
        </>

    )
}

export default Home;
```

---
title: Searching
description: How to implement Searching in the Products Array using the search bar

---

## Searching 
**[Ask the learners]**
What is the algorithm that you can use to find the objects from the text? When a user types the letters it should match with the objects containing a letter of the object.

Should the stateVariable change?
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/599/original/upload_080318a5454ec219fab312043a2cc844.png?1699680981)
-> We have to hide it and not change it.

* the product that is mapping function is rendering my products.
* What we can do is we can create a copy of the searchTerm that is filtered -> filteredArray and we map with the filter array instead of a general array.
* we are using the useEffect when the searchTerm is modified.

```jsx
let filteredArr = products;
    if (searchTerm != "") {
        filteredArr = filteredArr.filter((product) => {
            let lowerSearchTerm = searchTerm.toLowerCase();
            console.log("20", product.title)
            let lowerTitle = product.title.toLowerCase();
            return lowerTitle.includes(lowerSearchTerm);
        })
    }
```

**Home.jsx**
```jsx
import React, { useState, useEffect } from 'react'

function Home() {
    const [searchTerm, setSearchTerm] = useState("");
    const [products, setProducts] = useState(null);
    useEffect(() => {
        (async function () {
            const resp = await fetch(`https://fakestoreapi.com/products`)
            const productData = await resp.json();

            setProducts(productData);
        })()
    }, [])



    let filteredArr = products;
    if (searchTerm != "") {
        filteredArr = filteredArr.filter((product) => {
            let lowerSearchTerm = searchTerm.toLowerCase();
            console.log("20", product.title)
            let lowerTitle = product.title.toLowerCase();
            return lowerTitle.includes(lowerSearchTerm);
        })
    }

    return (
        <>
            <header className = "nav_wrapper">
                <input
                    className = 'search_input'
                    type = "text"
                    value = {searchTerm}
                    onChange = {(e) => { setSearchTerm(e.target.value) }} />
            </header>

            <main className = "product_wrapper">
                {/* products will be there */}
                {filteredArr == null ? <h3> Loading...</h3> :
                    filteredArr.map((product) => {
                        return (<div className = "product">
                            <img src = {product.image} alt = ""
                                className = 'product_image' />
                            <div className = "product_meta">
                                <p className = "product_title">{product.title}</p>
                                <p className = 'Price'>$ {product.price}</p>
                            </div>
                        </div>
                        )
                    })}
            </main>
        </>

    )
}

export default Home;
```

---
title: Code revision
description: What we covered in previous lectures?

---

## Code revision
* We have navbar
* list of products from the FAKE STORE API
* We can also filter them out in the search bar
* We have App.jsx having few paths with dummy pages
* We also have a Dummy Navbar as well
* Next we have a home page, we have a search bar, below which all the products are listed and we have a header.
* Inside the main we have a loop to get the products.

**[Ask the learners]**

In which line of code am I making the request or from where I am getting the data?
Ans : we are getting that from useEffect through fakestore API and setting the product

How we were filtering the products using search term?
Ans : 
* Hiding Products - So based on the search term we are doing a linear search in the list of names of the products to find the search term. If the search term is found we show the product else we discard it.
* For example if we write `h` so all the products having `h` in the title are shown.


---
title: Material UI
description: Adding icons using Material UI

---

## Material UI

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/600/original/upload_782a2b1b29f9b3caed37470eebfc20db.png?1699681167)

* Have added these two icons up and down
*  This is done using the MaterialUI library by Google
* Many of the sites are built using Material UI
* We are only going to use the icons from MaterialUI
* We can simply add the link and receive the data.
* Go to material UI icons website  Show searching feature on it
* Copy the npm install link, go to the products folder where the project is hosted, and paste it into the terminal.
* Find Up and Down as shown in the image above.
* Copy and paste the import code from the website
* Add the element in the `Home.jsx`, these act as react components only.

```jsx
import ArrowCircleUpIcon from '@mui/icons-material/ArrowCircleUp';
import ArrowCircleDownIcon from '@mui/icons-material/ArrowCircleDown';
```
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/601/original/upload_00a51c9f17eb2a99ee49d7b907db1191.png?1699681217)

* Provide `style` and `fontSize` props to the imported React components.
```jsx
        <header className = "nav_wrapper">
                <div className = "search_sortWrapper">
                    <input
                        className = 'search_input'
                        type = "text"
                        value = {searchTerm}
                        onChange = {(e) => { setSearchTerm(e.target.value) }} />
                    <div className = "icons_container">
                        <ArrowCircleUpIcon
                            style = {{ color: "white" }}
                            fontSize = "large"
                            onClick = {() => { setsortDir(1) }}
                        ></ArrowCircleUpIcon>
                        <ArrowCircleDownIcon
                            fontSize = "large"
                            style = {{ color: "white" }}
                            onClick = {() => { setsortDir(-1) }}
                        ></ArrowCircleDownIcon>
                    </div>
                </div>
```

* Navigate to the [Material UI Icons](https://mui.com/material-ui/material-icons/)  documentation and explain the icons and a few of its props.
* Material UI is a website that ready-to-use React Material Icons. These icons make the buttons of the page self explanatory.
* It can be installed as follows in `npm`
```bash
npm install @mui/icons-material @mui/material @emotion/styled @emotion/react
```
* Material UI contains thousands of icons, we can search the icons with the help of search bar.
* We can also filter the icons based on the style.
![image.png](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/602/original/upload_33e4c407abeee2d612d2a90ccf73fbf4.png?1699681535)
* The purpose of these libraries is to give you components so that these features can be added and after that, they can also give you functions based on your library.
* To use these icons, click on the icon.
![image.png](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/603/original/upload_94951c21a3f1aef86a5d7b8e64d01a7d.png?1699681573)
* Copy the import code and use it as an React Component.

---
title: Styling for category wrappers
description: Styling category wrappers

---

## Styling
* You can either have multiple CSS files or a single CSS file but the HTML document is always going to be one so all the stylings are always applied to the same document.
* Go inspect in Chrome and show the various CSS files in the HTML document.
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/604/original/upload_9d6eeb72ffe2eaefefabbfd665431952.png?1699681623)
* In App.css, add styles to it as follows and explain styling.
```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
}

.nav_wrapper {
  background-color: black;
  height: 7rem;
}

.search_sortWrapper {
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 2rem;
  height: 4rem;

}

.search_input {
  height: 2rem;
  width: 15rem;
}

.categories_wrapper {
  background-color: rgb(226, 218, 218);
  height: 3rem;
  padding-right: 3rem;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  gap: 1rem;
}

.category_option {
  height: 70%;
  width: 8rem;
  border-radius: 10px;
  background-color: transparent;
}


/*products section  */
.product_wrapper {
  margin-top: 3rem;
  margin-left: 2rem;
  margin-right: 2rem;
  display: flex;
  flex-wrap: wrap;
  gap: 2rem;
  justify-content: space-evenly;

}

.product {
  border: 1px solid gray;
  padding: 1rem;
  text-align: center;
  height: 18rem;
  width: 20rem;
  overflow-y: auto;
}

.product_image {
  height: 200px;
  aspect-ratio: 3/4;
  text-align: center;
}
```

---
title: Sorting
description: Implementing Sorting using UP and DOWN buttons

---

## Sorting
* Now when we click on the UP button the products should be lined up in increasing order and similarly, when we click on the DOWN button the products must align in decreasing order concerning their price.

**[Ask the learners]**
Can anyone think of any algorithm that we can use to implement this?
-> For searching we are just hiding a few products we are not changing the data.
Similarly, for sorting we are arranging the data, we are not modifying it.

* We will have another state variable and by default its value will be 0. 
* It can be sorted in increasing order (1), decreasing order(-1), or not sorted (0).
* We will use the `filteredArr` and `.sort()` function of JavaScript to sort the elements.
**[Ask the Learners]**
What is the default type of sorting given to you by JavaScript
-> Lexicographical sorting/ alphabetical sorting

* We will have two comparator functions `incComparator` and`decComparator`.
* These will take two parameters to be compared.
* We have to tell which one is larger and which one is smaller.
* So we will compare based on price here as follows:
```jsx
function incComparator(product1, product2) {
    if (product1.price > product2.price) {
        return 1
    } else {
        return -1;
    }
}
function decComparator(product1, product2) {
    if (product1.price < product2.price) {
        return 1
    } else {
        return -1;
    }
}

let filteredSortedArr = filteredArr;
    if (sortDir != 0) {
        // increasing 
        if (sortDir == 1) {
            filteredSortedArr = filteredSortedArr.sort(incComparator);
        }
        //    decreasing order
        else {
            filteredSortedArr = filteredSortedArr.sort(decComparator);
        }
    }
```

---
title: Adding Category Buttons
description: Implementing categorization/ groupBy logic

---

## Adding Category buttons

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/605/original/upload_2441b6654d33e5fb591445c0301944fa.png?1699681935)
* Listing these buttons and printing them in the navbar
* Add its stylings
* Categories will be dynamic inferring that you have some category but no products in it will not make any sense or some new category product but the user doesn't have that category option only.
* So what we want to do is, list the categories dynamically for the products we are having.
* Visit Fake Store and fetch the categories from the API.
* We will create a new state variable for categories
```jsx
const [categories, setCategories] = useState([]);
useEffect(() => {
        (async function () {
            const resp = await fetch(`https://fakestoreapi.com/products/categories`)
            const categoriesData = await resp.json();
            setCategories(categoriesData);
        })()
    }, [])
```
```jsx
<div className = "categories_wrapper">
    <button className = "category_option">All categories</button>
    {categories.map((category) => {
        return <button className = "category_option">{category}</ button>
    })}
</div>
```
* We are first fetching the categories from the URL and then mapping them.
* The buttons are displayed on the navbar.

**[Ask the learners]**
How can we implement the category feature? With each product, there is a category associated.
-> We have to use the Category feature
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/606/original/upload_e5c7b6586c51bbe84b6f2b16801a99cb.png?1699681994)
-> We will be using the filter on the category feature.
Here when `All Categories` is selected initially we have to show all the products. Otherwise, we have to select products from a given category.
![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/607/original/upload_e51c3b2232a1b6719a724295743ebfb0.png?1699682054)
So, do we need states for this as well?
-> Yes, again we are required to use the states for the same.

* Create a state variable for the current category, and add the logic function using the filter.
```jsx
const [currCategory, setCurrCategory] = useState("All categories");
```

```jsx
let filteredSortedgroupByArr = filteredSortedArr;
    if (currCategory != "All categories") {
        filteredSortedgroupByArr = filteredSortedgroupByArr.filter((product) => {
            return product.category == currCategory
        })
    }
```
```jsx
<div className = "categories_wrapper">
    
    <button className = "category_option" onClick = {() => {setCurrCategory("All categories")}}>All categories</button>
    {categories.map((category) => {
        return <button className = "category_option" onClick = {() => {
                setCurrCategory(category)
            }}>{category}</ button>
    })}
</div>
```

---
title: Refactoring Code
description: extracting categories and ProductList component

---

## Refactoring Code

### Extracting Products List
* Let us first refactor the code such that code is easy to understand
* Segregate our application into multiple components to easily understand what is happening in it.
* So there are two major moving parts -> ProductList.jsx and Categories.jsx
* `rfce` in ProductList and copy the code from the `Home.jsx` and replace this code with ProductList element and pass the necessary props like productList i.e. final array `filteredSortedgroupByArr`.

**ProductList.jsx**
```jsx
import React from 'react'

function ProductList(props) {
    const { productList } = props;
    return (
        <>
            {productList == null ? <h3> Loading...</h3> :
                productList.map((product) => {
                    return (<div className = "product">
                        <img src = {product.image} alt = ""
                            className = 'product_image' />
                        <div className = "product_meta">
                            <p className = "product_title">{product.title}</p>
                            <p className = 'Price'>$ {product.price}</p>
                        </div>
                    </div>
                    )
                })}
        </>
    )
}

export default ProductList
```

**In Home.jsx**

```jsx
<main className = "product_wrapper">
                {/* products will be there */}
                <ProductList productList = {filteredSortedgroupByArr}> ̰</ProductList>
</main>
```


* We will also do the same for Categories and do rfce and copy the code from the `Home.jsx` to the `Categories.jsx`.
* Call this file from the `Home.jsx` and pass the required props to it. Firstly, we passed the `categories` prop to it.
* Next once all of these are done, what is the next thing to be done?
* Categories are dynamic! 
* We also have to pass the `setCategories` function as well.

### Extracting Categories

**Categories**

```jsx
import React from 'react'
function Categories(props) {
    const { categories, setCurrCategory
    } = props
    return (
        <>
            <button className = "category_option"
                onClick = {() => {
                    setCurrCategory("All categories")
                }}
            >All categories</button>
            {categories.map((category) => {
                return <button className = "category_option"
                    onClick = {() => {
                        setCurrCategory(category);

                    }}
                > {category}</button>
            })}
        </>
    )
}

export default Categories
```

```jsx
<div className = "categories_wrapper">
    <Categories categories = {categories} setCurrCategory = {setCurrCategory}>
    </Categories>
</div>
```

* The code is now a bit easier to understand.
* We are rendering two portions the header and the ProductList
* The header portion contains input and the two buttons.
* Next there are categories and we sent the props required as well as the methods required.
 
---
title: Refactoring filtering, sorting, and logic
description: Refactoring filtering, sorting, and logic

---

### Segregating Logic
* Let us segregate the logic part from the `Home.jsx` as well
* Add a folder called `utility` and create a file called basixOps.js.

```jsx
export default function basicOps(products, searchTerm, sortDir, currCategory, pageNum, pageSize) {
    if (products == null) {
        return;
    }
    /*************filtering -> hiding  products*************/
    let filteredArr = products;
    if (searchTerm != "") {
        filteredArr = filteredArr.filter((product) => {
            let lowerSearchTerm = searchTerm.toLowerCase();
            let lowerTitle = product.title.toLowerCase();
            return lowerTitle.includes(lowerSearchTerm);
        })
    }
    /***********************sorting -> rearrange**********************************/
    let filteredSortedArr = filteredArr;
    if (sortDir != 0) {
        // increasing 
        if (sortDir == 1) {
            filteredSortedArr = filteredSortedArr.sort(incComparator);
        }
        //    decreasing order
        else {
            filteredSortedArr = filteredSortedArr.sort(decComparator);
        }
    }

    /**************************categorization**********************************************/
    let filteredSortedgroupByArr = filteredSortedArr;
    if (currCategory != "All categories") {
        filteredSortedgroupByArr = filteredSortedgroupByArr.filter((product) => {
            return product.category == currCategory
        })
    }
}

// total elem /elemperPage-> totalNumPages 

function incComparator(product1, product2) {
    if (product1.price > product2.price) {
        return 1
    } else {
        return -1;
    }
}
function decComparator(product1, product2) {
    if (product1.price < product2.price) {
        return 1
    } else {
        return -1;
    }
}
```

* We have to pass products, current category, searchTerm, and sortDirection to the basicOps. This will give us `filteredSortedgroupByArr`.

**Home.jsx**

```jsx
import React, { useState, useEffect } from 'react';
import ArrowCircleUpIcon from '@mui/icons-material/ArrowCircleUp';
import ArrowCircleDownIcon from '@mui/icons-material/ArrowCircleDown';
import KeyboardArrowLeftIcon from "@mui/icons-material/KeyboardArrowLeft";
import ChevronRightIcon from '@mui/icons-material/ChevronRight';
import ProductList from './ProductList';
import Categories from './Categories';
import basicOps from './utility/basicOps';
import { usePaginationContext } from './contexts/PaginationContext';

function Home() {
    // preserver -> pagination
    /***single source of truth for all the products***/
    const [products, setProducts] = useState(null);
    /************ all the categories -> a product**********/
    const [categories, setCategories] = useState([]);
    /**********Action***********/
    /*********************** state ->term with which you want to filter the product list*****************************/
    const [searchTerm, setSearchTerm] = useState("");
    /**************************sort : 0: unsorted, 1: increasing order, -1: decreasing order ************************************/
    const [sortDir, setsortDir] = useState(0);
    /**************************** currcategory: category group you result **********************************/
    const [currCategory, setCurrCategory] = useState("All categories");
    // page number and page size
    const { pageSize, pageNum,
        setPageNum,
        setPageSize } = usePaginationContext();
    /****************get all the products*********************/
    useEffect(() => {
        (async function () {
            const resp = await fetch(`https://fakestoreapi.com/products`)
            const productData = await resp.json();
            setProducts(productData);
        })()
    }, [])

    /**************getting all the categories ********************/
    useEffect(() => {
        (async function () {
            const resp = await fetch(`https://fakestoreapi.com/products/categories`)
            const categoriesData = await resp.json();
            setCategories(categoriesData);
        })()
    }, [])
    
    const object = basicOps(products, searchTerm, sortDir, currCategory);
    const filteredSortedgroupByArr = object.filteredSortedgroupByArr;
    const totalPages = object.totalPages;
    return (
        <>
            {/* header */}
            <header className = "nav_wrapper">
                <div className = "search_sortWrapper">
                    <input
                        className = 'search_input'
                        type = "text"
                        value = {searchTerm}
                        onChange = {(e) => {
                            setSearchTerm(e.target.value)
                        }} />
                    <div className = "icons_container">
                        <ArrowCircleUpIcon
                            style = {{ color: "white" }}
                            fontSize = "large"
                            onClick = {() => {
                                setsortDir(1)
                            }}
                        ></ArrowCircleUpIcon>
                        <ArrowCircleDownIcon
                            fontSize = "large"
                            style = {{ color: "white" }}
                            onClick = {() => {
                                setsortDir(-1)
                            }}
                        ></ArrowCircleDownIcon>
                    </div>
                </div>

                <div className = "categories_wrapper">
                    <Categories categories = {categories}
                        setCurrCategory = {setCurrCategory}
                        
                    ></Categories>
                </div>

            </header>

            {/* main area  */}
            <main className = "product_wrapper">
                {/* products will be there */}
                <ProductList productList = {filteredSortedgroupByArr}> ̰</ProductList>
            </main>
        </>

    )
}

export default Home;


/***
 * 1. Steps/ 
 *  - INtial Data 
 *  a. Searching
 *  b. Sorting
 *  c. Categorization
 *  d. Pagination
 *  e. Render the Results
 * 
 * 2. Data 
 *      1. Products
 *      2. Categories
 * 
 * 
 * **/

```

### Code summarization

**[Ask the learners]**

**What is the usecase of the productState?**
-> Single source of truth for all the products
-> It contains all the products, sorting, filtering, and categorization, all are derived from the products.

**What do our categories state contain?**
-> It contains all the categories for our products

**What is the use case of the search term?**
-> A single term with which you want to filter the product list

**What is this state variable sortDir doing here?**
-> It has three possible values: `0 => unsorted`, `1 => increasing order`, and`-1 => decreasing order`.

**Current category?**
-> It stores the current category to which you want to group the result

**The first useEffect?**
-> To get all the products

**The Second useEffect?**
-> To get all the categories

**firterSortedgroupByArr?**
-> Navigate to basicOps file 
-> Here we have two helper functions for comparators
-> Firstly we are filtering, then sorting, and lastly categorizing all to get the final answer.

In return
-> First is header => Here we have search and sorting buttons and below it we have categories
-> Navbar improves end-user experience by facilitating functionalities
-> Search => It simply changes the search term, product list changes based on it. The state searchTerm changes and so basicOps is passed and a new list is derived from it.
-> icon_container => The up sets the direction to `1` and down changes it to `-1`. This changes the sorting order to increasing and decreasing respectively.
-> categorize => As per the selected category, its state changes and so does the products list.
-> Next is the main area which is taking ProductList and printing the data.

Gist -
* We have a webpage, and some user interaction happened on the page.
* State of some variable changed.
* This state change modifies the list or its order. The new list is then returned to the page.

![](https://d2beiqkhq929f0.cloudfront.net/public_assets/assets/000/056/608/original/upload_eaafc3d4f8ae6149af0ac9907f9fc32e.png?1699684408)

