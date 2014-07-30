#ChocolateChip-UI

ChocolateChip-UI provides a set of custom form elements that provide the user with an experience they are already familiar with on their mobile devices. Developers implementing forms for a mobile Web app often make the mistake of wanting to use standard Web form elements in their app. This is a very bad approach. It will make you app look like a Web page instead of like native software. In this chapter we will look at how to use ChocoalteChip-UI's form widgets to create interfaces with form elements that look and act like native software forms.

##Forms

Forms, the essential tool for getting user data and choices, are the number one most irritating thing about using an app. Developers love building forms and users hate using them. Even with privacy concerns, users would rather use Facebook, Twitter, Linkedin, Yahoo, Gmail or any other quick way to login to an app than go though a lengthy registration process. Chances are you also don't want to be bothered with a tedious signup process for an app, neither do your users.

Therefore, when implementing forms, think hard about if you really need them. And if you do, what can you get by without, or can you use a social media signin. With forms, less really is more. If the purpose of the form is to gather user data, think about gamifying it, or make it optional. Or give your uses the ability to use your app with limited features until they complete the registration.

###Using Default Elements

There are a number of form elements that you can put in your app and ChocolateChip-UI will automatically style them to look like native versions instead of Web ones. These are:


```
<input type="text">
<input type="number">
<input type="email">
<input type="tel">
<input type="url">
<input type="password">
<input type="range">
<input type="search">
```

Elements that are for text entry will be styled according to how they should look on that platform. Please note that support on older mobile browsers will fall back to type="text" for types they don't support. 

The range input will be styled appropriately for the native look of each platform. You can wire up event listeners to it to capture the value.

Here's an simple login form example:


![Login form](images/forms/inputs.png)

####Range Input

```
<ul class='list'>
  <li>
    <h3>Point for Point Increase<span></span></h3>
    <div>
      <label>Current Value: <span id='rangeValue'></span></label>
      <input width type="range" id="rangeControl" value="10" min="0" max="40">
    </div>
  </li>
</ul>
```

To set up the range input we can use this:

```
$(function() {
  $('#rangeValue').html($('#rangeControl').val());
  $('#rangeControl').on('input', function() {
    $('#rangeValue').html($(this).val());
  });
});
```

We use the `input` event instead of the usual `change` event because Chrome desktop and mobile will not track changes with `change` until the user stops dragging the range thumb.


![Range input](images/forms/range.png)


####Search Input

For the search input, ChocolateChip-UI provides the following solution. Mobile platforms tend to position the serch element at the top of the screen. To make this work consistently across platforms, ChocolateChip-UI uses a method `$.UISearch` to create them. You'll need to pass the method some options to tell it what article the input is associated with, placeholder text and how many results to show and an id for the input.

```
$(function() {
  $.UISearch({id: 'mySearchInput', articleID:'#main',placeholder:'Search',results: 5});
});
```

This will output the following markup:

```
<div class="searchBar">
  <input placeholder="Search" type="search" results="5" id="mySearchInput">
</div>
```

This search input does not have the ability to actually perform searches. You will need to put together some code to do that based on what you are using for implementing data-binding, etc. Possible frameworks would be Angularjs, Backbone, Knockout, etc.


![Search input](images/forms/search.png)

####Textarea

To implement a textarea, just put a regular textarea element in your markup. ChocolateChip-UI will style it appropriately for each platform:

```
<textarea name="" id="" cols="30" rows="10"></textarea>
```

####Select Control

```
<select name="" id="">
  <option value=""></option>
</select>
```

One thing to note about the select element on a mobile browser, the default look is like any select element on a Web page for that browser. But when the user interacts with it, the browser switchs to a native picker control that it displays over the screen. If you want to change the look of the select control, you could encose it in a div with a class and style it to how you want it to look. In most cases it's better to leave the select box alone as users know what it is and how it works.


![Select box](images/forms/select-box.png)


![Select box open](images/forms/select-box-open.png)


###Date/Time

Like the select box described above, you can also use the HTML5 data and time inputs to provide these choices to your users. Just designate the input's type as `date` or `time`. When the user taps the input, the mobile device will display the appropriate interface for each.

```
<input autocorrect="off" autocapitalize="off" type="date" id="date_of_birth" required>
```

![Date input](images/forms/date-time.png)

###Checkboxes and Radio Buttons

You might have noticed two types of inputs are missing: checkboxes and radio buttons. That's because ChocolateChip-UI provides an alternative to these Web elements with a solution that matches the native approach. For checkboxes, read about switches. Switches are actually just fancy checkboxes. For radio buttons, read about select lists. These are just fancy looking radio button groups.

####Switches

Instead of checkboxes, ChocolateChip-UI uses switches. A switch is just a span with the class `switch` and a checkbox input inside it. You can create a switch in two ways: manually or dynamically. The manual approach requires that you use markup like the following:


```
<span class="switch on" id="sleepSwitch">
   <input type="checkbox" value="Sleep" name="sleepSwitch">
</span>
```

Here's an important layout tip. You want to put your switches in a comp list layout. The switch should always be in the last aside:

```
<ul class="list">
  <li class="comp">
    <div>
      <h3>Sleep</h3>
    </div>
    <aside>
      <span class="switch on" id="sleepSwitch" role="checkbox">
        <input type="checkbox" name="activity.choice" checked="checked" value="sleep">
      </span>
    </aside>
  </li>
</ul>
```

The above markup will automatically be rendered by ChocolateChip-UI as switches appropriate for each platform. At load time ChocolateChip-UI looks for any existing switches and initilizes some default behavior for them. That is, it enables you to tap them to throw a switch, or to use left and right swipes to change their state.

Once a switch exists, you can hook up an event listener to handle user interaction with it. In the code below, we're attching an event listener:


```
$('.switch').on('singletap swipeleft swiperight', function() {
  if (this.classList.contains('on')) {
    $('#response').html($(this).find('input').val());
  } else {
    $('#response').empty();
  }
});
```

####$.UICreateSwitch

You can also create switches dynamically. Just use the `$.UICreateSwitch` method and pass it the options you want:


```
// Define the options for the switch:
var sleepSwitch = {
  id : "sleepSwitch",
  state : "on",
  name : "activity.choice",
  value: "sleep"
};

// Append the switch to the document:
$('.myListItem aside').prepend($.UICreateSwitch(sleepSwitch));
```

This will create the same markup as the manual example shown earlier. Since the document is already loaded, we need to initialize the switch so that it is interactive, as well as provide a callback to do something:


```
$('#sleepSwitch').UISwitch();

// Define a callback for the switch:
var handleSwitch = function(_switch) {
  var value = '';
  if (_switch.classList.contains('on')) {
    value = $(_switch).find('input').val();
    $('#response').html(value);
  } else {
    $('#response').empty();
  }
};
$.body.on('singletap swipeleft swiperight', '.switch', function() {
  handleSwitch(this);
});
```


![Switches](images/forms/switches.png)


####Select List

If you need the functionality of radio button groups, use the select list. This turns a ChocolateChip-UI list into a group of radio buttons. When the user selects a list item, it automatically selects the associated radio button. This gets styled appropriately for each platform. In order for ChocolateChip-UI to know what the values of the radio buttons should be, you need to put `data-select-value` attributes on each list item:


```
<ul class='list'>
  <li data-select-value='eat'>
    <h3>Go eat something</h3>
  </li>
  <li data-select-value='nap'>
    <h3>Take a nap</h3>
  </li>
  <li data-select-value='work'>
    <h3>Get some work done</h3>
  </li>
  <li data-select-value='play'>
    <h3>Play a game</h3>
  </li>
</ul>
```


To turn a simple list into a select list, just run the `UISelectList` method on it. This function expects a selector for the list, a default selection, which is zero-based, and a callback to execute when the user makes a choice. The name value will be the name that each radio button will have so that they are identified as a group. If you do not provide a name, ChocolateChip-UI will create a unique random group name for them. Best practice is to provide the name so that if you can access the button values conveniently at submit time.


```
$(function() {
   $('#seletlist').UISelectList({
      name: 'myRadioButtons',
      selected: 0,
      callback: function() {
         $("#response").html($(this).text());
      }
   });
});
```

After the above script runs, the method will inject a group of radio buttons into the list items, one per item. They are hidden by default.

You can add list items dynamically after the initial app load or after the creation of a list. You just need to make sure that you've included a data value, `data-select-value`, for each new list item. To work, the new list items need to have the same markup structure as the others, and they need a radio input with the same name as that used to create the other list items. If you provided a name for the radio inputs in your options, then use the same one. You can grab that value from one of the existing list items to use for the new list items. If you create the new list items following all of these requirements, upon appending them to an existing selectable list, they will be selectable automatically. No initialization required, and any callback you have will work with them.


![Select list](images/forms/select-list.png)

###Stepper

Sometimes you need to give the user a convenient way to choose between a small set of numerical values. When that's the case, you can use ChocolateChip-UI's stepper control. To create a stepper all you need is a span tag with the class `stepper`:

```
<span class='stepper' id="digits"></span>
```

Then you need to initalize it. This involves passing the `UIStepper` method several options: 

- start
- end
- defaultValue

```
$(function() {
   $('#digits').UIStepper({
      start: 1,
      end: 8,
      defaultValue: 3
   });
});
```
This will turn the stepper into a control with a plus and minus button on each side. This enables the user to manipulate the value of the stepper between the start and end values that you used to initialize the stepper. By providing a default value, you can indicate what value you want the stepper to initially display.



![Stepper](images/forms/stepper.png)



