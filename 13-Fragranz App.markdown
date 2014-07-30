#ChocolateChip-UI

##Building Fragranž

In this Chapter we'll look at how to build out an app for exploring perfumes and colones for women, men and kids. Since there are three groups, the fragrances need to be navigable based on that. After selecting a genre, the user gets that collection filtered. Tapping a list item would lead the user to a detail view for that fragrance. The user could choose to add it to the shopping cart. At the shopping cart the user would be able to place an order, or cancle it. Of course at any time the user can use back navigation to return to any earlier screen.

We are not going to create a real shopping cart, as this is about conceptual parts to create and app. A fully functioning shopping cart is beyond the scope of this tutorial.

The work flow will be like this:


```
main screen (women, men, kids) =>

chosen genre of fragrance =>

chosen fragrance =>

shopping cart =>

confirmation page
```

Our app will have five screens that the user can navigate to complete a purchase. At any stage the user will be able to return to an earlier screen by tapping the back button in the top left.


####Templating

Since this is a very complex layout, we are going to need some sophisticated templates that will enable us to create everything dynamically as the user makes choices. That are lots of good choices for this. We're going to use [Soma-template](https://soundstep.github.io/soma-template/). The reason we chose this one is because it is closely modeled after the way Angularjs handles templating. It provides tokens, repeaters and scopes just like the Angularjs ones. You also have the ability to write custom template helpers, similar to Angular's filters. Soma-template has two big advantages over Angular: size and speed. Soma-template minified is only 27kb. In performance tests on www.jsperf.com it renders really efficiently. 

To make Soma-templates work with ChocolateChip-UI's touch and gesuture modules, we had to make some minor adjustments to the Soma-template initialization code. Our version checks for the presence of ChocolateChip-UI and then adds in all of our gestures so that they are available from within the templates.

We are not going to create models and controllers for this. We're just going to manage user interaction and subsequent data loading. As a developer you will choose the manner you prefer for handling your application code. This tutorial is just to show you how assemble the different parts of ChocolateChip-UI to create a realistic app experience.

It is possible to create and app where you can switch out the theme to support different operating systems. You can learn more about how to do this in Chapter 11. Here we're going to keep it simple and just persent how to put together an app with the iOS theme. Because of that the screenshots and app icons will be for iOS. Android and Windows Phone 8 would require icons and other resources with dimensions appropriate for those platforms. 

One thing we did for our app, we opened the source code for the them, the LESS files, and modified the color to match the mode of the app, a rich, warm red color. Please read Chapter 10 Themes. By modifying just a few colors values in the colors.less file of a theme, you can create a beautiful custom theme with the colors you want for your app.

##Getting Started

This project with all its resources is available on [Github](https://github.com/sourcebits-robertbiggs/Fragranz) for download.

To start, we need to create the basic shell for our app. This will consist of links to all the resources our app will need. We are using linking to all the icon and startup images with different sizes. Then we includ all the files for ChocolateChip: chui-ios-3.6.1.css, style.css (app styles), jquery-2.1.1.min.js, chui-3.6.1.js, soma-template.js, app.js.

Styles.css will hold styles specifically for the app and app.js will hold all the JavaScript to create the interactions of the app.

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <title>Fragranž</title>  
    <link href="images/apple-touch-icon-120x120.png" sizes="120x120" rel="apple-touch-icon">
    <link href="images/apple-touch-icon-114x114.png" sizes="114x114" rel="apple-touch-icon">
    <link href="images/apple-touch-startup-image-640x1096.png" rel="apple-touch-startup-image" sizes="640x1096">
    <link href="images/apple-touch-startup-image-640x920.png" media="(device-width: 320px) and (device-height: 480px) and (-webkit-device-pixel-ratio: 2)" rel="apple-touch-startup-image">
    <link rel="stylesheet" href="chui/chui-ios-3.5.5.css">
    <link rel="stylesheet" href="css/style.css">
    <script type='text/javascript' src='chui/jquery-2.1.1.min.js'></script>
    <script type='text/javascript' src='chui/chui-3.5.5.js'></script>
    <script src="js/soma-template.js"></script>
    <script type='text/javascript' src='js/app.js'></script>
  </head>
  <body>
    <!-- Content will go here -->
  </body>
</html>
```

Because of the links to icons and startup images, when the user pins the Web page to the device's homescreen, they'll get a home scren icon and start screen like these:


Home screen icon:

![View of available genres](images/fragranz/startscreen-icon.png);


Startup screen:

![View of available genres](images/fragranz/launch-screen.png);


To the body tag we'll add our first navbar:

```
<nav class='current'>
  <h1 id='mainTitle'>Fragranž</h1>
</nav>   
```

Followed by an article, this is the article the user will see when the app finishes loading.

```
<article id='main' class='current'>
  <section>
    <h2 class='wrap normal-case'>Choose from one of the categories below to see which fragrances are available.</h2>
    <ul class='list' id='perfumeGenres'>
      <li class='nav'>
        <h3>text here</h3>
      </li>
    </ul>
    <div id='fragranz_small' class='horizontal centered'>
      <img src='images/fragranz_small.png'>
    </div>
  </section>
</article> 
```

The list with the id of `perfumeGenres` will hold the three genres of fragrances: women, men and kids. The list items will need to navigate to another list where the fragranes for each genre will be displayed. At load time we only need to render this list. Other lists will be generated as the user moves through the work flow.

In the app.js file we'll also create the shell for our app. We'll use the jQuery DOM load shortcut. Inside this we'll be defining the code to wire up the Soma templates.

```
$(function () {

  //======================
  // Create app namespace:
  //======================
  var app = {};

});
```

Let's take a look at the data we'll be using. It's an array of objects with information about the fragrance. Below is a sample of just two fragrances:

```
[
  {
  "sku" : "3962",
  "genre": "ladies",
  "short_description" : "My Life Blossom Mary J. Blige Eau de Parfum and Rollerball Duo",
  "long_description" : "Lorem ipsum dolor sit amet, te mei prompta laboramus. Quo et postea noluisse, id eam iriure interpretaris. Mea feugait hendrerit consequuntur no. Quod utinam qui no, duo graecis tincidunt cotidieque id.",
  "img_prev" : "images/women/women_1.png",
  "product_title" : "My Life Blossom",
  "wholesale_price" : "15.00"
  },
  {
  "sku" : "3983",
  "genre": "ladies",
  "short_description" : "Vince Camuto Eau de Parfum Spray",
  "long_description" : "Cu veri clita sapientem eos, mea no quot tantas antiopam, vis te stet labores. Mutat delenit accommodare ut nam, mucius oporteat facilisi sed ei. Vim an assum delicata, nonumy soluta sed cu. Tollit nostrum neglegentur per eu, eos utamur invidunt voluptaria te. Minimum salutatus vim id.",
  "img_prev" : "images/women/women_2.png",
  "product_title" : "Vince Camuto",
  "wholesale_price" : "5.00"
  }
];
```

Each object has:

- sku
- genre
- short description
- long description
- img preview
- product title
- wholesale price

Normally to we would want to filter the entire array to extract all possible genres, then reduce the result to unique values. In this case we will not do that since we know that there are only three genres. We just create a simple array for genres in your code to handle them.

####Defining a Repeater


Now that we know the data, let's see how to create our first template. Like Angularjs, Mustache and Handlebars, Soma-templates uses double curly braces to delimit a token. And like Angularjs, Soma provides a repeater to output the items of an array. The repeater's format is like Angularjs where you use a "key in object" type of iteration. The root of our template will be the list with id of "perfumeGenres". When we initialize a template of the name "perfumeGenres", we'll be able to add to its scope to make those things available to the template. Take a look at the following markup:


```
<ul class='list' id='perfumeGenres'>
  <li class='nav' data-repeat="genre in genres">
    <h3>{{genre}}</h3>
  </li>
</ul>
```

The template will expect that on its scope there will be an array named "genres" overwhich it will iterate to output the value of the index as "genre". The result will be our simple list of genres. Now we need to intialze the template for this. In our app.js file we'll add the following:

```
// Pass the id of the DOM template to soma.teplate.create:
app.perfumeGenres = soma.template.create($('#perfumeGenres')[0]);

// Attach the array of genres to the template through its scope:
app.perfumeGenres.scope.genres = ['ladies', 'men', 'kids'];

// Render the template to the browser:
app.perfumeGenres.render();
```

In the above code we store the template in the variable `app.perfumeGenres`, then we attach the array of genres using the `scope` property. Using the template's scope, we can add properties and methods to extend the capabilities of a template. In fact, we can use scope to assign a value from one template to another. This is the technique we will use to make the user choices move from view to view as the user navigates through the app. The last line is very important. Because the the scoped array of genres has the same name used in the repeater, the template can loop over it to output its contents.

That last line is very important:

```
app.perfumeGenres.render()
```

The render method will cause the parsed template to be rendered in the browser. Without that final line, you would just see the braces with the token. Any time the data that a template consumes changes, we need to execute the render method to update the template. 

####Defining a Template Helper

When we defined our array for genres, we used text that was all lower case. That's because we will use those values in programatic ways to filter the array of frangrances based on the genre choice the user makes. But for our list, we want the text to be capitalized. We can create a helper method for our templates that will capitalize the text. Soma-templates has an interface for create template helpers:

```
//=================================
// Define a template helper.
// Capitalize first letter of word:
//=================================
soma.template.helpers({
  capitalize : function ( str ) {
    if (!str) return;
    return str.charAt(0).toUpperCase() + str.slice(1);
  }
});
```

With this helper defined, we can use it in our template to capitalize the token for the genre:

```
<ul class='list' id='perfumeGenres'>
  <li class='nav' data-repeat="genre in genres">
    <h3>{{capitalize(genre)}}</h3>
  </li>
</ul>
```

If we load our app in it's current state in a browser, we will see the following:

![View of available genres](images/fragranz/genres.png)

Nice! We've got the first view rendering. Now we need to make the list so that it can navigate to the next vew where all the fragrances of the chosen genre will be presented. That needs another navbar/article and some attributes in our genre template. To make the list items navigate, we need to put the ChocolateChip-UI `data-goto` attributes with a value pointing to the article. In this case we're going to direct the user to the article "perfumeList". 

You might notice that when this page loads, for a brief moment you can see the curly braces before the browser renders the template. We can avoid this by using the class `data-clock`. Like its namesake in Angularjs, we can use this to hide an element until it is rendered. We just add it as a class. In the style.css file we define the class to display none. When Soma-template see that class, after rendering the template it will remove the class, cause the template to appear fully rendered.

```
.data-cloak {
  display: none;
}
```

Like Angularjs, Soma-templates has events that can be included in a template. Because of the modification we made in this version of Soma-template, you can also use all of ChocolateChip-UI's gestures in a template. So we'll use the single tap gesture on the template to execute a method defined on the template's scope. This method will determine which genre the user choice. Here's how our template will look now:

```
<ul class='list' id='perfumeGenres'>
  <li class='nav data-cloak' data-singletap='getGenre()' data-goto='perfumeList' data-genre='{{genre}}' data-repeat="genre in genres">
    <h3>{{capitalize(genre)}}</h3>
  </li>
</ul>
```

Now we'll need to define `getGenre` on the scope of the template. But first we need to do something very important. We need to bring the data into our app. At the moment it sits ourside our app as a JSON file. We need to perform an Ajax request to get the data. Then we can expose that data to our app for consumption by the templates. We are going to create some namespaced holders for this data. So our app initialization will look like this:

```
var app = {};

// Default objects:
app.purchases = [];
app.perfumesCollection;
```
We'll use jQuery to perform an Ajax request. After getting our data, we'll hold it in the `app.perfumesCollection` variable. After the data has arrived, we'll render the genres template. We don't want it rendering until the data for the perfumes is loaded. Otherwise the user could try and navigate to a genre before the Ajax request has completed.

```
$.getJSON('data/perfumes.json', function(data) {
  // Make acquired data available to templates:
  app.perfumesCollection = data;
  // Render first template:
  app.perfumeGenres.render();
});
```

Before we define the `getGenre()`, we need to create the template that it will affect. We'll need to define tokens to output the details about each fragrance:

```
<nav class='next'>
  <a href='#' class='button back'>Perfumes</a>
  <h1 id='perfumesGenreTitle'>{{title}}</h1>
</nav> 
<article class='next' id='perfumeList'>
  <section>
    <ul class='list' id='available_perfumes'>
      <li data-sku='{{perfume.sku}}' data-singletap='getChosenPerfume()' class='comp' data-repeat='perfume in selectedGenre' data-goto='detail'>
        <aside>
          <img width='60' data-src='{{perfume.img_prev}}'>
        </aside>
        <div>
          <h3 class='productTitle'>{{perfume.product_title}}</h3>
          <h4>{{perfume.sku}}</h4>
          <h4>{{perfume.short_description}}</h4>
        </div>
        <aside>
          <span class='counter'>${{perfume.wholesale_price}}</span>
          <span class='show-detail'></span>
        </aside>
      </li>
    </ul>
  </section>
</article>
```

Notice that in the navbar we have a token for the title. This will be the value of the genre the user chose. We have another repeater on the list "available_perfumes". The list items will contain details about each fragrance: name, description, image, price, etc. This output needs to be filtered from the array based on genre. That's where our previous template's properties come into play. `data-singletap='getGenre()` will allow us to grab the genre, filter out all fragrances that do not match the genre, then pass this filtered array to this template to loop and render.


```
app.perfumeGenres.scope.getGenre = function(event) {
  var title = app.perfumesGenreTitle.scope.title = event.target.getAttribute('data-title');
  var perfumeGenre = event.target.getAttribute('data-genre');
  //===================================
  // Update the title of the next view:
  //===================================
  app.perfumesGenreTitle.render();
  //=============================================
  // Filter the data based on the user selection:
  //=============================================
  var whichPerfumes = app.perfumesCollection.filter(function(item) {
    return item.genre === perfumeGenre;
  })
  app.available_perfumes.scope.selectedGenre = whichPerfumes;
  //===============================================
  // Update the next template for the chosen genre:
  //===============================================
  app.available_perfumes.render();
};
```

For the above code to work we need to initialize templates for `perfumesGenreTitle` and `available_perfumes`.


```
app.perfumesGenreTitle = soma.template.create($('#perfumesGenreTitle')[0]);
app.available_perfumes = soma.template.create($('#available_perfumes')[0]);
```

When this list is rendered, depending on which genre the user chose, we will get one of the follow three views:

Women:

![View of available genres](images/fragranz/genre-women.png);

Men:

![View of available genres](images/fragranz/genre-men.png);

Kids:

![View of available genres](images/fragranz/genre-kids.png);


On our above list for available fragrances we have another `data-goto` property on the list items. This time its pointing to another article with an id of "detail". This will be the detail view for the chose fragrance. Only the chosen frangrance will be displayed. Since each fragrance has an SKU, we output that on the list with the `data-sku` property. This is so we can access that property to identify which fragrance it is when the user taps the list item. We also have attached a template as we did in the previous list, but here we've put the method `getChosenPerfume()`. Like `getGenre()`, this will be executed on the scopoe of this template, so we'll need to define it on its template as we did with `getGenre()`.

Next lets put together the navbar and article for the detail view. We also need to include a toolbar after the article. This will hold a button to add the chosen fragrance to the shopping cart and another to view the contents of the shopping cart:

```
<nav class='next' id='detailNavbar'>
  <a href='#' class='button back' id='backToGenre'>{{chosenPerfume.product_title}}</a>
  <h1 id='detailTitle'>{{chosenPerfume.product_title}}</h1>
</nav>
<article class='next' id='detail'>
  <section>
    <ul class='list' id='perfumeDetail'>
      <li>
        <img data-src="{{chosenPerfume.img_prev}}">
        <h3 class="productTitle">{{chosenPerfume.product_title}}</h3>
        <h4><span class="sku">SKU: {{chosenPerfume.sku}}</span></h4>
        <h4 class="counter flush">${{chosenPerfume.wholesale_price}}</h4>
        <p class="longDescription">{{chosenPerfume.long_description}}</p>
      </li>
    </ul>
  </section>
</article>
<div class='toolbar next'>
  <a href="javascript:void(null)" id='addToCart' class="button add"></a>
  <a href="javascript:void(null)" data-disabled="{{disabled}}" id="shoppingCart" class="butt`on align-flush"></a>
</div>
```

We need two templates here: one for the navbar to render the title and value of the Back button, and another to output the detail in the list. These templates again will be defined on the ids of the elements: "detailNavbar" and "perfumeDetail".


```
app.detailNavbar = soma.template.create($('#detailNavbar')[0]);
app.perfumeDetail = soma.template.create($('#perfumeDetail')[0]);
```

Now we can define the `getChosenPerfume()` method on the scope of the chosen genre list:


```
//================================================
// Get the chosen perfume and render its template:
//================================================
app.available_perfumes.scope.getChosenPerfume = function(e) {
  var item = e.target.nodeName === 'LI' ? e.target : $(e.target).closest('li')[0];
  var sku = item.getAttribute('data-sku');
  var chosenPerfume = app.available_perfumes.scope.selectedGenre.filter(function(perfume) {
     return perfume.sku === sku;
  });

  //=========================
  // Update the navbar title:
  //=========================
  app.detailNavbar.scope.chosenPerfume = chosenPerfume[0];
  app.detailNavbar.scope.chosenPerfume.genreTitle = app.perfumesGenreTitle.scope.title;
  app.detailNavbar.render();

  //========================
  // Update the detail view:
  //========================
  app.perfumeDetail.scope.chosenPerfume = chosenPerfume[0];
  app.perfumeDetail.render();
};
```

If you examine the above method, you'll see that when the user taps the list item, we get the SKU of the fragrance. We use that to pluck it out of the array. We assign that fragrance array to these variables: `chosenPerfume` and `app.available_perfumes`. First we attach the chosen fragrance that we plucked from the array to the navbar template: `app.detailNavbar.scope.chosenPerfume = chosenPerfume[0]`. Then we render it's template. We also do the same for the detail list: `app.perfumeDetail.scope.chosenPerfume = chosenPerfume[0]`.



When this renders, we would get something like this:


![View of available genres](images/fragranz/detail-view.png);

This detail list does not lead anywhere. However, in the toolbar we have two action buttons. One will add the current chosen fragrance to the shopping cart, the other will let us see what is in the shopping cart. To make the Add button aware of what the current fragrance is, we'll wire it up as a template and pass it the relevant information so that it can add the fragrance to the shopping cart. 

In reality, the shopping cart is nothing but another template with an array of values that we update each time the user adds a fragrance. That's why we created the `app.purchases` array, to store those fragrances in the cart. If you were creating a real app, you'd also need a model to hold this information which you would persist on the server. Before we enable the Add button, let's put the navbar and article for the shopping cart in place:

```
<nav class="next">
  <a href='#' class='button back' id='backToPerfume'>{{perfumeName}}</a>
  <h1>Cart</h1>
</nav>
<article id="cart" class="next">
  <section>
    <h2>Your Purchase:</h2>
    <ul class="list" id="purchaseItems">
      <li class='comp' data-repeat="item in purchases">
        <div>
          <h3>{{item.product_title}}</h3>
          <h4>SKU: {{item.sku}}</h4>
        </div>
        <aside><p>Price: ${{item.wholesale_price}}</p></aside>
      </li>
    </ul>
    <h2>Total:</h2>
    <ul class="list">
      <li class='comp'>
        <div><h3>Total Items:</h3></div>
        <aside><h4 id="totalItems">{{getTotalItems()}}</h4></aside>
      </li>
      <li class='comp'>
        <div><h3>Total Cost:</h3></div>
        <aside><h4 id="totalCost">${{getTotalCost()}}</h4></aside>
      </li>
    </ul>
    <div id='orderButtons'>
      <a id="placeOrder" href="#" class="button action">Place Order</a>
      <a id="cancelOrder" href="#" class="button action">Cancel Order</a>
    </div>
  </section>
</article>
```

As you can see from this template, we'll need to initialize a template on the id of the Back button to render the value of the previous view, which was the chosen fragrance. For the shopping cart we're going to use the id of the article as the root scope of the template. Here's our template initializers:


```
app.backToPerfume = soma.template.create($('#backToPerfume')[0]);
app.cart = soma.template.create($('#cart')[0]);
```

Now let's wire up the Add button in the toolbar of the detial view. We're going to use regular jQuery events to handle both toolbar buttons. The event handlers will manipulate template scope values as we've done previously.


```
//======================
// Add to Shopping Cart:
//======================
$('#addToCart').on('singletap', function() {
  $.UIGoToArticle('#cart');
  // Update cart with added perfume data:
  app.perfumeDetail.scope.chosenPerfume.genreTitle = app.perfumesGenreTitle.scope.title;
  // Render cart template:
  app.cart.scope.purchases.push(app.perfumeDetail.scope.chosenPerfume);
  app.cart.scope.disabled = false;
  app.cart.render();
  // Update 'Back Button' text template:
  app.backToPerfume.scope.perfumeName = app.perfumeDetail.scope.chosenPerfume.product_title;
  app.backToPerfume.render();
});
```
We use the `$.UIGoToArticle` method to direct the user to the shopping cart article. Next we update the scope of the cart title template and the scope of the cart template. Then we render all the templates. This should all start looking familiar to you at this point. From one template to the next we're using similar techniques to pass values to the next template and then rendering it.

For the button to view the shopping cart, we're going to create a helper method to see if the cart is empty or not. If it's empty, we're going to create a ChocolateChip-UI popup to let the user know and advise them to add some items to the cart before trying to view it.


```
// Popup for when cart is empty:
app.cartIsEmpty = function() {
  $.UIPopup({
    id: "warning",
    title: 'Empty Cart!', 
    cancelButton: 'Close', 
    message: 'The shopping cart is empty. Add some items using the "+" button on the lower left.'
  });
};

$('#shoppingCart').on('singletap', function() {
  // If shopping cart is empty, show popup message:
  if (!app.cart.scope.purchases.length) {
    app.cartIsEmpty();
    return;
  }
  // Otherwise, go to cart view:
  $.UIGoToArticle('#cart');
});
```

To complete the shopping cart, we need to define two scope methods, one to determine the total number of items the user has placed in the shopping cart and another to determine the total cost. If you look at the shopping cart template again, you'll find these two methods: `getTotalItems()` and `getTotalCost()`.


```
//===========================================
// Calculate how many items have been chosen.
// This is used in the shopping cart:
//===========================================
app.cart.scope.purchases = app.purchases;
app.cart.scope.disabled = true;

app.cart.scope.getTotalItems = function() {
  if (!app.cart.scope.purchases.length) return 0;
  else return app.cart.scope.purchases.length;
};


//==========================
// Render Confirmation view:
//==========================
app.confirmation.scope.getTotalCost = app.cart.scope.getTotalCost = function() {
  var total = 0;
  app.cart.scope.purchases.forEach(function(item) {
    total += Number(item.wholesale_price);
  });
  return total.toFixed(2);
};  
```

By the way, using the JavaScript method `toFixed(2)` will return the values with two decimal places. No magic here.

When the user adds something to the cart, the Add button will direct the user to the cart so he or she can see what they've place in there. It might look like this:


![View of available genres](images/fragranz/shopping-cart.png)

The shopping cart does not lead the user anywhere. However, there are two actions buttons, one to place the order and the other to cancel. We can attach normal events to make these work:

```
//=============
// Place Order:
//=============
$('#placeOrder').on('singletap', function() {
  // Go to order confirmation view:
  $.UIGoToArticle('#confirmation');
  // Create a uuid for the order:
  $('#confirmationNum').text($.Uuid());
  // Update the confirmation view template:
  app.confirmation.scope.purchases = app.cart.scope.purchases;
  app.confirmation.render();
});

//==============
// Cancel Order:
//==============
$('#cancelOrder').on('singletap', function() {
  // Return to the main view:
  $.UIGoBackToArticle('#main');
  // Reset the shopping cart:
  app.cart.scope.purchases = [];
  app.cart.render();
}); 
```

The Cancel button will send the user back to the main screen using `$.UIGoBackToArticle('#main')` and it will set the purchases array to empty – `app.cart.scope.purchases = []` – and render the shopping cart template again so that it is empty.

The Place Order button will direct the user to one more view, the confirmation page. This does nothing but display a confirmation code, the names of the items purcased and the total cost. Of course there is a back button in the navbar.

Here's the template for the confirmation view:


```
<nav class="next">
  <a href='#' class='button back' id='backToCart'>Cart</a>
  <h1>Confirmation</h1>
</nav>
<article id="confirmation" class="next">
  <section>
    <ul class="list">
      <li>
        <h3>Your Order Was Successful!</h3>
        <h4>Confirmation number: <span id="confirmationNum"></span></h4>
      </li>
    </ul>
    <h2>Purchase Details:</h2>
    <ul class="list">
      <li data-repeat="item in purchases">
        <h3>{{item.genreTitle}}: {{item.product_title}}</h3>
      </li>
    </ul>
    <h2>Total Amount Charged:</h2>
    <ul class="list">
      <li>
        <h3><span id="confirmTotalCost">${{getTotalCost()}}</span></h3>
      </li>
    </ul>
  </section>
</article>
```


We initialize this template like this:


```
app.confirmation = soma.template.create($('#confirmation')[0]);
```

In the Place Order event handler we're attaching the purchases to the confirmation template's scope so that the repeat and output the items in the cart.


We hope you were able to follow the process for putting this app together. It really isn't doing anything to serious. The main thing to understand is how the to pass values from one template's scope to another as the user interacts with them.


Depending on the items in the cart, the confirmation view might look like this:

![View of available genres](images/fragranz/order-confirmation.png);


```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <title>Fragranž</title>  
    <link href="images/apple-touch-icon-120x120.png" sizes="120x120" rel="apple-touch-icon">
    <link href="images/apple-touch-icon-114x114.png" sizes="114x114" rel="apple-touch-icon">
    <link href="images/apple-touch-startup-image-640x1096.png" rel="apple-touch-startup-image" sizes="640x1096">
    <link href="images/apple-touch-startup-image-640x920.png" media="(device-width: 320px) and (device-height: 480px) and (-webkit-device-pixel-ratio: 2)" rel="apple-touch-startup-image">
    <link rel="stylesheet" href="chui/chui-ios-3.5.5.css">
    <link rel="stylesheet" href="css/style.css">
    <script type='text/javascript' src='chui/jquery-2.1.1.min.js'></script>
    <script type='text/javascript' src='chui/chui-3.5.5.js'></script>
    <script src="js/soma-template.js"></script>
    <script type='text/javascript' src='js/app.js'></script>
  </head>
  <body>
  <nav class='current'>
    <h1 id='mainTitle'>Fragranž</h1>
  </nav>   
  <article id='main' class='current'>
    <section>
      <h2 class='wrap normal-case'>Choose from one of the categories below to see which fragrances are available.</h2>
      <ul class='list' id='perfumeGenres'>
        <li class='nav data-cloak' data-singletap='getGenre()' data-goto='perfumeList' data-genre='{{genre}}' data-title='{{capitalize(genre)}}' data-repeat="genre in genres">
          <h3>{{capitalize(genre)}}</h3>
        </li>
      </ul>
      <div id='fragranz_small' class='horizontal centered'>
        <img src='images/fragranz_small.png'>
      </div>
    </section>
  </article> 
  <nav class='next'>
    <a href='#' class='button back'>Perfumes</a>
    <h1 id='perfumesGenreTitle'>{{title}}</h1>
  </nav> 
  <article class='next' id='perfumeList'>
    <section>
      <ul class='list' id='available_perfumes'>
        <li data-sku='{{perfume.sku}}' data-singletap='getChosenPerfume()' class='comp' data-repeat='perfume in selectedGenre' data-goto='detail'>
          <aside>
            <img width='60' data-src='{{perfume.img_prev}}'>
          </aside>
          <div>
            <h3 class='productTitle'>{{perfume.product_title}}</h3>
            <h4>{{perfume.sku}}</h4>
            <h4>{{perfume.short_description}}</h4>
          </div>
          <aside>
            <span class='counter'>${{perfume.wholesale_price}}</span>
            <span class='show-detail'></span>
          </aside>
        </li>
      </ul>
    </section>
  </article>
  <nav class='next' id='detailNavbar'>
    <a href='#' class='button back' id='backToGenre'>{{chosenPerfume.product_title}}</a>
    <h1 id='detailTitle'>{{chosenPerfume.product_title}}</h1>
  </nav>
  <article class='next' id='detail'>
    <section>
      <ul class='list' id='perfumeDetail'>
        <li>
          <img data-src="{{chosenPerfume.img_prev}}">
          <h3 class="productTitle">{{chosenPerfume.product_title}}</h3>
          <h4><span class="sku">SKU: {{chosenPerfume.sku}}</span></h4>
          <h4 class="counter flush">${{chosenPerfume.wholesale_price}}</h4>
          <p class="longDescription">{{chosenPerfume.long_description}}</p>
        </li>
      </ul>
    </section>
  </article>
  <div class='toolbar next'>
    <a href="javascript:void(null)" id='addToCart' class="button add"></a>
    <a href="javascript:void(null)" data-disabled="{{disabled}}" id="shoppingCart" class="button align-flush"></a>
  </div>
  <nav class="next">
    <a href='#' class='button back' id='backToPerfume'>{{perfumeName}}</a>
    <h1>Cart</h1>
  </nav>
  <article id="cart" class="next">
    <section>
      <h2>Your Purchase:</h2>
      <ul class="list" id="purchaseItems">
        <li class='comp' data-repeat="item in purchases">
          <div>
            <h3>{{item.product_title}}</h3>
            <h4>SKU: {{item.sku}}</h4>
          </div>
          <aside><p>Price: ${{item.wholesale_price}}</p></aside>
        </li>
      </ul>
      <h2>Total:</h2>
      <ul class="list">
        <li class='comp'>
          <div><h3>Total Items:</h3></div>
          <aside><h4 id="totalItems">{{getTotalItems()}}</h4></aside>
        </li>
        <li class='comp'>
          <div><h3>Total Cost:</h3></div>
          <aside><h4 id="totalCost">${{getTotalCost()}}</h4></aside>
        </li>
      </ul>
      <div id='orderButtons'>
        <a id="placeOrder" href="#" class="button action">Place Order</a>
        <a id="cancelOrder" href="#" class="button action">Cancel Order</a>
      </div>
    </section>
  </article>
  <nav class="next">
    <a href='#' class='button back' id='backToCart'>Cart</a>
    <h1>Confirmation</h1>
  </nav>
  <article id="confirmation" class="next">
    <section>
      <ul class="list">
        <li>
          <h3>Your Order Was Successful!</h3>
          <h4>Confirmation number: <span id="confirmationNum"></span></h4>
        </li>
      </ul>
      <h2>Purchase Details:</h2>
      <ul class="list">
        <li data-repeat="item in purchases">
          <h3>{{item.genreTitle}}: {{item.product_title}}</h3>
        </li>
      </ul>
      <h2>Total Amount Charged:</h2>
      <ul class="list">
        <li>
          <h3><span id="confirmTotalCost">${{getTotalCost()}}</span></h3>
        </li>
      </ul>
    </section>
  </article>
  </body>
</html>
```

And here's the app.js file:

```
$(function () {


  /////////////////
  // Initial setup.
  /////////////////

  //======================
  // Create app namespace:
  //======================
  var app = {};

  // Default objects:
  app.purchases = [];
  app.perfumesCollection;


  //=================================
  // Define a template helper.
  // Capitalize first letter of word:
  //=================================
  soma.template.helpers({
    capitalize : function ( str ) {
      if (!str) return;
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  });



  /////////////////////////////////
  // Initalize the app's templates:
  /////////////////////////////////


  //==============================================
  // Define the templates.
  // This creates templates from all DOM ids which 
  // correspond to Somajs Templates in the DOM,
  // thus enabling calling them later by name
  // without having to define them individually.
  //============================================== 
  app.perfumeGenres = soma.template.create($('#perfumeGenres')[0]);
  app.perfumesGenreTitle = soma.template.create($('#perfumesGenreTitle')[0]);
  app.available_perfumes = soma.template.create($('#available_perfumes')[0]);
  app.detailNavbar = soma.template.create($('#detailNavbar')[0]);
  app.perfumeDetail = soma.template.create($('#perfumeDetail')[0]);
  app.backToPerfume = soma.template.create($('#backToPerfume')[0]);
  app.cart = soma.template.create($('#cart')[0]);
  app.confirmation = soma.template.create($('#confirmation')[0]);



  //==============================
  // Get the data to be displayed:
  //==============================

  $.getJSON('data/perfumes.json', function(data) {
    // Make acquired data available to templates:
    app.perfumesCollection = data;
    // Render first template:
    app.perfumeGenres.render();
  });


  //================================================================
  // Set up genres for inital page load. This will be used to
  // filter perfumes based on which genre the user selects.
  // Define a method to get the genre of perfume (ladies, men, kids).
  // This filters the genre from the total perfumes object.
  // Then renders the template on the next screen with that genre.
  //================================================================
  app.perfumeGenres.scope.genres = ['ladies', 'men', 'kids'];
  
  app.perfumeGenres.scope.getGenre = function(event) {
    var title = app.perfumesGenreTitle.scope.title = event.target.getAttribute('data-title');
    var perfumeGenre = event.target.getAttribute('data-genre');
    //===================================
    // Update the title of the next view:
    //===================================
    app.perfumesGenreTitle.render();
    //=============================================
    // Filter the data based on the user selection:
    //=============================================
    var whichPerfumes = app.perfumesCollection.filter(function(item) {
      return item.genre === perfumeGenre;
    })
    app.available_perfumes.scope.selectedGenre = whichPerfumes;
    //===============================================
    // Update the next template for the chosen genre:
    //===============================================
    app.available_perfumes.render();
  };


  //================================================
  // Get the chosen perfume and render its template:
  //================================================
  app.available_perfumes.scope.getChosenPerfume = function(e) {
    var item = e.target.nodeName === 'LI' ? e.target : $(e.target).closest('li')[0];
    var sku = item.getAttribute('data-sku');
    var chosenPerfume = app.available_perfumes.scope.selectedGenre.filter(function(perfume) {
       return perfume.sku === sku;
    });

    //=========================
    // Update the navbar title:
    //=========================
    app.detailNavbar.scope.chosenPerfume = chosenPerfume[0];
    app.detailNavbar.scope.chosenPerfume.genreTitle = app.perfumesGenreTitle.scope.title;
    app.detailNavbar.render();

    //========================
    // Update the detail view:
    //========================
    app.perfumeDetail.scope.chosenPerfume = chosenPerfume[0];
    app.perfumeDetail.render();
  };


  //===========================================
  // Calculate how many items have been chosen.
  // This is used in the shopping cart:
  //===========================================
  app.cart.scope.purchases = app.purchases;
  app.cart.scope.disabled = true;
  
  app.cart.scope.getTotalItems = function() {
    if (!app.cart.scope.purchases.length) return 0;
    else return app.cart.scope.purchases.length;
  };


  //==========================
  // Render Confirmation view:
  //==========================
  app.confirmation.scope.getTotalCost = app.cart.scope.getTotalCost = function() {
    var total = 0;
    app.cart.scope.purchases.forEach(function(item) {
      total += Number(item.wholesale_price);
    });
    return total.toFixed(2);
  };    




  ///////////////////////////
  // Setup User Interactions:
  ///////////////////////////

  //======================
  // Add to Shopping Cart:
  //======================
  $('#addToCart').on('singletap', function() {
    $.UIGoToArticle('#cart');
    // Update cart with added perfume data:
    app.perfumeDetail.scope.chosenPerfume.genreTitle = app.perfumesGenreTitle.scope.title;
    // Render cart template:
    app.cart.scope.purchases.push(app.perfumeDetail.scope.chosenPerfume);
    app.cart.scope.disabled = false;
    app.cart.render();
    // Update 'Back Button' text template:
    app.backToPerfume.scope.perfumeName = app.perfumeDetail.scope.chosenPerfume.product_title;
    app.backToPerfume.render();
  });


  //====================
  // View Shopping Cart:
  //====================

  // Popup for when cart is empty:
  app.cartIsEmpty = function() {
    $.UIPopup({
      id: "warning",
      title: 'Empty Cart!', 
      cancelButton: 'Close', 
      message: 'The shopping cart is empty. Add some items using the "+" button on the lower left.'
    });
  };

  $('#shoppingCart').on('singletap', function() {
    // If shopping cart is empty, show popup message:
    if (!app.cart.scope.purchases.length) {
      app.cartIsEmpty();
      return;
    }
    // Otherwise, go to cart view:
    $.UIGoToArticle('#cart');
  });

  //=============
  // Place Order:
  //=============
  $('#placeOrder').on('singletap', function() {
    // Go to order confirmation view:
    $.UIGoToArticle('#confirmation');
    // Create a uuid for the order:
    $('#confirmationNum').text($.Uuid());
    // Update the confirmation view template:
    app.confirmation.scope.purchases = app.cart.scope.purchases;
    app.confirmation.render();
  });

  //==============
  // Cancel Order:
  //==============
  $('#cancelOrder').on('singletap', function() {
    // Return to the main view:
    $.UIGoBackToArticle('#main');
    // Reset the shopping cart:
    app.cart.scope.purchases = [];
    app.cart.render();
  });  
});
```


Please go to our Github account and download the [complete project](https://github.com/sourcebits-robertbiggs/Fragranz) so you can run it and see how it all works together.

If you use some other solution for templating, you will need to use more events to pass the chosen data from user actions on to the next template and render it so that the app shows only the relevant information.
