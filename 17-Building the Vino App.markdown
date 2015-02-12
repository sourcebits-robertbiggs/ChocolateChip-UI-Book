#Building the Vino App

In this chapter we’ll explain how we created Vino, an app about the wines of Northern California. The reason we created it was not because we like California wines, although we do, but because we wanted something that could serve as a reference for how to build an app that could function as a standalone Web app, or as the basis for a hybrid app.

When you launch the app, you are presented with top picks of red and white wines, as well as the ability to search for a wine by type, body and price. You can tap a wine in the returned search list to see more details about the wine. From there you can choose to purchase or you can check the location of the winery. On Android it opens the winery in Google Maps, on iOS, in Apple Maps, and on Windows in Bing. From there you can see customer reviews, ratings, driving directions, etc. Note that each operating system has its own branch in the Git repository. The default is iOS.

Because this is a reference app, we are not actually doing a complete purchase process. We are just showing what that could look like. The purchase process would depend on what commerical solution you have chosen to use for your app.
We have organized this app into three platform-specific versions for a very good reason. Instead of using the same app and switching out the theme for each platform, we want each version to serve as a standalone app that can be converted into a hybrid app.

In this article we will cover all the major parts of the app. There are a number of minor details we will not cover when they are not part of the main workflow. You can get this app on Github. The main branch is the iOS version, but it also has a branch for Android and Windows Phone 8.

##Data

We gathered data on over two hundred wines. They vary by type, body and price to give us a good range of available wines. We also created a small subset as top picks for reds and whites. The top picks have the following structure:

```
{
  "id": "002",
  "name": "Leonardini Merlot",
  "winery": "Whitehall",
  "year": "2010"
}
```

We use this data to output the bare minimal information about the wine into a set of carousels, allowing the user to scroll through them to explore what is available. Each wine has a unique id in the main data. And the ids in the top picks match those. This will allow us to get the full details of a wine if the user taps on a carousel item. Here’s an example of a complete wine object:


```
{
  "id": "001",
  "name": "2011 Napa Valley Merlot",
  "type": "Red",
  "varietal": "Merlot",
  "description": "Our perennial favorite Napa Valley Merlot.  Always a robust, complex blend anchored by exceptional Napa Valley Merlot.",
  "body": "Medium",
  "winery": "Whitehall Lane Winery",
  "region": "Napa",
  "address": "1563 St. Helena Hwy South, St. Helena, CA 94574",
  "location": "Whitehall+Lane+Winery",
  "price": "$28.00",
  "label": {
    "name": "Napa Valley Merlot",
    "winery": "Whitehall",
    "year": "2011"
  },
  "lat": "38.477548",
  "lng": "-122.437839"
}
```

As you can see, each wine has a lot of useful information which we can use to filter or show to the user.

Large data sets are terrible for mobile apps. We don’t want the user to have to scroll through unnecessarily long lists. So an important aspect of this app will be to provide ways to reduce the data into convenient chucks that they user can examine. Because our data is ultimate just an array of wine objects, we can use the JavaScript array methods to filter and map the data to what we need.

##Customization

This app has themes for Android, iOS and Windows Phone 8. The design team wanted certain customizations of the default look and feel of certain stock controls. These customizations are in the `css/styles.css` file. Because of differences in mobile platforms, we made some variations in HTML, CSS and JavaScript. These are limited to each system’s dedicated Git branch.

##Development Approach

We started this project off with the idea of an app for exploring the wines of Northern California. We live in Northern California and really appreciate the incredibles variety of wines that the area produces. To make the app we’d need data. So, we began coming hundreds of websites of wineries, gather information about the wines and the wineries. In the end we had more data than we could use, so we reduced it to just over 200. If we were going to do a real, commercial app, we’d probably settle around 400-500.

Before gathering the data, we had spoken with our design team that we were going to make a wine app, so they were already aware. Now that we had our data, we approached them again. We alread had an idea in our mind of how the app would work. The main screen would have some carousels of top picks. Tapping one would take you to the detail view for that wine. Or, from the same home screen, you could perform a search based on several parameters. Get the search right took the longest, there was a lot of back in forth over how to enable the user to choose the serach parameters. Like the search experiece, we iterated quite a bit on what the purchase experience should be.

All the JavaScript to make the app work will reside in a file called `app.js`.

Here are some screens of the design.

![](images/vino/Home_iPhone 6+.png)

For search, we decided to change the controls to fit available screen size. This meant that on the large iPhone 6 Plus, we would use two select lists, for the iPhone 6 we would turn one select list into a segmented control, and for the iPhone 5s and earlier we would only use segemented controls. This way all the search controllers fit on the screens of the particular devices without required scrolling to see all.

Search on iPhone 6 Plus:

![](images/vino/Find Wine_iPhone 6+.png)

Search on iPHone 6:

![](images/vino/Find Wine_iPhone 5.png)

Here’s the search sheet on an iPhone 5s:

![](images/vino/iphone-5.png)

Search results:

![](images/vino/Wines List_iPhone 6+.png)

Tapping on a wine will take you to the detail view for that wine:

![](images/vino/Detail View.png)

And here is the purchase flow:

![](images/vino/Purchase Flow.png)


With these we were able to start work building the app. We made various changes as the work proceded to accomodate the data, and the nature of Web app development.

##Home Screen

For the home screen hero, we’ll use the navbar. By attaching a background image to the navbar and another one for the text in the H1, we can achieve our design goals. We do need to increase the height of the navbar, and that means we need to change the top position of the article that follows it:

```
<nav class='current' id='california-wines-header'>
  <h1 id='mainTitle'>California Wines</h1>
</nav> 
```

##Initial Data Setup

```
// Get JSON data for wines:
//=========================
var wines;

// Get the JSON for all available California wines:
$.getJSON('./data/californiaWines.js', function(data) {
  wines = data;
});

// Get the JSON for top picks of red and white wines:
$.getJSON('./data/bestWines.js', function(data) {
  var bestReds = data[0].data;
  var bestWhites = data[1].data;

  // Publish the results to notify
  // any template subscribers that
  // the data is available:
  $.publish('wine-top-picks', [bestReds, bestWhites]);
});
```

##Horizontal Scroll Panels

Next up, we need two scroll panels, one for red wines and one for whites. Each wine in a scroll panel will be presented as a line label with basic information. When the user taps a scroll panel item, the app will navigate to the wine’s detail view. Rather than defining the scroll panel multiple times, we’ll use a template. And to make that simple, we’ll define the template in a script tag. We’ll use the template engine native to ChocolateChip-UI:

```
<script type='text/x-template' id='topPicksTemplate'>
  <li data-id='[[=wine.id]]'>
    <div>
      <h3>[[= wine.name ]]</h3>
      <h4>[[= wine.winery ]]</h4>
      <p>[[= wine.year ]]</p>
    </div>
  </li>
</script>
```

This template will be inserted into the following markup:

```
<article id='main' class='current'>
  <section>
    <h2 class='wrap'>Rich Reds</h2>
    <div class="horizontal-scroll-panel">
      <!-- Template for select red wines -->
      <ul id='picksRed'></ul>
    </div>
    <h2 class='wrap'>Bright Whites</h2>
    <div class="horizontal-scroll-panel">
      <!-- Template for select white wines -->
      <ul id='picksWhite'></ul>
    </div>
  </section>
</article>
```

With a template defined, we can now access it from code. This is a two-stage process. First we get the contents of the script template, then we parse it. This turns the template into a function that can receive data before output the resulting markup.

```
// Top Picks (Red & White):
//=========================
// Get template from script tag:
var topPicksTmpl = $('#topPicksTemplate').html();

// Parse the template string and use "wine" variable:
var parsedTopPicks = $.template(topPicksTmpl, 'wine');
```

To output our scroll panels we need to subscribe to a publication. Previously we defined a publication in our Ajax request for the top picks wines. The subscription listens for the publication and when it occurs, it outputs the template with the received data into the two stubs for the scroll panels.

```
// Subscribe to 'wine-top-picks' publication. This
// publication sends the top picks of red and white
// wines through an Ajax request in the Model section.
//=====================================================
var bestRedWines = $.subscribe('wine-top-picks', function(event, wines) {
  // Append rendered items to red picks list:
  wines[0].forEach(function(wine) {
    $('#picksRed').append(parsedTopPicks(wine));
  });
  // Append rendered items to white picks list:
  wines[1].forEach(function(wine) {
    $('#picksWhite').append(parsedTopPicks(wine));
  });
});
```

Here’s the finished scroll panels:

![](images/vino/vino-carousels.png)

##The Detail View

Now that the scroll panels are up and running, we need to make them tappable and lead the user to a detail view of the tapped wine. The detail view looks like this:

```
<nav class="next" id='detailNavbar'>
  <button class="back">Wines</button>
  <!-- selected wine type: will be updated by Mediator -->
  <h1>{{wine.type}}</h1>
  <button id='viewWinery' class='align-flush'>Visit</button>
</nav>
<article class="next" id='selectedWine'>
  <section>
    <div class="hero"></div>
    <!-- selected wine varietal: will be updated by Mediator -->
    <h2>{{wine.varietal}}</h2>
    <!--  template for selected wine -->
    <ul class="list"></ul>
  </section>
</article>
```

We’ll need one template for the list in the article that will hold the wine’s detials. The H1 and H2 will get their contents updated at the same time. Here’s the template for the wine:

```
<script type='text/x-template' id='selectedWineTemplate'>
  <li class='comp'>
    <div>
      <h3>[[= wine.name ]]</h3>
      <h4>Body: [[= wine.body ]]</h4>
      <h4>[[= wine.winery ]]</h4>
      <p>[[= wine.description ]]</p>
    </div>
    <aside>
      <span class="counter">[[=wine.price]]</span>
    </aside>
  </li>
</script>
```

But before we can render this template, we need to set up the tap handler for the carousels so that we know which wine the user selected. We’ll use a delegated event. And then we’ll publish the selected wine in a publication. This decouples the selection process from the template rendering:

```
// Define handler to get wine from 
// main screen wine carousels
// when user taps a wine square:
//=================================
$('.horizontal-scroll-panel').on('singletap', 'li', function() {
  var wine = $(this).attr('data-id');

  // Filter the choice from the wines object:
  var tappedWine = wines.filter(function(item) {
    return item.id === wine;
  });

  // Publish the selected wine:
  $.publish('top-pick-selected-wine', tappedWine[0]);
}); 
```

To render the selected wine, we will create a mediator that subscribes to `top-pick-selected-wine`.

```
// Define Mediator to output chosen wine:
//=======================================
var TopPicsMediator = $.subscribe('top-pick-selected-wine', function(event, wine) {
  renderSelectedWine(wine);
});

// Define method to be used by TopPicsMediator:
//=============================================
var renderSelectedWine = function(wine) {
  // Output selected wine template:
  $('#selectedWine ul').html(parsedSelectedWine(wine));

  // Update screen title and list tiles:
  $('#detailNavbar h1').html(wine.type);
  $('#selectedWine h2').html(wine.varietal);

  // Output image to detail hero:
  outputHeroImg();

  // Go to detail view:
  $.UIGoToArticle('#selectedWine');
};
```

The method `renderSelectedWine` does several things, it gets the data from the publication `top-pick-selected-wine`. Then it updates the H1 and H2 with its data and outputs the template for the details into the list. After that, it navigates the user to the detail view. There’s also an interesting method to output a random image on the detail page `outputHeroImg`.

##Bottom Toolbar

This toolbar holds to buttons, one to show info about the app, and another to show a search sheet. As I mentioned earlier, we decided to have different controls in the search sheet depending on the size of the phone. We therefore need to test at load time what the dimensions of the device.

```
//////////////////////////////////////////
// Initialization
// Check for larger iPhones (6 and 6 Plus)
//////////////////////////////////////////
var isiPhone6Plus = false;
var isiPhone6 = false;
if ($.isiOS && window.innerWidth >= 414) isiPhone6Plus = true;
if ($.isiOS && window.innerWidth === 375) isiPhone6 = true;
if (!$.isStandalone) $('body').addClass('inBrowserMode');
if (!$.isiOS && window.innerWidth >= 414) isiPhone6Plus = true;
if (!$.isiOS && window.innerWidth >= 320 && window.innerWidth < 414) isiPhone6 = true;
```

To create the search sheet, we need a number of different templates. That’s because we will be assembling pieces based on the size of the phone.

```
<!-- Template for the search panel navbar -->
<script type='text/x-template' id="searchNavBarTemplate">
  <nav id='searchNav'>
    <button id='cancelSearch'>Cancel</button>
    <h1 style='width: 120px;'>Find Wine</h1>
    <button id='startSearch'>Search</button>
  </nav>
</script>
<!-- Template for the iPhone 6 -->
<script type='text/x-template' id="searchiPhone6TypeTemplate">
  <h2>Type</h2>
  <ul id='wineType' class='list'>
    <li><h3>Red</h3></li>
    <li><h3>White</h3></li>
    <li><h3>Sparkling</h3></li>
    <li><h3>Dessert</h3></li>
  </ul>
</script>
<!-- Template for the iPhone 6 Plus -->
<script type='text/x-template' id="searchiPhone6PlusBody">
  <h2>Body</h2>
  <ul id='wineBody' class='list'>
    <li><h3>Light</h3></li>
    <li><h3>Medium</h3></li>
    <li><h3>Heavy</h3></li>
  </ul>
</script>
<!-- Template for the iPhone 6 -->
<script type='text/x-template' id="searchiPhone6Body">
  <h2>Body</h2>
  <ul class='list'>
    <li>
      <div id='varietySegmentedPanel' class='horizontal centered'>
        <div class='segmented' id='varietySegmented'>
          <button role='radio'>Light</button>
          <button role='radio' aria-checked='true' class='selected'>Medium</button>
          <button role='radio'>Heavy</button>
        </div>
      </div>
    </li>
  </ul>
</script>
<!-- Template for the price range control -->
<script type='text/x-template' id="priceSelector">
  <h2 id='choosePriceLabel'>Price: <label id='priceRangeOutput'>$<span data-model='selectedWinePrice'></span></label></h2>
  <ul class='list'>
    <li>
      <div id='pricePanel' class='horizontal centered'>
        <input data-controller='selectedWinePrice' type='range' name='priceRangeInput' id='priceRangeInput' step='20' value='20' min='20' max='100'>
      </div>
    </li>
  </ul>
</script>
<!-- Template for the search panel -->
<script type='text/x-template' id="searchPanel">
  <h2>Type</h2>
  <ul class='list'>
    <li>
      <div id='typeSegmentedPanel' class='horizontal centered'><div class='segmented' id='typeSegmented'><button role='radio' aria-checked='true' class='selected'>Red</button><button role='radio'>White</button><button role='radio'>Sparkling</button><button role='radio'>Dessert</button></div></div>
    </li>
  </ul>

  <h2>Body</h2>
  <ul class='list'>
    <li>
      <div id='varietySegmentedPanel' class='horizontal centered'><div class='segmented' id='varietySegmented'><button role='radio'>Light</button><button role='radio' aria-checked='true' class='selected'>Medium</button><button role='radio'>Heavy</button></div></div>
    </li>
  </ul>

  <h2 id='chosePriceLabel'>Price: <label id='priceRangeOutput'>20.00</label></h2>
  <ul class='list'>
    <li>
      <div id='pricePanel' class='horizontal centered'>
        <input type='range' name='priceRangeInput' id='priceRangeInput' step='20' value='20' min='20' max='100' style='background-size: -1px 10px;'>
      </div>
    </li>
  </ul>
</script>
```

##Implementing Search

We had to write a complex handler to check the values that we initalized based on the device’s width to build the search sheet correctly. Regardless whether it ouputs a select list or segmented control, it initializes them upon inserting them into the document.

```
// Define handler to populate
// search sheet based on
// width of device. This should
// produce optimal sheet for
// iPhone 5 - 5S, iPone 6 & 6 Plus,
// iPad and desktop Safari.
//=================================
var assembleSearchSheet = function () {
  // For iPhone 6 Plus:
  if (isiPhone6Plus) {
    $('#searchSheet section').html(searchNavBar + searchiPhone6Type + searchiPhone6PlusBody + priceSelector);

    // Initialize select lists:
    $('#wineType').UISelectList({
      selected: 0,
      callback: function() {
        searchParameters.type = $(this).text();
      }
    });

    $('#wineBody').UISelectList({
      selected: 0,
      callback: function() {
        searchParameters.body = $(this).text();
      }
    });

  // For iPhone 6:
  } else if (isiPhone6) {
    $('#searchSheet section').html(searchNavBar + searchiPhone6Type + searchiPhone6Body + priceSelector);

    // Initialize segemented control behavior:
    $('#varietySegmented').UISegmented({
      selected: 1, 
      callback: function() {
        searchParameters.body = $(this).text();
      }
    });

    // Initialize select list:
    $('#wineType').UISelectList({
      selected: 0,
      callback: function() {
        searchParameters.type = $(this).text();
      }
    });

  // For other iPhones:
  } else {
    $('#searchSheet section').html(searchNavBar + searchPanel);

    // Initialize segemented control behavior:
    $('#typeSegmented').UISegmented({
      selected: 0, 
      callback: function() {
        searchParameters.type = $(this).text();
      }
    });

    $('#varietySegmented').UISegmented({
      selected: 1, 
      callback: function() {
        searchParameters.body = $(this).text();
      }
    });
  }
};
```

This complex assortment of templates and size checks gives us the best screen use for each phone size. We can now create the search sheet and populate if with this method. The first method creates and empty sheet with an id of searchSheet and the seond populates that sheet based on the available screen real estate:

```
// Create the search sheet:
//=========================
$.UISheet({id:'searchSheet', handle: false});

// Populate search sheet:
//=======================
assembleSearchSheet();
```

##Data Binding for Range Input

Since the search sheet has a range input to indicate the price range, we can bind the value of the range input to the price label above it. If you examine the template for the price range input, you’ll see that the range input has an attribute `data-controller='selectedWinePrice'` and the label has the attribute `data-model='selectedWinePrice'`. Using the value `selectedWinePrice` we can bind the value of the label to the value of the range control:

```
$.UIBindData('selectedWinePrice');
```

If you look at the template for the sheets navbar, it has a button to cancel the search and one to perform the search. To enable the cancel button we attach a handler to it:

```
// Hide search sheet when user taps Cancel:
//=========================================
$('#searchSheet').on('singletap', '#cancelSearch', function() {
  $.UIHideSheet();
  // Turn off window resize for price range input:
  window.onresize = null;
});
```

To perform a search we need to create a handler that will check what are the current values of the select sheets, segmented controls and range input in the search sheet and use those to filter down the available wines. `searchParameters` is an object initialized in our search sheet handler. When the user interacts with the select lists, segmented controls and range input, their values are updated on that object. We access those values off of that object to filter the wines.

After the user taps the search button, we also need to close the shearch sheet, then navigate to the results page. Before output the result, we check to see if the array has a length of 0, which means there was no match. If so, we mention that in the results page and encourage the user to go back and change the search parameters for another search. It is possible to choose parameters of type, body and price with no search results.

```
// Reference to search results output:
var filteredWinesList = $('#filteredWines');

// Event listener to search for wines:
$('#searchSheet').on('singletap', '#startSearch', function() {

  // Filter wines based on user choices.
  // These are stored on searchParameters object:
  var filteredWines = wines.filter(function(wine) {
    return wine.type === searchParameters.type && wine.body === searchParameters.body && parseInt(wine.price.split('$')[1],10) <= searchParameters.price;
  });

  // Close the search sheet:
  $.UIHideSheet();

  // Handle no match with chosen parameters:
  //========================================
  if (filteredWines.length === 0) {
    $.UIGoToArticle('#noMatch');

  // Otherwise, show results,
  // and update templates:
  //=========================
  } else {
    // Go to wine list and show results:
    $.UIGoToArticle('#wineList');

    // Clear any existing results from list:
    filteredWinesList.empty();

    // Render search results:
    filteredWines.forEach(function(wine) {
      filteredWinesList.append(parsedSearchResults(wine))
    });
  }
});
```

##Search Results

And here’s the template that the above code uses:

```
<script type='text/x-template' id="searchResultsTemplate" >
  <li class="comp" data-id='[[= wine.id ]]'>
    <aside></aside>
    <div>
      <h3>[[= wine.name ]]</h3>
      <h4>[[= wine.varietal ]]</h4>
    </div>
    <aside>
      <span class="nav"></span>
    </aside>
  </li>
</script>
```

The list created from these search results is a typcial iOS navigation list. Tapping and item will take the user to a detail view, the same detail view we created earlier. Now that we are back at the detail page again, we can look at navigating

##Back to Detail Screen

To go to the detail page and show the selected wine, we have this handler. It gets the id of the selected wine, filters the wine data down to just that one, goes to the detail page and publishes a broadcast with the selected wine. Since it publishes the same channel that we defined previously to show the detail for a wine selected from the carousel, we do not need to do any more to render the chosen output:

```
// Register handler to capture which wine
// the user tapped and then show its detail:
//==========================================
$('#filteredWines').on('singletap', 'li', function() {

  // Get id of tapped wine:
  var id = $(this).attr('data-id');

  // Method to filter wines by chosen wine id:
  var selection = wines.filter(function(wine) {
    return wine.id === id;
  });

  // Update detail template:
  $.publish('top-pick-selected-wine', selection[0]);
  $.UIGoToArticle('#selectedWine');
});
```

##Payment Workflow

Detail view for a wine:

![](images/vino/Detail View.png)

In the above image, notice the price tag. We want to enable the purchase of the wine by tapping it. Before anything happens, we want the tap to pop up a dialog that notifies the user that he or she is about to buy and item and show its name and amount:

![](images/vino/Purchase Dialog.png)

To make this happen, we need to define an event handler on the price label that will launch the dialog:

```
// Enable showing purchase sheet for 
// purchase completion. This will
// open a popup dialog, offering the
// user the chance to cancel to
// purchase the chosen wine.
//=================================== 
$('#selectedWine').on('singletap', '.counter', function(e) {
  var wine = $(this).parent().prev().find('h3').text();
  var price = $(this).text();
  $.UIPopup({
    id: "purchasePopup",
    message: 'Do you want to purchase ' + wine + ' for ' + price, 
    cancelButton: 'Cancel',
    continueButton: 'Purchase',
    callback: function() {
      setTimeout(function() {
        $.UIShowSheet('#purchaseSheet');
        progressInterval = setInterval(processProgress, pval);
      });
    }
  });
});
```

The purchase process has two stages. The first shows a progress bar indicating how much time is left. When this finished, the user is presented with a message indicating that the purchase was complete. Tapping “OK” will dispel the purchase sheet. Below are images of the process:

![](images/vino/processing-purchase.png)

![](images/vino/purchase-complete.png)

Here’s the template for the purchase sheet that will enable the two-step purchase flow. We’re using an HTML5 progress element, so we’ll have to write some code to animate it:

```
<script type='text/x-template' id="purchaseSheetTemplate">
  <div id='progressPanel'>
    <p>
      <progress max='500' value='0'></progress>
      <h2>Processing...</h2>
    </div>
    <div id='confirmationPanel'>
      <h2>Purchase Complete</h2>
      <p>Thanks for buying. Oh wait, this is just a stub. You'll need to use whatever e-commerce solution suits your purpose.
    </p>
    <button class='action'>OK</button>
  </div>
</script>
```

We need to initialize the purchase sheet and then populate it with out template. We also need to set up some variables we will need.

```
// Initialize purchase sheet:
//===========================
$.UISheet({id:'purchaseSheet', handle: false});

// Populate purchase sheet
$('#purchaseSheet').find('section').html(purchaseSheetTemplate);

// Make purchase of wine:
//=======================
var progress = $('progress');
var pval = 0;
var progressInterval;
```

And here’s the method to animate the progress element. We hide the confirmation panel and show the progress panel, animate the progress element. When that is doen, we hide it and show the confirmation panel. There’s a different in height between these two stages. The progress panel is shorter than the confirmation panel, so we have to adjust the height of the purchase sheet depending on which panel is showing.


```
// Define method for purchase process.
// This will animate the progress bar:
//====================================
var processProgress = function() {
  if (pval === 500) {
    // Make sure we are starting clean:
    clearInterval(progressInterval);

    // Show the progress panel:
    $('#progressPanel').toggle();

    // Hide the confirmation panel:
    $('#confirmationPanel').toggle();
    $('#purchaseSheet').css('height', 200);

    // Set inital value for progress animation:
    pval = 0;
  } else {
    // Increate the value for animation:
    pval++;

    // Update the progress bar with new value:
    progress.val(pval);
  }
};
```

Once the progress animation has finished, the user is left with the confirmation panel. We have to wireup an event handler to dispel the purchase sheet. But this handler should also do some clean up, setting the progress and confirmation panels back to there original states for the next purchase:

```
// Define handler to display purchase sheet:
//==========================================
$('#confirmationPanel button').on('singletap', function() {
  $.UIHideSheet();


  $('#confirmationPanel').hide();

  $('#purchaseSheet').css('height', 80);

  // Delay showing the progress bar so
  // it doesn't show while hiding the sheet:
  setTimeout(function() {
    $('#progressPanel').show();
  }, 200);
}); 
```

This is the Vino app’s main workflow. You can download this from our [Github repository](https://github.com/chocolatechipui/Vino). It uses client-side browser sniffing to serve up a theme for Android, iOS and Windows Phone.


