# vsscrollbar
**Virtual scroll with filtering and custom scrollbar - AngularJS UI component**

## Description
AngularJS directive which implements the virtual scroll, filtering of the items and the customizable scrollbar

### 1. virtual scroll
* good performance even millions of the items in the list
* only visible items are rendered in the browser

### 2. filtering
* if scrollbar items are array of strings - filtering by string value
* if scrollbar items are array of objects - filtering by string value with multiple properties

### 3. customizable scrollbar
* custom scrollbar is used instead of native scrollbar of the browser
* scrollbar can be easily customised using the CSS
* scrollbar is looking similar in all browsers

## Usage

* add the **vsscrollbar-0.1.0.min.js** and the **vsscrollbar-0.1.0.min.css** files into your project. The files can be found from the **dist/min/** folder of this project.
```html
<script src="vsscrollbar-0.1.0.min.js"></script>
<link href="vsscrollbar-0.1.0.min.css" rel="stylesheet" type="text/css">
```
* inject the **vsscrollbar** module into your application module.
```js
angular.module('vsscrollbarapp', ['vsscrollbar']);
```
* add **vsscrollbar** HTML tag into your HTML file. See the **HTML example** paragraph below.
* add needed Javascript code. See the **Javascript example** paragraph below.

### HTML example
```html
<vsscrollbar items="allItems" items-in-page="5"
             on-scroll-change-fn="onScrollChange(topIndex, maxIndex, topPos, maxPos, 
                                  filteredPageCount, filteredItemCount, visibleItems)">
    <!- parent implements this part - begin -->                              
    <div id="item" ng-repeat="item in visibleItems track by $index" ng-click="itemClicked($index, item);">
        <div id="itemtext">{{item}}</div>
    </div>
    <!- parent implements this part - end -->
</vsscrollbar>
```

It is also possible to use **ng-model="response"** instead of **on-scroll-change-fn** callback to get response of the scrollbar directive.

### Tags
| Tag  | Description | Mandatory | 
| :------------ |:---------------|:---------------:|
| vsscrollbar | scrollbar tag | yes | 
| div inside the vsscrollbar | parent is responsible to implement this part | yes | 


### Attributes
| Attribute | Description | Mandatory | 
| :------------ |:---------------|:---------------:|
| items | items passed to the vsscrollbar - parent can change content of the items variable by using service methods | yes |
| items-in-page | visible item count in the vsscrollbar | yes |
| height | height of the scrollbar | no |
| on-scroll-change-fn | callback function which is called by the vsscrollbar when the scroll change occurs | yes if *ng-model* is not used |
| ng-model | updated by the vsscrollbar when scroll change occurs | yes if *non-scroll-change-fn* is not used |


### Javascript example
```js
var sampleapp = angular.module('vssampleapp', ['vsscrollbar']);
sampleapp.controller('vsScrollbarCtrl', function ($scope, vsscrollbarEvent, vsscrollbarConfig) {

$scope.visibleItems = [];

// Scroll change callback - **visibleItems** are shown in the scrollbar using the **ng-repeat**. See above.
$scope.onScrollChange = function (topIndex, maxIndex, topPos, maxPos, 
                                  filteredPageCount, filteredItemCount, visibleItems) {
    ...
    $scope.visibleItems = visibleItems;
};
```

By injecting the **vsscrollbarEvent** the parent can send events to the vsscrollbar component by calling service functions. 

For example add item *My item* to index *2*.

```js
vsscrollbarEvent.addItem($scope, 2, 'My item');
```

| Function | Parameters | Description | 
| :------------ |:---------------|:---------------|
| setIndex | $scope, index | Set the vsscrollbar index to this position. Moves the scroll if needed. |
| setPosition | $scope, position | Set the vsscrollbar position. Moves the scroll if needed. |
| filter | $scope, filterStr | Filter items from the vsscrollbar by the given string. See below **Filtering items**|
| addItem | $scope, index, item | Adds item to the position of the index. Value of the item is **string** or **JSON object** depending of the types of the items in the list. |
| updateItem | $scope, index, item | Replaces item in the position of the index. Value of the item is **string** or **JSON object** depending of the types of the items in the list. |
| deleteItem | $scope, index | Deletes item from the position of the index. |


#### Filtering items
1) Items are array of strings
```js
/*
['Item #20', 'Item #21', 'Item #30', 'Item #40']
*/

vsscrollbarEvent.filter($scope, '2');
```
Result of the above filter command is array of items. Two items contain the string **2**.

2) Items are array of objects
```js
/*
[{
    "id":0,
    "code":"EI",
    "name":"ABC"
},
{
    "id":1,
    "code":"DD",
    "name":"DEF"
}]
*/

vsscrollbarEvent.filter($scope, 'e');
```
Result of the above filter command is array of objects. Two objects contain the string **e**.


In the **examples** folder of this project has the sample app and the online demo 
is [here](http://embed.plnkr.co/H90QoIRISKqp99vOe8Rn/preview)

## Dependencies
Depends on AngularJS. Implemented using the AngularJS version 1.3.10.

## Compatibility (tested with)
* IE 9+
* Safari (Win) 5.1.7
* Firefox 36.0.4
* Google Chrome 41.0.2272.101
* Opera 28.0

## Licence
* License: MIT

## Author
* Author: kekeh
