#ChocolateChip-UI

##ChUI.js

ChUI.js is the file that brings all the layouts and widgets to life. It provides methods to create widgets on the fly, or to initialize existing markup. There is only one ChUI.js file for all three operating systems. It detects the operating system at load type and when needed, makes adjustments to widgets. If you are experiencing some OS specific problem that you would like to adjust for, you can use the properties that ChocolateChipJS exposes. Please consult the documentation for ChocolateChipJS to see these.




##Events

ChUI.js provides a number of variables to abstract the user input interaction from the device input. This allows you to use one event input for mouse, finger or stylus on desktop and mobile.

###$.eventStart

This is equivalent to a mousedown, touchstart or MSPointerDown/pointerdown event.

###$.eventMove

This is equivalent to a mousemove, touchmove or MSPointerMove/pointermove event.

###$.eventEnd

This is equivalent to a mouseup, touchend or MSPointerUP/pointerup event.

###$.eventCancel

This is equivalent to a mousecancel, touchcancel or MSPointerCancel/pointercancel event.

###$.gestureLength

This is used by ChUI.js to define how far a user must drag the mouse, stylus or finger before a swipe gesture is registered. The default value is 30 pixels. Decreasing this might cause swipe gestures being registered too easily for the user to interact with other controls that expect taps/clicks/touches. Increasing it will make the shorter swipes harder to detect. If you want to register swipe events, 30 to 50 is the optimal amount.




##$.Uuid

This is a method that creates a uuid. This is used internally by ChUI when it needs to dynamically create ids for elements that don't have one. You can also use it when you need a unique identification value.




##$.UITrackHashNavigation

This method is used internally by ChUI.js with navigation lists. It puts a hash value in the url of the window.location, allowing you to use client side routing with your navigation.




##$.UISetHashOnUrl

This method is used by $.UITrackHashNavigation.




##$.UIGoBackToArticle

This method is used for non-linear navigation back to an arbitrary article. By default a button with the class 'back' will always navigate back to the previous article (which will also have a class of 'previous'). If you want the enable the user to return to an article further back in the navigation history, you can use this method. It will also reset the navigation history array so that it reflects the current navigation state.




##$.UIGoBack

This method returns the user to the previous article. It gets executed automatically when the user interacts with a back button. You do not every need to use it.




##$.isNavigating

This is a value used by ChUI.js to keep track of the progress of a navigation animation. By default it is false, when navigation begins it is set to true, and when navigation is complete it is set back to false. This is tested during user interaction with navigation lists so that the user cannot fire more than one navigation event at a time.




##$.UIGoToArticle

This method is used by navigation lists. When you set up a navigation list, user interaction with it will automatically execute this method. You can also use it to direct a user to a specific article, perhaps through interaction with a button. In that case you would need to hook up an event listener to do so. To navigate to a particule article, just provide its id. The method will implement the animate effect for you:


```
$.UIGoToArticle('#picutres');

```



##NavigationEnd Event

ChocolateChip-UI creates a navigationend event that you can use to know when an article has finished transitioning. This event fires everytime an article transitions into view, so this will happen when the user naivgates forward and backward. For Android and iOS this only fires for navigation lists. On Windows 8.x or Windows Phone 8 this also happens with tab bars because they slide the article in and out.

The following example shows how to filter out back navigation to an article with an id of "main":

```
$(function() {
  $('article').on('navigationend', function() {
    if (this.id !== 'main') {
      console.log('Navigation just ended: ' + this.id);
    }
  })
});

```



##$.UIDeletable

This method will enable the deletion of list items. It does this by inserting an 'Edit' button on the right side of the nav. You can also provide a callback to execute when and item is deleted.


```
$(function() {
  $.UIDeletable({
    list: '#myList',
    callback: function(item) {
      var text = $(item).siblings('h3').text();
      $('#response').html('You deleted: <strong>' + text + '</strong>');
    }
  });
});

```


If you allow the user to delete items from a list, you may also want to also update your server side data. You could do this by posting the necessary info when the user deletes an item.

If you want to add list items to a deletable list after it's inital load, you can do so. Just insert the list items with the markup necessary to match your other list items, then reinitialize the deleteable list with the same options that you used originally. If you are going to do this often, you might want to just assign the `$.UIDeletable` setup to a variable so you can just use that:


```
var initDeletables = $.UIDeletable({
  list: '#myList',
  callback: function(item) {
    var text = $(item).siblings('h3').text();
    $('#response').html('You deleted: <strong>' + text + '</strong>');
  }
});

```


Then later, after adding more items to the deletable list you can reinitialize it like this:


```
initDeletables();

```




##$.UIPaging

To make a paging control, you first need to have the necessary markup for the paging mechanism. Initialization is simple: `$.UIPaging()`. You need to have a segmented control that will toggle an article's sections. 

####Example:


```
<nav>
   <h1>Paging</h1>
   <div class='segmented paging horizontal align-flush'>
     <a class='button' href='javascript:void(null)' title='previous panel'></a>
     <a class='button' href='javascript:void(null)' title='next panel'></a>
   </div>
</nav>
<article id="main" class="current paging">
   <section class='current'>
     <h2>First Article</h2>
     <ul class='list'>
       <li>
         <h3>Thing One</h3>
       </li>
     </ul>
   </section>
   <section>
     <h2>Second Article</h2>
       <ul class='list'>
         <li>
            <h3>Item One</h3>
         </li>
     </ul>
   </section>
   <section>
     <h2>Third Article</h2>
     <ul class='list'>
       <li>
         <h3>Item 1</h3>
         <cellsubtitle>Item 1 Subtitle</cellsubtitle>
       </li>
     </ul>
   </section>
</article>

```





##$.UISlideout

This method creates an empty slide out menu for your app. It also puts the slide out icon in the top left of your nav bar. By executing the function, all interactions are automatic, you just need to provide the interactive content inside the slide out for your uses to use. You could use the `[].append()` method or a template to do this or any of the other ways to output content to the slide out. Your choice.

####Example:


```
// Create an empty slide out menu:
$.UISlideout();

```


The easiest way to make the slide out interactive is to use the `$.UISlideout.populate` method described below.




##$.UISlideout.populate

As of Version 3.0.6, ChocolateChip-UI offers this method to quickly create an interactive navigable list for slide outs.

This method takes one argument, an array of objects with key/values. The key is the id of the article you to show when touched, and the value is the Label that will show in the slide out list.


```
// Create an empty slide out menu:
$.UISlideout();
// Populate the slide out:
$.UISlideout.populate([{music:'Music'},{pictures:'Pictures'},{recipes:'Recipes'},{contacts:'Contacts'}]);

```


The above would result in a list as follows:


```
<ul class="list">
  <li data-show-article="music">
    <h3>Music</h3>
  </li>
  <li data-show-article="pictures">
    <h3>Pictures</h3>
  </li>
  <li data-show-article="recipes">
    <h3>Recipes</h3>
  </li>
  <li data-show-article="contacts">
    <h3>Contacts</h3>
  </li>
</ul>

```


You can also add headers to the slide out list using the keyword `header` and a value for its label. This will be output a list item with the class `slideout-header` and an `h2` for the header itself. We'll take the above example and add two headers to break the list up. We just need to add two new key/value pairs with a key of `header` and the value for our header. The headers will be "Media" and "Miscellaneous."



```
// Create an empty slide out menu:
$.UISlideout();
// Populate the slide out:
$.UISlideout.populate([{header:'Media'},{music:'Music'},{pictures:'Pictures'},{header:'Miscellaneous'},{recipes:'Recipes'},{contacts:'Contacts'}]);

```


This will produce the following:


```
<ul class="list">
  <li class="slideout-header">
    <h2>Media</h2>
  </li>
  <li data-show-article="music">
    <h3>Music</h3>
  </li>
  <li data-show-article="pictures">
    <h3>Pictures</h3>
  </li>
  <li class="slideout-header">
    <h2>Miscellaneous</h2>
  </li>
  <li data-show-article="recipes">
    <h3>Recipes</h3>
  </li>
  <li data-show-article="contacts">
    <h3>Contacts</h3>
  </li>
</ul>

```


Because the populate method uses the keyword `header` to create headers in the slide out, you wont be able to have it work with an article that has an id of "header". Anyway, you should never have an article named header. And article is not a header for anything.



<h3>Customize the Slide Out</h3>

<p>You can pass in some options to modify the behavior of the slide out. Possible values are `dynamic: true` or a callback. Passing the "dynamic" value allows you to override the default multi-particle nature of the slide out and instead have just one article which you can populate based on which slide out menu item the user tapped. In this case you'd also want to define a callback to handle user interaction. It will get passed a refer to the item the user tapped. Using that you can determine which item and then update the article appropriately.</p>

<p>Here's what your markup might look like:</p>


```
&lt;nav class="current"&gt;
  &lt;h1&gt;&lt;/h1&gt;
&lt;/nav&gt;
&lt;article class="current" id="main"&gt;
  &lt;section&gt;
    &lt;aside&gt;&lt;img src="" alt="" height="60" /&gt;&lt;/aside&gt;
    &lt;h2&gt;Benefits&lt;/h2&gt;
    &lt;ul class='list' id='benefits'&gt;
      &lt;li&gt;
        &lt;h3&gt;&lt;/h3&gt;
      &lt;/li&gt;
    &lt;/ul&gt;
     &lt;h2&gt;Uses&lt;/h2&gt;
     &lt;ul class="list" id='uses'&gt;
     &lt;/ul&gt;
  &lt;/section&gt;
&lt;/article&gt;
```


You might initialize your slide out with something like this. Notice that the second argumet to the callback will be the element tapped by the user `li`.

```
$.UISlideout({dynamic: true, callback: function(e, li) {
  var fruit = $(li).index();
  renderChosenFruit(fruit);
}});

// Populate the menu:
$.UISlideout.populate([
  {header:'Fruits'},
  {Apples:'Apples'},
  {Oranges:'Oranges'},
  {Bananas:'Bananas'},
  {Mangos:'Mangos'},
  {Avocados:'Avocados'}
]);

// We're assuming "fruitsData" is an array 
// of data which you've gotten somehow (Ajax):

var renderChosenFruit = function(fruit) {
  $('h1').text(fruitsData[fruit].name);
  $('aside img')[0].src = fruitsData[fruit].image;
  $('#benefits h3').text(fruitsData[fruit].benefit);
  $('#uses').empty();
  fruitsData[fruit].uses.forEach(function(use) {
    $('#uses').append('<li><h3>' + use + '</h3></li>');
  });
};
```


##$.UICreateSwitch

A switch control can be created manually using a span with a class `switch`. You can set its state to on by adding it as a class. Without the class 'on' is the off state. Add a checkbox input and give it a value. At load time ChUI.js will initialize the switch with basic interactivity. You'll need to register and event to do something with it. Check the example below:


```
<li class='comp'>
  <div>
    <h3>Sleep</h3>
  </div>
  <aside>
    <span class="switch on" id="sleepSwitch">
     <input type="checkbox" value="Sleep" name="sleepSwitch">
    </span>
  </aside>
</li>

<script>
  $('#sleepSwitch').on('singletap', function() {
    if (this.classList.contains('on')) {
      $('#response').html($(this).find('input').val());
    } else {
      $('#response').empty();
    }
  });
</script>

```


You can also create a switch dynamically using the `$.UICreateSwitch` method.
You give it an object with the values for your switch:


```
var sleepSwitch = {
  id : "sleepSwitch",
  state : "on",
  name : "activity.choice",
  value: "sleep"
};
// Insert switch into the aside of list item 4:
$('.list > li').eq(3).find('aside').prepend( $.UICreateSwitch(sleepSwitch) );
// Initialize the switch:
$('.switch').UISwitch();

```




##[].UISwitch

This method initializes any uninitialized switches. You execute it by doing a search for switches using the `.switch` selector:


```
$('.switch').UISwitch();

```




##$.UICreateSegmented

You can create a segmented control dynamically with this method. You supply the following values:


```
var segmentedOptions = {
  id: 'mySegmented',
  labels : ['Radioactive','Hurt','Yeah, Yeah'],
  selected: 0
};
var newSegmented = $.UICreateSegmented(segmentedOptions);
// Insert the segemented control into the document:
$('#segmentedPanel').append(newSegmented);

```


After inserting the segmented control, you need to initialize it, at which time you can also attach and event listener to handle user interaction:


```
// The parameter "item" will refer to the selected segmented:
var segmentedResponse = function(item) {
  // Output the index number of the selected segment:
  $('#output').find('h3').html(($(item).index()));
};
// Initialize the segmented control:
$('#mySegmented').UISegmented({ callback:segmentedResponse });

```




##[].UISegmented

This method is executed on a segmented selector to initialize it. You can provide a callback to execute when the user interacts with it:


```
$('#mySegmented').UISegmented({ callback:segmentedResponse });

```




##$.UIPopup

You can create a popup using the `$.UIPopup` method. Every popup will have at least a cancel button. This button will close the popup without you needed to write any code. You can also provide another button to perform an affirmative action (continueButton). You can provide a callback that will be executed when this button is pressed. You can provide label values for both buttons to customize them. 

When you show a popup, the method will also create a semi transparent mask over the app to prevent user interaction. When you dispel the popup, the mask is also dispelled. 

You can register an event to dynamically create and show the popup:


```
// Bind event to show the popup:
$("#openPopup").bind("singletap", function() {
  $.UIPopup({
    selector: "#main",
    id: "warning",
    title: 'Attention Viewers!',
    message: 'This is a message from the sponsors. Please be seated while we are getting ready. Thank you for your patience.',
    cancelButton: 'Skip',
    continueButton: 'Stay for it',
    callback: function() {
      var popupMessageTarget = document.querySelector('#popupMessageTarget');
      popupMessageTarget.textContent = 'Thanks for staying with us a bit longer.';
      popupMessageTarget.className = "";
      popupMessageTarget.className = "animatePopupMessage";
    }
  });
});

```





##[].UIPopupClose

This method will close the popup, dispelling the mask as well. This method gets executed automatically when the user presses the cancel or continue buttons in the popup.




##$.UICenterPopup

This method is used by ChUI.js to center any active popup. You don't need to ever use it yourself.




##$.UITabbar

This method is used to create a tab bar interface. For arguments, it expects an id for the tab bar, the number of tabs you want, labels for the tabs, icons for the tabs (these only get display on iOS), and a selected tab. By default the first tab will be selected, so if that is your choice, you do not need to provide a value. If you want another tab to be the selected one, provide its zero-based position. Unlike a paging control, which enables you to navigate through an article's collection of sections, the tab bar is for toggling through a group of articles. A tab bar is used when your app has a small number of articles with each doing something unique and they do not navigate to other articles. 


```
$(function() {
  var opts = {
    tabs : 5,
	  imagePath : "../icons-ios/",
	  icons : ["music", "pictures", "documents", "downloads", "favorites"],
	  labels : ["Music", "Pictures", "Documents", "Downloads", "Favorites"],
	  selected : 1
  };
  $.UITabbar(opts);
});

```





##$.UISheet

This method is used to create a generic sheet. This is an overlay that slides across the screen, covering it. It has a handle across the top. Pressing it will close the sheet. It also has a section tag which provides vertical scrolling for any content you put within it. On iOS the sheet slides up from the bottom. On Android and Windows PHone 8 it slides down from the top. You create a sheet as follows:


```
$(function() {
  // Create an empty sheet:
  $.UISheet();

  // Get the sheet and append some markup:
  $('.sheet').find('section').append("<ul class='list'></li><li><a class='button' href='javascript:void(null)'>Save</a></li><li><a class='button' href='javascript:void(null)'>Delete</a></li><li><a class='button' href='javascript:void(null)'>Cancel</a></li></ul>");
  $('.sheet .list').append ('<h2 style="text-align: center; margin: 20px;">The End</h2>');

  // After append the sheet to the document,
  // register an event to handle the buttons, etc.:
  $('.sheet .list').on('singletap', '.button', function() {
    $.UIHideSheet();
  });

  // Initialize button to show sheet:
  $('#showSheet').on('singletap', function() {
    $.UIShowSheet();
  });
});

```





##$.UIShowSheet

After creating a sheet as illustrated above, you can show it by registering the `$.UIShowSheet` method on a user action:


```
  // Initialize button to show sheet:
  $('#showSheet').on('singletap', function() {
    $.UIShowSheet();
  });

```





##$.UIHideSheet

This method will dispel the sheet. You can register an event to do so:


```
$('.sheet .list').on('singletap', '.button', function() {
  $.UIHideSheet();
});

```





##$.body

This variable is a shortcut for the body element. Use it instead of `$('body')`. As a matter of fact, you should always assign an element to a variable when you will be accessing it multiple times, as that will be faster than doing a selector query each time.

####Example:


```
$.body.on('singleta', 'li', function() {
  alert($(this).text());
});

```





##$.UINavigationHistory

This is an array of the articles visited during a navigation event. It is used by ChUI.js to know which article to return to when the user hits the back button.




##[].UIPanelToggle

This method sets up a segmented control to toggle a series of panels.  This expects the following markup: a container div, no particular class required. Within it should be a series of divs as containers for the content to be toggles. Each one of these child divs constitutes a toggleable panel. The structure should be like this:



```
<article id="main" class="current">
  <section>
    <div class='horizontal centered'>
      <div class='segmented'>
        <a class='button'>Radioactive</a>
        <a class='button'>Hurt</a>
        <a class='button'>Permanent</a>
      </div>
    </div>
    <!-- Container for toggleable panels -->
    <div id="toggle-panels">
      <!-- toggleable panel  -->
      <div>
        <ul class='list'>
          <li>
            <h3>Imagine Dragons</h3>
            <h4>Radioactive</h4>
          </li>
        </ul>
      </div>
      <!-- toggleable panel -->
      <div>
        <ul class='list'>
          <li>
            <h3>The Hurry and the Harm</h3>
            <h4>Hurt</h4>
          </li>
        </ul>
      </div>
      <!-- toggleable panel -->
      <div>
        <ul class='list'>
          <li>
            <h3>David Cook</h3>
            <h4>Permanent</h4>
          </li>
        </ul>
      </div>
    </div>
  </section>
</article>

```


To make the above markup work as a toggleable panel control, we do the following:


```
$(function() {
  $('.segmented').UIPanelToggle('#toggle-panels',function(){$.noop;});
});

```





##[].UISelectList

This method turns a simple list into a selectable list. This just a fancy way of presenting the user with a group of radio buttons. In fact, ChUI.js inserts a radio button into each list item, although these are not visible to the user. You can set a selected state for the list, otherwise it will load with none selected. You can also provide a callback to execute when the user presses a list item. You can mark a list as a select list by putting the class 'select' on it. If you don't, the initialization process will add it for you.


```
$(function() {
  $('#seletlist').UISelectList({
    selected: 0,
    callback: function() {
      $("#response").html($(this).text());
    }
  });
});

```


You can create a list with data values on the list items using the attribute: `data-select-value`. When UISelectList runs, it will attach these values onto the radio button that belongs to each list item. That way you can get the value of the radio button when the user selects that list item.

You can add list items dynamically after the initial app load. You just need to make sure that you're added list items with everything they expect to work. The initialization added a data value to the list `data-select-value`. It also adds a hidden radio input with the form data for that item. This input needs to have the same name as the other radio inputs. If you provided a name for the radio inputs in your options, then use the same one. If you let `$.UISelectList` apply on automiatically, you'll need to grab that value from one of the list items to use for the new list items. If you create the new list items following all of these requirements, upon appending them to an existing selectable list, they will be selectable automatically.




##[].UIStepper

This method will initialize a stepper control. You create a stepper by putting the markup in your document, either manually or dynamically. See the markup below:


```
<span class='stepper' id="digits"></span>

```


To initialize it, we could do the following:


```
$(function() {
  $('#digits').UIStepper({
    start: 1,
    end: 8,
    defaultValue: 3
  });
});

```





##[].UIBusy

This method will create a busy indicator. The appearance will vary per operating system. You can customize the busy indicator with three values: color, size and position. The position option is for when you insert the busy indicator into a nav. Giving it a position of `right` will put it flush against the right side of the screen. On a right-to-left layout this will be pinned to the left. If you insert the busy indicator into a large container, you can center it using the UICenter method. Check out these examples:


```
$('nav').find('.busy').css('top','10px');
$('nav').UIBusy({size: '20px', color:'#666', position: 'right'});

$('#busy1').UIBusy({'color':'rgba(200,0,0,0.75)', 'size': '50px'});
$('#busy1').find('.busy').UICenter();

$('#busy2').UIBusy({size:'40px', color: 'gold'});
$('#busy2').find('.busy').UICenter();

$('#busy3').UIBusy({color: '#fff'});
$('#busy3').find('.busy').UICenter();

```


You can create a modal busy indicator by first creating a popup and then inserting the busy indicator into it:


```
$(function() {
  // When the user presses the showModalBusyIndicator button:
  $('#showModalBusyIndicator').on($.eventStart, function() {
    $.UIPopup({empty: true});
    $('.popup').UIBusy({'color':'blue', 'size': '80px'})
    setTimeout(function() {
      $('.popup').UIPopupClose()
    },50000000);
  });
});

```





##[].UICenter

This method will center an absolutely positioned element on the screen. Just execute it directly on the element you want to center. It gets used by popups.




##[].UIBlock

This method will create a semi-transparent mask over the screen. You can provide a value for the amount of opacity you want. It is also invoked automatically by popups and popovers. 

####Example:


```
$.body.UIBlock();

```





##[].UIUnblock

This method will remove a mask from the screen. 

####Example:


```
$.body.UIUnblock();

```



##$.UIPopover

You can create a popup using the `$.UIPopup` method. You pass several values to it to define how you want it to displayed. These are:

- title: show only on iOS
- id: (optional);
- callback: executed when the user taps the popover content


```
// Define content for popover:
var popoverContent = "<ul class='list'><li><h3>Apples</h3></li><li><h3>Oranges</h3></li><li><h3>Bananas</h3></li><li><h3>Pears</h3></li><li><h3>Plums</h3></li><li><h3>Cherries</h3></li><li><h3>Apricots</h3></li><li><h3>Lemons</h3></li><li><h3>Peaches</h3></li><li><h3>Pineapples</h3></li><li><h3>Strawberries</h3></li><li><h3>Guavas</h3></li><li><h3>Grapefruit</h3></li></ul>";

// Define handler for popover interaction:
var popoverEventHandler = function(whichPopover) {
  // Attach event to catch user interaction.
  // Use singletap to allow user to scroll content.
  $('.popover').on('singletap', function(e) {
    var listItem;
    if (e.target.nodeName === 'LI') {
      $(results).html(e.target.textContent.trim());
    } else {
      listItem = $(e.target).closest('li')[0];
      $(whichPopover).html(listItem.textContent.trim());
    }
    // Close the popover:
    $.UIPopoverClose();
  });
};

//////////////////////////////////////////
// Initialize popover.
// Attach event to button to show popover:
//////////////////////////////////////////
$('#showPopover').on('singletap', function() {
  // Save reference to button:
  var trigger = this;
  $.UIPopover({
    title: 'Fruits',
    trigger: trigger,
    content: popoverContent,
    callback: function() {
      popoverEventHandler('fruitsPopover');
    }
  });
});

```




##$.UIAlignPopover

This method is used by ChUI.js to center any active popover. You don't need to ever use it yourself.




##$.UIPopoverClose

This method is used to dispel an active popover. This will also remove the mask created by UIBlock.



##$.UIHideNavBar

This method is used to temporarily hide the navbars. This can free up some extra space on screen. Remember to provide a way for the user to get the nav bars back. See `$.UIShowNavBar` below.

```// Hide all nav bars:
$.UIHideNavBar();
```

When using this method, be sure to provide some way to show the nav bars again. If you are doing this in some process that might error out, make sure that you use error detection to also reveal the nav bars again.



##$.UIShowNavBar

This method enables you to show all nav bars that were hidden using the method `$.UIHideNavBar()`

```// Show all hidden nav bars:
$.UIShowNavBar();
```


##Gestures:

ChUI.js provides a number of gestures for you to use in your app. These get translated to appropriate events on each platform. They are as follows:


- tap
- singletap
- longtap
- doubletap
- swipe
- swipeleft
- swiperight
- swipeup
- swipedown


Tap is a generic tap, this could be a singletap, doubletap or longtap event. The same with swipe, it will be either a left, right, up or down swipe. These two are not very useful unless you want to check for a very generic user interaction.

A singletap has a delay of 150 milliseconds. In general, if you want to interact with anything that is in a scrollable container, you want to use a singletap event. This will allow you to scroll without triggering an event on any actionable items in the container. You can register a singletap and a doubletap on the same element, or a longtap too, to set up different results. Swipes work just like any other events as well.

To use these gestures, put them in quotes just as you would any normal DOM event:


```
$('a').on('singletap', function() {
  // Do stuff here
  alert('Single Tap');
});
$('a').on('doubletap', function() {
  // Do stuff here
  alert('Double Tap');
});
$('a').on('longtap', function() {
  // Do stuff here
  alert('Long Tap');
});
$('a').on('swipeleft', function() {
  // Do stuff here
  alert('Swipe Left');
});
$('a').on('swiperight', function() {
  // Do stuff here
  alert('Swipe Right');
});

```





<h1>Pub/Sub</h1>
Pub/sub is a popular and useful programming pattern to help decouple code. This allows you to set up methods that "subscribe" to a topic, something like a radio channel. When you publish data to a topic, all methods that are subscribed to that topic will execute with that data. It thus provides an easy way to pass the same data to multiple functions without having to enclose them in one function.

When implementing a pub/sub pattern, be careful not to over use it, as it can sometimes be difficult to debug or run tests against.




##$.subscriptions
The is a cache for all subscriptions. If you unsubscribe for a topic, the subscription will be removed.

By convention, topics used for subscriptions use forward slashes to demark namespaces, something like a rest interface.


```
// Possible topics:
'user/new'
'user/current'
'purchases/songs'
'purchases/apps'
```





##$.subscribe
To subscribe to a topic, you pass two arguments, a topic and a callback to execute when data gets published. It's a good idea to put in some data checks to see what kind of data is being received. That way you can choose not to do anything, or to use only certain parts of the data. The type of data could be any valid JavaScript type: string, array, object, etc. It's up to you to figure out where this data that you publish comes from. It might reside on the server, it could be dynamically generated by the server. It might come from any number of possible sources: Ajax requests, Web services, etc.

Below is an example of how to subscribe to a topic:


```
var arraySubscriber = function( data ){
  $('.list').append('<li><h3>' + topic + '</h3><h4>' + data + '</h4></li>');
var newsSubscription = $.subscribe( 'news/update', arraySubscriber );
};

```





##$.publish
The `$.publish()` method lets you send data to all topics that are subscribed to a topic.


```
$.publish('news/update', 'The New York Stock Exchange rose an unprecedented 1000 points in just three minutes. Analysts and investors are confused and uncertain how to respond.');

```





##$.unsubscribe
After setting up a subscription to a topic, you may want to unsubscribe when certain conditions occur. You can do this with the `$.unsubscribe()` method. Just pass it the name to the subscription you set up, that would be one with a topic and callback as its arguments. See below:


```
$.unsubscribe(newsSubscription);

```

After unsubscribing from a topic, any further attempts to publish data to the method will do nothing.


```
var arraySubscriber = function(data) {
  $('.list').append('<li><h3>' + topic + '</h3><h4>' + data + '</h4></li>');
var newsSubscription = $.subscribe('news/update', arraySubscriber);
};
// This news item gets published:
$.publish( 'news/update', 'The New York Stock Exchange rose an unprecedented 1000 points in just three minutes. Analysts and investors are confused and uncertain how to respond.' );
$.unsubscribe(newsSubscription);
// Due to being unsubscribed above, this does nothing:
$.publish('news/update', 'We have nothing further to comment at this time.');

```





##$.UISearch
This method creates a search field at the top of the article styled appropriately for each OS. You pass in a object of options to setup the search field. This will prepend the search field to the article's section that you specified with the article ID, otherwise it will prepend it to the first article of your app.

```
$.UISearch({
  articleId: '#products',
  id: 'productSearch',
  placeholder: 'Find a product',
  results: 5
});

```





##$.UISetupCarousel
This method creates a swipable carousel. It uses mouse gestures to enable desktop testing. It also adjusts direction automatically when the document direction is set to `rtl` for "right-to-left" alphabets (Arabic, Hebrew, Farsi, Urdu, etc.).
The carousel actually only has three panels, reducing the amount of memory it requires. As the user swipes, the widget updates the position and content of the panels on either side of the current panel. To initialze the carousel, you pass it an object of key-value parameters:

```
var panels = [
  "<h2>Panel 1</h2><h2></h2><img src='../images/american-football.jpg'>",
  "<h2>Panel 2</h2><img src='../images/couple.jpg'>",
  "<h2>Panel 3</h2><img src='../images/daisy.jpg'>",
  "<h2>Panel 4</h2><img src='../images/dependent.jpg'>",
  "<h2>Panel 5</h2><img src='../images/dreamy.jpg'>",
  "<h2>Panel 6</h2><img src='../images/girl.jpg'>",
  "<h2>Panel 7</h2><img src='../images/lacrosse.jpg'>",
  "<h2>Panel 8</h2><img src='../images/hot-air-balloon.jpg'>
]
{
  target: '#carousel',
  panels: panels,
  loop: true,
  pagination: true
}

```

The target is a wrapper div that will hold the carousel. The carousel itself is just and unordered list. Each list item will be a panel in the carousel. You provide content for the panels by passing in an array of markup as strings. As long as the markup is valid and can fit within the panel, it should be fine. If you intend to put more content than can fit in a panel, just style the list items for the carousel so that they are scrollable like this:

```
.carousel-track > li {
  overflow-y: auto !important;
}

```


By default the carousel has its `loop` property set to cycle infinitely. If you want to make it only swipe between the first and last, but not past them, pass in a false vale for the loop attribute:

```
{
  target: '#carousel',
  panels: panels,
  loop: false
}

```


Carousels can have pagination. By default they do not. You need to pass a pagination value of true:

```
{
  target: '#carousel',
  panels: panels,
  loop: false,
  pagination: true
}

```


When pagination is set to true, a pagination indicator is output for each carousel panel. The user can navigate the carousel by tapping on the pagination indicators. Also, as the user swipes through the carousel, the appropriate pagination indicators will be highlighted. Please be aware that on a mobile device, if you have more than 18 paginations, it will be difficult for the user to accurately select one to navigate the carousel. Carousels work best with fifteen or less indicators.

When you initialize a carousel, each carousel is a unique instance of `$.UICarousel`. This is stored on the carousel wrapper indicated by the target attribute in the setup function. You can retrieve the instance of the carousel like this (assuming we have a carousel wrapper with the id "myCarousel":

```
var myCarousel = $('#myCarousel').data('carousel');
$('button5').on('singletap', function() {
  // Go to panel number 6:
  myCarousel.goToPanel(5);
});

```

The dimensions of the carousel are controlled by CSS, so you can change the height and width to fit your needs. Please be aware that if you make a carousel too big for a mobile screen, the user may be in a situation where he or she cannot scroll vertically to see other content above or below the carousel. Test on various screen sizes to make sure the carousel doesn't prevent access to other content. You can always use media queries to change the size of the carousel based on the devices dimensions. To style the dimensions, just put the CSS directly in your application. You can use OS classes to target each operating system:

```
.isAndroid .carousel {
  width: 240px;
  height: 320px;
}
.isiOS .carousel {
  width: 320px;
  height: 300px;
}
.isWindows .carousel {
  width: 320px;
  height: 300px;
}

```