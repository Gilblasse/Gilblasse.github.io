---
layout: post
title:      "Rails Art of Abstraction"
date:       2020-02-17 04:09:46 +0000
permalink:  rails_art_of_abstraction
---

Rails builds on the idea of simplicity. This concept allows for dynamic websites, clean code, and reusable functionality. The architecture behind this idea is called MVC which stands for Model View Controller.



### Quick Look Into MVC 

The architecture behind a model view controller spans beyond rails. There multiple other frameworks that utilize this concept as well. The Model  represents the real world logic about an object. It encapsulates related attributes storing and reading information about that object from the database. The view is where the users interact with the application. It displays data that's provided by the controller and supplies ways for the user to modify the data when necessary. The controller is the liaison between the view and the model. It handles url request by the user and retrieves data from models and responds with the appropriate views.





![](https://miro.medium.com/max/1080/0*Qf1s2lG86MjX-Zcv.jpg)





### Helpers , Partials and Concerns

While this solves the need of separation of concerns, we can still fall victim to repeated code. This is where rails introduces Helpers, Partials and Concerns. This fellows the Dry principle which permits our code to be manageable and easily distributed throughout our application. Before helpers, partials and concerns, developers had to write the same logic in multiple views, which made debugging a nightmare. It would take endless hours to adjust one bug. However, since rails added theses features to the framework we're no longer afraid of bugs in our code. Our logic is separated by concerns and we can easily work in one place, modify our code and see our changes throughout our application. Again this makes it a convenient way to reuse certain fragments of code in other areas of our app. 

A great example to demo this idea would be form. Forms can be used to gather data and modify it as well. This means we would need a view to show a form and another view to edit a form. Now we can create 2 forms but that not adhering to the Dry concept. Instead it's best if we created one form and utilize that form partial in our view instead. If for some reason we need to modify the form we can easily access that one partial and our changes would naturally persist to the other views.

**Form Partial:**

```erb
<%= form_for @modle do |f| %>
    <%= f.text_field :name %>
    <%= f.text_field :age %>
    <%= f.submit %>
<%= end %>
```



**New Page:**

```erb
<%= render 'form' %>
```



**Edit Page** 

```erb
<%= render 'form' %>
```



The question now is when do we use Helpers vs Partials vs Concerns. Does rails have a standard when it comes to using these to maintain or code logic. 

The answer is Yes. The overall idea is to keep things simple. Rails prefers to extract complex logic out of the the views. Since that only creates more convolution especially during debugging.  Instead rails prefers us to adhere to these rules.



**Helpers** 

Helpers are ruby code used to construct logic for our views. We can use complex case statements, if else , and methods to design the proper flow for our templates. Doing this makes our views more dynamic, maintainable, and easily reusable throughout our views.



**Partials**: 

Partials are similar to helpers since they are both used to primarily to construct the views.  However it deferrers slightly since it mainly uses html code instead of heavy ruby logic.  **Note**: if you notice your partial clutter with too much embedded ruby it is time to transfer some of your logic to the helpers.



**Concerns**: 

Concerns clears out redundancy between models and controllers.  Mainly it supports logic that are similar across  multiple models. An example would be if we had two models Singer and  Manager. For some reason you wanted to create a salary method for our models. Keeping with the DRY principle it would be best to add this method within a concerns folder and share it between the our models.



### Conclusion

The secret to Rails simplicity is its ability to adhere to certain programing design principles like, separation of concerns, MVC, and abstractions. With this rails provides tools to reduce development time, provides ease of maintainability / extendibility, and helps create an environment to produce robust applications.
