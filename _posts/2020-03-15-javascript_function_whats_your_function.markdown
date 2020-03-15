---
layout: post
title:      "JavaScript, Function, What's Your Function"
date:       2020-03-15 19:52:18 +0000
permalink:  javascript_function_whats_your_function
---


Your making the worlds best program that outlines the optimal path a train must take in order to deliver supplies on time. You break down each step into manageable pieces examining the parts of each task. Then you place these task into a logical order for proper execution. These task created in modular pieces are considered functions. JavaScript functions are blocks of code used to define a specific task. 

Functions are considered first class citizens. This means they are considered value expressions that can be used anywhere in your program with little to no restrictions. 

Another awesome fact is they can be defined in multiple forms. There're named functions, anonymous functions, and IIFE's.



### Named Functions

In the world of named functions there're 2 different known types. Statement and Inferred function name.

The proper syntax to use to define these functions is below.



###### Statement Name function

```javascript
function defineNameOfFunction (paramerters){
	// Add Block Of Code In Here
}
```



###### Inferred function

```javascript
const object = {
  defineNameOfFunction: function(paramerters) {}
}

const defineNameOfFunction = function(paramerters) {}
```



`function` is a special keyword such as (for, in, static, const, ect...) which tells the JavaScript engine your defining a function. Upon its definition its eventually hoisted.

`defineNameOfFunction`  or `object.defineNameOfFunction` assigns a name to the function. This makes it possible to access that function definition anywhere in your code. The function name can be anything you want. The following link outlines naming rules https://www.informit.com/articles/article.aspx?p=131025&seqNum=3

`parameters` this is where you can place your optional arguments. These arguments an be in the form of primitive values or functions themselves. For more parameter rules follow this link https://www.w3schools.com/js/js_function_parameters.asp





###  Anonymous Functions

Anonymous functions are unlike named functions since they are unnamed not using the conventional function declaration. Instead they use the function operator to encapsulate its block of code. Anonymous functions usually are used within callbacks, however they can be seen within Inferred functions.  Now there are a couple ways we can create this type of function expression.  There syntax is as follows.

###### Anonymous function expression 

```javascript
function(){ /* code */ }
```



###### Arrow function

```javascript
() => {}
```



###### Function constructor name

```javascript
new Function(1 + 2); 
```



Interestingly these types of anonymous functions are only declared at runtime and are unable to be executed outside of a variable or callback. However, there is one other type of anonymous function that can be immediately executed without the need anything but itself. Its called the Immediately Invoked Function Expression or IIFE for short. 

Again IIFE is a way to declare a function and have it immediately execute upon runtime. Its very helpful with encapsulating your code which helps isolate your declarations from the global name space. IIFE syntax can be defined as either a normal Anonymous function expression or arrow function wrapped within parenthesis.



 ###### Immediately Invoked Function Expression Syntax

```javascript
(function() {
  /* */
})()


(() => {
  /* */
})()
```



The first enclosing parenthesis that wrap over the function makes the function an expression and the appended invoking parenthesis towards the bottom tells the JavaScript compiler to invoke this anonymous function immediately. However if they were removed it would subsequently create a syntax error. 

The advantage in using IIFE's is to protect against declaring variables in the global scope and to create closures. Many JavaScript libraries protect your variables form the global scope so there are no conflicts with variable names .# Enter your title here

The content of your blog post goes here.
