---
layout: post
title:      "Two Major Life-Cycle Events - (Classes vs Functional) "
date:       2020-05-07 00:57:27 -0400
permalink:  classes_and_functional_component_life-cycle-events
---




Ideally all rendering is controlled via state and props, but sometimes there is need to control the flow over how and when components update. These methods should be used sparingly, and with care.

A react component has many stages in its life-cycle, however we'll only focus on the first 2 stages. Mounting (aka Birth), Update (aka Growth) . Let’s take a look at these life-cycle methods and see how they differ between classes and functional components. 





![](https://i2.wp.com/programmingwithmosh.com/wp-content/uploads/2018/10/Screen-Shot-2018-10-31-at-1.44.28-PM.png?resize=1024%2C567&ssl=1)



### Birth Stage - (mounting)



##### Component Did Mount  &  Use Effect

*Class Component*

componentDidMount() is called as soon as the component is mounted. Caution no state changes should happen in this method since this could create an infinite loop. It allow  us to do all kinds of advanced interactions like animations related to elements within the DOM itself.  Typically this is were we perform API calls to load data from an remote endpoint.

Basically, here you want to do all the setup you couldn’t do without a DOM, and start getting all the data you need. Typically this is where AJAX calls are made and other initialization methods.



```react
import React, { Component } from "react";
 
class Component extends Component {
  componentDidMount() {
    console.log("Code Executed immediately after Component Mounts");
  }
 
  render() {
    return <h1>Hello World</h1>;
  }
};
```





*Functional Component*

useEffect()

For imitating the functionality of componentDidMount, we need to pass an empty array(**[]**). When you pass an empty array as 2nd argument, the useEffect function will only get invoked once.

When does useEffect invoke?
The function passed to the useEffect gets fired after layout and paint, during a deferred event and asynchronously



```react
import React, { useEffect } from "react";
 
const Component = () => {
  useEffect(() => {
    console.log("Code Executed Just Before Component Mounts");
  },[]);
 
  return <h1>Hello World</h1>;
};
```







### Growth Stage - (updating)

When a component receives new props, or new state, it should update. However there are times we would like to control when these changes occur and only update if the props you care about change.. Therefore we can improve the performance of our components by not creating unnecessary  re-renders. While this is great news caution should be demonstrated with this method since React will not update this component normally. Remember it only exists for certain performance optimizations.



##### shouldComponentUpdate & React.memo 



*Class Component*

In the shouldComponentUpdate method our component first ask for permission whether to re-rendering. Its  called with nextProps as the first argument, and nextState is the second, and expects a boolean return value.  If false, component, will not re-render, however the default is always true. Word of caution you cannot update component state in *shouldComponentUpdate()* lifecycle.



```react
ShouldComponentUpdate(nextProps, nextState){
   return this.props.id !== nextProps.id || this.state.input !== nextState.input
}
```





*Functional Component*

Functional components relay on memoization to decide wither to re-render a component. Memorization is basically an optimization technique which passes a complex function to be memoized or remembered. In memoization, the result is remembered, when the same exact parameters are passed-in subsequently. 

React memo wraps around the component and provides a shallow comparison to see if it should re-render. When we add a second parameter we now have access to the previous and next props, and within that function we can return a true or false value to determine if the component should render. 



```react
const APPComponent = React.memo(() => (props) {
    
}, (prevProps, nextProps) => {
    return prevProps.count !== nextProps.count
});
```



Lastly, if we decide that the component should re-render based on component state we have the option to use a react hook called *UseMemo()*.  It accepts two parameters, the first is an anonymous function that will only return a memorized value based on the second parameter values in its array



```react
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);

const memoizedComponent = useMemo(() => <childComponent/>, [a]);
```


