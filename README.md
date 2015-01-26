# Auto-Scroll-To-Directive

While working on one of my recent projects I encountered a situation where I had to **scroll** to a particular element on a page. This was easily achieved using the **$anchorScroll** service in **angularJS**. It simply checks the current value of **$location.hash()** and **scroll**s to the related element.

You must be thinking what exactly is **$anchorScroll**? What is **$location.hash()**? How does **$anchorScroll** works?

Let's go ahead:

**$location** is a **service** in **angularJS** which exposes the current URL in the address bar so that it can be observed or manipulated. It represents the **URL** object as set of methods protocol, host, port, path, search, hash.

**hash()** method sets/returns the anchor part of the **URL**. Anchor basically refers to internal section of the web document.

**$anchorScroll** watches **$location.hash()** and automatically **scroll**s to match any anchor whenever it changes.

It's time for some action now. Have a look at the piece of HTML given below:

 ```HTML
<!DOCTYPE html>
<html ng-app="myApp">
<head lang="en">
    <meta charset="UTF-8">
    <title>My Page</title>
</head>
<body>
<div id="FirstDiv">
    <h1>Section 1</h1>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
</div>
<div id="SecondDiv">
    <h1>Section 2</h1>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
</div>
<div id="ThirdDiv">
    <h1>Section 3</h1>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
</div>
<div id="FourthDiv">
    <h1>Section 4</h1>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
</div>
<div id="FifthDiv" auto-scroll-to="FifthDiv">
    <h1>Section 5</h1>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
</div>
<div id="SixthDiv">
    <h1>Section 6</h1>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
</div>
<div id="SeventhDiv">
    <h1>Section 7</h1>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
</div>
<div id="EighthDiv">
    <h1>Section 8</h1>
    <br>
    <br>
    <br>
    <br>
    <br>
    <br>
</div>
<script src="angular.min.js"></script>
<script src="autoScrollTo.js"></script>
</body>
</html>
 ```
Now, suppose we want that every time we land to the 'myPage.html', it should automatically **scroll** to section 5 of the page. As explained above, this can be easily achieved using the **anchorScroll()** **service** of **Angular**. But what if tomorrow same requirement comes up for some other web page. Since **scrolling** to a particular section on a page is a very common use case, therefore I thought of making a **directive** so that it can be used wherever and whenever required.

Here we go:

 ```JavaScript
(function (ng) {
    'use strict';
    var app = ng.module('myApp', []);
    app.directive('autoScrollTo', ['$location', '$anchorScroll', '$timeout', function ($location, $anchorScroll, $timeout) {
        return {
            restrict: 'A',
            scope: false,
            link: function (scope, elem, attr) {
                //code to scroll on the basis of provided id
                $timeout(function () {
                    $location.hash(attr.autoScrollTo);
                    $anchorScroll();
                });
            }
        }
    }]);
}(angular));
```

In the above **JavaScript** snippet we have basically done the following:

1. Defined a **directive** named as **autoScrollTo**.
2. While declaring the **directive**, we have also declared the dependencies : **$location** , **$timeout** and **$anchorScroll**.
3. We would be using this **directive** as an attribute of the element, hence we have given **restrict** property as ```A```. In case you want to have deeper understanding on how to make custom **directive**s, please have a look at this very informative [blog by Amit Thakkar](http://codechutney.in/blog/angularjs/component-in-angularjs/).
4. Then, there is a **link** **function** which is executed while attaching the template to the DOM. **Link** **function** basically has  parameters:
    1. **scope** - **angular** **scope** object
    2. **element** - jqLite-wrapped element that this **directive** matches
    3. **attribute** - hash object with key-value pairs of normalized **attribute** names and their corresponding **attribute** values
5. We have placed our code for **scrolling** into a self executing function and we have applied a delay using **$timeout**.

    > You must be thinking that we have forgotten to specify the time of delay. Well, we have deliberately not mentioned the delay time as omitting the delay time defaults it to ```0```. But the code block contained in it is executed only after ```DOM``` has been manipulated by Angular and after the browser renders.
6. Then we have used **$location.hash()** to specify the element to which we need to automatically **scroll**. We know that attribute would give us the value of the corresponding **attribute**, so we are finding here the value of the **autoScrollTo** **attribute** which was placed in the ```div``` with id ```FifthDiv```.
7. And then, off course in the end we have used our **$anchorScroll()** **service** to **scroll** to the part of the page, defined through **$location.hash()** method.

Your **autoScrollTo** **directive** is now ready to use anywhere!

**NOTE**: You can checkout the full working source code from [here](https://github.com/NamitaMalik/Auto-Scroll-To-Directive).