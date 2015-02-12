#ChocolateChip-UI

##Your First App

In this chapter we're going to put together an app with several different navigation strategies. Which navigation approach you use depends on the information architecture your app is based on. Apps with a simple assortment of data might be fine with a tabbar interface or a slide-out menu. Deeply nested and complex data relations will require a navigation list system to access.


###App Shell

Regardless of what kind of app you're making, you're going to need a basic shell to get started. This consists the HTML tag, HEAD tag, BODY tag. The following example expects the app to use English, change the `lang` attribute's value to the language you actually use. Please note that you should organize the resources however you like. You do not need to follow the examples. You should use the structure and naming convention you are most comfortable with. In that case, change the path names below to match your actual project setup:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="msapplication-tap-highlight" content="no">
  <title>My Great App</title>
  <link rel="stylesheet" href="./css/chui-ios-3.6.0.css">
  <script src="./js/jquery-2.0.3.min.js"></script>
  <script src="./js/chui-3.6.0.js"></script>
</head>
<body>

</body>
</html>

```

Regardless of which approach you take to put together your app, you will need to use three main tags for each screen of your app: nav, article and div with the class toolbar. The minimal markup to create a "view" or screen is a nav and an article. Every article should have a unique and meaningful id to identify it. Navbars and toolbars do not need unique ids since they get associated with their corresonding article. If you need to access them from JavaScript, you can do so using the id of the article:

```
<nav>
	<h1>Navbar</h1>
</nav>
<article id="main">
	<section></section>
</article>

```

```
$(function() {
	// Change the article's navbar to red:
	$('#main').prev().css('background-color', 'red');
});
```

Notice in the above example that the article has a section tag. Every article must have a section tag as its first child. The section tag provides the scrolling mechanism for when there is more content than can fit on the screen. All of your content will go inside the section tag. It needs no class. As the first child of the article, it will automatically implement physics scrolling on desktop and mobile.

Notice that the navbar has an H1 tag. This serves as the title for that article. Each article should have a title informing the user where they are or what the purpose of that screen is. You might also have additional buttons in the navbar.



###Simple Navigation App

To make a navigation list, we need a list. The list items will point to where each item navigates to. If your content is limited and static, you can hard code these lists and their corresponding destinations as articles. If you need dynamic content, skip ahead to the part about "Dynamic Content".

####Static Content

The basis of layout in ChocolateChip-UI is the list. These are created using and unordered list tag with the class `list`. To give you greater control over the visual appearance of the content in the list items, it is recommended that you use the `comp` class on `li` tags. This allows you to create columns within each list item to organize the contents of the list. See the chapter "Layouts" for more information about the wide variety of layouts available.

ChocolateChip-UI uses the data attribue `goto` to indicate which article a navigation link should go to. Behind the scenes, at load time, ChocolateChip-UI scans your app and attaches an event listener that will fire the `$.UIGoToArticle()` method when the user interacts with that item. Of course, you could execute `$.UIGoToArticle()` from any element/button for the same result. To learn more about `$.UIGoToArticle()`, read about it in chapter "ChUIJS Documentation".

For iOS, navigable list items should show a chevron indicating the direciton the navigation will lead. ChocolateChip-UI lets you display that with the class `nav` on a list item. Since neither Android nor Windows Phone 8 uses navigation indicators, they ignore this class.



With the above understanding of how navigation works, we can put together the following code for a navigable list item:

```
<li class='comp' data-goto="apples">
  <div>
    <h3>Apples</h3>
  </div>
  <aside>
    <span class='nav'></span>
  </aside>
</li>
```

Using this as the template, we can put a whole list together. This would give us the first screen in our navigation list app:


```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="msapplication-tap-highlight" content="no">
  <title>Fruits</title>
  <link rel="stylesheet" href="./css/chui-ios-3.6.0.css">
  <script src="./js/jquery-2.0.3.min.js"></script>
  <script src="./js/chui-3.6.0.js"></script>
</head>
<body>
  <nav class="current">
    <h1>Fruits</h1>
  </nav>
  <article id="main" class="current">
    <section>
      <h2>Available Varieties</h2>
      <ul class='list' role='list'>
        <li class='comp' data-goto="apples">
          <div>
            <h3>Apples</h3>
          </div>
          <aside>
            <span class='nav'></span>
          </aside>
        </li>
        <li class='comp' data-goto="oranges">
          <div>
            <h3>Oranges</h3>
          </div>
          <aside>
            <span class='nav'></span>
          </aside>
        </li>
        <li class='comp' data-goto="bananas">
          <div>
            <h3>Bananas</h3>
          </div>
          <aside>
            <span class='nav'></span>
          </aside>
        </li>
        <li class='comp' data-goto="plumbs">
          <div>
            <h3>Plumbs</h3>
          </div>
          <aside>
            <span class='nav'></span>
          </aside>
        </li>
        <li class='comp' data-goto="pineapples">
          <div>
            <h3>Pineapples</h3>
          </div>
          <aside>
            <span class='nav'></span>
          </aside>
        </li>
      </ul>
    </section>
  </article>
</body>
</html>

```

You can view the above code in Chrome or Safari on [Codepen](http://codepen.io/rbiggs/pen/d8723943c9f32805a4c394fc0114379c).

Here's what it looks like:


![Navigation list, first screen](images/first-app/fruits-1.png)


You'll notice that if you mouse over the list items, you get hover effects. This is because of the presence of the `data-goto` attribute.

Of course, this list is useless unless it can actually go somewhere. For that to happen we need to code up some destinations. The `data-goto` attribute indicates what the destination should be for each list item. Since these are not yet visible but are upcoming, we give the navbar and article the class `next`. By putting and id of `apples` on the article, the list item with the `data-goto` value of `apples` will navigate to that article. To allow the user to navigate back from the Apples article, we can put an anchor tag with the classes `button back` in the navbar. ChocolateChip-UI will automatically handle the back navigation for you, no scripting necessary:


```
<nav class='next'>
  <button class="back">Fruits</button>
  <h1>Apples</h1>
</nav>
<article id="apples" class="next">
  <section>
    <h2>Uses</h2>
    <ul class="list">
      <li>
        <h3>Pies</h3>
      </li>
      <li>
        <h3>Juice</h3>
      </li>
      <li>
        <h3>Fruit Salad</h3>
      </li>
    </ul>
  </section>
</article>

```

You can see this live on [Codepen:](http://codepen.io/rbiggs/pen/fb94b0bcf14c61dc11092b2c41451e59?editors=100)


This second page, when we navigate to it, looks like this:


![Navigation list, first screen](images/first-app/fruits-2.png)

Based on this pattern, we can easily put together the more navbars and articles for the other navigable list items.

In the example below, we've updated the list item layout to include an image of each fruit and a subtitle:


```
<nav>
  <h1>Fruits</h1>
</nav>
<article class="current" id="fruits">
  <section>
    <h2>Available Varieties</h2>
    <ul class='list'>
      <li class="comp" data-goto='apples'>
        <aside><img src="./images/apple.png" alt="" height="60" /></aside>
        <div>
          <h3>Apples</h3>
          <h4>Lower Colesterol</h4>
        </div>
        <aside><span class="nav"></span></aside>
      </li>
      <li class="comp" data-goto='oranges'>
        <aside><img src="./images/orange.png" alt="" height="60" /></aside>
        <div>
          <h3>Oranges</h3>
          <h4>Immune Booster</h4>
        </div>
        <aside><span class="nav"></span></aside>
      </li>
      <li class="comp" data-goto='bananas'>
        <aside><img src="./images/banana.png" alt="" height="60" /></aside>
        <div>
          <h3>Bananas</h3>
          <h4>Brain Booster</h4>
        </div>
        <aside><span class="nav"></span></aside>
      </li>
      <li class="comp" data-goto='mangos'>
        <aside><img src="./images/mango.png" alt="" height="60" /></aside>
        <div>
          <h3>Mangos</h3>
          <h4>Antioxidants</h4>
        </div>
        <aside><span class="nav"></span></aside>
      </li>
    </ul>
  </section>
</article>
<nav class="next">
  <button class="back">Fruits</button>
  <h1>Apples</h1>
</nav>
<article class="next" id="apples">
  <section>
     <h2>Uses</h2>
     <ul class="list">
        <li>
           <h3>Juice</h3>
        </li>
        <li>
           <h3>Pastries</h3>
        </li>
        <li>
           <h3>Cider</h3>
        </li>
     </ul>
  </section>
</article>
<nav class="next">
  <button class="back">Fruits</button>
  <h1>Oranges</h1>
</nav>
<article class="next" id="oranges">
  <section>
     <h2>Uses</h2>
     <ul class="list">
        <li>
           <h3>Juice</h3>
        </li>
        <li>
           <h3>Fruit Salad</h3>
        </li>
        <li>
           <h3>Smoothies</h3>
        </li>
     </ul>
  </section>
</article>
<nav class="next">
  <button class="back">Fruits</button>
  <h1>Bananas</h1>
</nav>
<article class="next" id="bananas">
  <section>
     <h2>Uses</h2>
     <ul class="list">
        <li>
           <h3>Smoothies</h3>
        </li>
        <li>
           <h3>Fruit Salad</h3>
        </li>
        <li>
           <h3>Pudding</h3>
        </li>
     </ul>
  </section>
</article>
<nav class="next">
  <button class="back">Fruits</button>
  <h1>Mangos</h1>
</nav>
<article class="next" id="mangos">
  <section>
     <h2>Uses</h2>
     <ul class="list">
        <li>
           <h3>Smoothies</h3>
        </li>
        <li>
           <h3>Pastries</h3>
        </li>
        <li>
           <h3>Fruit Salad</h3>
        </li>
     </ul>
  </section>
</article>

```


You can see the above example live on [Codepen](http://codepen.io/rbiggs/pen/hnaCq).

####Dynamic Content

In the previous example we built a simple app with navigation and static content. Its purpose was to show how the combination of layout, classes and attributes automatically enable navigation in ChocolateChip-UI, no JavaScript need. There was one major limitation. All the content was static. If you're looking into making an app, you will most likely need to accomodate dynamic content. This is not difficult to accomplish with ChocolateChip-UI. We'll reuse the structures from the previous example, but load the content dynamically. 

If you are a developer, you probably already have a preferred framework or group of frameworks that you use to provide ways to manage your app's model, view and controllers. Because there are so many possibilities, we are not going to use any at this point. We'll just use whatever jQuery has to offer and the templating that ChocolateChip-UI provides. 

To handle dynamic content, we'll first need some data. We're going to use JSON. Our JSON object will be like this:

```
[
	{
		"id": "apples",
		"name": "Apples",
		"benefit": "Lower Colesterol",
		"image": "/images/apple.png",
		"uses": [
		"Juice",
		"Pastires",
		"Cider"
		]
	},
	{
		"id": "oranges",
		"name": "Oranges",
		"benefit": "Immune Booster",
		"image": "/images/orange.png,
		"uses": [
		"Juice",
		"Fruit Salad",
		"Smoothies"
		]
	},
	{
		"id": "bananas",
		"name": "Bananas",
		"benefit": "Brain Booster",
		"image": "/images/banana.png,
		"uses": [
		"Smoothies",
		"Fruit Salad",
		"Pudding"
		]
	}
]

```

Since we'll generate the main list dynamically, we only need the following structure with an empty list:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="msapplication-tap-highlight" content="no">
  <title>Fruits</title>
  <link rel="stylesheet" href="./css/chui-ios-3.6.0.css">
  <script src="./js/jquery-2.0.3.min.js"></script>
  <script src="./js/chui-3.6.0.js"></script>
</head>
<body>
  <nav class="current">
    <h1>Fruits</h1>
  </nav>
  <article id="main" class="current">
    <section>
      <h2>Available Varieties</h2>
      <ul class='list' role='list'></ul>
    </section>
  </article>
</body>
</html>

```

To create the list items, we will use the following template:

```
var list = '<li class="comp" data-goto="detail" id="[[= data.id ]]" class="nav">\
  <aside><img src="[[= data.image ]]" alt="" height="60" /></aside>\
  <div>\
    <h3>[[= data.name ]]</h3>\
    <h4>[[= data.benefit ]]</h4>\
  </div>\
  <aside><span class="nav"></span></aside>\
</li>';
```

You could also do this as a script template in your docuemnt like this:

```
<script id='listTempl8' type='text/x-texplate'>
  <li data-goto="detail" id="[[= data.id ]]" class="nav"><h3>[[= data.title ]]</h3></li>
</script>
```

We could then access the template like this:

```
var list = $('#listTempl8').html();
```

To learn more about templating in ChocolateChip-UI, please read the chapter "Templates".

Before we can use this template, we need to parse it. This will enable the template to consume the data we provide it, afterwhich we can inject it into the page.

```
var tempList = $.template(list);
```

We need to get our JSON object. For your app, you're probably going to have to make an Ajax request to the server. When the data is returned, you can pass it to the template and output the result to the page:

```
var fruitList = $('#fruitsList');
var list = '<li class="comp" data-goto="detail" id="[[= data.id ]]" class="nav">\
  <aside><img src="[[= data.image ]]" alt="" height="60" /></aside>\
  <div>\
    <h3>[[= data.name ]]</h3>\
    <h4>[[= data.benefit ]]</h4>\
  </div>\
  <aside><span class="nav"></span></aside>\
</li>';
var tempList = $.template(list);
var fruits;
$.ajax({
  url : '/data/fruits.json',
  type: 'GET',
  success: function(data) {
    fruits = JSON.parse(data);
    fruits.forEach(function(ctx, idx) {
      //console.dir(ctx)
      fruitList.append(tempList(ctx));
    });
  }
});
```

You can see this on [Codepen](http://codepen.io/rbiggs/pen/ea81af9586c7abd1ef0c528be36810b8).


Next we need to arrange that when the user taps a fruit, the app navigates to that fruit's detail view. Rather than output an article for each fruit, we're going to conserver resources and memory by using a single article for all of them. Since we'll only have one detail view, each fruit's list item `data-goto` attribute should point to that same one. We call it 'fruitDetail' for clarity:

```
<nav class="next">
	<button class="back">Fruits</button>
	<h1 id="fruitTitle"></h1>
</nav>
<article id="fruitDetail" class="next">
	<section>
		<h2>Uses</h2>
		<ul class="list" id="fruitUses">
		</ul>
	</section>
</article>
```

In order to know which fruit the user chose, we need to change our fruit list template to hold the id of each fruit. We'll use an attribute `data-id` that will hold the fruit id:

```
var list = '<li class="comp" data-id="[[= data.id ]]" data-goto="fruitDetail" id="#fruitDetail" class="nav">\
  <aside><img src="[[= data.image ]]" alt="" height="60" /></aside>\
  <div>\
    <h3>[[= data.name ]]</h3>\
    <h4>[[= data.benefit ]]</h4>\
  </div>\
  <aside><span class="nav"></span></aside>\
</li>';
```

Then, for the JavaScript, we want to get the fruit id when the use taps a list item. After that we need to filter the array of fruits down to match the chosen fruit. We'll use the array filter method for that. Once we have the filtered fruit item, we can access its uses array, loop it and output the contents into the fruit detail list. We'll also update the h1 title of the navbar.


```
var fruitList = $('#fruitList');
fruitList.on('singletap', 'li', function() {
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

And here's the completed dynamic list on [Codepen](http://codepen.io/rbiggs/pen/JaLHx?editors=101).

Here's what it would look like:


![Dynamic navigation list, first screen](images/first-app/navigation-1.png)

![Dynamic navigation list, second screen](images/first-app/navigation-2.png)

###Tabbar App

If your app only has a handfull of screens, it's a good canditate for a tabbar interface. Rather than presenting the user with a navigation list to get to the screen, the user can just tap the tabbar button to access them.

To start with we'll use a basic shell for our app:



```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="msapplication-tap-highlight" content="no">
  <title>Fruits</title>
  <link rel="stylesheet" href="./css/chui-ios-3.6.0.css">
  <script src="./js/jquery-2.0.3.min.js"></script>
  <script src="./js/chui-3.6.0.js"></script>
  <script>
    $(function() {

    });
  </script>
</head>
<body>

</body>
</html>

```


Because we are using tabs, we have no need for the main navigation list. We opt to put each fruit into its own tab. By tapping a tab, the user can immediate switch to another fruit. Because there is no longer a list, we set the navbar and article of apples to have a navigation state of current, since they will be the default selected state of our tabbar interface.

A tabbar interface is unlike the previous dynamic navigation list in that there are at most five tabs in the tabbar. As such, with a finite number of possible tabs, we will be static content that loads when the app launches. Be aware that the data for these could be dynamically provided with a service request at load time. In such a case, as we did in the previous dynamic navigation list example, you would load the data and iterate over it with a template to create each navbar and article for the tabbar. The navbar and article would be empty stubs that you populate.




```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="msapplication-tap-highlight" content="no">
  <title>Fruits</title>
  <link rel="stylesheet" href="./css/chui-ios-3.6.0.css">
  <script src="./js/jquery-2.0.3.min.js"></script>
  <script src="./js/chui-3.6.0.js"></script>
  <script>
    $(function() {
      // Initialize the tabbar here:

    });
  </script>
</head>
<body>
  <nav class="current">
    <h1>Apples</h1>
  </nav>
  <article class="current" id="apples">
    <section>
      <aside><img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/77047/apple.png" alt="" height="60" /></aside>
      <h2>Benefits</h2>
      <ul class='list'>
        <li>
          <h3>Lowers Colesterol</h3>
        </li>
      </ul>
       <h2>Uses</h2>
       <ul class="list">
          <li>
             <h3>Juice</h3>
          </li>
          <li>
             <h3>Pastries</h3>
          </li>
          <li>
             <h3>Cider</h3>
          </li>
       </ul>
    </section>
   </article>
   <nav class="next">
      <h1>Oranges</h1>
   </nav>
   <article class="next" id="oranges">
      <section>
      <aside><img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/77047/orange.png" alt="" height="60" /></aside>
      <h2>Benefits</h2>
      <ul class='list'>
        <li>
          <h3>Boosts Immune System</h3>
        </li>
      </ul>
        <h2>Uses</h2>
        <ul class="list">
          <li>
             <h3>Juice</h3>
          </li>
          <li>
             <h3>Fruit Salad</h3>
          </li>
          <li>
             <h3>Smoothies</h3>
          </li>
        </ul>
      </section>
   </article>
   <nav class="next">
      <h1>Bananas</h1>
   </nav>
   <article class="next" id="bananas">
      <section>
      <aside><img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/77047/banana.png" alt="" height="60" /></aside>
      <h2>Benefits</h2>
      <ul class='list'>
        <li>
          <h3>Improves Brain Performance</h3>
        </li>
      </ul>
        <h2>Uses</h2>
        <ul class="list">
          <li>
             <h3>Smoothies</h3>
          </li>
          <li>
             <h3>Fruit Salad</h3>
          </li>
          <li>
             <h3>Pudding</h3>
          </li>
        </ul>
      </section>
   </article>
   <nav class="next">
      <h1>Mangos</h1>
   </nav>
   <article class="next" id="mangos">
      <section>
      <aside><img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/77047/mango.png" alt="" height="60" /></aside>
      <h2>Benefits</h2>
      <ul class='list'>
        <li>
          <h3>Good Source of Antioxidants</h3>
        </li>
      </ul>
        <h2>Uses</h2>
        <ul class="list">
          <li>
            <h3>Smoothies</h3>
          </li>
          <li>
            <h3>Pastries</h3>
          </li>
          <li>
            <h3>Fruit Salad</h3>
          </li>
        </ul>
      </section>
   </article>
   <nav class="next">
      <h1>Avocados</h1>
   </nav>
   <article class="next" id="avocados">
      <section>
      <aside><img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/77047/avocado.png" alt="" height="60" /></aside>
      <h2>Benefits</h2>
      <ul class='list'>
        <li>
          <h3>Provides Omega Fats</h3>
        </li>
      </ul>
        <h2>Uses</h2>
        <ul class="list">
          <li>
            <h3>Smoothies</h3>
          </li>
          <li>
            <h3>Salad</h3>
          </li>
          <li>
            <h3>Fresh</h3>
          </li>
        </ul>
      </section>
   </article>
</body>
</html>

```


Now that we have the basic structure for out tabbar interace, we need to add the code that will initialize the tabbar for us. We'll use the `$.UITabbar` method. This takes an object of options for setting up the tabbar. The posible options are:

1. tabs: 2 - 5 (at leat two, maximum 5 tabs)
2. imagePath: this is the path to the tab button icons that are shown on iOS.
3. icons: a class name for each tab button icon. You'll use these to define what icons the tabbar buttons show.
4. labels: the text for each tabbar button.
5. selected: the default selected tabbar button. The default is 1.

With the above information we can now initialize our tabbar:

```
$(function() {
  var opts = {
    tabs : 5,
    imagePath : "../fruit-icons/",
    icons : ["apples", "oranges", "bananas", "mangos", "avocados"],
    labels : ["Apples", "Oranges", "Bananas", "Mangos", "Avocados"],
    selected : 1
  };
  $.UITabbar(opts);
});

```

We also need to add in the following CSS to style the icons for iOS:


```
aside {
  padding-left: 12px;
}
.isiOS .tabbar > button > .icon,
.isDesktopSafari .tabbar > button > .icon {
  background-color: #929292;
  -webkit-mask-position: center center;
  -webkit-mask-size: 100%;
  -webkit-mask-repeat: no-repeat;
}
.isiOS .tabbar > button.selected > .icon,
.isiOS .tabbar > button:hover > .icon,
.isDesktopSafari .tabbar > button.selected > .icon,
.isDesktopSafari .tabbar > button:hover > .icon {
  background-color: #007aff;
}
.tabbar > button.apple > .icon  {
  -webkit-mask-image: url('data:image/svg+xml;utf8,<svg width="30px" height="30px" viewBox="0 0 30 30" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"><g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd"><g id="Artboard-1" stroke="#000000"><path d="M12.7832251,28.0544501 C13.1640044,28.0544501 9.95252242,28.0544501 16.0187817,28.0544501 C22.0850409,28.0544501 26.9535377,22.3433257 27.2734107,16.0000001 C27.6643752,8.2468749 20.9825164,3.5015617 18.7409115,4.60755853 C15.6786316,6.11847239 13.6578113,4.60911335 13.0010975,4.22518447 C9.26435977,2.04060772 1.71377842,7.69406959 2,16 C2.14555945,20.2240234 5.60352296,28.0544501 12.7832251,28.0544501 Z" id="Oval-1" stroke-width="2"></path><path d="M15.4458452,4.55486506 C15.4458452,4.55486506 15.137429,3.28125003 15.6081321,2.85795455 C16.0788352,2.43465906 17.0559304,1.28551136 17.0559304,1.28551136 L18.6067116,2.04314631 C18.6067116,2.04314631 17.4909446,2.09854412 16.8680753,3.12322443 C16.245206,4.14790474 16.0138494,4.65216619 16.0138494,4.65216619 L15.4458452,4.55486506 Z" id="Path-1"></path></g></g></svg>');
}
.tabbar > button.orange > .icon  {
  -webkit-mask-image: url('data:image/svg+xml;utf8,<svg width="30px" height="30px" viewBox="0 0 30 30" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"><g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd"><g id="Artboard-1" stroke="#000000"><circle id="Oval-1" stroke-width="2" cx="15" cy="15" r="12"></circle><path d="M14.2589989,7.73032575 L12.438701,9.13132818 L15.3099792,9.11506083 L16,10.760439 L16.8901052,8.43713285 L20.3395139,8.40441537 L17.1322876,6.76123053 L18.2563796,5.63933191 L16,6.15842478 L12.5693881,5.36644259 L14.2589989,7.73032575 Z" id="Path-1"></path><circle id="Oval-2" fill="#D8D8D8" cx="6.5" cy="13.5" r="0.5"></circle><circle id="Oval-2-copy" fill="#D8D8D8" cx="6.5" cy="18.5" r="0.5"></circle><circle id="Oval-2-copy-2" fill="#D8D8D8" cx="13.5" cy="19.5" r="0.5"></circle><circle id="Oval-2-copy-3" fill="#D8D8D8" cx="8.5" cy="22.5" r="0.5"></circle><circle id="Oval-2-copy-3" fill="#D8D8D8" cx="17.5" cy="24.5" r="0.5"></circle></g></g></svg>');
}
.tabbar > button.banana > .icon {
  -webkit-mask-image: url('data:image/svg+xml;utf8,<svg width="30px" height="30px" viewBox="0 0 30 30" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"><g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd"><g id="Artboard-1" stroke="#000000" stroke-width="2"><path d="M3.19554265,24.0322177 L2.96962758,24.677611 L4.158789,24.9590918 C4.158789,24.9590918 6.77907745,26.2137903 7.69172784,26.211312 C16.5228865,26.1873309 19.9213425,23.7827918 22.4403631,20.5159117 C24.9593837,17.2490317 24.8651964,12.1457634 24.4253454,10.394698 C23.9854943,8.64363254 22.991806,7.6571938 22.9918069,7.12276775 C22.9918078,6.5883417 24.9956159,3.59257061 24.9956159,3.59257061 L22.6843728,3.20507875 L20.3091579,6.37227637 C20.3091579,6.37227637 19.5309409,10.4358236 19.3632755,11.3928553 C19.19561,12.349887 18.249236,15.52403 16.0217065,18.0935395 C13.794177,20.663049 7.69113728,21.6791936 6.98492067,21.9428698 C6.27870406,22.206546 3.45417522,22.8308112 3.45417522,22.8308112 L3.19554265,24.0322177 Z" id="Path-1"></path><path d="M4.13332454,19.3744407 L3.97929153,19.9075917 L4.79008341,20.1401193 C4.79008341,20.1401193 6.57664371,21.1766094 7.19890534,21.1745621 C13.2201499,21.1547516 15.537279,19.1683932 17.254793,16.4696662 C18.9723071,13.7709392 18.9080884,9.55519585 18.60819,8.10866354 C18.3082916,6.66213123 17.6307768,5.84724705 17.6307774,5.40576466 C17.630778,4.96428227 18.9970108,2.48951485 18.9970108,2.48951485 L17.4211633,2.16941288 L15.8016986,4.78579352 C15.8016986,4.78579352 15.2710961,8.14263688 15.1567787,8.93322831 C15.0424613,9.72381973 14.3972064,12.3459379 12.8784363,14.4685761 C11.3596661,16.5912144 7.19850269,17.4306382 6.71699137,17.6484577 C6.23548004,17.8662772 4.30966493,18.3819744 4.30966493,18.3819744 L4.13332454,19.3744407 Z" id="Path-2" transform="translate(11.500000, 11.500000) rotate(14.000000) translate(-11.500000, -11.500000) "></path></g></g></svg>');
}
.tabbar > button.mango > .icon  {
  -webkit-mask-image: url('data:image/svg+xml;utf8,<svg width="30px" height="30px" viewBox="0 0 30 30" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"><g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd"><g id="Artboard-1" stroke="#000000" stroke-width="2"><path d="M15,27 C21.6274173,27 26.0895505,19.7045547 26.7082564,12.758215 C27.2628011,6.5322272 21.2077007,1.18207877 16.1141983,3.05104111 C14.8595216,3.51142048 15,3.38712682 14.1197062,3.05104111 C7.92937375,0.687645905 2.50207845,5.984785 3.08678905,12.7582154 C3.67149965,19.5316457 8.37258267,27 15,27 Z" id="Oval-1"></path><path d="M18.0127349,6.01048574 C18.0127349,6.01048574 20.3654535,6.49821127 21.5016088,9.39160585 C22.6377642,12.2850004 22.632894,13.8590882 21.5016088,16.6592713 C20.3703237,19.4594545 17.4949638,22.5018951 16.7184604,22.8171016" id="Path"></path></g></g></svg>');
}
.tabbar > button.avocado > .icon {
  -webkit-mask-image: url('data:image/svg+xml;utf8,<svg width="30px" height="30px" viewBox="0 0 30 30" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"><g id="Page-1" stroke="none" stroke-width="1" fill="none" fill-rule="evenodd"><g id="Artboard-1" stroke="#000000"><path d="M14,27 C18.5060694,27.0000008 24.1330805,20.1170882 25.0879192,12.27312 C25.8167241,6.28601196 19.0459709,0.855269586 15.7497311,3.37195614 C15.0607444,3.89799902 14.5220184,4.35695364 13.5220184,3.37195614 C10.1617969,0.0621463502 3.05666119,6.14152614 3.73711743,12.7313473 C4.34814825,18.6488237 9.4939306,26.9999992 14,27 Z" id="Oval-1" stroke-width="2"></path><path d="M14.5,18 C16.5964077,18 18.8546011,15.4209428 19.213031,12.3423929 C19.5714609,9.26384306 16.9852814,8 14.5,8 C12.0147186,8 9.65057227,9.24958634 9.99474546,12.3423929 C10.3389187,15.4351995 12.4035923,18 14.5,18 Z" id="Oval-2" fill-opacity="0.130463089" fill="#000000"></path></g></g></svg>');
}

```

You can view this on [Codepen](http://codepen.io/rbiggs/pen/14015537c934e6dda40ebcc9fc66173b/).

This is what it would look like on iOS:


![Tabbar, first tab](images/first-app/tabbar-1.png)

![Tabbar, third tab](images/first-app/tabbar-2.png)


Now let's look at reproducing the above static code with a template version. We'll use an a template that will serve for all tab sections.


```
<nav class="current">
  <h1></h1>
</nav>
<article class="current" id="apples">
  <section>
    <aside><img src="" alt="" height="60" /></aside>
    <h2>Benefits</h2>
    <ul class='list' id='benefits'>
      <li>
        <h3></h3>
      </li>
    </ul>
     <h2>Uses</h2>
     <ul class="list" id='uses'>
     </ul>
  </section>
 </article>
 <div class="tabbar" id="fruitTabbar">
  <button class="apple"><span class="icon"></span><label>Apples</label></button>
  <button class="orange"><span class="icon"></span><label>Oranges</label></button>
  <button class="banana"><span class="icon"></span><label>Bananas</label></button>
  <button class="mango"><span class="icon"></span><label>Mangos</label></button>
  <button class="avocado"><span class="icon"></span><label>Avocados</label></button>
 </div>

```

Next we do an Ajax request for data and then populate our tab panel on load, and use a method to do the same when each tabbar button is pressed.


```
$(function() {

  var data = [];
  var fruit = 0;
  var tabButtons = $('#fruitTabbar button');

  $.ajax({
    url : 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/77047/fruits_2.js',
    type: 'GET',
    success: function(data) {
      data = JSON.parse(data);
      $.renderChosenFruit = function(fruit) {
        tabButtons.removeClass('selected');
        tabButtons.eq(fruit).addClass('selected');
        $('h1').text(data[fruit].name);
        $('aside img')[0].src = data[fruit].image;
        $('#benefits h3').text(data[fruit].benefit);
        $('#uses').empty();
        data[fruit].uses.forEach(function(use) {
          $('#uses').append('<li><h3>' + use + '</h3></li>');
        });
      };
      $.renderChosenFruit(0);
    }
  });

  $('#fruitTabbar').on('click', 'button', function() {
    var fruit = $(this).index();
    $.renderChosenFruit(fruit);
  });
});

```

You can see this on [Codepen](http://codepen.io/rbiggs/pen/49eaaf58440e15584caa4198cef80b25).

###Slide Out App

For our last example, we'll create an app that uses a slide out menu to access its content. We use a single destination article for our content. Then we'll create the slide out and populate it with our fruits. When the user taps a fruit, we populate the main article.


```
<nav class="current">
  <h1></h1>
</nav>
<article class="current" id="main">
  <section>
    <aside><img src="" alt="" height="60" /></aside>
    <h2>Benefits</h2>
    <ul class='list' id='benefits'>
      <li>
        <h3></h3>
      </li>
    </ul>
     <h2>Uses</h2>
     <ul class="list" id='uses'>
     </ul>
  </section>
 </article>
```

To initialize the slide out, we need to use the $.UISlide method and we'll pass in some options. Then we'll populate the slide out with our menu items. We'll define a callback that gets run when the user taps a slide out menu item. And we need to fire this callback when the app loads as well. Note that we pass an option value of `dynamic: true`. This prevents the normal behavior of toggling mutliple articles associated with each slide out menu item. 


```
$(function() {
  $.fruitData = {};
  var fruits = ["Apples", "Oranges", "Bananas", "Mangoes", "Avocados"]
  $.UISlideout({dynamic: true, callback: function(e, li) {
    var fruit = $(li).index();
    $.renderChosenFruit(fruit);
  }});
  var slideout = $('.slide-out');
  slideout.append('<ul class="list" id="slideMenu"></ul>');
  fruits.forEach(function(fruit) {
    $('.slide-out ul').append('<li data-show-artcile="' + fruit + '"><h3>' + fruit + '</h3></li>')
  });

  // Ajax request performed at load:
  $.ajax({
    url : 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/77047/fruits_2.js',
    type: 'GET',
    success: function(data) {
      $.fruitData = JSON.parse(data);
      $.renderChosenFruit = function(fruit) {
        $('h1').text($.fruitData[fruit].name);
        $('aside img')[0].src = $.fruitData[fruit].image;
        $('#benefits h3').text($.fruitData[fruit].benefit);
        $('#uses').empty();
        $.fruitData[fruit].uses.forEach(function(use) {
          $('#uses').append('<li><h3>' + use + '</h3></li>');
        });
      };
      // Populate list items when 
      // Ajax request completes:
      $.renderChosenFruit(0);
    }
  });
});
```

You can view this on [Codepen](http://codepen.io/rbiggs/pen/feb723bbcd398a8de4fbe46ab985a09a/).

Here's what our slide out would look like in action:


![Slide out, first screen](images/first-app/slide-out-1.png)

![Slide out, opened](images/first-app/slide-out-2.png)

![Slide out, second screen](images/first-app/slide-out-3.png)
