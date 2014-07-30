#ChocolateChip-UI

##Widgets

ChocolateChip-UI provides you with a number of useful widgets, such as popups, popovers, busy indicators, sheets, and deletable lists.

###Segmented Control

The segmented control is a group of buttons for executing a small set of related tasks. The segmented control is also used for toggleing panels and paging sections, which are both descibed in the chapter "Navigation".

There are two ways to create a segmented control, manually or dynamically. The segmented control is just a div with the class `segmented` which contains several anchors with the class `button`:


```
<div class='segmented'>
  <a class='button selected'>Radioactive</a>
  <a class='button'>Hurt</a>
  <a class='button'>Permanent</a>
</div>
```

Notice that I gave the first button the class `selected`. This will make that button show its selected state. To initalize the segmented control, you use the function `UISegmented`. This function is executed directly on the segmented control. You need to pass it a few options:

```
$('#mySegmentedControl').UISegmented({
  selected: 1,
  callback: function() {
    alert($(this).text());
  }
});
```


![Segmented Control, segment 1](images/widgets/segmented-1.png)

![Segmented Control, segment 2](images/widgets/segmented-2.png)


####$.UICreateSegmented

You can use `$.UICreateSegmented` to create a segmented control dynamically. To do so, you need to pass this function some options telling it how you want the segmented control put together. The options should be an id for the segmented control, text values for the labels that will appear on the buttons, and a value for which one you want to be selected by default. The selected value is zero-based, so 0 would be the first, 1 the second, etc.

```
var segmentedOptions = {
  id: 'mySegmented',
  labels : ['Radioactive','Hurt','Yeah, Yeah']
};
```
Once we have defined the options for our segmented control, we can pass it to the $.UICreateSegmented method. In the code below, we're passing in the variable that holds the options we defined above and storing the resulting output of the segmented control in the variable "newSegmented", which we then append to a container with an id of "segmentedPanel":

```
var newSegmented = $.UICreateSegmented(segmentedOptions);
// Insert the segemented control into the document:
$('#segmentedPanel').append(newSegmented);
```

After inserting the segmented control, we need to initialize it, at which time we would also attach an event listener to handle user interaction. We need to define a callback that will handle what happens when the user taps any of the segmented control's buttons:

```
// The parameter "item" will refer to the selected segmented:
var segmentedResponse = function(item) {
   // Output the index number of the selected segment:
   $('#output').find('h3').html(($(item).index()));
};
// Initialize the segmented control:
$('#mySegmented').UISegmented({ 
    selected: 2, 
    callback: segmentedResponse 
});
```

![Popup Control, before](images/widgets/popup-1.png)


![Popup Control, visible](images/widgets/popup-2.png)

###Popup

You can create a modal popup using the `$.UIPopup` method. Every popup will have at least a cancel button. This button will close the popup without needing any code. You can also provide another button to perform an affirmative action (continueButton). You can provide a callback that will be executed when this button is pressed. You can provide label values for both buttons to customize them.

When you show a popup, the method will also create a semitransparent mask over the app to prevent user interaction. When you dispel the popup, the mask is also dispelled.

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
Be aware that particularly on older versions of the native Android browser, you may find situations when the user can interact with elements below the screen shield. This is not a bug in ChocolateChip-UI but is a fairly prevalent bug in the native Android browser where events leak through to elements below. If this is causing you serious problems you may have no other recourse than to temporarily set the visibility of the content below the popup to none. Then show them again when you close the popup.

###Popover

You can create a modal popover using the method `$.UIPopover`. In general, a button in the navbar will trigger a popover, so you would wire up the $.UIPopover method to fire when the user taps a button. The popover expects a title, but it only gets displayed on iOS. Android and Windows Phone 8 do not show titles on their version of the popover. The popover needs to know what element triggered its appearance so that it knows where to position itself. To do this you need to provide a reference to the button tapped as the trigger. 

A popover can display any kind of content you want—a series of action buttons, a navigable list, some information to view. It's your choice. In most cases the content of a popover is a series of actionable items the user can execute to act on the current article. This might be a list or a collection of buttons.

 Examine the following example to see how the properties are defined:


```
// Content for popover:
var popoverContent = "<ul class='list'><li><h3>Apples</h3></li><li><h3>Oranges</h3></li><li><h3>Bananas</h3></li><li><h3>Pears</h3></li><li><h3>Plums</h3></li><li><h3>Cherries</h3></li><li><h3>Apricots</h3></li><li><h3>Lemons</h3></li><li><h3>Peaches</h3></li><li><h3>Pineapples</h3></li><li><h3>Strawberries</h3></li><li><h3>Guavas</h3></li><li><h3>Grapefruit</h3></li></ul>";

// Define a callback for popover:
var popoverEventHandler = function(whichPopover) {
  // Attach event to catch user interaction.
  // Use singletap to allow user to scroll content.
  $('.popover').on('singletap', function(e) {
    var listItem;
    if (e.target.nodeName === 'LI') {
      $('#fruitsChoice').html(e.target.textContent.trim());
    } else {
      listItem = $(e.target).closest('li')[0];
      $('#fruitsChoice').html(listItem.textContent.trim());
    }
    // Close the popover after 
    // the user interacts with it:
    $.UIPopoverClose();
  });
};

/////////////////////////////////
// Initialize the popover
// when the user taps the button:
/////////////////////////////////
$('#showPopover').on('singletap', function() {
  // Reference to button tapped:
  var trigger = this;
  $.UIPopover({
    title: 'Fruits',
    trigger: trigger,
    // Content to fill the popover:
    content: popoverContent,
    // Callback for when the user taps it:
    callback: function() {
      popoverEventHandler('fruitsPopover');
    }
  });
});

```


![Popover, before](images/widgets/popover-1.png)

![Popover, visible](images/widgets/popover-2.png)

###Busy Control

The busy control creates an indicator that lets the user know something is going on. You would use it for an Ajax request, especially since you can't be sure how long a response will take. You could also use it with any other process that may take some time to complete.

You execute the `UIBusy` method on the container where you want the busy control to appear. The appearance will vary per operating system. You can customize the busy indicator with three values: color, size and position. The position option is for when you insert the busy indicator into a nav. Giving it a position of 'right' will put it flush against the right side of the screen. If you insert the busy indicator into a large container, you can center it using the `UICenter` method. Check out the examples below:


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

####Remove a Busy Control

If you are using the busy control in a container that will receive the result of an Ajax request, you can wait for the request to finish and let the new content replace the busy control. Or, if you are using the busy control in some other circumstance, you can delete it by using the id you gave it at creation time:

```
\\ For a busy control with id "busy_01":
$('#busy_01').delete();
```

####Modal Busy Control

You can create a modal busy indicator by first creating a popup and then inserting the busy indicator into it:

```
$(function() {
   // When the user presses the showModalBusyIndicator button:
   $('#showModalBusyIndicator').on($.eventStart, function() {
      $.UIPopup({empty: true});
      $('.popup').UIBusy({'color':'blue', 'size': '80px'})
      setTimeout(function() {
         $('.popup').UIPopupClose()
      },50000);
   });
});
```

Here's a busy indicator in a modal dialog:


![Modal Busy Indicator](images/widgets/busy-modal.png)


###Sheets

ChocolateChip-UI provides sheets as a way for you to offer the user access to secondary information or actionable items. `$.UISheet` will create an empty modal sheet with a close widget at the top. You just need to populate the sheet with the content you want. On iOS the sheet slides up from the bottom. On Android and Windows Phone 8 it slides down from the top. You create a sheet as follows:

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

After creating a sheet as illustrated above, you can show it by registering the `$.UIShowSheet` method on a user action:

```
// Initialize button to show sheet:
$('#showSheet').on('singletap', function() {
  $.UIShowSheet();
});
```

The sheet is created with a chevron inside its top bar which when tapped will close it. Inside the sheet you can wire up buttons or other elements to close it using the method `$.UIHideSheet`:

```
$('.sheet .list').on('singletap', '.button', function() {
   $.UIHideSheet();
});
```


###Deletable Lists

You can turn any type of list into a deletable list, enabling the user to delete its list items. You simply execute the method `$.UIDeletable`, passing it options with a selector for the list and an optional callback to execute when an item is deleted. 

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


You can also execute this method directly on the list you want to make deletable, in which case you should not pass in the list selector, as this method will execute directly on the list:


```
$(function() {
   $('#myList').UIDeletable({
     callback: function(item) {
       var text = $(item).siblings('h3').text();
       $('#response').html('You deleted: <strong>' + text + '</strong>');
     }
   });
});
```



If you allow the user to delete items from a list, you may also want to update your server side data. You could do this by posting the necessary info in an Ajax response when the user deletes an item.

If you want to add list items to a deletable list after it's initial load, you can do so. Just insert the list items into the list however you want, then reinitialize the deleteable list with the same options that you used originally. If you are going to do this often, you might want to just assign the $.UIDeletable initialization to a variable so you can just use that:

```
var initDeletables = function() {
  $.UIDeletable({
    list: '#myList',
    callback: function(item) {
      var text = $(item).siblings('h3').text();
      $('#response').html('You deleted: <strong>' + text + '</strong>');
    }
  };
});
```



Then later, after adding more items to the deletable list you can reinitialize it like this:

```
initDeletables();
```


![Deletable list with Edit button](images/widgets/deletable-1.png)


![Deletable list, showing indicators](images/widgets/deletable-2.png)


![Deletable list with delete buttons](images/widgets/deletable-3.png)


###$.UIBlock & $.UIUnblock

Sometimes you're doing some work on your app and you don't want the user to interact with things. Usually you would use a modal busy indicator. However, if it is going to be very brief, you might just want to block the screen for a few moments. You can use the method `$.UIBlock` for that:

```
$('body').UIBlock();
```

To remove the blocker, you can use:

```
$('body').UIUnblock();
```
Be aware that from a user's perspective, it's always better to use a busy indicator since this gives them a better idea that something is going on.
