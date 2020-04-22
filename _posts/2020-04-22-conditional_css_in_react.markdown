---
layout: post
title:      "Conditional CSS In React"
date:       2020-04-22 23:31:48 +0000
permalink:  conditional_css_in_react
---


In React we commonly use conditional class to appropriately render the view for the user. Our condition could be based on incoming props, data, or some state. In this blog we will explore the many ways to conditionally create classes for our components. 



In our code examples we'll discover multiple ways to build a Light and Dark mode. This will solidify our understanding of conditionally based classes. 



![](https://media.giphy.com/media/S5iIDKRjhRJVjtsZpE/giphy.gif)





### Example Skeleton

Let's create the barebones of our example. We'll start by creating a `isClicked` state set to `false` and a  handler responsible for inverting its state. Then we'll create button with a ternary which returns either "Dark "or "Light "  based on our `isClicked` state. Lastly, we must create our CSS styles for both Light and Dark modes. We'll call it `bkg-dark,` and `bk-light`.



![](https://im3.ezgif.com/tmp/ezgif-3-e2add181a2c0.png)





### Ternary Operations

Approaching our example with a ternary solution would be fairly simple. Within our app component div we would utilize ES6 Template literals. This would allow us to merge our string literals with our embed JS through interpolation. Here we can interpolate our ternary operation which would decided whether to add our dark or light mode based on our state.



![](https://im3.ezgif.com/tmp/ezgif-3-3957bf9660f9.png)



```react
// isClicked evaluates to TRUE 

<div className={`App ${isClicked ? "bkg-dark" : "bkg-light"}`}> // returns "bkg-dark"
      <button className="btn" onClick={handleToggle}>
        <strong>{isClicked ? "Light" : "Dark"}</strong>
      </button>
</div>


// isClicked evaluates to FALSE 

<div className={`App ${isClicked ? "bkg-dark" : "bkg-light"}`}> // returns "bkg-light"
      <button className="btn" onClick={handleToggle}>
        <strong>{isClicked ? "Light" : "Dark"}</strong>
      </button>
</div>


// OR you can try this: "Only returns bkg-dark if isClicked evaluates to TRUE"
<div className={`App ${isClicked && "bkg-dark"}`}> 
      <button className="btn" onClick={handleToggle}>
        <strong>{isClicked ? "Light" : "Dark"}</strong>
      </button>
</div>
```



This solution is mostly used when applying conditional CSS. However there are situations where a simple ternary becomes inadequate to provide the necessary class accommodations needed . 

HOW?

Well, what if we wanted to add multiple CSS classes when our state `isClicked` is either true or false. Or what if we wanted to add different classes to our background based on multiple states within our App component. Yes,  these what if's our outside of the realm of this example however these our great demonstrations that illustrate the limits of ternary operations.  It can be debated that ternaries can be used to dynamically add multiple classes however it would extremely verbose and difficult to read.

A perhaps simpler solution would be to use a utility NPM package called `classNames`



### Class Names

This library was designed to offer a simpler solution for applying conditionally CSS within your React application and other JS frameworks. There are other honorable mentioned library's such as `clsx` that offer the same solution, however we'll only highlight the Class Names utility. it works with any data type you provide and determines if its `Truthy` or `Falsey`. The truthy values will be added to your chosen element class. 

Example:

```javascript
classNames("App", "bkg-dark", "bkg-light") //=> "App bkg-dark bkg-light"

classNames(["App", "bkg-dark", "bkg-light"]) //=> "App bkg-dark bkg-light"


// isClicked evaluates to TRUE
classNames("App",{"bkg-dark": isClicked,"bkg-light": !isClicked})  //=> "App bkg-dark"

// isClicked evaluates to FALSE
classNames("App",{"bkg-dark": isClicked,"bkg-light": !isClicked})  //=> "App bkg-light"

```

 

Being equipped with this new understanding we'll install it with `yarn add classnames` or `npm install classnames`. Then import it into our component. 

In order to keep our code legible we'll restrain from adding our `Classename` function inline and instead save it to a variable. 



![](https://im3.ezgif.com/tmp/ezgif-3-49184eef6d1c.png)



And now lets test it out .......  Hooray !!!!! It Works Perfectly



![](https://media.giphy.com/media/l4JySAWfMaY7w88sU/giphy.gif)








