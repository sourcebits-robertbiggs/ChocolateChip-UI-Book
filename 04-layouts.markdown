#ChocolateChip-UI

##Markup & Themes

The beautiful layouts and interface artifices in ChocolateChip-UI are created by a combination of HTML5 markup and CSS3 themes. The CSS is created by the LESS preprocessor. It is therefore easy to modify one of the LESS files and regenerate a theme to create a custom look. 

In order to have a decent app you need a way to present your data in a convenient, understandable and useful manner to the user. ChocolateChip-UI solves this for you by providing layouts. These are implemented with a few standard HTML5 tags. 

To start with, we need to consider the way a standard mobile app is layed out. Take the following picture of iOS:

![Stardard App Interface](images/layouts/screen_layout-1.png)

![Stardard App Interface](images/layouts/screen_layout-2.png)

In the above images you can see certain repeating components:

1. A header with a title and other optional content or controls.
2. A main content aread extending downward with content displayed in lists.
3. A bottom tabbar or footer for controls that act on the content in the main area.

To create the typical header, body and toolbar combination so common for apps, ChocolateChip uses groups of 3 tags: a nav, an article and a div with the class toolbar or tabbar. Your header title is just and h1. Your main content is an article. Inside that is a section tag, which automatically makes its content scrollable with a vertical swipe gesture. You put all the content inside the section tag. Table lists are just unordered list with the class 'list'. Toss your content into list items. 

There are a number of meta tags that make a Web page mobile friendly. Below is a barebones example of how to put together some ChUI markup for a very simple app. Please note that the paths to the framework files will be different depending on how you set up your project.


```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="msapplication-tap-highlight" content="no">
  <title>ChocolateChip-UI iOS</title>
  <link rel="stylesheet" href="chui/chui-ios-3.5.4.css">
  <script src="http://code.jquery.com/jquery-2.0.3.min.js"></script>
  <script src="chui/chui-3.5.4.js"></script>
</head>
<body>
  <nav class='current'>
    <h1>Title</h1>
  </nav>
  <article id='main' class='current'>
    <section>
    </section>
  </article>
</body>
</html>
```

In the above markup you will notice that we put the class `current` on both the nav and the article. In this type of situation where there is only one nav and one article, this class is not that important. The class 'current', is used to let ChocolateChip-UI know which pair of navs and article should be visible, and which ones should be hidden from view until the user navigates to them. If at page load no nav and article are found to have the class "current", ChocolateChip-UI will automatically add it to the first occuring nav/article pair in the markup. Although ChocolateChip-UI will do this for you automatically, where you have multiple sets of navs and articles, you want to indicate which pair should be displayed as current to prevent some funny visual glitches happening during page load. Notice that the section tag is empty. This is where we will be putting our content. In the next part we will look at how to create lists of content using unordered HTML lists.

Here is what the above markup would look like. As you can see, there is no content, but the header with its title is in place:

![Basic App Shell](images/layouts/simple_lists/basic_app_shell.png)

###Simple List Layouts

To create a list, all you do is add the class `list` to an unordered list, then put your content into its list items. ChocolateChip-UI uses h3 tags to indicate the main list item title. Here's the simplest list layout you can have:

```
<ul class='list'>
  <li>
    <h3>People</h3>
  </li>
  <li>
    <h3>Places</h3>
  </li>
  <li>
    <h3>Things</h3>
  </li>
</ul>
```

The above type of markup would create a list like this:

![simple list with titles](images/layouts/simple_lists/simple_list_title.png)

This is the simplest of lists, just a title in the list item. Such a list might be to show the user a series of simple data. Or these might be a series of actionable items that would do something in the app.


####Navigation List

Often lists are navigable. On iOS navigability is indicated by a chevron pointing in the direction that navigation will transition to. For left-to-right languages, this will be a chevron pointing to the right. For right-to-left languages, this will be pointing to the left. To create the navigation indicator on a simple list, put the class `nav` on the list items that will be navigable.

On Android and Windows Phone 8 it is generally not acceptable to use visual markers to indicate navigation, although Google's latest design guides for Material Design have "see more" indicators like iOS navigation indicators that indicate the list item will navigate to a detail view of the item. In the end this is the same thing that iOS navigation list items are doing. When Android and Windows 8 do not show navigation indicators, they use a slight change in text color to show that the list item will trigger an navigation action.


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

The above markup would create a list like this for iOS:

![Simple List with Navigation Indicators](images/layouts/simple_lists/simple_list_nav.png)

####List with Subtitles

By their very nature, simple list just stack up their contents, one on top of the other. They have no special layout capabilites. For that you would need to use the comp list layouts explained further ahead.

For list item titles you use h3 tags for subtitles, use h4 tags. Just put the h4 content directly after the h3.

```
<ul class='list'>
  <li class='nav'>
    <h3>People</h3>
    <h4>Your Friends</h4>
  </li>
  <li class='nav'>
    <h3>Places</h3>
    <h4>Interesting Destinations</h4>
  </li>
  <li class='nav'>
    <h3>Things</h3>
    <h4>Stuff that's Just Weird</h4>
  </li>
</ul>
```

![Simple List with Subtitles](images/layouts/simple_lists/simple_list_subtitle.png)

####List with Item Detail

You might also want to provide a detail explanation about each list item. You do this by putting the content in a paragraph tag. You put the paragraph tag after the title and subtitle. You can put several paragraph tags, one after the other, if needed.

```
<ul class='list'>
  <li class='nav'>
    <h3>People</h3>
    <h4>Your Friends</h4>
    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea.</p>
  </li>
  <li class='nav'>
    <h3>Places</h3>
    <h4>Interesting Destinations</h4>
    <p>Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo.</p>
  </li>
  <li class='nav'>
    <h3>Things</h3>
    <h4>Stuff that's Just Weird</h4>
    <p>Nemo enim ipsam voluptatem quia voluptas sit aspernatur aut odit aut fugit, sed quia consequuntur magni dolores eos qui ratione voluptatem sequi nesciunt. Neque porro quisquam est, qui dolorem ipsum quia dolor sit amet.</p>
  </li>
</ul>
```

![Simple List with Item Detail](images/layouts/simple_lists/simple_list_item_detail.png)

####List with Icons &amp; Titles

If you need to show icons for some type of data, maybe apps or whatever, you can do so easily. In this case put your icon first, followed by the list item title. To output and icon, use a span tag with a class of `icon`. You will also need to give each icon another class or id to identify them uniquely. This is so you can define the CSS to show their icons. Below is the markup for the list:

```
<ul class='list'>
  <li>
    <span class='icon weather'></span>
    <h3>Weather</h3>
  </li>
  <li>
    <span class='icon music'></span>
    <h3>Music</h3>
  </li>
  <li>
    <span class='icon mail'></span>
    <h3>Mail</h3>
  </li>
</ul>
```

And here are the styles for the icons. As you can see, you need to define a background image for each icon. By using the background size property, you can create icons that can scale to different sizes depending on device screen size and media queries.

```
<style>
  .icon {
    display: block;
    height: 40px;
    width: 40px;
    background-repeat: no-repeat;
    background-position: center center;
    background-size: auto 70%;
  }
  .icon.weather {
    background-color: #29abe2;
    background-image: url(../images/app-icons/weather.png);
    background-size: 70% auto;
  }
  .icon.music {
    background-color: #f38133;
    background-image: url(../images/app-icons/music.png);
  }
  .icon.mail {
    background-color: #52c5d0;
    background-image: url(../images/app-icons/mail.png);
    background-size: 70% auto;
  }
</style>
```

The combination of the markup and CSS will give you the following list:

![Simple List with Icons &amp; Titles](images/layouts/simple_lists/simple_list_icon_title.png)

####List with Images &amp; Titles

Most often you will need to show images of things in your list layouts. For simple list this is very easy. Just put the image tags right before the list title:

```
<ul class='list'>
  <li>
    <img title='Blues' src="../images/music/Willy Moon.jpg" height='100px'>
    <h3>Willy Moon</h3>
  </li>
  <li>
    <img title='Classical' src="../images/music/The Olms.jpg" height='100px'>
    <h3>The Olms</h3>
  </li>
  <li>
    <img title='iTunes' src="../images/music/Kiss.jpg" height='100px'>
    <h3>Kiss</h3>
  </li>
</ul>  
```

If you have different sized images, you can use some CSS to make them more consistent for a better layout result. You might try give all the images the same height and allowing their widths to extend automatically. That way all your list items will have consisten heights.

```
.list > li > img {
  height: 60px;
  width: auto;
}
```

Here's how our image list will look:

![Simple List with Images &amp; Titles](images/layouts/simple_lists/simple_list_image_title.png)

####List with Images, Titles, Subtitles &amp; Details

We can also include subtitles and item details with images for a more full presentation of each item. Just put the image tag first, followed by the list title, subtitle and detail:

```
<ul class='list'>
  <li>
    <img title='Blues' src="../images/music/Willy Moon.jpg" height='100px'>
    <h3>Willy Moon</h3>
    <h4>Yeah Yeah</h4>
    <p>Temporibus autem quibusdam et aut officiis debitis aut rerum necessitatibus.</p>
  </li>
  <li>
    <img title='Classical' src="../images/music/The Olms.jpg" height='100px'>
    <h3>The Olms</h3>
    <h4>Wanna Feel It</h4>
    <p>Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium.</p>
  </li>
  <li>
    <img title='iTunes' src="../images/music/Kiss.jpg" height='100px'>
    <h3>Kiss</h3>
    <h4>This Kiss</h4>
    <p>At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias excepturi sint occaecati cupiditate non provident.</p>
  </li>
</ul>   
```

![Simple List with Images, Titles, Subtitles &amp; Details](images/layouts/simple_lists/simple_list_image_item_detail.png)

####List with Detail Indicator

Like navigation indicators, ChocolateChip-UI also provides detail indicators. These indicate that when the user interacts with the list item, they will reach a page with more details about that list item. Unlike the navigation chevrons, these indicators will also be displayed on Android and Windows Phone 8.


```
<ul class='list'>
  <li class='show-detail'>
    <img title='Blues' src="../images/music/Willy Moon.jpg" height='100px'>
    <h3>Willy Moon</h3>
  </li>
  <li class='show-detail'>
    <img title='Classical' src="../images/music/The Olms.jpg" height='100px'>
    <h3>The Olms</h3>
  </li>
  <li class='show-detail'>
    <img title='iTunes' src="../images/music/Kiss.jpg" height='100px'>
    <h3>Kiss</h3>
  </li>
</ul>  
```

![Simple List with Images, Titles &amp; Detail Indicators](images/layouts/simple_lists/simple_list_detail_indicator.png)

####List with Header

If you want to have a header or title before your lists, you can do so by just putting and h2 before each list.

```
<h2>
  Basic list header with a bunch of stuff in here
</h2>
<ul class='list'>
  <li>
    <h3>People</h3>
  </li>
  <li>
    <h3>Places</h3>
  </li>
  <li>
    <h3>Things</h3>
  </li>
  <li>
</ul> 
```

By default, the table header gets cut off if it is longer than the available space. You can make it so that it wraps if you want. You can change that behavior by putting the class `wrap` on the h2:

```
<h2 class='wrap'>
  Basic list header with a bunch of stuff in here
</h2>
<ul class='list'>
  <li>
    <h3>People</h3>
  </li>
  <li>
    <h3>Places</h3>
  </li>
  <li>
    <h3>Things</h3>
  </li>
  <li>
</ul>
```

You can also for the table headers to not be all uppercase by putting the class `normal-case` on the h2:

```
<h2 class='normal-case'>
  Basic list header with a bunch of stuff in here
</h2>
<ul class='list'>
  <li>
    <h3>People</h3>
  </li>
  <li>
    <h3>Places</h3>
  </li>
  <li>
    <h3>Things</h3>
  </li>
  <li>
</ul>
```

![Simple List with Header](images/layouts/simple_lists/simple_list_header.png)

####List with Footer

You can also provide your lists with footers. These are important information about the content of the list that you display at the bottom of the list. You can have multiple footers after a list as well. To create a list footer, just use a paragraph tag right after the list:

```
<ul class='list'>
  <li>
    <h3>People</h3>
  </li>
  <li>
    <h3>Places</h3>
  </li>
  <li>
    <h3>Things</h3>
  </li>
</ul>
<p>A quick summary about all of the items in this list. You can put as much as you like in here and the text will simply wrap around.</p>
<p>To create a footer, just put a paragraph tag  right after the list. And yes, as you can see, you can put multiple paragraphs after the list for more complex list footers.</p>
```

![Simple List with Footer](images/layouts/simple_lists/simple_list_footer.png)

###Comp List Layouts

Simple lists are easy to implement and for many situations they may provide for you layout situations. However, because they only stack their content vertically, you may find them limited when you want to present data in more complex ways. To accomodate those situations, ChocolateChip-UI provides the comp list layouts. 

To create comp lists you use an unordered list with a class of `list` just like for simple lists. But you put the class `comp` on each list item:

```
<ul class='list'>
  <li class='comp'></li>
  <li class='comp'></li>
  <li class='comp'></li>
</ul>  
```

When a list item has a class of `comp` it displays its contents differently than a simple list does. The comp list item is configured to display its contents in columns, side by side. A comp list item can have two types of containers for columns:

- div
- aside

You can combine these in three ways:

- aside + div
- div + aside
- aside + div + aside

```
<ul class='list'>
  <!-- aside + div -->
  <li class='comp'>
    <aside></aside>
    <div></div>
  </li>
  <!-- div + aside -->
  <li class='comp'>
    <div></div>
    <aside></aside>
  </li>
  <!-- aside + div + aside -->
  <li class='comp'>
    <aside></aside>
    <div></div>
    <aside></aside>
  </li>
</ul> 
```

Lets take a look at what these various combinations can do. If we choose a combination of `aside + div`, whatever we put in the aside will be centered horizontally and stacked vertically from the bottom up. Whatever is put in its following div will be stacked vertical and aligned to the left.

If we use a `div + aside` combo, the div will behave the same as the div in the previous combination—stacked and aligned to the left. However, when the aside comes after the div, it centers its content horizontally, like the version when the aside comes first, but its content gets centered vertically.

Comp list layouts are rendered the same across mobile platforms. The only difference is in colors, padding, margins, and navigation indicators are always hidden on Android and Windows Phone 8. 

Using these combinations enables you to create a wide range of complex layouts. Lets see what we can do with these.

####Comp List with Nav Indicators

You might have noticed a problem with the navigation indicators in the simple list examples. They are pinned to the top left of the list item, even when there is more vertical content. Using the combination of a div for content and an aisde for a navigation indicator we can create layouts where the indicators always stay vertically centered, no matter how tall the list item is. Here's the markup. Note that to make a navigation indicator, we put the class `nav` on a span tag and put that in the last aside of a list item:

```
<ul class='list'>
  <li class='comp'='people'>
    <div>
      <h3>People</h3>
      <h4>Your Friends</h4>
    </div>
    <aside>
      <span class='nav'></span>
    </aside>
  </li>
  <li class='comp'='places'>
    <div>
      <h3>Places</h3>
      <h4>Interesting Destinations</h4>
    </div>
    <aside>
      <span class='nav'></span>
    </aside>
  </li>
  <li class='comp'='animals'>
    <div>
      <h3>Animals</h3>
      <h4>Critters from Around the Globe</h4>
    </div>
    <aside>
      <span class='nav'></span>
    </aside>
  </li>
</ul> 
```

The nice thing about this method is that the resulting navigation indicator will always be perfectly centered vertical, regardless the height of the list item.

![Comp List with Navigation Indicators](images/layouts/comp_lists/comp_list_nav-1.png)


####Comp List with Title, Subtitle and Detail

In order to create a comp list with a combination of title, subtitle and detail, put them in a div.

```
<ul class='list'>
  <li class='comp'>
    <div>
      <h3>People</h3>
      <h4>Your Friends</h4>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</p>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</p>
    </div>
    <aside>
      <span class='nav'></span>
    </aside>
  </li>
  <li class='comp'>
    <div>
      <h3>Places</h3>
      <h4>Interesting Destinations</h4>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</p>
    </div>
    <aside>
      <span class='nav'></span>
    </aside>
  </li>
  <li class='comp'>
    <div>
      <h3>Funny Animals</h3>
      <h4>Critters from Around the Globe</h4>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</p>
    </div>
    <aside>
      <span class='nav'></span>
    </aside>
  </li>
</ul>  
```


![Comp List with Navigation Indicators](images/layouts/comp_lists/comp_list_nav-2.png)


####Comp List with Images

To incorporate images into a comp list, put them in the first aside. Put your title, subtitles and item details in the following div.

```
<ul class='list'>
  <li class='comp'>
    <aside>
      <img title='Imagine Dragons' src="../images/music/Imagine Dragons.jpg" height="80px">
    </aside>
    <div>
      <h3>Imagine Dragons</h3>
      <h4>Radioactive</h4>
      <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea.</p>
    </div>
  </li>
  <li class='comp'>
    <aside>
      <img title='Hurry and Harm' src="../images/music/Hurry and Harm.jpg" height="80px">
    </aside>
    <div>
      <h3>The Hurry and the Harm</h3>
      <h4>Hurt</h4>
      <p>Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo.</p>
    </div>
  </li>
  <li class='comp'>
    <aside>
      <img title='Permanent' src="../images/music/Permanent.jpg" height="80px">
    </aside>
    <div>
      <h3>David Cook</h3>
      <h4>Permanent</h4>
      <p>Nemo enim ipsam voluptatem quia voluptas sit aspernatur aut odit aut fugit, sed quia consequuntur magni dolores eos qui ratione voluptatem sequi nesciunt. Neque porro quisquam est, qui dolorem ipsum quia dolor sit amet.</p>
    </div>
  </li>
</ul>
```

Here's what that would look like:

![Simple List with Images, Titles and Subtitles](images/layouts/comp_lists/comp_list_images.png)

####Comp List with Detail Indicator

Add a detail indicator to a comp list item is similar to doing a navigation indicator. You put a span with the class 'show-detail' inside the last aside tag in a comp list item:

```
<ul class='list'>
  <li class='comp'>
    <aside>
      <img title='Imagine Dragons' src="../images/music/Imagine Dragons.jpg" height="80px">
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
  <li class='comp'>
    <aside>
      <img title='Hurry and Harm' src="../images/music/Hurry and Harm.jpg" height="80px">
    </aside>
    <div>
      <h3>The Hurry and the Harm</h3>
      <h4>Hurt</h4>
      <p>Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo.</p>
    </div>
    <aside>
      <span class='show-detail'></span>
    </aside>
  </li>
  <li class='comp'>
    <aside>
      <img title='Permanent' src="../images/music/Permanent.jpg" height="80px">
    </aside>
    <div>
      <h3>David Cook</h3>
      <h4>Permanent</h4>
      <p>Nemo enim ipsam voluptatem quia voluptas sit aspernatur aut odit aut fugit, sed quia consequuntur magni dolores eos qui ratione voluptatem sequi nesciunt. Neque porro quisquam est, qui dolorem ipsum quia dolor sit amet.</p>
    </div>
    <aside>
      <span class='show-detail'></span>
    </aside>
  </li>
</ul>
```

As you can see in the following image, the detail indicators get centered vertically to the comp list item, regardless of the height.

![Comp List with Images, Titles &amp; Detail Indicators](images/layouts/comp_lists/comp_list_detail_indicators-1.png)

####Comp List with Title and Subtitle on Same Level

Sometimes you may want to have the list item title and subtitle on the same level. Using the comp list layout, this is easy to accomplish. Just put the list title in a div, and the subtitle in the last aside. In the following markup, note that we also have the navigation indicator in the last aside as well. They will be rendered side by side.

```
<ul class='list'>
  <li class='comp'='people'>
    <div>
      <h3>People</h3>
    </div>
    <aside>
      <h4>Your Friends</h4>
      <span class='nav'></span>
    </aside>
  </li>
  <li class='comp'='places'>
    <div>
      <h3>Places</h3>
    </div>
    <aside>
      <h4>Destinations</h4>
      <span class='nav'></span>
    </aside>
  </li>
  <li class='comp'='animals'>
    <div>
      <h3>Animals</h3>
    </div>
    <aside>
      <h4>Funny Critters</h4>
      <span class='nav'></span>
    </aside>
  </li>
</ul> 
```

And here's the result:

![Comp List with Titles and Subtitle on Same Level](images/layouts/comp_lists/comp_list_title_subtitle.png)


####Comp List with Title and Subitle Switched

There's a certain style of layout where you actually want to emphasize the list item subtitles over the titles. You do that by using the `switched` layout class. Instead of putting a comp class on the list item, put the class `switched` on it. Then, put your markup in the normal order. The swtiched class will cause ChocolateChip-UI to visually render the title and subtitle switched around, with the subtitle slightly larger and the title text in the app's tint color.

```
<ul class='list'>
  <li class='switched'>
    <h3>People</h3>
    <h4>Your Friends</h4>
  </li>
  <li class='switched'>
    <h3>Places</h3>
    <h4>Interesting Destinations</h4>
  </li>
  <li class='switched'>
    <h3>Funny Animals</h3>
    <h4>Critters from Around the Globe</h4>
  </li>
</ul> 
```

This will give us:


![Comp List with Title and Subtitle Switched](images/layouts/comp_lists/comp_list_switched.png)


####Comp List with Date/Time Indicators

If you need to display a date or time value in your list items, you can use the `date-time` class. Just put a span with the date-time class in the last aside. If you also need to have navigation indicators, make sure the date-time tag comes first. See the following markup:

```
<ul class='list'>
  <li class='comp'>
    <div>
      <h3>People</h3>
    </div>
    <aside>
      <h4>Your Friends</h4>
      <span class='date-time'>Monday</span>
      <span class='nav'></span>
    </aside>
  </li>
  <li class='comp'>
    <div>
      <h3>Places</h3>
    </div>
    <aside>
      <h4>Interesting Destinations</h4>
      <span class='date-time'>3/14/10</span>
      <span class='nav'></span>
    </aside>
  </li>
  <li class='comp'>
    <div>
      <h3>Funny Animals</h3>
    </div>
    <aside>
      <h4>Critters from Around the Globe</h4>
      <span class='date-time'>12:30 PM</span>
      <span class='nav'></span>
    </aside>
  </li>
</ul>
```
This will produce the following:


![Comp List with Date/Time Indicators](images/layouts/comp_lists/comp_list_date-time.png)

####Comp List with Counters

If you need to show some type of numerical information about the contents in a list item, you can use the `counter` class on a span in the last aside. Like the data-time class, you want this to be first, before either a navigation or detail indicator.

```
<ul class='list'>
  <li class='comp'>
    <div>
      <h3>People</h3>
    </div>
    <aside>
      <h4>Your Friends</h4>
      <span class='counter'>22</span>
      <span class='nav'></span>
    </aside>
  </li>
  <li class='comp'>
    <div>
      <h3>Places</h3>
    </div>
    <aside>
      <h4>Destinations</h4>
      <span class='counter'>32</span>
      <span class='nav'></span>
    </aside>
  </li>
  <li class='comp'>
    <div>
      <h3>Animals</h3>
    </div>
    <aside>
      <h4>Funny Critters</h4>
      <span class='counter'>5</span>
      <span class='nav'></span>
    </aside>
  </li>
</ul>  
```
This will give us:

![Simple List with Counter Indicators](images/layouts/comp_lists/comp_list_counters.png)


####Comp List with Icons

If you need to display some icons as part of the content of a list item, just put a span with the class `icon` in the first aside of the list item. You will also need some classes to identify each icon so that you can style the background image that the icon will display:

```
<ul class='list'>
  <li class='comp'>
    <aside>
      <span class='icon weather'></span>
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
  <li class='comp'>
    <aside>
      <span class='icon music'></span>
    </aside>
    <div>
      <h3>Music</h3>
    </div>
    <aside>
      <h4>New Purchases</h4>
      <span class='counter'>16</span>
      <span class='nav'></span>
    </aside>
  </li>
  <li class='comp'>
    <aside>
      <span class='icon mail'></span>
    </aside>
    <div>
      <h3>Mail</h3>
    </div>
    <aside>
      <h4>New</h4>
      <span class='counter'>134</span>
      <span class='nav'></span>
    </aside>
  </li>
</ul> 
```

To make this list render the icons properly, we could use the following CSS. If you need the style to work across platforms, you can use the `.isiOS` and `.isAdroid` and `.isWindows` classes to do something specific to each platform.

```
<style>
  .icon {
    height: 40px;
    width: 40px;
    background-repeat: no-repeat;
    background-position: center center;
    background-size: auto 70%;
    border-radius: 10px;
  }
  .isWindows .icon {
    border-radius: 0;
  }
  .isAndroid .icon {
    border-radius: 50px;
  }  
  .icon.weather {
    background-color: #29abe2;
    background-image: url(../images/app-icons/weather.png);
    background-size: 70% auto;
  }
  .icon.music {
    background-color: #f38133;
    background-image: url(../images/app-icons/music.png);
  }
  .icon.mail {
    background-color: #52c5d0;
    background-image: url(../images/app-icons/mail.png);
    background-size: 70% auto;
  }          
</style>
```

This list would look like this:

![Simple List with Icons](images/layouts/comp_lists/comp_list_icons.png)


###Buttons: Types and Placement

Besides layouts, ChocolateChip-UI provides a number of button types and ways of positioning them in layouts. A button is just an anchor tag with the class button. 

####Default Buttons

Depending on what platform you are on, you will get the default look. The markup is very simple:

```
<ul class='list'>
  <li>
    <a href="#" class='button'>Button</a>
  </li>
</ul>
```

We put the button in an empty list item. You could also put it in the navbar out on the page somewhere, or in a toolbar at the bottom. Below is what this will look like on each platform:

Android:

![Basic Button: Android](images/layouts/comp_lists/button-1.png)

iOS:

![Basic Button: iOS](images/layouts/comp_lists/button-2.png)

Windows Phone 8:

![Basic Button: Windows Phone 8](images/layouts/comp_lists/button-3.png)


####Back Buttons

When implementing navigation list, you will need a back button. This shows the user that by tapping it they will return to the previous article. Back buttons are almost always going to be found in the top navbar of your app, to the left of the title. On a right-to-left setup the back button would be displayed to the right of the title.

You don't have to worry about implementing this button's look. Just put the class `back` on an anchor with the class `button` and you'll get a proper back button for each platform. In the markup below, notice that the back class comes after the button class:

```
<nav class='next'>
  <a href='#' class='button back'>My App</a>
  <h1>People</h1>
</nav>
<article id="people" class='next'>
  <section>
    <h2>This is a detail page about people.</h2>
    <ul class='list' >
      <li>
        <h3>People, what they do</h3>
        <p>Lorem ipsum dolor sit amet.</p>
      </li>
    </ul>
  </section>
</article>
```

On iOS the back button has a chevron with the title of the previous article:

![Back Button: Android](images/layouts/comp_lists/back_button-1.png)

On Android the back button has a back arrow:

![Back Button: iOS](images/layouts/comp_lists/back_button-2.png)

On Windows Phone 8 the back button only displays a circle with a back arrow inside it:

![Back Button: Windows Phone 8](images/layouts/comp_lists/back_button-3.png)

For certain situations you might need to set up a form of navigation with the user can hit a back button and return to a non linear place in the navigation history. To make this easier, ChocolateChip-UI has a special class `backTo`. You use this the same as the back class. These buttons render identical to normal back buttons.

To learn more about how back buttons work with navigation, please read the chapter "Navigation Lists".

####Action Buttons

ChocolateChip-UI provides a special type of button called an action button. These will usually sit by themselves on the screen. Their purpose is to enable the user to perform some very important function. You implement them by putting the class `action` on an anchor tag:

```
<article id='main' class='current'>
  <section>
    <ul class='list'>
      <li>
        <h3>Item</h3>
      </li>
    </ul>
    <br><br>
    <a id='goToLast' href='javascript:void(null)' class="button action">Straight to Detail Article</a>
  </section>
</article>
```

Here's how action buttons look on Android:

![Action Button: Android](images/layouts/comp_lists/action_button-3a.png)

![Action Button: Android](images/layouts/comp_lists/action_button-3b.png)


Here's how action buttons look on iOS:

![SAction Button: iOS](images/layouts/comp_lists/action_button-1a.png)

![Action Button: iOS](images/layouts/comp_lists/action_button-1b.png)


Here's how action buttons look on Windows Phone 8:

![Action Button: Windows Phone 8](images/layouts/comp_lists/action_button-2a.png)

![Action Button: Windows Phone 8](images/layouts/comp_lists/action_button-2b.png)

###Grids

To use ChocolateChip-UI's grids, you need to understand some of the concepts of the flex box model. ChocolateChip-UI provides several classes that enable you to create flex grids using a series of divs. These grids allow you to create a series of rows with columns.

ChocolateChip-UI has a module for CSS grids. These use the latest flex box specification with fallbacks for earlier non-standard versions implimented by the browser vendors. Each row is a flex container that arranges its contents—your columns—horizontally. Columns are indicated by using the class `col`. All columns in a grid row will stretch to be equal to the height of the tallest column. How much a column flexs to fill the available horizontal space is determined by its second class, which starts with flex followed by a numerical value. The possible classes for columns are:

- .flex-1
- .flex-2
- .flex-3
- .flex-4
- .flex-5
- .flex-6
- .flex-7
- .flex-8
- .flex-9
- .flex-10


ChococlateChip-UI grids are designed to accomodate up to ten columns in a row. As a matter of fact, the best way to figure out the widths of your columns is to make the math so that they add up to 10. Here are some possible combinations you could use:


```
<div class="grid">
  <div class="col flex-1"><p>1</p></div>
  <div class="col flex-1"><p>1</p></div>
  <div class="col flex-1"><p>1</p></div>
  <div class="col flex-1"><p>1</p></div>
  <div class="col flex-1"><p>1</p></div>
  <div class="col flex-1"><p>1</p></div>
  <div class="col flex-1"><p>1</p></div>
  <div class="col flex-1"><p>1</p></div>
  <div class="col flex-1"><p>1</p></div>
  <div class="col flex-1"><p>1</p></div>
</div>

<div class="grid">
  <div class="col flex-2"><p>2</p></div>
  <div class="col flex-2"><p>2</p></div>
  <div class="col flex-2"><p>2</p></div>
  <div class="col flex-2"><p>2</p></div>
  <div class="col flex-2"><p>2</p></div>
</div>

<div class="grid">
  <div class="col flex-3"><p>3</p></div>
  <div class="col flex-2"><p>2</p></div>
  <div class="col flex-2"><p>2</p></div>
  <div class="col flex-3"><p>3</p></div>
</div>

<div class="grid">
  <div class="col flex-4"><p>4</p></div>
  <div class="col flex-2"><p>2</p></div>
  <div class="col flex-2"><p>2</p></div>
  <div class="col flex-2"><p>2</p></div>
</div>

<div class="grid">
  <div class="col flex-5"><p>5</p></div>
  <div class="col flex-5"><p>5</p></div>
</div>

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

<div class="grid">
  <div class="col flex-9"><p>9</p></div>
  <div class="col flex-1"><p>1</p></div>
</div>

<div class="grid">
  <div class="col flex-10"><p>10</p></div>
</div>
```

The way the flex box model works, if the number of items don't fit the available horizontal space, they extend out past the edge of the container. It therefore takes some care to get your columns right to avoid this. If, however, your columns are being generated dynamically and you don't know how many there will be, you can force them to wrap. Just put the class `wrap` on the grid itself:

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

When all the columns don't fit on a row, the grid will wrap the ones that don't fit to the next row. Their widths will therefore be calculated based on the available space on that row, not the previous. 

![Example of Grids](images/layouts/comp_lists/grids-1.png)

####Grids With Gutters

You can put gutters around your columns. ChocolateChip-UI has two classes for gutters:

- .gutter-5
- .gutter-10

You put one of these classes on the grid holding the columns. 

```
<!-- Grids with a 5px gutter -->
<div class="grid center gutter-5">
  <div class="col flex-3"><p>Centered: 3</p></div>
  <div class="col flex-2"><p>Centered: 2</p></div>
</div>
```

```
<!-- Grids with a 10px gutter -->
<div class="grid center gutter-10">
  <div class="col flex-2"><p>Centered: 2</p></div>
  <div class="col flex-3"><p>Centered: 3</p></div>
</div>
```

####Centered Columns

You can center columns in a grid row by putting the class `center` on the grid:


```
<div class="grid center gutter-5">
  <div class="col flex-3"><p>Centered: 3</p></div>
</div>

<div class="grid center gutter-5">
  <div class="col flex-4"><p>Centered: 4</p></div>
  <div class="col flex-4"><p>Centered: 4</p></div>
</div>

<div class="grid center gutter-5">
  <div class="col flex-3"><p>Centered: 3</p></div>
  <div class="col flex-2"><p>Centered: 2</p></div>
</div>
```

####Fixed Width Columns

While grid columns have flexible widths, if you need, you can designate a fixed width for a column. In such a case the other columns will flex to take up the space that that column does not.


```
<div class="grid wrap gutter-10">
  <div class="col fixed1">
    <p>Fixed Width Column</p>
  </div>
  <div class="col"><p>Dynamic Width Column</p></div>
</div>
```

For the above markup to create a fixed width column, we need to define a width for column "fixed1". To do that, we need to override its flex property by setting it to "none", followed by its width:

```
.fixed1 {
  -webkit-box-flex: none;
  -webkit-flex: none;
  -ms-flex: none;
  flex: none;
  width: 400px;
}
```
![Example of Grids with Margins and Centered](images/layouts/comp_lists/grids-2.png)

