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



