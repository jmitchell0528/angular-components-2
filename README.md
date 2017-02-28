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

### 2. An Edit Profile Component
Let's add to our app with an edit profile component. We'll pass a user object and two functions to our component so that we can edit the user properties. 

#### 2.1 Create the Component
Create the profile component directory and files:
```
--js
  |--components
    |--profileComponent
      |--profileComponent.js
      |--profileComponent.html
```

Set up the component in `profileComponent.js`:
```
angular.module("componentApp").component("profileComponent", {
    templateUrl: "./js/components/profileComponent/profileComponent.html"
})
```
Now set up the template in `profileComponent.html`:
```
<form>
    <label for="username">Change Username:</label>
    <input type="text" id="username" placeholder="New username">
    <input type="submit" value="Submit username">
</form>
<form>
    <label for="email">Change Email:</label>
    <input type="text" id="email" placeholder="New email">
    <input type="submit" value="Submit email">
</form>
```
Finally, insert the component in your `index.html`:
```
<div ng-app="componentApp">
    <div ng-controller="mainCtrl">
        <!--Let's insert our components here-->
        <nav-component></nav-component>
        <profile-component></profile-component>
    </div>
</div>
```
And don't forget the script tag:
```
<!--App JS Files-->
<script src="js/app.js"></script>
<script src="js/mainCtrl.js"></script>
<script src="js/components/navComponent/navComponent.js"></script>
<script src="js/components/profileComponent/profileComponent.js"></script>
```

#### 2.2 Pass the Data
Now let's create a user object to pass to the component.
Add a user object to `mainCtrl.js`:
```
angular.module("componentApp").controller("mainCtrl", function($scope) {

  $scope.user = {
      name: "You",
      email: "you@you.com"
  }  

})
```
Then pass the data to the component in `index.html`:
```
<profile-component
    user="user"
>
</profile-component>
```
Now let's bind the data to the component in `profileComponent.js`:
```
angular.module("componentApp").component("profileComponent", {
    templateUrl: "./js/components/profileComponent/profileComponent.html",
    bindings: {
        user: "=",
})
```
And, finally, pass the data to view in `profileComponent.html`
Note that in the component view, we have to refer to any property on scope by prefacing it with $ctrl. We could change this name, but $ctrl is the default. 
```
<form>
    <p>Current Username: {{$ctrl.user.name}}</p>
    <label for="username">Change Username:</label>
    <input type="text" id="username" placeholder="New username">
    <input type="submit" value="Submit username">
</form>
```
Now you should see the controller name in the view. 

#### 2.3 Bind Functions to Component
Now let's add some functions to change the user properties on the main controller. (We could, of course, just change these on the component, but we want to practice passing functions, and we'll later learn that this data flow is more standard.)

Create the functions in `mainCtrl.js`:
```
angular.module("componentApp").controller("mainCtrl", function($scope) {

  $scope.user = {
      name: "Brian",
      email: "brian@brian.com"
  }  

  $scope.changeUsername = function(username) {
      $scope.user.name = username;
  }

  $scope.changeUserEmail = function(email) {
      $scope.user.email = email;
  }
})
```
Now pass the functions to the component in `index.html`:
```
<div ng-app="componentApp">
    <div ng-controller="mainCtrl">
        <!--Let's insert our components here-->
        <nav-component></nav-component>
        <profile-component
            user="user"
            change-username="changeUsername(username)"
            change-user-email="changeUserEmail(email)"
        >
        </profile-component>
    </div>
</div>
```
Then bind the functions to the component in `profileComponent.js`:
```
angular.module("componentApp").component("profileComponent", {
    templateUrl: "./js/components/profileComponent/profileComponent.html",
    bindings: {
        user: "=",
        changeUsername: "&",
        changeUserEmail: "&"
    },
})
```
And let's add one of these functions to the view in `profileComponent.html`:
```
<form ng-submit="$ctrl.changeUsername({username: $ctrl.newUsername})">
    <p>Current Username: {{$ctrl.user.name}}</p>
    <label for="username">Change Username:</label>
    <input 
        type="text" 
        id="username" 
        placeholder="New username" 
        ng-model="$ctrl.newUsername"
    >
    <input type="submit" value="Submit username">
</form>
```
In addition to adding the `ng-submit` on the form element, we also needed a `ng-model` on the input element to take in a new username. We'll copy this code and change a few values for the email. 
```
<form ng-submit="$ctrl.changeUserEmail({email: $ctrl.newEmail})">
    <p>Current Email: {{$ctrl.user.email}}</p>
    <label for="email">Change Email:</label>
    <input 
        type="text" 
        id="email" 
        placeholder="New email" 
        ng-model="$ctrl.newEmail"
    >
    <input type="submit" value="Submit email">
</form>
```
Now try running your code and see if you can change the user properties.

### 4. Controllers
Just to introduce you to the new controller syntax, let's clear the inputs with a controller function. 

First, let's add a controller to `profileComponent.js`:
```
angular.module("componentApp").component("profileComponent", {
    templateUrl: "./js/components/profileComponent/profileComponent.html",
    bindings: {
        user: "=",
        changeUsername: "&",
        changeUserEmail: "&"
    },
    controller: function() {
    },
})
```
We don't take in the $scope object on a component controller. Instead, we refer to properties on the controller as `this.property`. 

Now let's add a function to clear the inputs. 
```
angular.module("componentApp").component("profileComponent", {
    templateUrl: "./js/components/profileComponent/profileComponent.html",
    bindings: {
        user: "=",
        changeUsername: "&",
        changeUserEmail: "&"
    },
    controller: function() {
        this.clearInputs = function() {
            this.newUsername = ""
            this.newEmail = ""
        }
    },
})
```
And let's call that function on the view in `profileComponent.html`:
```
<button ng-click="$ctrl.clearInputs()">Clear Inputs</button>
```
We can also append the function to our `ng-submit` directives, too:
```
<form ng-submit="$ctrl.changeUsername({username: $ctrl.newUsername}); $ctrl.clearInputs()">
```
```
<form ng-submit="$ctrl.changeUserEmail({email: $ctrl.newEmail}); $ctrl.clearInputs()">
```

Give it a test and see if it works. 

### 5. Component-Based Architecture

### 6. One-Way Data Bindings

##Resources
https://blog.thoughtram.io/angularjs/2015/01/02/exploring-angular-1.3-bindToController.html

https://scotch.io/tutorials/how-to-use-angular-1-5s-component-method

https://docs.angularjs.org/guide/component