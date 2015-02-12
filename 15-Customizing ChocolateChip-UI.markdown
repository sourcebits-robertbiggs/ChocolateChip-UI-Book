#Customizing ChocolateChip-UI Widgets

Because ChocolateChip-UI is just a combination of HTML, CSS and JavaScript, it's easy to customize the layouts and controls to suit your purposes. In this chapter we will look at how to customize or change the default behaviors of a number of controls.


- Toggle Panels
- Tabbar
- Slide Out Menus
- Split Layouts


###Toggle Panels

ChocolateChip-UI has the UIPanelToggle method to allow you to use a segmented control to toggle a set of panels. This is really easy to set up. To initialize a set of togglable panels, you execute the `UIPanelToggle` method on the segemented control and pass it a selector for the container of panels you want to toggle:

```
$('.segmented').UIPanelToggle('#toggle-panels');
```

If you want a callback to be execute when the user taps a segmented control button, you can provide that in the initialization of the segmented control itself:

```
// Define callback for segmented control:
var segmentedResponse = function(e) {
  e.stopPropagation();
  $('#output').find('h3 > span').html(($(this).index() + 1));
};

// Initialize the segmented control with the callback:
$('.segmented').UISegmented({
  selected: 1, 
  callback:segmentedResponse
});
```

####Customizing the Toggle Panel

You may prefer to have a single panel and load the content dynamically when the user taps a segmented control button. This is easy to accomplish. Here is the data and markup we are going to use for a dynamic toggle panel:

```
var fruits = [
  {
    "name": "Apples",
    "uses": ["Pastries", "Juice", "Cider"],
    "benefits": ["Reduce bad cholesterol", "Reduce risk to several types of cancer"],
    "image": "./images/apple.png"
  },
  {
    "name": "Oranges",
    "uses": ["Juice", "Salad", "Desserts"],
    "benefits": ["Boost immune system", "High in fiber"],
    "image": "./images/orange.png"
  },
  {
    "name": "Bananas",
    "uses": ["Smoothies", "Desserts", "Salad"],
    "benefits": ["Good source of potassium", "Contain C, B-6, manganese and fiber"],
    "image": "./images/banana.png"
  }
];
```

Notice that we just have one panel with stubs for where the data will go:

```
<body>
    <nav class="current">
      <h1>Toggle Panels</h1>
    </nav>
    <article id="main" class="current">
      <section>
        <h2>Dynamic Content</h2>
        <div id='segmentedPanel' class='horizontal centered'>
        </div>

        <div id="dynamicContent">
          <ul class="list" id='fruit'>
            <li class="comp">
              <aside><img src="" alt=""></aside>
              <div>
                <h3></h3>
              </div>
            </li>
          </ul>
          <h2>Uses</h2>
          <ul id='uses' class='list'>
          </ul>
          <h2>Benefits</h2>
          <ul id='benefits' class='list'>
          </ul>
        </div>
      </section>
    </article>
</body>
```

To get started, we need to create a segmented control. We’ll use the following options to initialize it:


```
var segmentedOptions = {
  id: 'mySegmented',
  labels : ['Apples','Oranges','Bananas']
};
```

We can create or segmented control by passing the above options to $.UICreateSegmented(), storing the result in the variable `fruitSegmentedCtrl`:

```
var fruitSegmentedCtrl = $.UICreateSegmented(segmentedOptions);
```

We can then insert the segmented control into our document. We want to insert it into the div with the id `segmentedPanel` in our above markup:

```
$('#segmentedPanel').append(newSegmented);
```

Although this inserts the segmented control into the desired location, it has no functionality or interactivity yet. We’ll need to initialize it for that.

```
$('#mySegmented').UISegmented({
  selected: 0, 
  callback: getSelection
});
```

We’ve indicated that we want the first segmented button selected and that we will use a callback called `getSelection`. Before we write this callback, though, we need a helper function to determin which button the user tapped. We can find this out by checking the `event.target` property that gets passeds to the callback to determin which button it is in the collection:


```
var getSelection = function(event) {
  var selection = $(event.target).index();
  RenderFruitSelection(selection);
};
```

Since we are dealing with simple data, and ChocolateChip-UI’s list are simple as well, we don’t need any complicate templates to do this. We can get by using string contcatonation.


```
// Define callback for segmented control:
//=======================================
function RenderFruitSelection (index) {
  $('#fruit h3').text(fruits[index].name);
  $('#fruit img').attr('src', fruits[index].image)
  target.find('#uses').empty();
  fruits[index].uses.forEach(function(ctx) {
    target.find('#uses').append('<li><h3>' + ctx +'</h3></li>')
  });
  target.find('#benefits').empty();
  fruits[index].benefits.forEach(function(ctx) {
    target.find('#benefits').append('<li><h3>' + ctx +'</h3></li>')
  });
}
```

Here’s the complete code sample:

```
var target = $('#dynamicContent');

// Define options for segmented control:
//======================================
var segmentedOptions = {
  id: 'mySegmented',
  labels : ['Apples','Oranges','Bananas']
};

// Get which segment the user tapped:
//===================================
var getSelection = function(event) {
  var selection = $(event.target).index();
  RenderFruitSelection(selection);
};

// Define callback for segmented control:
//=======================================
function RenderFruitSelection (index) {
  $('#fruit h3').text(fruits[index].name);
  $('#fruit img').attr('src', fruits[index].image)
  target.find('#uses').empty();
  fruits[index].uses.forEach(function(ctx) {
    target.find('#uses').append('<li><h3>' + ctx +'</h3></li>')
  });
  target.find('#benefits').empty();
  fruits[index].benefits.forEach(function(ctx) {
    target.find('#benefits').append('<li><h3>' + ctx +'</h3></li>')
  });
}

// Render first choice at load time:
//==================================
RenderFruitSelection(0);

// Create segmented control:
//==========================
var fruitSegmentedCtrl = $.UICreateSegmented(segmentedOptions);

// Append segmented control to document:
//======================================
$('#segmentedPanel').append(newSegmented);

// Init Segmented Control:
//========================
$('#mySegmented').UISegmented({
  selected: 0, 
  callback: getSelection
});
```


###Slide Out Menus

ChocolateChip-UI has a slide out menu that allows you to keep the navigation out of sight until the user wants to see it. In this post we’ll look at how to make a slide out menu app that loads data dynamically.

Here is a slide out menu hidden, with the “Hamburger” button in the top left:

![Segmented Control, segment 1](images/widgets/Slide-Out-1.png)

And here is the menu open:

![Segmented Control, segment 1](images/widgets/Slide-Out-2.png)

To create a slide out menu we use the method:

```
$.UISlideout();
```

This creates an empty slide out menu. It also inserts the “hamburger” button into the top left of the navigation bar so that the user can toggle it open and shut. If the you tap the Hamburger Button, the slide out will open, and tapping it again will close the slide out. To populate the slide out menu you would use the following method:

```
// Initialize slide out menu:
$.UISlideout();

// Populate slide out menu:
$.UISlideout.populate([{music:'Music'},{pictures:'Pictures'},{recipes:'Recipes'},{contacts:'Contacts'}]);
```

Notice that each object has a key and value. The key will be the id of the article that the item will show and the value is the label that will appear for that article in the slide out menu. This makes it very simple to create navigation of articles with a slide out menu. However, you will probably prefer to have the content that gets display be dynamic. In that case this arrangement will not work for you.

To change the behavior of the slide out menu so that we can have dynamic content that gets displayed when the user selects a menu item, we can change how we initalize the slide out Instead of simple executing the $.UISlideout method with no arguments, we instead provide it with an object with two items: a dynamic key set to true and a callback.

```
$.UISlideout({dynamic: true, callback: function(li) {
  // Do stuff here.
}});
```

In the above code, notice that the callback gets a parameter of li. This will be the list item that the user taps. By checking the index of the list item, we can tell which one was tapped and then make decisions on how to show data.

So, let’s do something interesting and yummy. We’re going to make a slide out that show recipes. When the user taps on an item, we want to show that recipe. Here’s the markup for our app:

```
<nav class='current'>
  <h1 id='choiceTitle'></h1>
</nav>
<article id='recipes' class='current'>
  <section>
    <ul class="list">
      <li id='recipe'></li>
    </ul>
  </section>
</article>
```

Here’s the data for our recipes:

```
var recipes = [
  {
    "name": "Italian Style Meatloaf",
    "image": "images/meatloaf.jpg",
    "ingredients" : [
      "1 1/2 pounds ground beef",
      "2 eggs, beaten",
      "3/4 cup dry bread crumbs",
      "1/4 cup ketchup",
      "1 teaspoon Italian-style seasoning",
      "1 teaspoon garlic salt",
      "1 (14.5 ounce) can diced tomatoes, drained"
    ],
    "directions": [
      "Preheat oven to 350 degrees F (175 degrees C).",
      "In a large bowl, mix together ground beef, eggs, bread crumbs and ketchup. Season with Italian-style seasoning, oregano, basil, garlic salt, diced tomatoes and cheese. Press into a 9x5 inch loaf pan, and cover loosely with foil.",
      "Bake in the preheated oven approximately 1 hour, or until internal temperature reaches 160 degrees F (70 degrees C)."
    ]
  },
  {
    "name": "Chicken Marsala",
    "image": "images/chicken-marsala.jpg",
    "ingredients" : [
      "1/4 cup all-purpose flour for coating",
      "1/2 teaspoon salt",
      "1/4 teaspoon ground black pepper",
      "1/2 teaspoon dried oregano",
      "4 skinless, boneless chicken breast halves - pounded 1/4 inch thick",
      "4 tablespoons butter",
      "4 tablespoons olive oil",
      "1 cup sliced mushrooms",
      "1/2 cup Marsala wine",
      "1/4 cup cooking sherry"
    ],
    "directions": [
      "In a shallow dish or bowl, mix together the flour, salt, pepper and oregano. Coat chicken pieces in flour mixture.",
      "In a large skillet, melt butter in oil over medium heat. Place chicken in the pan, and lightly brown. Turn over chicken pieces, and add mushrooms. Pour in wine and sherry. Cover skillet; simmer chicken 10 minutes, turning once, until no longer pink and juices run clear."
    ]
  },
  {
    "name": "Chicken Breasts with Lime Sauce",
    "image": "images/chicken-breast-lime.jpg",
    "ingredients" : [
     "4 skinless, boneless chicken breast halves - pounded to 1/4 inch thickness",
     "1 egg, beaten",
     "2/3 cup dry bread crumbs",
     "2 tablespoons olive oil",
     "1 lime, juiced",
     "6 tablespoons butter",
     "1 teaspoon minced fresh chives",
     "1/2 teaspoon dried dill weed"
    ],
    "directions": [
      "Coat chicken breasts with egg, and dip in bread crumbs. Place on a wire rack, and allow to dry for about 10 minutes.",
      "Heat olive oil in a large skillet over medium heat. Place chicken into the skillet, and fry for 3 to 5 minutes on each side. Remove to a platter, and keep warm.",
      "Drain grease from the skillet, and squeeze in lime juice. Cook over low heat until it boils. Add butter, and stir until melted. Season with chives and dill. Spoon sauce over chicken, and serve immediately."
    ]
  },
  {
    "name": "Lemon Rosemary Salmon",
    "image": "images/salmon-lemon-rosemary.jpg",
    "ingredients" : [
      "1 lemon, thinly sliced",
      "4 sprigs fresh rosemary",
      "2 salmon fillets, bones and skin removed coarse salt to taste",
      "1 tablespoon olive oil, or as needed"
    ],
    "directions": [
      "Preheat oven to 400 degrees F (200 degrees C).",
      "Arrange half the lemon slices in a single layer in a baking dish. Layer with 2 sprigs rosemary, and top with salmon fillets. Sprinkle salmon with salt, layer with remaining rosemary sprigs, and top with remaining lemon slices. Drizzle with olive oil.",
      "Bake 20 minutes in the preheated oven, or until fish is easily flaked with a fork."
    ]
  },
  {
    "name": "Strawberry Angel Food Dessert",
    "image": "images/strawberry-shortcake.jpg",
    "ingredients" : [
      "1 (10 inch) angel food cake",
      "2 (8 ounce) packages cream cheese, softened",
      "1 cup white sugar",
      "1 (8 ounce) container frozen whipped topping, thawed",
      "1 quart fresh strawberries, sliced",
      "1 (18 ounce) jar strawberry glaze"
    ],
    "directions": [
      "Crumble the cake into a 9x13 inch dish.",
      "Beat the cream cheese and sugar in a medium bowl until light and fluffy. Fold in whipped topping. Mash the cake down with your hands and spread the cream cheese mixture over the cake.",
      "In a bowl, combine strawberries and glaze until strawberries are evenly coated. Spread over cream cheese layer. Chill until serving."
    ]
  }
];
```

Next were going to create the slide out menu with our configuration and then populate it with our recipes:

```
////////////////////////
// Initialize slide out:
////////////////////////
$.UISlideout({dynamic: true, callback: function(li) {
  // Do something to show a recipe.
}});

///////////////////////////
// Populate slide out menu:
///////////////////////////
$.UISlideout.populate([
  { meatloaf: 'Italian Style Meatloaf' },
  { chicken_marsala: 'Chicken Marsala' },
  { chicken_breasts: 'Chicken Breasts with Lime Sauce' },
  { lemon_salmon: 'Lemon Rosemary Salmon' },
  { angel_food: "Strawberry Angel Food Dessert"}
]);
```

Notice in the above code that we provided keys and value. The value will appear as the label of the slide out menu. The keys do not matter, we could make them anything. When the slide out menu is dynamic, these values will not be used for anything. It’s up to you if you want the keys to make sense or not.

We’re going to define a mediator to handle the execution of outputting a recipe to the app. We therefore need to notify it when the user selects a recipe which one that was. To do this, we will need to add a publish command to our slide out callback with the value of the chosen item. We’ll use the jQuery index method to find out which list item of the list was selected and then we’ll publish that:

```
////////////////////////
// Initialize slide out:
////////////////////////
$.UISlideout({dynamic: true, callback: function(li) {
  var choice = $(li).index();
  $.publish('chosen-item', choice);
}});
```

Now that we have a publication of the name “chosen-item” with the index of the chose list item, we can create a mediator, `ChosenRecipeMediator`, to output a recipe:

```
/////////////////////////////////////
// Define mediator to display recipe:
/////////////////////////////////////
ChosenRecipeMediator = $.subscribe('chosen-item', function(event, choice) {
  // Close slide out:
  $('.slide-out').removeClass('open');
  // Render template to screen:
  showRecipe(recipes[choice])
});
```

So, our mediator listen for the broadcast of a recipe choice. When that happens, it executes the function showRecipe to output the recipe with the index of the tapped list item (choice). Now we just need to define showRecipe. Since these recipes are fairly simple, we don’t need any complex templating. Simple string concatanation will be sufficient:

```
////////////////////////
// Create recipe markup:
////////////////////////
app.showRecipe = function(recipe) {
  var recipeScope = $('#recipe');
  recipeScope.empty();
  recipeScope.append("<h3>" + recipe.name + "</h3>");
  recipeScope.append("<img title='" + recipe.name + "' src='" + recipe.image + "'>");
  var ingredients = '<h4>Ingredients</h4><ul>';
  recipe.ingredients.forEach(function(item) {
    ingredients += "<li>" + item + "</li>";
  });
  ingredients += "</ul>";
  recipeScope.append(ingredients);
  var directions = "<p>Directions</p><ol>";
  recipe.directions.forEach(function(item) {
    directions += "<li>" + item + "</li>";
  });
  directions += '</ol>'
  recipeScope.append(directions);
  $('#choiceTitle').text(recipe.name);
};
```

With all of these defined, we now have a working slide out that loads content dynamically. We only need one navigation bar and one article. We do need to do one more thing. We want the first recipe to show when the app loads, otherwise the screen will be empty. To do this we just need to publish the index for the first recipe:

```
///////////////////////////////
// Initial setup for load time.
// Load first recipe:
///////////////////////////////
$.publish('chosen-item', 0);
```






###Split Layouts

Normally, a split layout consists of two parts: a master article and a detail article. The master is the main navigation control on the side. When the user interacts with it, the content in the detail gets updated. The structure looks like this:

```
  <nav>
    <h1>Pick One</h1>
  </nav>
  <article class='master'>
    <section id="scroller2">
      <ul class='list'>
      </ul>
    </section>
  </article>
  <div class="toolbar current">
    <button>Button 1</button>
  </div>
  <nav>
    <h1 id='detailTitle'>Detail 1</h1>
  </nav>
  <article class='detail'>
    <section>
      <ul class='list'>
    </section>
  </article>
  <div class="toolbar current">
    <button>Button 2</button>
    <button class='align-flush'>Button 3</button>
  </div>
  ```

  To make this work, you would register a delegated event on the list in the master article. This would let you know which item the user tapped so you could update the content of the detail.

  ```
$('.master ul').on('singletap', 'li', function() {
  $.UISetHashOnUrl('#' + $(this).attr('data-url'));
  $(this).siblings().removeClass('selected');
  $(this).addClass('selected');
  $('#detailTitle').text(this.textContent);
  var detail = $('.detail section');
  detail.empty();
  detail.UIBusy({'color':'#000', 'size': '100'});
  var url = '../data/splitlayout/' + this.getAttribute('data-url') + '.html';
  // Delay 1/2 second to show busy indicator while loading.
  // Please do not do this for real world app!
  setTimeout(function() {
    $.get(url, function(data) {
      detail.html(data);
    });
  }, 500);
});
```

This is fine, but you might need some flexibility in how the master article works, especially if you have more hierarchical data. You might actually prefer to have a navigation list in the master article, allowing the user to drill down to a specific article and from there enable updating of the detail article.

To enable such behavior, we need to have two master articles in our document, one with a class of `current` and the other with a class of `next`:

```
<nav class='current'>
  <h1>Pick Region</h1>
</nav>
<article id='master1' class='master current'>
  <section>
    <ul class='list'>
      <li data-goto="master2" data-region='West' class='nav'>
        <h3>West Coast</h3>
      </li>
      <li data-goto="master2" data-region='East' class='nav'>
        <h3>East Coast</h3>
      </li>
      <li data-goto="master2" data-region='Midwest' class='nav'>
        <h3>Midwest</h3>
      </li>
      <li data-goto="master2" data-region='South' class='nav'>
        <h3>South</h3>
      </li>
    </ul>
  </section>
</article>
<nav class='next'>
  <button class="back">Back</button>
  <h1>Stuff here</h1>
</nav>
<article id='master2' class='master next'>
  <section>
    <ul class='list'>
      <li>
      </li>
    </ul>
  </section>
</article>
<nav>
  <h1 id='detailTitle'>Detail 1</h1>
</nav>
<article class='detail'>
  <section>
    <ul class='list'>
  </section>
</article>
```

If you examine the first master article, you will notice that we've hardcoded it to have four navigable list items. They all lead to the second master article `data-goto="master2"`. In the markup, the content of the second master article is empty. We will populate it when the user taps one of the list items in the first master article.

Since this is just a tutorial, we are not going to provide all 50 states. So if you don't see your state, don't take offense. Here is the data we are going to use. Notice that each state has a region associated with it. This will enable use to navigate to a group of states based on which list item the user tapped in the first master article.

```
var masterData = [
  { title: "California", "data_url": 'california', region: "West"},
  { title: "Oregon", "data_url": 'oregon', region: "West" },
  { title: "Washington", "data_url": 'washington', region: "West" },
  { title: "Colorado", "data_url": 'colorado', region: "West" },
  { title: "Nevada", "data_url": 'nevada', region: "West" },
  { title: "Arizona", "data_url": 'arizona', region: "West" },
  { title: "New York", "data_url": 'newyork', region: "East" },
  { title: "Massachussetts", "data_url": 'massachussetts', region: "East" },
  { title: "Ohio", "data_url": 'ohio', region: "Midwest" },
  { title: "Illinois", "data_url": 'illinois', region: "Midwest" },
  { title: "Iowa", "data_url": 'iowa', region: "Midwest" },
  { title: "Texas", "data_url": 'texas', region: "South" },
  { title: "New Jersey", "data_url": 'newjersey', region: "East" },
  { title: "Georgia", "data_url": 'georgia', region: "South" },
  { title: "Florida", "data_url": 'florida', region: "South" },
  { title: "Virginia", "data_url": 'virginia', region: "South" },
  { title: "Nebraska", "data_url": 'nebraska', region: "Midwest" },
  { title: "Connecticut", "data_url": 'connecticut', region: "East" }
];
```

In order to get the states associated with the list item which the user tapped, we need to filter our above array of states. Once we have the array filtered, we can output the states to the second master list. Below is the template and the event handler to accomplish this. We are using ChocolateChip-UI's own template module for this:

```
// Define template for reginal states:
var masterTemplate = 
      '<li data-url="[[=data.data_url]]">[[=data.title]]</li>';
// Parse the template:
var myTmpl8 = $.template(masterTemplate);

// Define event handler for first master list.
// This will output the regional states 
// in the second master list:
$('#master1 ul').on('singletap', 'li', function() {
  var whichRegion = this.getAttribute('data-region');
  var states = masterData.filter(function(state) {
    return state.region === whichRegion
  });
  $('#master2 ul').empty();
  states.forEach(function(ctx) {
    $('#master2 ul').append(myTmpl8(ctx));
  });
});
```

With the above code, when the user tabs a region in the first master article, the app navigates to the second master article and populates it with states matching the region which the user chose.

Next we need to define an event handler for the second master list so that when the user taps a state, its information loads in the detail article.

```
$('#master2 ul').on('singletap', 'li', function() {
  $.UISetHashOnUrl('#' + $(this).attr('data-url'));
  $(this).siblings().removeClass('selected');
  $(this).addClass('selected');
  $('#detailTitle').text(this.textContent);
  var detail = $('.detail section');
  detail.empty();
  detail.UIBusy({'color':'#000', 'size': '100'});
  var url = '../data/splitlayout/' + this.getAttribute('data-url') + '.html';
  // Delay 1/2 second to show busy indicator while loading.
  // Please do not do this for real world app!
  setTimeout(function() {
    $.get(url, function(data) {
      detail.html(data);
    });
  }, 500);
});
```

You can find a working example of this in the example folder of ChocolateChip-UI. It's called `splitlayout-with-nav.html`. 

We took the default split layout and turned it into a dynamic, multi-level navigation interface for more meaningful access of the data for the user. Be aware that you don't want to make an app where the user has to navigate through many screens to get to where he or she needs to be. Keep the navigation as shallow and simple as possible.






