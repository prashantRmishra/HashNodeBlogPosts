# HashRouter in React Routing

##  What is BrowserRouter  ?

#### `BrowserRouter` will allow us to have well-formed URLs.



Example `/about` or `/contact` etc. What it means is that this `about.js`  is a file present inside a folder named about or contact for `/contact` for that matter.


See the below code snippet for a better understanding of the normal routing of `<BrowserRouter>`



```js
import { Component } from "react";

export class About extends Component{
   render(){
    return (
        <div>This is about section</div>
    );
   }
}
```

```js
import { Component } from "react";

export class Contact extends Component{
    render(){
        return (
            <div>This is the contact page of the application</div>
        );
    }
}
```

```js
import logo from './logo.svg';
import './App.css';
import { Link } from 'react-router-dom';

function App() {
  return (
    <div >
     <h1>Home page</h1>
     <Link to={'/about'}>About</Link>
     <br></br>
     <Link to ={'/contact'}>Contact</Link>
    </div>
  );
}

export default App;
``` 

```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { BrowserRouter, Route, Routes } from 'react-router-dom'
import { About } from './components/About';
import { Contact } from './components/Contact';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <Routes>
        <Route  path={'/'} element={<App></App>}></Route>
        <Route  path={'/about'} element={<About/>}></Route>
        <Route  path={'/contact'} element={<Contact/>}></Route>
      </Routes>
    </BrowserRouter>
  </React.StrictMode>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();

```
Routing in the browser : 
![about-router-url-image.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1657527461650/9uUnVAf27.PNG align="left")

****

##  HashRouter

While it's ok while development **it may fail if it is available on a live server**. This is because after the build everything will be available inside the same file and there won't be any `/about` or `/contact` folder to look into and in that case, it will lead to an issue.
Hence we need some marker like `#` or `!` that can act as an Id to render the required modules or components inside the browser in a live environment.

Hence, to overcome this disadvantage we use `HashRouter`


`HashRouter` is used when don't want to send the browser URL to the server-side

`HashRouter` will add # to after the base URL i.e. `localhost:3000/#/about`

Make the following changes in your `index.js` file

```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { BrowserRouter, Route, Routes,HashRouter } from 'react-router-dom'
import { About } from './components/About';
import { Contact } from './components/Contact';

const root = ReactDOM.createRoot(document.getElementById('root'));
/*
hashType : 
  default : slash /#/ example : /#/about
            noslash /# example : /#about
            hashbang /#!/ example : /#!/about
  */
root.render(
  <React.StrictMode>
    <HashRouter>
      <Routes>
        <Route  path={'/'} element={<App></App>}></Route>
        <Route  path={'/about'} element={<About/>}></Route>
        <Route  path={'/contact'} element={<Contact/>}></Route>
      </Routes>
    </HashRouter>
  </React.StrictMode>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();

```

![contact-hashrouter-url-image.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1657528243965/v3SR_x1Cm.PNG align="left")
