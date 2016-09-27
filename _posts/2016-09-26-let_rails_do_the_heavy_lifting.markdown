---
layout: post
title:  "Let Rails do the Heavy LiftiNg"
date:   2016-09-26 18:37:57 -0400
---


In my [last post](http://zachnewburgh.github.io/2016/09/12/i_gotta_feeling/), I provided an easy-to-follow guide for adding AngularJS to your Rails application!

Although it can be done in only 26 steps, it's not an simple process. So much of it, though, is abbreviated by the use of [$resource](https://docs.angularjs.org/api/ngResource/service/$resource).

Let's say, though, that you wanted to use [$http](https://docs.angularjs.org/api/ng/service/$http). How would you alter the configuration?

**Step 1: Create a 'Services' Directory**

In the Terminal:

```
$ mkdir app/assets/javascripts/angular/services
```

**Step 2: Change your Factory into a Service**

In the Terminal:

```
$ mv app/assets/javascripts/angular/models/[model_name].js app/assets/javascripts/angular/services/[Model_name]Service.js
```

Replace the contents with the following:

```
# app/assets/javascripts/angular/services/[Model_name]Service.js

function [Model_name]Service($http) {

  this.get[Model_name_pluralized] = function() {
    return $http.get('http://localhost:3000/api/v1/[model_name_pluralized]');
  };

  this.get[Model_name] = function(id) {
    return $http.get('http://localhost:3000/api/v1/[model_name_pluralized]/' + id);
  };

  this.create[Model_name] = function(data) {
    $http.post('http://localhost:3000/api/v1/[model_name_pluralized]', data);
  };

  this.update[Model_name] = function(id, data) {
    $http.put('http://localhost:3000/api/v1/[model_name_pluralized]/' + id, data);
  };

  this.delete[Model_name] = function(id) {
    return $http.delete('http://localhost:3000/api/v1/[model_name_pluralized]/' + id);
  };
}

angular
  .module('app')
  .service('[Model_name]Service', [Model_name]Service);
```

**Step 3: Replace your .query() call in [Model_name_pluralized]Controller with [Model_name]Service's $http Functions**

Inject [Model_name]Service:

```
# app/assets/javascripts/angular/controllers/[Model_name_pluralized]Controller.js

function [Model_name_pluralized]Controller([Model_name]Service, $location, $state) {
```

Replace `ctrl.[model_name_pluralized] = [Model_name].query();` with:

```
# app/assets/javascripts/angular/controllers/[Model_name_pluralized]Controller.js

[Model_name]Service.get[()
    .then(function(response) {
      ctrl.[model_name_pluralized] = response.data;
    });

ctrl.delete[Model_name] = function([model_name]) {
    [Model_name]Service.delete[Model_name]([model_name].id);
    $state.reload();
  };
```

**Step 4: Replace your 'new' Instance and $save call in New[Model_name]Controller with [Model_name]Service's $http Function**

Inject [Model_name]Service:

```
# app/assets/javascripts/angular/controllers/New[Model_name]Controller.js

function New[Model_name]Controller([Model_name]Service, $location) {
```

Replace this:

```
# app/assets/javascripts/angular/controllers/New[Model_name]Controller.js

ctrl.[model_name] = new [Model_name]();

ctrl.add[Model_name] = function() {
  ctrl.[model_name].$save(function() {
    $location.path('[model_name_pluralized]');
  });
};
```

with this:

```
# app/assets/javascripts/angular/controllers/New[Model_name]Controller.js

ctrl.add[Model_name] = function() {
  var data = {[attribute1]: this.[model_name].[attribute1], [attribute2]: this.[model_name].[attribute2]};
  [Model_name]Service.create[Model_name](data);
  $location.path('[model_name_pluralized]');
};
```

**Step 5: Replace your .get() call in View[Model_name]Controller with [Model_name]Service's $http Function**

Inject [Model_name]Service:

```
# app/assets/javascripts/angular/controllers/View[Model_name]Controller.js

function View[Model_name]Controller([Model_name]Service, $stateParams) {
```

Replace `ctrl.[model_name] = [Model_name].get({id: $stateParams.id});` with this:

```
# app/assets/javascripts/angular/controllers/View[Model_name]Controller.js

[Model_name]Service.get[Model_name]($stateParams.id)
    .then(function(response) {
      ctrl.[model_name] = response.data;
    });
```

**Step 6: Replace your .get() and $update calls in Edit[Model_name]Controller with [Model_name]Service's $http Functions**

Inject [Model_name]Service:

```
# app/assets/javascripts/angular/controllers/Edit[Model_name]Controller.js

function Edit[Model_name]Controller([Model_name]Service, $location, $stateParams) {
```

Replace 

```
# app/assets/javascripts/angular/controllers/Edit[Model_name]Controller.js

ctrl.[model_name] = [Model_name].get({id: $stateParams.id});

ctrl.edit[Model_name] = function() {
    ctrl.[model_name].$update(function() {
      $location.path('[model_name_pluralized]');
    });
};
``` 

with this:

```
# app/assets/javascripts/angular/controllers/Edit[Model_name]Controller.js

[Model_name]Service.get[Model_name]($stateParams.id)
  .then(function(response) {
    ctrl.[model_name] = response.data;
  });

ctrl.edit[Model_name] = function() {
  var data = {[attribute1]: this.[model_name].[attribute1], [attribute2]: this.[model_name].[attribute2]}
  [Model_name]Service.update[Model_name](ctrl.[model_name].id, data);
  $location.path('[model_name_pluralized]');
};
```
