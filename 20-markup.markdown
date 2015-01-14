#ChocolateChip-UI

##Markup

ChocolateChipUI uses the following HTML5 tags for creating all of its layouts and widgets:

- article
- nav
- section
- div
- aside
- header
- footer
- p
- span
- a
- ul
- li
- h1 - h5
- input
- textarea
- select

Except for the `a` tag and `span` tag, these are block elements. By adding classes to these tags, ChocolateChip-UI provides them with additional properties for layout and user interaction. You can easily extend ChocolateChip-UI by creating your own classes for these tags to add new layouts, widgets or behaviors.

###Right-to-Left Alphabets

If you need to support right-to-left alphabets, such as Arabic, Farsi, Urdu or Hebrew, all you have to do is add the attribute `dir="rtl"` to the HTML tag of your app. ChocolateChip-UI will automatically adjust your layouts, navigation and user interactions for proper behavior for a right-to-left scenario. Please be aware that JavaScript does not allow mathematical operations on non-ascii numbers. So, if you need to do so, you'll need to handle it with normal numbers first, then convert them to the numerical system appropriate to the alphabet you are using. Or you can just leave the numbers in their default. If your numbers are being switched around in the wrong order, put them in their own span and define their `dir` as "ltr":


```
<h2>السائر: <span dir='ltr'>1-8</span> حد</h2>
```

###article

This is the basic building block of ChocolateChip-UI. It is used to create the software equivalent of a view. Think of it as everything on the screen between the navbar and the bottom of the screen. If your app is very simple, you may only need one article tag. However, if you have various screens that you want to present to the user, you'll need to use a tab bar interface or a navigation list. These allow the user to switch from one article to another with animations appropriate to the device platform.

When using a navigation list, ChocolateChip-UI attaches three class to the articles to indicate their navigation state: previous, current, next. Similarly, the tab bar interface attaches the two classes, current and next. As a developer, you don't have to do anything to make this happen, the interface handles this for you automatically.


```
<article id='main' class='current'>
   <!-- article content here -->
</article>
<article id='pictures' class='next'>
   <!-- article content here -->
</article>
```

Every article should have a unique id so that it can be identified by the navigation system. These must be valid HTML ids. At load time, ChocolateChip-UI checks to see if you have manually set the navigation state of your articles. If not, it will set the first article as current and the others as next. This will mean that initially your app may momentarily load in a state of disarray before it is arranged properly. As such, we strongly recommend that you always put a state on each article so that it loads cleanly and orderly.

###nav (navbar)

The nav tag creates a navbar used to hold the article's title, as well as any interactive buttons which you may require for its article:


```
<nav class='current'>
   <h1>My App</h1>
   <button id='editButton'>Edit</button>
</nav>
```

At load time, ChocolateChip-UI creates a global navbar that it prepends to your app as the first child of the body tag. This is the visible background for all you other navbars. They get positioned over it. You can see it in the Web inspector. It's the only nav that has an id of `globalNav` and has no content.

To associate a nav with an article, simply place it before its article. An app with multiple articles will have this kind of structure:

```
<body>
   <nav class='current'>
      <h1>My App</h1>
   </nav>
   <article id='main' class='current'>
      <!-- Content here -->
   </article>
   <nav class='next'>
      <h1>My App</h1>
   </nav>
   <article id='Music' class='next'>
      <!-- Content here -->
   </article>
   <nav class='next'>
      <h1>My App</h1>
   </nav>
   <article id='Movies' class='next'>
      <!-- Content here -->
   </article>
</body>
```

###section

The section tag is always the first child of an article. It provides the vertical scrolling when the content exceeds the viewable screen. The section tag is the container for all the content that you will present to the user.

####current, previous, next

These are the classes that ChocolateChip-UI uses to manage the visible appearance of your layouts. They are used on articles, navs, and paging sections. (See the article examples above for usage.)

###Container Layout

The following classes are used for designating layout containers. You can use these on a section or div tag.

####horizontal

When put on a block element, this class will display its contents distributed horizontally.

####vertical

When put on a block element, this class will display its contents stacked vertically.

####centered

When used with the horizontal class, this class will center the contents horizontally in the container.

####scroller-vertical

When put on a div or other block element, this class will make the container scroll vertically.

####scroller-horizontal

When put on a div or other block element, this class will make the container scroll horizontally.

###h1 (navbar title)

This is the title in the navbar.

###h2 (list title)

Use this as the title of a list. Place it directly befor the list that it represents.


```
<h2>Fruits</h2>
<ul class='list'>
   <li>Apples</li>
   <li>Orange</li>
   <li>Bananas</li>
</ul>
```


###h3 (list item title)

Use this as the title of a list item.


```
<ul class='list' id='vegetables'>
   <li>
      <h3>Letuce</h3>
   </li>
   <li>
      <h3>Tomatoes</h3>
   </li>
   <li>
      <h3>Onions</h3>
   </li>
</ul>
```


###h4 (list item subtitle)

Use this as the subtitle of a list item.


```
<ul class='list' id='vegetables'>
   <li>
      <h3>Letuce</h3>
      </h4>Type: leaf</h4>
   </li>
   <li>
      <h3>Tomatoes</h3>
      </h4>Type: fruit</h4>
   </li>
   <li>
      <h3>Onions</h3>
      </h4>Type: root</h4>
   </li>
</ul>
```


###h5

You can use this for any other lower titling needs you might have.

###normal-case

By default the h2 before a list is always in uppercase. Putting this class on it will force it to display normal case:


```
<h2 class='normal-case'>Fruits</h2>
<ul class='list'>
   <li>Apples</li>
   <li>Orange</li>
   <li>Bananas</li>
</ul>
```


###button

As of version 3.8.0 a button is created by using a button element. Earlier versions used an anchor tag with the class button. Regardless of the version, the button will be styled appropriately for the device's operating system.


```
<button>Whatever</button>
```

####Back &amp; BackTo Buttons

For navigation purposes you may need to display back buttons. You can use the following classes on your buttons to do so. They will get the appropriate look for the device operating system:

- `back` (used to return to previous article)
- `backTo` (used for non-linear returns)

The `back` and `backTo` buttons look identical on all platforms. There purpose is different. The back button automatically returns the user to the previous article. The backTo button does not. It is used for returning the user to an arbitrary position in the navigation history according to the need. For example, if the users have navigated through several screens of a process and you want to enable them to return to the beginning, you would use the backTo button along with a bound event that would trigger the navigation to a specific article using the method `$.UINavigateBackToArticle()`. Read the tutorial on navigation to see how to implement it.

A back button:


```
<nav class='current'>
   <button class='back'>Back</button>
   <h1>Candies</h1>
</nav>
```

Or a backTo button:

```
<nav class='current'>
   <button class='backTo'>Back</button>
   <h1>Candies</h1>
</nav>
```

When you put a button in the navbar and want it to be flush against the right side of the screen, you can put the class `align-flush` on it, when text direction is set to right-to-left it will be aligned to the left:


```
<nav class='current'>
   <button class='back'>Back</button>
   <h1>Songs</h1>
   <button class='align-flush'>Edit</button>
</nav>
```

###action button

On iOS buttons have lost their background colors and borders. They merely exhibit their text. You can override this by giving your buttons the class `action`. This gives them a rounded border and forces them to stretch to fill the available space. The border color is set by the value of LESS variable `@actionButton` in the theme's color.less file. On Android and Windows Phone 8 action buttons will appear like regular buttons. If you have color needs apart from these, you can define a class for you buttons, based on the existing class definitions, and adjust the colors for borders, backgrounds and text.


```
<div>
   <button class='action'>Buy</button>;
   <button class='action'>Cancel</button>;
</div>
```


This will put the button flush to the right. On a right-to-left alphabet, this will instead put the button flush to the left.

###p (list detail)

The paragraph tag is used in a list item as the item detail, as well as after a list as a footer. It can also be used as the message container in a popup or sheet. You can put more than one p tag in a list item or after a list to accomodate your needs.

###list

The ul tag (unordered list) is the element used for displaying tabular data. To use it for layout always put the class `list` on it. Otherwise it will appear as a normal unordered list with bullets.


```
<ul class='list'>
   <li>Apples</li>
   <li>Orange</li>
   <li>Bananas</li>
</ul>
```


###li

The list item is the cell for your data. You can put the following elements in them and they will stack vertically in order: img, h2, h3, p.


```
<ul class='list'>
   <li>
      <img src='/images/photos/me.jpg' width='100'>
      <h3>Me</h3>
      <h4>Thing I like</h4>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea.</p>
   </li>
</ul>
```


If you want to make a simple navigation list, just put the `nav` class on the list items to indicate that. On iOS these display the disclosure indicator (chevron). On Android and Windows Phone 8 these are ignored. Note that the new design direction from Google called Material Design does have what are called "see mores", which are basically identical to the iOS navigation indicators. So if you are using a theme that supports that view, you will get them on Android.


```
<ul class='list'>
   <li class='nav'>
      <h3>People</h3>
   </li>
   <li class='nav'>
      <h3>Places</h3>
   </li>
   <li class='nav'>
      <h3>Things</h3>
   </li>
</ul>
```


You can also make a list item show a detail disclosure by putting the class `show-detail` on it:


```
<ul class='list'>
   <li class='show-detail'>
      <h3>People</h3>
   </li>
   <li class='show-detail'>
      <h3>Places</h3>
   </li>
   <li class='show-detail'>
      <h3>Things</h3>
   </li>
</ul>
```

###comp (composite layout)

If you have layout needs that are more complicated than the simple stacking order of the default list item, you can put the `comp` class on the list item. Comp stands for complex or composite. This makes the list item spread its items across horizontally, allowing you to create columns inside the list item. To do this you can use several combinations of `asides` and `divs`. The `aside` is for secondary data and the `div` is for the main data:


```
aside - div
div - aside
aside - div - aside
```


Note that with the comp list item style, the last aside always displays its content horizontally. This is done using flexible box model styles. If comp lists do not cover your layout needs, create some classes to style the layouts as needed. 

Here are some examples of comp list layouts:


```
<li class='comp' data-goto='people'>
   <div>
      <h3>People</h3>
      <h4>Your Friends</h4>
   </div>
   <aside>
      <span class='nav'></span>
   </aside>
</li>
```


In the above example, notice that the aside has a span with a class of `nav`. On iOS this will be vertically centered to the height of the list item. On Android and Windows Phone 8 this will not be displayed.


```
<li class='comp'>
   <div>
      <h3>Places</h3>
      <h4>Interesting Destinations</h4>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore.</p>
   </div>
   <aside>
      <span class='nav'></span>
   </aside>
</li>

<li class='comp'>
   <aside>
      <img title='Hurry and Harm' src="../images/music/Hurry and Harm.png" height="80px">
   </aside>
   <div>
      <h3>The Hurry and the Harm</h3>
      <h4>Hurt</h4>
   </div>
   <aside>
      <span class='show-detail'></span>
   </aside>
</li>
```


###show-detail

Sometimes you want to let the user know that there is some detail information that they can see by tapping a list item. For this you can put the `show-detail` class on the list item, or you can put a span with the class as the last item in the list item:


```
<li class='comp'>
   <aside>
      <img title='Imagine Dragons' src="../images/music/Imagine Dragons.png" height="80px">
   </aside>
   <div>
      <h3>Imagine Dragons</h3>
      <h4>Radioactive</h4>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea.</p>
   </div>
   <aside>
      <span class='show-detail'></span>
   </aside>
</li>
```


###switched

In certain situations you might want to stress a list item's subtitle over its title. You can do this by putting the `switched` class on the list item. This will visually switch the two around so that the subtitle comes first and is larger than the title while displaying them on the same line:


```
<li class='switched'>
   <h3>People</h3>
   <h4>Your Friends</h4>
</li>
```

###nav (class)

This is a class, not the element. You put it on a list item when it will navigate to another article. This will display the iOS left chevron automatically. No navigation marker will be shown on Android or Windows, as they do not use these affordances. If you are doing a comp layout, you can put the 'nav' class on a span and put that in the last aside. This will result in the chevron being centered vertically to the height of the list item, which looks better for list items with a lot of content.

Here's the nav class directly on a list:


```
<li class='nav'>
   <h3>Deserts</h3>
</li>
```


Here's the nav class on a span in a comp layout:


```
<li class='comp'>
   <div>
      <h3>Recipes</h3>
   </div>
   <aside>
      <h4>Deserts</h4>
      <span class='nav'></span>
   </aside>
</li>
```


###counter/date-time

ChocoalteChip-UI provides two classes to handle displaying special data types in your list items: `counter` and `data-time`. Counter is used to display a numerical value, such as the number of messages associated with that list item. `Date-time `is used to display a date or time value. This could be numerical or text. To implement either of these, put their class on a span in the last aside. You can have them both in the same aside if needed. See the examples below:

####Date-time:


```
<li class='comp'>
   <div>
      <h3>To Do</h3>
   </div>
   <aside>
      <h4>Apointments</h4>
      <span class='date-time'>Monday</span>
      <span class='nav'></span>
   </aside>
</li>

<h2>Shows</h2>
<li class='comp'>
   <div>
      <h3>Comedy</h3>
   </div>
   <aside>
      <h4>SNL</h4>
      <span class='date-time'>8:00 PM</span>
      <span class='nav'></span>
   </aside>
</li>
```


####Counter with nav


```
<li class='comp'>
   <div>
      <h3>Recipes</h3>
   </div>
   <aside>
      <h4>Deserts</h4>
      <span class='counter'>43</span>
      <span class='nav'></span>
   </aside>
</li>
```


####Combination date-time and counter:


```
<li class='comp'>
   <aside>
      <img src="../images/ios/icon-weather.png" height='40px'>
   </aside>
   <div>
      <h3>Weather</h3>
   </div>
   <aside>
      <h4>Alerts</h4>
      <span class='counter'>2</span>
      <span class='nav'></span>
   </aside>
</li>
```


When the content exceeds the width of the container it will be clipped and display an ellipsis. For ellipsis to work the container must have some kind of width declared. It will not work if the width is set to auto.


###busy

You never need to put a busy indicator tag in your document. ChocolateChip-UI has a special method to do that: `$.UIBusy()`. This method will output a div with that class for iOS and Android that gets styled to look like the native activity indicator of each platform. On Windows Phone 8, ChocolateChip-UI outputs a native Windows 8 progress bar. You can provide a color for you indicators that will be applied appropriately for each operating system. See the Chapter "Widgets" for more details.

###segmented

You can create a segmented control by putting the class `segmented` on a div. It should have at least two children, which buttons. Please be aware that mobile devices in portrait orientation may not be able to display segmented controls with more than three buttons. If you need to show more than three or four items, consider using a Paging control. At load time, ChocolateChip-UI initializes any segmented control markup so that the buttons select and deselect properly. You can also attach a delegate event to the segmented control to handle user interaction.

You can also create a segmented control dynamically using the `$.UICreateSegmented()` method. See ChUI.js for more details.

Example:


```
<div style='' class='horizontal centered'>
   <div class='segmented'>
      <button>Radioactive</button>
      <button>Hurt</button>
      <button>Permanent</button>
   </div>
</div>
```

###paging

The class `paging` is used to implement a paging control. You put this on a segmented control in the article's navbar. You must also indicate the orientation for the paging control. You have two choices: horizontal or vertical. Here is what it should look like:


```
<nav>
   <h1>Paging</h1>
   <div class='segmented paging horizontal align-flush'>
      <button title='previous panel'></button>
      <button title='next panel'></button>
   </div>
</nav>
```


For this to work, ChocolateChip-UI expects that the nav's article will have a series of sections that you want to page through. Depending on whether you put horizontal or vertical, the sections will slide in and out horizontally or vertically. There is no limit to how many sections the control can page. In the example below you can see the structure for the article.


```
<article id="main" class="current paging">
   <section>
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


After completing your markup, you need to initialize it with the method `$.UIPaging()`. Since the paging control requires initialization to work, you could create all the parts dynamically, then execute `$.UIPaging()`. Please read the chapter "Navigation" for more details about setting up a paging control.


###popup

The popup control is created dynamically by the `$.UIPopup` method. It creates a div with the class 'popup'. It has two states, indicated by the following clases: opened, closed.

The popup has a header tag with an `h1` inside it for the title. There is a div with the class `panel` that holds a p tag. This is where the popup's message is displayed. Lastly is a footer tag which holds at least one button for closing the popup. You could also initialize it with a second button to perform some action. This control gets created with the $.UIPopup method. Read the chapter 'Widgets' for more information about popups.

###popover

The popover is a type of menu control. You create it using the `UIPopover` method. It's really just an empty container for you to present the user with a series of actionable items. On iOS it has the typical appearance of a popover with a pointer off the top. On Android it resembles a typical drop down spinner. On Windows Phone 8 it resembles a typical popup menu. The popover has a nav with an `h1` tag for its title, this is for the default behavior expected for an iOS popover. This header is not displayed on Android or Windows Phone 8. Lastly, it has a section tag. This will be a vertically scrollable area. You can append whatever you want in it. Use a delegated event to handle the user interactions. See the chapter "Widgets" for more details.

###mask

A div with the class `mask` will cover the screen and be semi-transparent and click-proof. You create it using the `$.UIBlock()` method. You can specify the opacity. This gets created automatically for you when you create a popup, popover or modal busy control. See the chapter "Widgets" for more details about `$.UIBlock`.

###select list

When a list has the class `select`, you are able to click on its list items to select them. A select list is really just a fancy way of displaying a group of radio buttons. As a matter of fact, in the background ChocolateChip-UI adds a radio button to each list item of a select list. You need to initialize a select list with the method `UISelectList()`. Putting the class `select` on the list is optional. If you execute UISelectList on a list and it doesn't have the class, it will add it. This method creates selection indicators appropriate for each operating system. See the chapter "Widgets" for more details.

###deletable list

Another type of list allows the user to delete list items. This could be any list. You use the method `$.UIDeletable` to set that up. It creates a deletion indicator for each list item, along with a delete button. These all get styled appropriate for the operating systems. When a list is initialized for allowing deletable items, it has the class `deletable`. See chapter "Widgets" for more details.

###switch

The switch control is an inline element. You can create one manually using a span with the class `switch`, and including an input of type checkbox inside the span. See the markup below:


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
```


You can also create switches dynamically using the `UISwitch` method. See the chapter "Widgets" for more details.


###stepper

A stepper is a control for offering the user a very small set of values to manipulate. It is not appropriate for long value lists. In that case you should use the HTML select element with options for values. This will display the appropriate popup for each operating system.

A stepper is just a span with the class `stepper`:


```
<span class='stepper' id="digits"></span>
```


You need to initialize it with the `UIStepper` method, which allows you to define its start and end values. This will create an increase and decrease button with a label to display the current value between them. See the chapter "Widgets" for more details.

###tabbar

A tab bar is a div with the class `tabbar`. You create a tab bar using the `UITabbar` method. On iOS the tab bar is at the bottom of the screen. To adjust the articles to its presence, the method also adds a class `hasTabBar` to the body tag. ChocolateChip-UI's CSS will then adjust everything for you. For Android and Windows Phone 8, the tab bar gets placed at the top, and ChocolateChip-UI adjusts your layouts to manage that properly.

The tab bar holds up to five buttons. You designate these in the options passed to the `UITabbar` method. You can also indicate icons to use for each button. On iOS the tab bar usually displays an icon with the button text. You can supply that icon information when you execute the method.
###Icons in Tab Bar Buttons

On Android and Windows Phone 8, both Microsoft and Google have designed their tab bars to have no icons. The tabs are plain text only. On iOS tab bar buttons are expected to have icons. ChocolateChip-UI checks to see if the device is running desktop Safari or iOS. If it is, in injects a span with the class icon inside of each tab button. You can use this along with the `button` class to define a background image. You can use a png, or SVG, or event a symbol font. Go to the chapter "Themes" to read about styling icons.

###Toolbar

A toolbar can accompany an article tag. You designate a toolbar by using a div with the class `toolbar`. The toolbar should be the first tag after the article it belongs to. You can use a toolbar on any layout, except for tab bar apps, as the tab bar already occupies the place where the toolbar would be.

If the article and its nav have a class of `current`, the toolbar should have a class of `current` too, overwise it should have a class of `next` like other articles and navbars.

A toolbar is a convenient container to hold buttons and icons that the user can use to interact with content in an article.


```
<article>
  <section>
  <!-- Your content here -->
  </section>
</article>
<div class="toolbar next">
  <button>Food</button>
  <button>Ingredients</button>
</div>
```

###range

The range control is a normal HTML5 range input. Just put an input of type `range`. Set its value, as well as its min and max values. If you want it to increase and decrease in set amounts, you can also provide a step attribute. See the examples below:


```
<input type="range" id="rangeControl1" value="10" min="0" max="40">
<input type="range" id="rangeControl2" step='5' value="20" min="0" max="40">
```


You can attach an event listener for `input` to get the value when it changes. ChocolateChip-UI styles these so that they look appropriate for the device operating system. See the chapter "Widgets" for more information.

###sheet

ChocolateChip-UI provides a generic sheet control. This is an overlay that slides over the screen. This is a container for you to provide the user with actions or options to affect the content currently on the screen. It consists of a div with the class `sheet` and has two children: a div of class `handle` and a section. The handle is interactive. If the user touches it, the sheet will close. The section tag creates a vertically scrollable container. You can put anything in it that you want: lists, buttons, etc.

You create a sheet using the `$.UISheet()` method. Then populate it by inserting markup in its section tag:


```
$.UISheet();
   $('.sheet').find('section').append("<ul class='list'></li>");
   $('.sheet .list').append("<li><button>Save</button></li><li><button>Delete</button></li><li><button>Cancel</button></li>");
   $('.sheet .list').append ('<h2 style="text-align: center; margin: 20px;">The End</h2>');

$('.sheet .list').on('singletap', 'button', function() {
   $.UIHideSheet();
});
```


You would show the sheet using any user initiated event, probably with a button. See ChUI.js for more details on how to create sheets.

###slide-out

A slide-out is a special type of app layout which offers the user a slide out menu as the means to toggle the content on the screen. You would use this type of app layout instead of a navigation list or tab bar. It is initialized at load time with the `$.UISlideout` method. Structurally, you build your app like you would for a multi-article app, using navbars followed by articles. The `$.UISlideout` method will create a button with the class `side-out-button` which it inserts in the global navbar, making it available at all times. When the user presses this button, the menu panel will slide out from the side.

The slide-out is a div with the class `slide-out` and a section tag within. This section is the container for what the slide out will display. The slide out can hold anything you want, however, as a navigation control you would do best by doing the following. After creating the slide out, populate it with a list or lists. If you have more than one list, consider giving each list a corresponding h2 as its title. That way you can create a title list of available choices. On each list item you'll need to put the attribute: `data-show-article`. This attribute is the id of the article that a list item will reveal when the user presses it.


```
<h2>Music</h2>
<ul class='list'>
   <li data-show-article='ImagineDragons'>
      <h3>Imagine Dragons</h3> 
   </li>
   <li data-show-article='WillyMoon'>
      <h3>Willy Moon</h3> 
   </li>
   <li data-show-article='Kiss'>
      <h3>This Kiss</h3> 
   </li>
</ul>
```


You initialize a slide-out with the `$.UISlideout()` method. This will create the basic slide out, its toggle button and the toggling of articles when the user presses a list item with an attribute of `data-show-article` inside of it.

###splitlayout

ChocolateChip-UI provides a split layout specifically for tablets. To create a split layout, you first put the class `splitlayout` on the body tag. Then you put in a nav and an article with the class `master`, followed by another navbar and an article with the class `detail`.

The master article is for the user to select different items to see in detail, which will be displayed in the detail article. Examine the following example carefully:


```
<body class='splitlayout'>
   <nav>
      <h1>Pick One</h1>
   </nav>
   <article class='master'>
      <section id="scroller2">
         <ul class='list'>
         </ul>
      </section>
   </article>
   <nav>
      <h1 id='detailTitle'>Detail 1</h1>
   </nav>
   <article class='detail' class='master'>
      <section>
         <ul class='list'>
      </section>
   </article>
</body>
```



###Grids

ChocolateChip-UI version 3.5.1 has CSS Grids to enable you to create more complex layouts. These are specifically for use on tablets. Most apps targetting phones will be fine with the layouts provided by lists and comp lists.

ChocolateChip-UI uses the flexible box model to create grids. This means you should be aware of two things. 

1. You're layouts will be proportinal to the available screen real estate. 
2. Browsers may not align rows of grids to pixel perfection.

ChocolateChip-UI uses a series of classes to enable you do define your grids. The basic build block is the class `grid`. Putting this class on a div will mark it as a container for a series of columns arranged in a row side by side.


```
<div class="grid"></div>
```


You create columns by putting the class `col` on a tag:


```
<div class="grid">
   <div class="col"></div>
   <div class="col"></div>
   <div class="col"></div>
</div>
```


You define the width of columns using a flex class. The grid row is designed to hold up to ten columns, so the maximum sum of all your flex values must not be greater than ten. If there are more than can fit on the row, they will extend off to the right. You can change this behavior by added the class `wrap` to the grid.

The available flex classes are:

- flex-1
- flex-2
- flex-3
- flex-4
- flex-5
- flex-6
- flex-7
- flex-8
- flex-9
- flex10


###Equal Heigth Columns

By default if one column has more content than others and is therefore taller, the other columns will stretch to match its height.

A grid with ten columns equal width:


```
<div class="grid">
   <div class="col flex-1"></div>
   <div class="col flex-1"></div>
   <div class="col flex-1"></div>
   <div class="col flex-1"></div>
   <div class="col flex-1"></div>
   <div class="col flex-1"></div>
   <div class="col flex-1"></div>
   <div class="col flex-1"></div>
   <div class="col flex-1"></div>
   <div class="col flex-1"></div>
</div>
```


A grid with 3 columns equal width:


```
<div class="grid">
   <div class="col flex-1"></div>
   <div class="col flex-1"></div>
   <div class="col flex-1"></div>
</div>
```


Other possible column width proportions:


```
<div class="grid">
 <div class="col flex-6"><p>6</p></div>
 <div class="col flex-4"><p>4</p></div>
</div>

<div class="grid">
 <div class="col flex-7"><p>7</p></div>
 <div class="col flex-3"><p>3</p></div>
</div>

<div class="grid">
 <div class="col flex-8"><p>8</p></div>
 <div class="col flex-2"><p>2</p></div>
</div>
```


To allow colums that exceed the grid width to wrap to the next row, just put the class `wrap` on the grid. Please note that the columns will exapnd to fill their available space based on their flex values.


```
<div class="grid wrap">
   <div class="col flex-1"><p>1</p></div>
   <div class="col flex-1"><p>1</p></div>
   <div class="col flex-1"><p>1</p></div>
   <div class="col flex-4"><p>4</p></div>
   <div class="col flex-5"><p>5</p></div>
   <div class="col flex-3"><p>3</p></div>
   <div class="col flex-1"><p>1</p></div>
   <div class="col flex-1"><p>1</p></div>
</div>
```


###Fixed Width Columns

You can also create a mix of flexible and fixed width columns. Just put a class on the columns you want to have a fixed with and define that width. They will retain that width, forcing the other columns to adjust their width to the available space.

Please take a look at grid.html in the examples to see the different ways that you can implement grids.