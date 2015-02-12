
#ChocolateChip-UI

##Navigation

###Navigation Lists

ChocolateChip-UI provides a convenient solution for implementing navigation lists. This is a common way for uses to get to deeper screens of information. Unlike Web pages, where navigation leads to a new page and subsequent page load, ChocolateChip-UI apps are a single page. The user transitions from one part of the page to another. The navigation module creates the experience of navigating to another place just like in native apps.

Navigation is from one article tag to another. During this process, the current article and its nav slide off out of view while the destination article and its nav slide into view. You don't need to do anything to make this happen other than set up your articles for navigation.

ChocolateChip-UI uses the HTML5 data attribute to mark a list item as navigable. Its name is `data-goto`. This attribute indicates the id of the article to which the list item will navigate. List items with this attribute will also have hover states on the desktop and touch states on mobile devices.

Notice in the example below we have the data-goto attribute pointing to an article with an id of "people". We also put the class "nav" on the list item. On iOS this displays the right pointing chevron, which iOS uses to mark navigable items. This gets ignored on Android and Windows Phone 8 since these use no markers for navigation.

```
<li class='nav' data-goto="#people">
    <h3>People</h3>
</li>
```

Here's a comp list item leading to the a detail article for a song. You would probably want one detail page for all the songs and load the correct data into it based on what list item the user pressed. You could accomplish this by putting an id or a data attribute with the information to identify each song whose detail information you will need to show. This would be easy if your data structure already has a unique id for each song:

```
<li class='comp' data-goto="#songDetail" data-id='Radioactive'>
    <aside>
        <img title='Imagine Dragons' src="../images/music/Imagine Dragons.png" height="80px">
    </aside>
    <div>
        <h3>Imagine Dragons</h3>
        <h4>Radioactive</h4>
    </div>
    <aside>
        <span class='show-detail'></span>
    </aside>
</li>
```

With navigation lists you can create any number of drill down events. However, your users will probably get irritated if they are required to drill down past two or three screens on a regular basis to accomplish their tasks.

####Navigation Events

When the users selects an item in a navigation list, a transition occurs as the target is brought into view. When the target article finishes transitioning, the `navigationend` event is fired. You can register a `navigationend` event on any article in a navigation list to listen for this event. By examing the event.target, you can get information about the current article:


```
$(function() {
   $('#people').on('navigationend', function(e) {
      alert(e.target.id) // alerts "people"
   });
});
```

ChocolateChip-UI also uses a pub/sub pattern to publish the ids of all the articles involved in any navigation. These are available through the following four channels:

- 'chui/navigate/leave'
- 'chui/navigate/enter'
- 'chui/navigateBack/leave'
- 'chui/navigateBack/enter'

By subsribing to these channels, you can decouple your code and listen for which articles the user is leaving and entering and act accordingly. 


```
$(function() {
  // Function for revealing article:
  var subscribeArticleEnter = function(topic, id) {
     var article = $('#' + id).prev().find('h1').text();
     alert('You just entered ' + article);
  }
  // Function for leaving article:
  var subscribeArticleLeave = function(topic, id) {
     var article = $('#' + id).prev().find('h1').text();
     alert('You are leaving  ' + article);
  };
  // Handle entering a article:
  var enterArticleSubscription = $.subscribe('chui/navigate/enter', subscribeArticleEnter);
  // Handle leaving a article:
  var leaveArticleSubscription = $.subscribe('chui/navigate/leave', subscribeArticleLeave);
});
```


####Dynamic Navigation Lists

In the previous examples for navigation lists the content was static. This means that for each item in your navigation list you would need another static destination. If you have a lot of items to navigate to you will quickly find that you have an app with a lot of markup to load. This means a slow load time. It also means the browser needs to load all of that markup into memory, leaving less memory for your app. This could result in a terrible slow down in your app's performance, causing stuttering during navigation and other types of interactions. Unless you app has just a few screens, you want to look into loading the destinations dynamically.

To make this approach work you need to have all the items in the navigation list lead to the same article. When the user selects a list item, you populate the target article with the appropriate content. From the user's perspective, every time they choose something, they're navigating to a totally new destination, when in reality they're always landing on the same desitination article with new content.

We're going to need templating to make this work. To learn more about ChocolateChip-UI's templating engine, please read the chapter "Templates".

We're going to look at what it takes to make a dynamic navigation list for some recipes. The markup for this will be quite simple:


```
<nav class="current">
  <h1>Recipes</h1>
</nav>
<article id="recipes" class="current">
  <section>
    <ul class="list" id="recipesList">
      <li data-goto="detail" id="ItalianStyleMeatloaf" class="nav">
        <h3>Italian Style Meatloaf</h3>
      </li>
      <li data-goto="detail" id="ChickenMarsala" class="nav"><h3>Chicken Marsala</h3>
      </li>
      <li data-goto="detail" id="ChickenBreastsWithLimeSauce" class="nav">  <h3>Chicken Breasts with Lime Sauce</h3>
      </li>
      <li data-goto="detail" id="LemonRosemarlySalmon" class="nav">
        <h3>Lemon Rosemary Salmon</h3>
      </li>
      <li data-goto="detail" id="StrawberryAngleFood" class="nav">
        <h3>Strawberry Angel Food Dessert</h3>
      </li>
    </ul>
  </section>
</article>
<nav class="next">
  <button class="back">Recipes</button>
  <h1 id='recipeTitle'>Recipe</h1>
</nav>
<article id="detail" class="next">
  <section>
    <h2>Ingredients</h2>
    <ul class="list"><li><ul id='ingredients'></ul></li></ul>
    <h2>Directions</h2>
    <ul class="list"><li><ol id='directions'></ol></li></ul>
  </section>
</article>
```

When a user taps on a list of recipes, we want to grab the value of the `data-goto`
property so that we can fill out the destination article. Before we can do anything we need to get references to a few elements, define the template for the data we will need to show and also get the data we need from the server:


```
var templ8 = '<li><h3>[[=data]]</h3></li>';
var tmpTempl8 = $.template(templ8);
var recipesList = $('#recipesList');
var recipes;
$.ajax({
  url : 'https://quack-quack-quack-quack.com/recipes.js',
  type: 'GET',
  success: function(data) {
    recipes = JSON.parse(data);
    recipes.forEach(function(ctx, idx) {
      recipesList.append(tempList(ctx));
    });
  }
});
```

We need to register an event on the main list so we can find out which list item the user tapped. With that information we can get the id of the list item to know which chunk of data we need to display in the detail page. When the user taps a list item, we get the id of that list item which corresonds to the id of the recipe object. We can then use the array filter method to return only the recipe with that id. After plucking the recipe from the data, we can use our template `templ8` to render its values to the detail page. This all happens very quickly and efficiently in real time. 


```
recipesList.on('singletap', 'li', function() {
  var recipeID = $(this).attr('id');
  var recipe = recipes.filter(function(ctx) {
    return ctx.id === recipeID;
  });
  $('#ingredients').empty();
  recipe[0].ingredients.forEach(function(ctx) {
    $('#ingredients').append(tmpTempl8(ctx))
  })
  $("#directions").empty();
  recipe[0].directions.forEach(function(ctx) {
    $('#directions').append(tmpTempl8(ctx))
  })
  $('#recipeTitle').html(recipe[0].title);  
});

```

In action, our navigation list might look like this:

![Recipes Navigation List Main Page](images/first-app/recipes-1.png)

![Recipes Navigation List Detail Page](images/first-app/recipes-2.png)

For a more indepth explanation of how this works, please read the chapter on templates. This example will be explained fully there.

###Slide-outs

Another way to allow your users to navigate the content of your app is to use a slide out menu. Slide-outs are easy to implement. All you need is a group of articles you want to present to your users. The slide-out gets created at load time by an init script. 

First you need to fire the $.UISlideout method. This will create an empty slide-out. You'll find a slide-out button on the top left of your app. Tapping it will reveal an empty slide-out. Tapping the button again will close the slide-out. Obviously having an empty slide-out isn't very useful. ChocolateChip-UI provides a method that you can use to populate the slideout. In the following code we populate the slide-out with the $.UISlideout.populoate method. We pass it an array of objects. These objects consist of two values — an id for the article that will be shown when the user taps that slide-out item, and the text that will be show in the slide-out menu for that selection.

```
$(function() {
  // Create an empty slide out menu:
  $.UISlideout();

  // Populate the slide out:
  $.UISlideout.populate([
    { music: 'Music' },
    { pictures: 'Pictures' },
    { recipes: 'Recipes' },
    { contacts: 'Contacts' }
  ]);
});
```

This would create the following markup for the slide-out:

```
<div class="slide-out open">
  <section>
    <h2>Your Stuff</h2>  
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
  </section>
</div>
```

Notice the attributes `data-show-article`. These indicate the article that will be shown when the user taps. Slide-outs are that simple. On an app with its dir attribute set to "rtl" for right-to-left languages, the slide-out will appear form the right side.


Below is a side-out ready to show. Notice the "hamburger" icon button in the top left.

![Slide-out Menu](images/navigation/slide-out-1.png)

Opened slide-out menu showing the links that will reveal the various articles.

![Slide-out Menu](images/navigation/slide-out-2.png)

You can also set up a slide-out to load content dynamically instead of having a separate item for each slide-out item, you would have one target which the slide-out will populate depending on which item the user selects. To learn more about this technique, please read "Dynamic Split-layout" in the chapter "Navigation".

To learn how to load content dynamically when the user selected a slide-out menu item, please see the chapter "Templates".

###Tab Bars

ChocolateChip-UI provides the tab bar module as a convient way to implement an app with a limited set of screens through which the user can toggle. Unlike a navigation list, which allows the user to drill down to other screens, the tab bar interface presents the user with a small set of screens that are toggled by pressing the tab bar buttons. If you have used the iTunes app on iOS, this is the same experience.

The tab bar interface is implemented differently for each operating system. On iOS the tab bar is always at the bottom of the screen and its buttons have text and icons. Android and Windows Phone 8 always display the tab bar at the top of the screen and without icons. If you are using the new Material Design theme for Android, you did get the option to display either text or images. When you initialize the tab bar interface, ChocolateChip-UI takes care of making the appropriate adjustments for each OS.

Be aware that the portrait mode of mobile devices is quite limited as far as avaialable horizontal space is concerned. As such we recommend you do not try to put more than five tabs into a tab bar app. If you need more than five tabs, consider making your last tab a paging control. That will allow the user to page through multiple sections within that same article. See the paging tutorial for information about how to implement it. If you do implement a paging article, we strongly suggest you make it the last tab for a better user experience. Another option is to make the last tab load a navigation list. This will allow the user to drill down to deeper destinations while still maintaining the simplicity of the tab bar interace for the most commonly used screens.

The tab bar interface uses the same structure of navs and articles. In this case these which will constitute the screens which the tab bar toggles. In your script tag, include an initialization script to set the tab bar up. The tab bar method expects the following values:

- the number of tabs (an integer)
- the labels for the tabs (an array of quoted strings)
- the path to where you have icons for the tab buttons stored (a valid url)
- a name for each icon (this will allow you to style each tab button with an icon)
- a selected tab (one-based, if no value is provided, the first tab will be selected)

```
$(function() {
  var opts = {
    tabs : 5,
    icons : ["music", "pictures", "documents", "downloads", "favorites"],
    labels : ["Music", "Pictures", "Documents", "Downloads", "Favorites"],
    selected : 1
  };
  $.UITabbar(opts);
});
```

Based on the above code, here is some CSS to style the tab buttons with icons for iOS:

```
.tabbar > button.music > .icon  {
   -webkit-mask-image: url('../images/icons/Head_phones.svg');
}
.tabbar > button.pictures > .icon  {
   -webkit-mask-image: url('../images/icons/Camera.svg');
}
.tabbar > button.documents > .icon  {
   -webkit-mask-image: url('../images/icons/Documents.svg');
}
.tabbar > button.downloads > .icon  {
   -webkit-mask-image: url('../images/icons/Download.svg');
}
.tabbar > button.favorites > .icon  {
   -webkit-mask-image: url('../images/icons/Favorite.svg');
}    
```

You have several options of how to implement icons in the tab bar buttons. You could just use a colored png image. You could use a gray scale image as an mask image with a background color. This allows you to easily change the color of the icon on hover/touch. Or you could use an SVG image in either of these way. We recommend SVG for icons because they are vector-based, resolution independent and scalable. Please not that iOS has great support for image masks, Android has spotty support and Windows, none. Therefore you need to use background images for those platforms. You can consult the chapter "Themes" for more information about styling icons.

In action, our tab bar app might look like this:


![Tab bar, first tab ](images/first-app/tabbar-1.png)

![Tab bar, third tab](images/first-app/tabbar-2.png)

####Using a Nav List in a Tab Bar

If you have more screens that can fit in the limit of five tabs, you can make the last tab a navigation list. For the tab label put 'More' and for the icon name put 'more', ChocolateChip-UI will automatically provide the icon for this tab. Then, for that tab, put in a navigation list that directs the user to the extra screens. Because this is a navigation list, each article that it points to should have a 'Back' button in its navbar so that the user can return to the navigation list tab. Please see the navigation list section of this chapter.

The icon for the 'More' tab is an SVG data url on Android and iOS. On Windows it is part of the system font 'SegoeUI Symbol'. Here is how to change the color of the 'More' tab icon to whatever you want:

```
/* Android: */
.tabbar > button.more::before { 
  background-color: red;
}
/* iOS: */
.tabbar > button.more > .icon {
  background-color: red;
}
/* Windows Phone 8: */
.tabbar > button.more::before {
  color: red;
```

####Tab Bar Icons an iOS Feature

Tab bar icons will not be displayed on Android or Windows Phone 8. Icons in tab bar buttons is an iOS only feature. Changing the tab bar interface to show icons in the tab bar buttons on Android and Windows Phone 8 is a direct violation of the Human Interface Guidelines of both Google and Microsoft. However, if you add a 'More' tab for a navigation list, the 'more' icon will be displayed instead of the text label. On iOS, both the icon and the text will appear as with other tabs.



###Toggle Panels

Another type of navigation is limited to content within the same article. In this case the user is given the ability to toggle panels of content inside an article.

To set up togglable panels you need to execute the function `UIPanelToggle` on a segmented control and pass it some options to indicate the container of panels to toggle. Techinically, the segmented control could be anywhere on the page, same for the toggle panels. This expects the following markup: a container div, no particular class required. Within it should be a series of divs as containers for the content to be toggled. Each one of these child divs constitutes a togglable panel. The structure should be like this:

```
<article id="main" class="current">
   <section>
      <div class='horizontal centered'>
        <!-- segmented control for toggling panels  -->
        <div class='segmented'>
            <button>Radioactive</button>
            <button>Hurt</button>
            <button>Permanent</button>
        </div>
      </div>
      <!-- Container for togglable panels -->
      <div id="toggle-panels">
         <!-- togglable panel  -->
         <div>
            <ul class='list'>
               <li>
                  <h3>Imagine Dragons</h3>
                  <h4>Radioactive</h4>
               </li>
            </ul>
         </div>
         <!-- togglable panel -->
         <div>
            <ul class='list'>
               <li>
                  <h3>The Hurry and the Harm</h3>
                  <h4>Hurt</h4>
               </li>
            </ul>
         </div>
         <!-- togglable panel -->
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

To make the above markup work as a togglable panel control, we do the following:

```
$(function() {
   $('.segmented').UIPanelToggle('#toggle-panels',function(){$.noop;});
});
```

Please note that toggle panels are for when you have a small number of panels that you want to present to the user. If you have many panels, then you might be better off using the paging control described in the following section, or in a carousel which is presented after the secion on pagination.

The panels that you toggle could hold any content. Below is a very simple implementation of toggled panels:


![Toggle Panels, first panel](images/navigation/toggle-panel-1.png)

![Toggle Panels, second panel](images/navigation/toggle-panel-2.png)


###Paginators

ChocolateChip-UI provides a simple way to enable the user to page trough multiple sections of an article. In general, an article has but a single section, which implement's the vertical scrolling container for the article's content. But if you want to allow the user to access multiple sections of content within the same article, you can use the paging control.

The setup is very simple. You take a normal article and instead of one section, give it as many as you need. The paging control will take care of the paging for you. When using the paging control, each section can have whatever layout is suitable for the content it holds. Notice in the markup below how we've put a class of `paging` on the article. This lets ChocolateChip-UI know that this will be the article that contains the sections to paginate.

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

In that article's navbar put a segmented control with the extra class of `paging` and a class indicating the orientation for the paging: `horizontal` or `vertical`. Since you're putting the segmented control as the last item in the nav, you want to put the class align-flush on it as well:

```
<div class='segmented paging horizontal align-flush'>
   <button title='previous panel'></button>
   <button title='next panel'></button>
</div>
```

Then you just need to initialize the structure at load time:

```
$(function() {
   $.UIPaging();
});
```

Below are images of horizonal and vertical paginators:


![Horizontal Paginator](images/navigation/pagination-horizontal.png)

![Vertical Paginator](images/navigation/pagination-vertical.png)


###Carousels

ChocolateChip-UI also offers carousels with support for swipe gestures. These allow the user to quickly swipe through a large number of items, possibly images or other rich media.

The method `$.UISetupCarousel` creates a swipable carousel. It uses mouse gestures to enable desktop testing. It also adjusts direction automatically when the document direction is set to "rtl" for right-to-left alphabets (Arabic, Hebrew, Farsi, Urdu, etc.).

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

The target is a wrapper div that will hold the carousel. The carousel itself is just an unordered list. Each list item will be a panel in the carousel. You provide content for the panels by passing in an array of markup as strings. As long as the markup is valid and can fit within the panel, it should be fine. If you intend to put more content than can fit in a panel, just style the list items for the carousel so that they are scrollable like this:

```
.carousel-track > li {
  overflow-y: auto !important;
}
```

By default the carousel has its "loop" property set to cycle infinitely. If you want to make it only swipe between the first and last, but not past them, pass in a false value for the loop attribute:

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

When you initialize a carousel, you create a unique instance of $.UICarousel. A reference to this object is stored on the carousel wrapper indicated by the target attribute in the setup function. You can retrieve the instance of the carousel like this (assuming we have a carousel wrapper with the id "myCarousel":

```
var myCarousel = $('#myCarousel').data('carousel');

// Navigate to the fifth panel of the carousel:
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

Here's a carousel

![Carousel](images/navigation/carousel.png)



###Split-layouts

ChocolateChip-UI provides the split layout for tablets. This has two parts: master and detail. The master article is the area on the side with content or controls for the user to interact with. Those interactions will affect the content displayed in the detail article. You need to put the class split-layout on the body tag so that ChocolateChip-UI knows that it is dealing with a split layout. Each article also has its corresponding nav. Here is the typical structure:

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
          </ul>
      </section>
   </article>
</body>
```

ChocolateChip-UI will take care of making this layout display properly with orientation change. The master and detail articles have section tags within them that provide vertical scrolling when the content is greater than can fit on the screen.

Please note that this layout is static. Other than adjustments for orientation change, it provides no other functionality. Anything else you want to have happen will require writing code. The idea behind the split layout is that as the user selects something in the master article, it displays that in greater detail in the detail article. Open up the split layout file in the examples folder to see how it is implemented.

At page load you could perform an Ajax request for some JSON data that you use to populate the master article. You can easily create a simple template to do this. See the split layout example for details. Then you could just register a delegate event on the master list to insert live data into the detail:


```
$('.master ul').on('singletap', 'li', function() {
   // Handle the user interaction:
   $(this).siblings().removeClass('selected');
   $(this).addClass('selected');
   $('#detailTitle').text(this.textContent);
   var detail = $('.detail section');
   detail.empty();
   detail.UIBusy({'color':'#000', 'size': '100px'});
   var url = '../data/splitlayout/' + this.dataset.url + '.html';
   $.get(url, function(data) {
       detail.html(data);
   });
});
```

Here is what split layouts look like:


![Split layout landscape](images/navigation/split-layout-landscape.png)


![Split layout portrait](images/navigation/split-layout-portrait.png)

For a deeper look at how to handle dynamic content for the split-layout, please see the section Dynamic Split-layout in the chapter "Templates".


