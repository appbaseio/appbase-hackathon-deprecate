# ngAppbase Guide

Appbase provides a realtime graph backend, is designed to write complex applications completely clientside. Appbase fits perfectly as a backend in your AngularJS application.

ngAppbase - AngularJS binding for Appbase, makes it very simple to bind your data between AngularJS and Appbase backend.

[Here](http://appbase.io/tutorial.html)'s a quick tutorial for Appbase JS api.

## Integrating the awesome
Add these script tags in your HTML:
```html
<script src="https://cdn.appbase.io/2.0/appbase.js"></script>
<script src="https://cdn.appbase.io/2.0/ng-appbase-ref.js"></script>
```
Register `ngAppbase` as a dependency in the module, and the`$ngAppbaseRef` is available to be injected into any controller, service, or factory.
```js
var app = angular.module("myApp", ["ngAppbase"]);
app.controller("myCtrl", function($scope, $ngAppbaseRef) {
   ...
});
```
## Binding Objects
As Angular binds the data between JS models and the DOM, ngAppbase propagates any changes the data from Appbase . This way, any changes to Appbase automatically appears in the DOM.
Notice that, the changes in the JS model are *__not__* automatically sent to Appbase backend.

`$bindProperties` creates such a synchronized object, by binding properties of a vertex in Appbase, to a variable in JS.

### Example:
Assume the properties of the vertex at __'user/bella'__ : 
```json
{
	"name": "Isabella",
	"having": "Cashasa"
}
```
To bind this data to your view,
JS:
```js
app.controller("myCtrl", function($scope, $ngAppbaseRef) {
	//binds data at 'user/bella' with the scope variable 'user'
	$scope.user = $ngAppbaseRef('user/bella').$bindProperties();
});
```
HTML:
```html
<div ng-controller="myCtrl">
<p> {{user.name}} is having {{user.having}} <p>
</div>
```

As Appbase notifies for data changes in realtime, if the data at __'user/bella'__ changes, i.e. if `Appbase.ns('user').v('bella').setData({having: 'Cerveja'})` is called, this change will be reflected in the scope variable instantly, and Angular will update the view.

## Binding Arrays
`ng-repeat` in AngularJS binds a list in DOM to an array model in JS, and this array can be bound to the _edges_ of a vertex in Appbase, using `$bindEdges`.

When edges are added, removed, replaced, or the priority is changed, these changes appear in the DOM in realtime.

### Example
Data in the out vertex pointed by the edge:
```json
{
	message: 'Hello Pizza!'
}
```
To bind this data into you view:
JS:
```js
app.controller("myCtrl", function($scope, $ngAppbaseRef) {
	//binds edges of 'user/bella/tweets' with the scope variable 'tweets'
	$scope.tweets = $ngAppbaseRef('user/bella/tweets').$bindedges();
});
```

HTML:
```html
<div ng-controller="myCtrl">
	<ul>
		<li ng-repeat="tweet in tweets">{{tweet.properties.message}}</li>
	</ul>
</div>
```

## Modifying data
While `$ngAppbaseRef` provides powerful ways to bind your data, the Appbase javascript API can still be used in your code to modify data. 
```javascript
Appbase.ns('user').v('bella').setData({having: 'Cerveja'});
```
The  documentation for Appbase JS api is [here](/docs/js.md).  
## Next Steps

Checkout additional documentation for `$bindEdges()` and `$bindProperties()` [here](/docs/angular_advanced.md).

Checkout this opensource [Twitter clone](http://twitter.appbase.io/) we built using this Angular binding in under ~250 lines of javascript code.

Have a great time building awesome realtime applications!