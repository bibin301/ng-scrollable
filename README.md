ng-scrollable
=================

Superamazing scrollbars for AngularJS

Why ng-scrollable?
------------------

Since Firefox 30 did not support CSS styling of scrollbars, `overlay:auto` was no usable cross-browser alternative, other plugins out there required either jquery ([perfect-scrollbar](https://noraesae.github.io/perfect-scrollbar/)), were not flexible enough or unfriendly to layouts in larger single-page apps, I decided to create a pure JS/Angular solution.

Hope you can also make use of it in your projects.

Demo: https://echa.github.com/ng-scrollable/

Features
--------

* It's small. Minified size is 5.1k JS + 1.7k CSS.
* It's pure Angular. No jquery required.
* It's soft scrolling using CSS3 translate and transition.
* It's responsive and friendly to your layout.
* It's fully customizable. CSS, scrollbar position and behaviour.
* It's MIT licensed.

How to Use
----------

```html
<head>
    <link href="ng-scrollable.min.css" rel="stylesheet">
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.2.18/angular.min.js"></script>
    <script src="ng-scrollable.min.js"></script>
    <script>var app = angular.module('app', ['ngScrollable']);</script>
</head>
<body ng-app="app">
    <div ng-scrollable="{scrollX:'none',scrollY:'left'}" style="width: 100%; height: 300px;">
        <img src="image.png"/>
    </div>
</body>
```

If the size of your scrollable container or content changes, call
```javascript
$scope.$emit('content.changed');
```

from any of your content controllers inside ng-scrollable or

```javascript
$scope.$broadcast('content.changed');
```

from outside the ng-scrollable scope.

Events
-------

ng-scrollable may be controlled by events sent to the directive's scope, either using `$scope.$broadcast` from the outside or `$scope.$emit` from the inside. The events below take no parameters.

### scrollable.scroll.left
Scroll to the left edge of the content. Will change the horizontal position only.

### scrollable.scroll.right
Scroll to the right edge of the content. Will change the horizontal position only.

### scrollable.scroll.top
Scroll to the top edge of the content. Will change the vertical position only.

### scrollable.scroll.bottom
Scroll to the top edge of the content. Will change the vertical position only.



Optional parameters
-------------------

ng-scrollable supports optional parameters passed as JS object to the `ng-scrollable` attribute, e.g.
```
<div ng-scrollable="{scrollX:'none',scrollY:'left'}"></div>
```

### scrollX
Position where to display the horizontal scrollbar, either `top`, `bottom` or `none`.
**Default: bottom**

### scrollY
Position where to display the vertical scrollbar, either `left`, `right` or `none`.
**Default: right**

### minSliderLength
Minimum number of pixels below the slider will not shrink in size.
**Default: 10**

### scrollXSlackSpace
Number of pixels the content width may be larger than the container width without making the horizontal scrollbar appear. This allows for some extra room so that a scrollbar is not yet visible just because of a few pixels.
**Default: 0**

### scrollYSlackSpace
Number of pixels the content height may be larger than the container height without making the vertical scrollbar appear. This allows for some extra room so that a scrollbar is not yet visible just because of a few pixels.
**Default: 0**

### wheelSpeed
Scroll speed applied to wheel event.
**Default: 1**

### useBothWheelAxes
When set to true, and only one (vertical or horizontal) scrollbar is active then both vertical and horizontal scrolling events will affect the active scrollbar.
**Default: false**

### useKeyboard
When set to true, keyboard events are used for scrolling when the mouse cursor hovers above the content. Small steps (30px) are triggered by up, down, left, right keys. Large steps (1x container width or height) are triggered by PageUp, PageDown, Home and End keys. If horizontal scrolling is inactive (either because `scrollX=='none'` or `contentWidth < containerWidth`) Home and End keys jump to top or bottom in vertical direction.
**Default: true**

### updateOnResize
When set to true any window.resize event will trigger a full refresh of the scrollable.
**Default: true**

Optional attributes
-------------------

ng-scrollable supports optional attributes to set or spy on the current scroll position programmatically. Spies may be bound to any Angular expression such as a scope function or scope variable. If the expression evaluates to a settable entity (i.e. a variable), ng-scrollable will set it to the current scroll position in pixels.

### spyX
Spy on and control the horizontal scrollbar position.

### spyY
Spy on and control the vertical scrollbar position.


How does it work?
-----------------

* ng-scrollable is pure Javascript and only requires Angular.
* Content is wrapped with `ng-transclude` into an `overflow:hidden` container.
* Scrollbars are added as sibling element and positioned absolute over the content.
* Content and scrollbars are soft moving using CSS3 transform and transition.
* You can disable X and Y scrolling and choose where scrollbars are displayed.
* Window resize events are captured to recalculate scrollbar size and position.
* You can optionally signal content changes by emitting a `content.changed` event.

Cool, isn't it?


Install
-------

When you already use Bower, the easiest way to get started with ng-scrollable is

```
bower install ng-scrollable
```

You can also download the latest stable [release](https://echa.github.io/ng-scrollable/releases) from GitHub. If you are brave and can't wait for new releases go ahead and fetch the development version by cloning the [Github repository](https://github.com/echa/ng-scrollable/).

```
git clone https://github.com/echa/ng-scrollable.git
cd ng-scrollable
```

Developing
----------

You need `grunt` to lint and minimize sources. Otherwise the project is really simple. I assume you already know what you are doing.

Usage
```
grunt lint  - check source files
grunt build - build minified version
```


Limitations
------------

When using ng-scrollable in combination with other Angular directives on the same DOM element, keep in mind that ng-scrollable already injects a HTML template. If other directives have templates as well, they must be linked to separate DOM nodes.

So, for example, if you want to make a `ng-view` or `ui-view` scrollable, your HTML should look like

```
<div ng-scrollable>
	<div ui-view></div>
</div>
```

### Scrollable Container Positioning

The scrollable container must have position relative or absolute for scrollbars to be visible. The CSS class `.scrollable` already takes care of this, but keep in mind to not interfere when styling the container.


### Scrollbar and Content Styling

Scrollbars are placed absolute above the content and inside the scrollable container. Their CSS defines some transparency per default, but they don't *push*  content aside. If you want bars beeing displayed beside your content you need to specify explicit margins on your content yourself. To assist you, ng-scrollable inserts the following classes on the content wrapper element when a scrollbar is displayed:

```
scrollable-top
scrollable-right
scrollable-bottom
scrollable-left
```

Note that these classes get inserted only when scrolling is not disabled and when the content width or height is larger than the containers width or height.


### Scrollable Size Updates

In contrast to browser scrollbars, ng-scrollable does not update automatically with content size. To avoid polling the DOM for changes there is two explicit update mechanism.

1. ng-scrollable updates on window resize event (enabled by default)
2. ng-scrollable updates on user event `content.changed`

You may at any time trigger an update from a controller inside the transcluded content by calling

```
$scope.$emit('content.changed');
```

### Compatibility

ng-scrollable was tested to work with Angular 1.2.18. However it should be backwards compatible down to Angular 1.1 since it does not use any special features introduced in later versions.

This project only considers supporting recent browser versions to keep the source small and usable (hence, no IE 6/7/8 or other broken browser implementations). Since ng-scrollable doesn't use touch events yet, guestures on mobiles don't work.

I have verified this plugin works with the following Desktop browsers

* Firefox 30
* Chrome 35 and
* Safari 6.1.4 (OSX)

It may or may not work with other browsers. Let me know your experiences and send pull requests!

When you need support for older browsers, please fork the project and make your own changes.



Contribution
------------

I *really* welcome contributions! Please feel free to fork and send pull requests when...

* You have an idea about how to improve ng-scrollable!
* You have found or fixed a bug!

I'll test and integrate them when time permits.

Help
----

If you have any idea to improve this project or any problem using it, please create an [issue](https://github.com/echa/ng-scrollable/issues).


License
-------

The MIT License (MIT) Copyright (c) 2014-2015 Alexander Eichhorn and [contributors](https://github.com/echa/ng-scrollable/graphs/contributors).

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
