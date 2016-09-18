---
layout: post
title:  "I Gotta FeeliNg"
date:   2016-09-12 15:30:52 -0400
---

So, you've learned AngularJS, but do you know how to integrate it with Rails?

It's been just over six months since I started the Flatiron School's self-driven Full Stack Web Development program – and what a ride it's been!

When explaining my journey using Learn.co, I've witness my friends as they widened their eyes with both excitement and wonder. How someone with customer-facing, operations, and management experience could dive deep into code seemed to amaze them in ways that I hadn't expected.

Many of them claimed that programming was too hard, too opaque, too elusive, and that they just simply didn't have the mind for it.

Well, I'm not sure if that's so true.

For me, the challenging part of Learn.co wasn't the material itself. Sure, it took plenty of time, effort, and patience to learn new concepts – whether object-oriented programming, databasing, or front-ending – but the features themselves were more like puzzles whose pieces were coded in a logical language that really *could* be learned.

The most difficult part of the climb was putting it all together. Figuring out how to iterate over a hash of arrays, implement Regex for validation, or configure the database weren't so hard, but laying the foundation of a Gem, setting up a Sinatra app, scaffolding in Rails, and integrating JQuery from scratch proved to be my biggest hangups.

Creating a Rails app whose foundation depended on AngularJS was no different. I spent days searching the web for helpful guides, all for naught. Thankfully, when I finally reached out to the staff team at Learn.co, I was directed to Luke Ghenco's fantastically helpful **[four-part series](https://medium.com/@lukeghenco/create-an-angular-js-app-with-a-restful-rails-api-pt-1-58121eb6e4f9#.ozl4ni4lq)**.

Tutorials that instruct how to integrate AngularJS with Rails seem to be few and far in between. So, this blog post is dedicated to adding another that's clear and concise:

**Step 1: Generate the Rails App**

Open up your Terminal and create a new Rails app by typing:

```
$ rails new [app_name] --skip-javascript
```

**Step 2: Build the Model**

In Terminal:

```
$ rails g model [Model_name] [attribute]:[attribute_type]
```

**Step 3: Initialize the Database**

```
$ rails db:create

$ rails db:migrate
```

**Step 4: Set up the Routes**

Replace with the following:

```
# config/routes.rb

Rails.application.routes.draw do
  namespace :api, defaults:{format: :json} do
    namespace :v1 do
      resources :[model_name_pluralized]
    end
  end
end
```

**Step 5: Generate the Controller**

In the Terminal:

```
$ mkdir -p app/controllers/api/v1

$ touch app/controllers/api/v1/[model_name_pluralized]_controller.rb
```

Add the following:

```
# app/controllers/api/v1/[model_name_pluralized]_controller.rb

module Api
  module V1 
    class [Model_name_pluralized]Controller < ApplicationController 
      skip_before_filter :verify_authenticity_token 
      respond_to :json 
			
      def index 
        respond_with([Model_name].all.order("id DESC"))
      end 
			
      def show 
        respond_with([Model_name].find(params[:id]))
      end 
			
      def create 
        @[model_name] = [Model_name].new([model_name]_params) 
        if @[model_name].save 
          respond_to do |format|
            format.json { render json: @[model_name] }
          end 
        end 
      end 
			
      def update 
        @[model_name] = [Model_name].find(params[:id])
        if @[model_name].update([model_name]_params) 
          respond_to do |format| 
            format.json { render json: @[model_name] }
          end 
        end 
      end 
 
      def destroy 
        respond_with [Model_name].destroy(params[:id]) 
      end 
			
      private
			
      def [model_name]_params 
        params.require(:[model_name]).permit(:[attribute1], :[attribute2]) 
      end 
			
    end 
  end
end
```

**Step 6: Add 'Responders' to Gemfile**

Add the following line:

```
# Gemfile

gem 'responders'
```

In the Terminal:

```
$ bundle install
```

**Step 7: Add 'Bower' to Gemfile**

Add the following line:

```
# Gemfile

gem 'bower-rails'
```

In the Terminal:

```
$ bundle install
```

**Step 8: Initialize 'Bower'**

```
$ rails g bower_rails:initialize json
```

**Step 9: Add AngularJS Dependencies to 'Bower'**

```
# bower.json

{
  "lib": {
    "name": "bower-rails generated lib assets",
    "dependencies": {
      "angular": "v1.5.3",
      "angular-ui-router": "latest",
      "angular-resource": "v1.5.3"
    }
  },
  "vendor": {
    "name": "bower-rails generated vendor assets",
    "dependencies": {
    }
  }
}
```

In the Terminal:

```
$ rails bower:install
```

**Step 10: Create JavaScript Manifest and Add Requirements**

In the Terminal:

```
$ mkdir app/assets/javascripts

$ touch app/assets/javascripts/application.js
```

Add the following:

```
# app/assets/javascripts/application.js

//= require angular
//= require angular-ui-router
//= require angular-resource
//= require_tree .
```

**Step 11: Add JavaScript to Application Layout**

Replace the `<head>` section with the following:

```
# app/views/layouts/application.html.erb

<head>
  <title>[Website Title]</title>
  <%= csrf_meta_tags %>
  <%= javascript_include_tag 'application' %>
  <%= stylesheet_link_tag    'application', media: 'all' %>
</head>
```

**Step 12: Set the Root Page**

Above the namespacing, but below `Rails.application.routes.draw do`, add the following:

```
# config/routes.rb
 
root 'application#index'

```

**Step 13: Add the 'index' Action to ApplicationController**

Replace with the following:

```
# app/controllers/application_controller.rb

class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception

  def index
  end
  
end
```

**Step 14: Create the 'index' Template**


In the Terminal:

```

$ mkdir app/views/application

$ touch app/views/application/index.html.erb
```

**Step 15: Initialize the Angular Module**

Replace the `<body>` tag in your application's layout with the following:

```
# app/views/layouts/application.html.erb

<body ng-app="app">
```

**Step 16: Create the Angular Module and Inject 'ui.router' and 'ngResource' Dependencies**

In the Terminal:

```
$ mkdir -p app/assets/javascripts/angular

$ touch app/assets/javascripts/angular/app.js
```

Add the following:

```
# app/assets/javascripts/angular/app.js

angular
  .module('app', ['ui.router', 'ngResource']) 
```

**Step 17: Build the $resource Model**

In the Terminal:

```
$ mkdir -p app/assets/javascripts/angular/models

$ touch app/assets/javascripts/angular/models/[model_name].js
```

Make the factory by adding the following code:

```
# app/assets/javascripts/angular/models/[model_name].js

function [Model_name]($resource) {
  var [Model_name] = $resource('http://localhost:3000/api/v1/[model_name_pluralized]/:id.json', {id: '@id'}, {
    update: {method: 'PUT'}
  });
  return [Model_name];
};

angular
  .module('app')
  .factory('[Model_name]', [Model_name]);
```

**Step 18: Add 'ui-view' to 'index' Template**

Add the following:

```
# app/views/application/index.html.erb

<div class="angular-view-container" ui-view></div>
```

**Step 19: Configure the Routes**

Below `.module`'s line of code, add:

```
# app/assets/javascripts/angular/app/js

.config(function($stateProvider, $urlRouterProvider) {
   $stateProvider
     .state('home', {
       url: '/',
       templateUrl: 'home.html',
       controller: 'HomeController as ctrl'
     })
     .state('home.new', {
       url: 'new',
       templateUrl: 'home/new.html',
       controller: 'New[Model_name]Controller as ctrl'
     })
    .state('home.[model_name_pluralized]', {
       url: '[model_name_pluralized]',
       templateUrl: 'home/[model_name_pluralized].html',
       controller: '[Model_name_pluralized]Controller as ctrl'
     })
     .state('home.[model_name]', {
       url: '[model_name]/:id',
       templateUrl: 'home/show.html',
       controller: 'View[Model_name_pluralized]Controller as ctrl'
     })
    .state('home.edit', {
     url: 'edit/:id',
     templateUrl: 'home/edit.html',
     controller: 'Edit[Model_name]Controller as ctrl'
     });
  $urlRouterProvider.otherwise('/');
});
```

**Step 20: Configure Angular Templates**

Add the following line:

```
# Gemfile

gem 'angular-rails-templates'
```

In the Terminal:

```
$ bundle install
```

Add the following line:

```
# app/assets/application.js

//= require angular-rails-templates
```

Inject `'templates'` into the Angular module:

```
# app/assets/javascripts/angular/app.js

.module('app', ['ui.router', 'ngResource', 'templates'])
```

In the Terminal:

```
$ mkdir app/assets/javascripts/templates
```

**Step 21: Create the Angular 'home' Template**

Create a directory to store 'home''s nested templates by typing the following into the Terminal:

```
$ mkdir app/assets/javascripts/templates/home
```

Create the 'home' template by typing the following into the Terminal:

```
$ touch app/assets/javascripts/templates/home.html
```

Add the following:

```
# app/assets/javascripts/templates/home.html

<div class="navbar">
  <h1>[Website Title]</h1>
  <hr>
  <br>

  <a href="#" ui-sref="home.main" ui-sref-active="active">Home</a>
  <a href="#" ui-sref="home.new" ui-sref-active="active">New</a>
  <a href="#" ui-sref="home.[model_name_pluralized]" ui-sref-active="active">[Model_name_pluralized]</a>
</div>

<br>
<hr>

<div ui-view></div>
```

Create the HomeController by typing the following into the Terminal:

```
$ mkdir app/assets/javascripts/angular/controllers

$ touch app/assets/javascripts/angular/controllers/HomeController.js
```

Add the following:

```
# app/assets/javascripts/angular/controllers/HomeController.js

function HomeController() {

}

angular
  .module('app')
  .controller('HomeController', HomeController);
```

**Step 22: Create the Angular '[model_name_pluralized]' Template**

Create the '[model_name_pluralized]' template by typing the following into the Terminal:

```
$ touch app/assets/javascripts/templates/home/[model_name_pluralized].html
```

Add the following heading and form to the page:

```
# app/assets/javascripts/templates/home/[model_name_pluralized].html

<h1>[Model_name_pluralized]</h1>

<div ng-repeat="[model_name] in ctrl.[model_name_pluralized]">{% raw %}
  <br>{{[model_name].[attribute1]}}
  <br>{{[model_name].[attribute2]}}{% endraw %}

  <a href="#" ui-sref="home.[model_name]({id: [model_name].id})">View [Model_name]</a>
  <a href="#" ui-sref="home.edit({id: [model_name].id})">Edit [Model_name]</a>
  <a href ng-click="ctrl.delete[Model_name]([model_name])">Delete [Model_name]</a>
</div> 
```

Create the [Model_name_pluralized]Controller by typing the following into the Terminal:

```
$ touch app/assets/javascripts/angular/controllers/[Model_name_pluralized]Controller.js
```

Add the following:

```
# app/assets/javascripts/angular/controllers/[Model_name_pluralized]Controller.js

function [Model_name_pluralized]Controller([Model_name]) {

  var ctrl = this;
  ctrl.[model_name_pluralized] = [Model_name].query();

};

angular
  .module('app')
  .controller('[Model_name_pluralized]Controller', [Model_name_pluralized]Controller);
```

**Step 23: Create the Angular 'new' Template**

Create the 'new' template by typing the following into the Terminal:

```
$ touch app/assets/javascripts/templates/home/new.html
```

Add the following heading and form to the page:

```
# app/assets/javascripts/templates/home/new.html

<h1>Add a [model_name]</h1>

<form validate ng-submit="ctrl.add[Model_name]()">
  <label for="[attribute1]">[Attribute1]</label>
  <input type="text" ng-model="ctrl.[model_name].[attribute1]" required>

  <label for="[attribute2]">[Attribute2]</label>
  <input type="text" ng-model="ctrl.[model_name].[attribute2]" required>

  <input type="submit" value="Add [Model_name]">
</form>
```

Create the New[Model_name]Controller by typing the following into the Terminal:

```
$ touch app/assets/javascripts/angular/controllers/New[Model_name]Controller.js
```

Add the following:

```
# app/assets/javascripts/angular/controllers/New[Model_name]Controller.js

function New[Model_name]Controller([Model_name], $location) {

  var ctrl = this;
  ctrl.[model_name] = new [Model_name]();

  ctrl.add[Model_name] = function() {
    ctrl.[model_name].$save(function() {
      $location.path('[model_name_pluralized]');
    });
  };
};

angular
  .module('app')
  .controller('New[Model_name]Controller', New[Model_name]Controller);
```

**Step 24: Create the Angular 'show' Template**

Create the 'show' template by typing the following into the Terminal:

```
$ touch app/assets/javascripts/templates/home/show.html
```

Add the following:

```
# app/assets/javascripts/templates/home/show.html
{% raw %}
<h3>{{ctrl.[model_name].[attribute1]}}</h3>
<p>{{ctrl.[model_name].[attribute2]}}</p>{% endraw %}

<a href="#" ui-sref="home.edit({id: ctrl.[model_name].id})">Edit [Model_name]</a>

```

Create the View[Model_name]Controller by typing the following into the Terminal:

```
$ touch app/assets/javascripts/angular/controllers/View[Model_name]Controller.js
```

Add the following:

```
# app/assets/javascripts/angular/controllers/View[Model_name]Controller.js

function View[Model_name]Controller([Model_name], $stateParams) {
  var ctrl = this;
  ctrl.[model_name] = [Model_name].get({id: $stateParams.id});
}

angular
  .module('app')
  .controller('View[Model_name]Controller', View[Model_name]Controller);
```

**Step 25: Create the Angular 'edit' Template**

Create the 'edit' template by typing the following into the Terminal:

```
$ touch app/assets/javascripts/templates/home/edit.html
```

Add the following heading and form to the page:

```
# app/assets/javascripts/templates/home/edit.html

<h1>Edit [model_name]</h1>

<form validate ng-submit="ctrl.edit[Model_name]()">
  <label for="[attribute1]">[Attribute1]</label>
  <input type="text" ng-model="ctrl.[model_name].[attribute1]" required>

  <label for="[attribute2]">[Attribute2]</label>
  <input type="text" ng-model="ctrl.[model_name].[attribute2]" required>

  <input type="submit" value="Edit [Model_name]">
</form>
```

Create the Edit[Model_name]Controller by typing the following into the Terminal:

```
$ touch app/assets/javascripts/angular/controllers/Edit[Model_name]Controller.js
```

Add the following:

```
# app/assets/javascripts/angular/controllers/Edit[Model_name]Controller.js

function Edit[Model_name]Controller([Model_name], $location, $stateParams) {
  var ctrl = this;
  ctrl.[model_name] = [Model_name].get({id: $stateParams.id});

  ctrl.edit[Model_name] = function() {
    ctrl.[model_name].$update(function() {
      $location.path('[model_name_pluralized]');
    });
  };
};

angular
  .module('app')
  .controller('Edit[Model_name]Controller', Edit[Model_name]Controller);
```

**Step 26: Add the Delete Function**

Add the following function to the [Model_name_pluralized]Controller:

```
# app/assets/javascripts/angular/controllers/[Model_name_pluralized]Controller.js

ctrl.delete[Model_name] = function([model_name]) {
  [model_name].$delete(function() {
    $state.reload();
  });
};
```



