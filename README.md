#Components in Angular 1.5 & 1.6
##Intro
This lesson will introduce components in Angular 1.5 and 1.6. We will just begin to explore components by translating what we know about directives into component syntax. 

After you get used to creating components, begin thinking about how to organize an entire app as a tree of components.

Finally, to move from Angular 1 to Angular 2 or to React, you'll need to think about moving from two-way data binding to one-way data binding.  

##Instructions
We'll build components together, so I've just given you the file structure on the master branch. That way you don't have to worry about file structure, but you still get the practice of creating the module, controllers, and components. 

If you want to look at finished examples, I'll try to publish them on the branches of this repository. 

### 1. A Nav Template Component
Let’s begin by just wrapping some standard html in a component. We’ll start with a nav. 

#### 1.1 Hook up the Angular App
The script tags should already be in the index.html, and the app is bootstrapped, so now you just need to fill out the angular files. 

Declare your app in `app.js`: 
`angular.module("componentApp", [])`

Create your controller in `mainCtrl.js`: 
```
angular.module("componentApp").controller("mainCtrl", function($scope) {
    
})
```

#### 1.2 Create a Component

Create a component in `navComponent.js`:
```
angular.module("componentApp").component("navComponent", {
    templateUrl: "./js/components/navComponent/navComponent.html"   
})
```
*For now, we’re just giving it a templateUrl. We’ll add more later.*

Create a template for the component (feel free to be cleverer than I am): 
```
<nav>
    <ul>
        <li>One</li>
        <li>Two</li>
        <li>Three</li>
        <li>Four</li>
    </ul>
</nav>
```

#### 1.3 Insert the Component
Now place the component in your `index.html`: 
```      
<div ng-app="componentApp">
    <div ng-controller="mainCtrl">
        <!--Let's insert our components here-->
        <nav-component></nav-component>              
    </div>
</div>
```

#### 1.4 Check Your Results
Spin up your server and see your component.

##Resources
https://blog.thoughtram.io/angularjs/2015/01/02/exploring-angular-1.3-bindToController.html

https://scotch.io/tutorials/how-to-use-angular-1-5s-component-method

https://docs.angularjs.org/guide/component