#ChocolateChip-UI

##Templates

It would be impossible to make efficient Web apps without some kind of templates. ChocolateChip-UI provides its own template module. Templates are essential for efficiently creating markup in your app with dynamic content. There are many template engines out there. They are not all equal. In fact, some of the more famous and popular ones are down right slow. ChocolateChip-UI is not the fastest, but it's faster than most. And it's small and part of the framework. If you prefer another template solution, feel free to use it.

By their very nature, single page apps will not have complicated template needs, so ChocolateChip-UI's template module should fill the bill. There are several ways you can create a template. You can define it as a string assigned to a variable. You can assign it to the $.templates object. You can define it inside a script tag. You could even store your templates as an external file that you import with a script tag or though an Ajax request. Do what makes sense for you.

Creating and rendering a template is a three stage process. First you write the template. Then you must parse it. This turns the string template into a function that can output markup matched up with data passed to it. And finally you execute that parsed template function with data and output the result to the page:

```
// This assumes that masterData is an array
// of people's first and last names.

// Define the template:
var masterTemplate = 
'<li>[[= data.firstName ]] [[= data.lastName ]]</li>';
// Parse the template:
var myTmpl8 = $.template(masterTemplate);
// Loop through the data and output the template:
masterData.forEach(function(ctx) {
  // Pass the current chunk of data
  // and output the result to the document:
  $('.master ul').append(myTmpl8(ctx));
});
```

ChocolateChip uses pairs of square brackets as delimiters: [[ ... ]]. These allow you to include executable JavaScript in your templates. The following template would do nothing but launch an alert when the variable name is present:


```
var template1 = '[[ if (data.name) { ]]\
[[ alert('You have a name!) ]]\
[[ } ]]';
```

To output a variable into your template's markup you use an equals sign after the opening square brackets pair: [[= ... ]].

###Creating Templates

The most straightforward way to create a template is to define it as a string. If the template will be more than one line, you will need to put a back space on each line, except for the last. Because of the way the parsing is done, to refer to a data variable in the template, you need to use a name as the base. The default is `data`. You can use any other name that makes sense for the data you are using and pass that name to the parse function as the last parameter.


```
// Expected data: { "firstName" : "Joe", "lastName" : "Bodoni"}
// Define a template and assign it to the variable 'myTemplate'.
var myTemplate = '<li><h3>[[= data.lastName ]], [[= data.firstName ]]</h3></li>';
```


You could also just assign it to ChocolateChip's $.template object:

```
$.template.myTemplate = '<li><h3>[[= data.lastName ]], [[=data.firstName ]]</h3></li>';
```

To parse the template we just pass it to the $.template function:

```
var parsedTemplate = $.template($.templates.myTemplate);
```


You could also use the $.template object as a cache for your parsed templates. You would do this during load time. The advantage of stored, pre-parsed templates is that they are globally available and ready to use at any time:

```
// Define the temlpate:
var myTemplate = '<li><h3>[[= data.lastName ]], [[= data.firstName ]]</h3></li>';
// Parse it and cache it in $.templates:
$.templates.parseTempate = $.template(myTemplate);

```

###Custom Data Names

When parsing a template, you can also define a custom name for the data in your template. You would also need to use that custom name in your template or it will generate and error:


```
// Use 'user' instead of 'data':
var myTemplate = '<li><h3>[[= user.lastName ]], [[= user.firstName ]]</h3></li>';
// Provide 'user' as custom name when parsing:
var parsedTemplate = $.template(myTemplate, 'user');
```


Presuming we had a JSON object for students like this, we can easily put together a template to output it:

```
var studentObj = {
   "students": [
      { 
         "firstName":"Joe",
         "lastName": "Bodoni",
         "major":"Physics" 
       }, 
       { 
         "firstName":"Suzy",
         "lastName": "Que",
         "major":"Chemistry" }, 
       { 
         "firstName":"John",
         "lastName": "Doe",
         "major":"Mathematics" 
       }
   ],
   "university" : "Palo Alto",
   "year": "1950"
}

// 'student' will represent each iteration of the "studentObj.students" array above:
var studentsTpl = '<li><h3>[[= student.lastName]], [[= student.firstName ]]</h3><h4>[[= student.major ]]</h4></li>';
var parsedStudentTmp = $.template(studentsTpl, 'student');

var list = $('#studentList');
// Iterated over the Students array:
studentObj.students.forEach(function(ctx) {
   // Append template with data:
   list.append(parsedStudentTmp(ctx));
});
```

###Using a Script Tag as a Template

As mentioned earlier, you can store your template in a script tag. To get the template, you use a selector to get the tag and then grab its text content as the template. Remember to give the tag a custom type. This will prevent the browser from showing the contents of the script tag in the document. Browsers ignore the contents of script tags with an unknown type. We recommend using 'text/x-template', but you can use whatever you want, as long as it is not a known type. In the example below we gave the script an id so we can easily get it.

Markup:
```
<script id='studentTemplate' type='text/x-template'>
   <li><h3>[[= student.lastName]], [[= student.firstName ]]</h3><h4>[[= student.major ]]</h4></li>
</script>  
```

Script:
```
var studentTemplate = $('#studentTemplate').text();
var parsedStudents = $.template(studentTemplate, 'student');
var list = $('#studentList');
// Iterated over the Students array:
studentObj.students.forEach(function(ctx) {
    // Append template with data:
    list.append(parsedStudentTmp(ctx));
});
``` 

###JavaScript in Templates

ChocolateChip-UI allows you to execute JavaScript code inside your template. You could use this for some type of boolean check:


```
// Supposing the following data structure: songs = [ { title: 'whatever', artist: 'whoever', album: 'something', description: 'blah blah', genre: 'pop' }, etc. ];

var songs = "<li>\
[[ if (song.genre === 'pop') { ]]\
   <li>\
      <h3>[[= song.title ]]</h3>\
      <h4>[[= song.artist ]]</h4>\
      <p>[[= song.detail ]]</h4>\
   </li>\
[[ } else { ]]\
   <li>\
      <h3>No Match</h3>\
   </li>\
[[ } ]]";
```

Or, as a script template:

```
<script id='songTemplate' type='text/x-template'>
   <li>
   [[ if (song.genre === 'pop') { ]]
      <li>
         <h3>[[= song.title ]]</h3>
         <h4>[[= song.artist ]]</h4>
         <p>[[= song.detail ]]</h4>
      </li>
   [[ } else { ]]
      <li>
         <h3>No Match</h3>
      </li>
   [[ } ]]
</script>
```

###External

Generally, it is best to have as few static resources as possible load with your app. Each http request causes a delay in how fast the app can load. If for whatever reason you need to separate your templates from your app, you can load them dynamically. There are a couple of ways you can do this. You can put a template, just like for a script template but minus the script tags in an external file and then load it through an Ajax request, assigning the contents of the Ajax success to a variable, which you could then render with `$.template()`.

For example, here we have a file called "studentTemplate.txt" with this as content:

```
<li>
  <h3>[[= student.lastName]], [[= student.firstName ]]</h3>
  <h4>[[= student.major ]]</h4>
</li>
```

We can load this template with an Ajax request:

```
var studentTemplate = $.get("/templates/studentTemplate.txt");
var parsedStudents = $.template(studentTemplate, 'student');
var list = $('#studentList');

// Iterated over the Students array:
studentObj.students.forEach(function(ctx) {
  // Append template with data:
  list.append(parsedStudentTmp(ctx));
});
```

###Dynamic Data

Using templates allows you to create apps that load their content dynamically based on user interaction. This means your total app weight is less, freeing up precious mobile memory and making your app feel faster. In the following sections we'll look at how to use templates to make various types of dynamic navigation work efficiently.


####Dynamic Navigation List

Creating dynamic navigation lists using templates has already been discussed in the chapter "Navigation". Look for the section "Dynamic Navigation Lists".

####Dynamic Slide-out

A slide-out by its nature is dynamic. We create one like this:

```
$(function() {
  $.UISlideout();
  $.UISlideout.populate([{music:'Music'},{pictures:'Pictures'},{recipes:'Recipes'},{contacts:'Contacts'}]);
});
```

However, you might want to make the content that the slide-out displays be dynamic as well. Instead of having a separate article for each item that the slide-out will display, we could have just one and load its content based on the user's choice.

We can pass an object of options to the `$.UISlideout` function to allow us to have a callback with which we can load data dynamically. For this to work we need to set the property `dynamic: true` and define a callback:


```
// var myData = { a bunch of data in here };
// Define a template for the data you want to outputP
// var myTemplate = blah blah blah;
// Preprocess the template:
// var temp = $.template(myTemplate);
$.UISlideout({
  dynamic: true,
  callback: function (item) {
    var whichItem = myData.filter(item);
    $('#detail').find(section).html(temp(whichItem));
  }
});
```

In the above code, we assume that "myData" is a real data object of things that correspond to choices the user makes when selecting an item in the slide-out. We define a template for that data and preprocess it. Then, inside the callback, we first use the item passed into the callback to filter out everything but that choice from the myData object. Then we pass that result to the template and insert it into the detail article. Because we're using the html method, it will replace whatever is already in that article.

Using this simple method we can make a slide-out that dynamically loads data into the detail article based on the user's choice. 


####Dynamic Split-layout

In the chapter "Navigation", we took a look at the structure of the split-layout. This is a layout specifically for tablets. It has a master list on the side that allows you to change the content of the detail section. The basic structure for a split-layout is as follows:


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

As you can see, in each article we have a list that could hold content. At the moment they don't. We need to create some templates so that we can populate them both. First lets take a look at a sample of the data we could use for this:


```
var masterData = [
  { title: "California", "data_url": 'california' },
  { title: "Oregon", "data_url": 'oregon' },
  { title: "Washington", "data_url": 'washington' },
  { title: "Colorado", "data_url": 'colorado' },
  { title: "Nevada", "data_url": 'nevada' },
  { title: "Arizona", "data_url": 'arizona' },
  { title: "New Mexico", "data_url": 'newmexico' }
];
```
What we're going to do is use this list to populate the master list with actionable items so that we can load the "data_url" value in the detail list.

We need to define a template for the master list items before we can load them. This will happen when the app loads. We loop over the data array defined above and output the template based on each item:


```
// Define the template for the master list items:
var masterTemplate = 
'<li data-url="[[=data.data_url]]">[[=data.title]]</li>';

// Process the template:
var myTmpl8 = $.template(masterTemplate);

// Append the items to the master list:
masterData.forEach(function(ctx) {
  $('.master ul').append(myTmpl8(ctx));
});
```

We then need to define a delegated event listener so that when the user taps one of these, something userful happens. In the code below we setup our event listener. 


```
$('.master ul').on('singletap', 'li', function() {
  // Handle select of master list items:
  $(this).siblings().removeClass('selected');
  $(this).addClass('selected');

  // Set the value of the detail header title:
  $('#detailTitle').text(this.textContent);

  // Delete whatever is already in the master list:
  var detail = $('.detail section');
  detail.empty();

  // Show a busy indicator:
  detail.UIBusy({'color':'#000', 'size': '100px'});

  // Get the data-url of the item that was tapped.
  // Will use this to make an Ajax request for corresponding data:
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

###Other Templating Engines

ChocolateChip-UI provides its own templating engine. If you don't like it and prefer something else, please use whatever you want. Just include the templating script in your document and create your templates the way you are accustomed. You could use Mustache, Handlebars, Underscore, doT, Pure, etc. It's your choice.