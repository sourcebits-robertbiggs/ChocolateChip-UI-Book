#ChocolateChip-UI

##Events

Because ChocolateChip-UI is build on top of jQuery, handling events is exactly the same as you would expect. The main difference is that ChocolateChip-UI provides two additional things: an abstraction layer to handle the differences between mouse, finger and pointer events, and a gesture module to provide taps, long taps, double taps, swipes, swipe up, swipe down, swipe left and swipe right. You use these the same way you would use any other event.


###Touch or Click?

####Event Aliases

Click or touch, which should you use? Well, you can avoid the whole issue by using ChocolateChip-UI's event alias. These are as follows:

On desktop:

- $.eventStart: mousedown
- $.eventMove: mousemove
- $.eventEnd: mouseup
- $.eventCancel: mousecancel

On Android:

- $.eventStart: touchstart
- $.eventMove: touchmove
- $.eventEnd: touchend
- $.eventCancel: touchcancel

On iOS:

- $.eventStart: touchstart
- $.eventMove: touchmove
- $.eventEnd: touchend
- $.eventCancel: touchcancel

On Windows Phone 8 (IE10)

- $.eventStart: MSPointerDown
- $.eventMove: MSPointerMove
- $.eventEnd: MSPointerEnd
- $.eventCancel: MSPointerCancel

On Windows Phone 8 (IE11)

- $.eventStart: pointerdown
- $.eventMove: pointermove
- $.eventEnd: pointerup
- $.eventCancel: pointercancel

By using these event aliases you can write code that works across computing platforms:


```
$('.myButton').on('$.eventStart', function() {
  alert('You pushed by button!')
});
```


###Gestures

Besides event aliases, ChocolateChip-UI also provides the following gestures:

- singletap: 120 milliseconds delay
- longtap: 750 milliseconds
- doubletap: second tap occurs at most 250 milliseconds after first tap
- swipe: any direction, but be at least 30px in length 
- swipeup
- swipedown
- swipeleft
- swiperight

You use these just as you would any other events:


```
// Example of singletap:
$('.myList').on('singletap', 'li', function() {
    // Alert the content of the list item:
  alert($(this).text())
});

// Example of doubletap:
$('.myButton').on('doubletap', function() {:
  alert('You double tapped on me!')
});

// Example of swiperight:
$('.myList').on('swiperight', 'li', function() {
  // Alert the content of the item you swiped right on:
  alert($(this).text())
});
```

These gestures work reliably on all supported platforms, with the exception of longtap. On Windows Phone 8, Microsoft has coopted the long tap to show the right click popup menu customary on Windows. Currently there doesn't appear to be a way to get around this.

###To Click or Tap

On desktops, clicks are the normal way for users to interact with apps. Mobile operating systems also support a click, even though it would be instigated by a touch. If you set up a click event listener and the user touches it, the operating system will wait a full 300 to 450 milliseconds before executing the callback. This makes software that uses clicks feel sluggish and unresponsive. You can get around this delay by using the event alias: $.eventStart.

Generally, developers are frustrated by the delay that clicks have on mobile. However, we need to understand why. Suppose you have an app with a long navigation list. Because it's long, the user needs to scoll to get to the items off the screen. In this list there are also actionable items for navigation purposes. If the click were converted into a touch start with no delay, as soon as the user tried to scroll a navigation list, the touch list item would immediately react, directing the user to its target. The user would never be able to scroll the list. That's why click's have a delay.

ChocolateChip-UI uses singletaps for navigation lists, and touchstart for the back buttons. Since the back buttons are out of the way, touchstart events are fine and allow the user to return to the previous article quickly. Using singletap events allows the user to interact with an item in a reasonable amount of time, and allows the user to touch and scroll the page without immediately causing an interaction.


###Desktop Compatibility

ChocolateChip-UI has some features that are implemented for desktop compatibiliy. For example, on the desktop you will notice that many actionable items have hover states. These are not implemented with JavaScript events but purely through CSS using `:hover` pseudo-elements. When ChocolateChip-UI detects that an app is running in a desktop browser, it outputs the class `.isDesktop` on the body tag. In the themes there is a definition starting with ".isDesktop" to add hover states to items. That way these interactions are sandboxed for desktop only. The reason is that on some platforms, CSS hover states can be sticky. The browser sees the defined hover state and renders it when the user touches. However, sometimes, even when the user touches elsewhere, the browser does not dispose of the hover state. This can quickly result in a messing and confusing state for your app.

ChocolateChip-UI also has touch states defined for many interactive elements. This is done by putting the class `touched` or `selected` on the element. Unfortunately, because of the the sluggishness of mobile browsers and the need to use singletaps, sometimes the element may execute its intended action before the browser has time to render its selected state. I know, I can sense you disappointment. It has been gnawing at me for a long time. I'm always hoping I can figure out a reasonable workaround but haven't been able to find one yet.

###Using ChocolateChip-UI Events with Angularjs

If you want to use Anguljarjs 1.x with ChocolateChip-UI, you can create a plugin that exposes ChocolateChip-UI's events as directives that you can include in your app through dependency injection. Angularjs has a module called `ngTouch`, however this is quite limited in what it offers and is not compatible with ChocolateChip-UI. In order to be able to use ChocolateChip-UI's events with Angularjs, we need to create directives. Our directive will have the following format:

```
<p cc-singletap='doSomething()'>Click</p>
```

We will therefore create the following directives:


- cc-tap
- cc-singletap
- cc-longtap
- cc-doubletap
- cc-swipe
- cc-swipeleft
- cc-swiperight
- cc-swipeup
- cc-swipedown

Because this is a lot of events, we want to find a way to create them all at once with a factory pattern. 

 The best approach is to create a module that contains all the gesture directives. That you can use Angular's dependency injection to include the directives in your app instance with one module. Since the code to create each event directive would be identical, we can DRY the process by using a factory pattern. In this case we can create an array of the event names and use that to produce the directives. Without further ado, here it is:

```
//////////////////////////////////////////
// Initialize the CCGestures modules to 
// hold the event directives:
//////////////////////////////////////////

var CCGestures = angular.module('CCGestures',[]);

///////////////////////////////////////////
// Iterate over array of avaialable events
// and add them to the CCGestures module:
///////////////////////////////////////////
["tap", "singletap", "longtap", "doubletap", "swipe", "swipeleft", 
"swiperight", "swipeup", "swipedown"].forEach(function(gesture) {

  // Camel Case each directive:
  //===========================
  var ccGesture = "cc" + (gesture.charAt(0).toUpperCase() + gesture.slice(1));

  // Create module for each directive:
  //==================================
  CCGestures.directive(ccGesture, ["$parse",
    function($parse) {
      return {
        // Restrict to attribute:
        restrict: "A",
        compile: function ($element, attr) {
          // Define the attribute for the gesture:
          var fn = $parse(attr[ccGesture]);
          return function ngEventHandler(scope, element) {
            // Register event for directive:
            $(element).on(gesture, function (event) {
              scope.$apply(function () {
                fn(scope, {$event: event});
              });
            });
          };
        }
      };
    }
  ]);
});
```

In the above code we've defined a module named `CCGestures`. This is the guy you'll inject in your app. Then we loop over an array of ChocolateChip-UI gesture names and create directives on that module. Incude this code in your app before your app instantiation. Then you can include the module as normal:

```
var app = angular.module('MyApp', ['CCGestures']);
```

Having done this, you can then use the ChocolateChip-UI gesture directives in your markup to create the mobile user interactions you need.

```
<ul class='list' id='chosePerfumeGenre' ng-controller='pefumeCtrl'>
  <li cc-singletap='getChosenPerfume(perfume)' data-goto='perfumeDetail' ng-repeat="perfume in perfumesCollection">{{perfume.title}}</li>
</ul>
```

Because we pass the loop context `perfume` as a parameter of the `getChosenPerfume` method of `cc-singletap`, we will have access to it in our method. 


```
app.controller("pefumeCtrl", function ($scope) {
  $scope.getChosenPerfume = function(perfume) {
    // Update the scope with the chosen perfume:
    $scope.chosenPerfume = perfume;
  };
}
```


###Using ChocolateChip-UI Events with Somajs

Somajs is a very neat, small framework that enables you to write clean, decoupled code for your app. It was inspired by Angular, so it offers dependency injection and has injectors, mediators, commands, dispatchers, as well as a separate plugin for templates similar to Angular's scoped directives. Like angular, Somajs uses attribues in the markup to define directives, including events. These are called `data-events` because they use the HTML5 data attribute to implement them. We can easily add ChocolateChip-UI's events to Somajs as follows:

```
//////////////////////////
// Define new data events:
//////////////////////////
['tap', 'singletap', 'longtap', 'doubletap', 'swipe', 'swipeleft', 'swiperight', 'swipeup', 'swipedown'].forEach(function(gesture) {
  soma.template.settings.events['data-' + gesture] = gesture;

});
```

This will allow us to set up a Soma template like this:


```
<ul class='list' id='fragranceGenres'>
  <li class='nav data-cloak' data-singletap='getGenre()' data-goto='fragranceList' data-genre='{{genre}}' data-title='{{capitalize(genre)}}' data-repeat="genre in genres">
    <h3>{{capitalize(genre)}}</h3>
  </li>
</ul>
```

