#ChocolateChip-UI

##Introduction to ChocolateChipJS

ChocolateChipJS is an optional library for use with ChocolateChip-UI. It is similar to jQuery, but it is not a drop in replacement. There are many differences between the two. ChocolateChipJS was built for mobile devices, not for desktop use. It's therefore small and fast, up to 30% faster than jQuery at doing typical DOM manipulations which you would do in an app. It is up to four times faster than Zeptojs at DOM manipulation. You will notice the speed difference when you switch from one library to the other. Interactions with ChocolateChipJS make your app feel as responsive as native ones. jQuery, on the other hand, may appear somewhat sluggish. Because most developers only know and care about jQuery, we've made it the default for ChocolateChip-UI. However, if performance is important to you and your are experiencing some sluggish behavior with jQuery, consider trying ChocolateChipJS. It's not that different.

Let me repeat: ChocolateChipJS is not a drop in replacement for jQuery. If you have plugins or libraries that depend on jQuery, stick with jQuery. If you do not have any such jQuery dependencies and performance is a big deal for you, then do take a look at ChocolateChipJS.

The first thing to be aware of with ChocolateChipJS is that it does not switch parameters around as jQuery does when looping over collections. ChocolateChipJS maintains the normal JavaScript order: context first, followed by the index.


```
['one','two','three'].each(function(ctx, idx){
  // Access the context: 
    console.log(ctx)
  // Access the index:
    console.log(idx);
});

$('li').each(function(ctx, idx) {
  console.log(ctx.nodeName);
  $(ctx).css({'background-color':'red'});
  $(ctx).prepend(idx + ": ");
});

```

The reason for this is because all node collections that ChocolateChipJS returns are arrays. As such, the `each` method is just an alias for the JavaScript `Array.prototype.forEach` method. Hence the parameter order is the same as the native JavaScript foreach method.


Another big difference from jQuery is that ChocolateChipJS always returns the node that matches a property check, whereas jQuery tends to return booleans. With ChocolateChipJS you could write the following code where we test for a situation and then immediately act on it:



```
// Returns the tag if it is an "a" tag:
$('button').is('a').css({'background-color': 'red'});

// Returns the tag if it is not a list element:
$('button').isnt('li').css({'background-color': 'red'}); 

// Returns the ul tag it if has list items:
$('ul').has('li').css({'background-color': 'red'});

// Returns the ul if it does not have div children:
$('ul').hasnt('div').css({'background-color': 'red'});

// Returns the element if it has this class:
$('div').hasClass('silly').css({'background-color': 'red'});

// Returns the element if it has this attribute:
$('button').hasAttr('disabled').css({'background-color': 'red'});

```


As you can see, by returning the element in these checks, ChocolateChipJS gives you immediate access to the element so you can do something with it. To accompish the same tasks with jQuery would require something like this:


jQuery:


```
// Change background color if Div has class "attention":
$('div').each(function(idx, ctx) {
  if ($(ctx).hasClass('attention')) {
    $(ctx).css({'background-color': 'red'});
  }
});

```

ChocolateChipJS:

```
// Change background color if Div has class "attention":
$('div').hasClass('attention').css({'background-color': 'red'});

```

If you need to get the equivalent of jQuery's boolean return, you can just check for a single element using the index value of 0:


```
// Test if an element exists:
if ($('#main')[0]) {
  console.log('#main does exist!');
}

// Test if an element has descendants:
if ($('ul').has('li')[0]) {
  console.log('The list is not empty!');
}

// Test if element has a particular class:
if (('li').hasClass('comp')[0]) {
  console.log('This list item has the class "comp".');
} else {
  console.log('Empty!')
}
```

You can also check the length property on what they return:


```
// Test if element has a particular class:
if (('li').hasClass('comp').length) {
  console.log('This list item has the class "comp".');
} else {
  console.log('Empty!')
}
```


If you have experience with jQuery, you will find ChocolateChipJS easy to learn. The following documentation outlines how ChocolateChipJS works. Read it carefully and you will be productive in a short time.

##Extending Objects

ChocolateChipJS uses the `$.extend` method to extend objects and functions. You can use it to override or extend an existing object. This method allows you to extend an object. It uses ECMAScript 5's Object.keys and Object.define to extend objects. By default it sets the enumerable flag to false. This means that looping over an extended object will not expose those extensions. This is very different from using the prototype chain or the `proto` property to extend an object, which does expose extensions in loops. This method expects two arguments:

1. The object to extend
2. The object to extend with

```
var myObj = {
  name: "Wobba"
}
```

To extend the above object we can do this:

```
$.extend(myObj, {
  age: 100,
  job: 'rocket scientist'
  employed: true
});

/* 
This will make myObj equal:
{
  name: "Wobba",
  age: 100,
  job: 'rocket scientist'
  employed: true
}
*/
```

If you only pass in the object to extend with, ChocolateChipJS will assume you want to extned its $:

```
$({
  myFunc : function(msg) {
    console.log('This is the message: ' + msg);
  }
});

//Now you can do:
$.myFunc('This is the message.');
```

ChocolateChipJS also offer `$.fn.extend`. This allows you to extend the collection object so that you can add new methods to loopable arrays, particularly node elements. This version takes one argument, the property or properties to add to the `$.fn` object. Because this extend the collection object, you should always check the length of the object and return and empty array if it has no length or there are no arguments. Because this is a colleciton, we need to loop over the items to act on them. After doing whatever it is we want to do with the items in the collection, we return the collection using the keyword `this`.

```
$.fn.extend({
  changeTextColor: function(color) {
    if (!this.length || !arg) return [];
    this.each(function(node) {
      node.style.color = color;
    }
    return this;
  }
});
```

We can now do:

```
$(h1).changeTextColor('red');
```


##Traversing the DOM

ChocolateChipJS has a number of functions to traverse the DOM to access nodes.

- $()
- $().find
- $().each
- $().prev
- $().next
- $().first
- $().last
- $().children
- $().parent
- $().ancestor
- $().closest
- $().siblings
- $().unique
- $().eq
- $().index
- $().is
- $().isnt
- $().has
- $().hasnt
- $().hasClass
- $().hasAttr

###$()

The dollar sign is primarily used to get a node or set of nodes in your document. You pass it a selector, it returns you what it found. If it found nothing, it will return an empty array. You can always test if a node was returned by testing the result's length:

```
var elem = $('.someElement');
if (elem.length) {
  console.log('Found the element!');
} else {
  console.log("Didn't find anything.")
}
```
Selectors can be any valid CSS selectors:

```
$('ul') // tag
$('#list') // id
$('.list') // class
$('[checked]') // attribute
$('li:first-child') // pseudo-class
$('ul + p') // adjacent selector
```

###find

The `find` method allows you to search for an element using another as the starting point. This can save you time in finding an element since the route will be shorter. 

$('ul').find('a') // Find A tags inside of UL's LIs

The `find` method works like the dollar sign, accepting the same selector arguments.

###each 

The `each` method allows you to iterate over the collection returned by the dollar sign or find methods. Be aware that this uses the same parameter order as the JavaScript Array forEach method: context followed by index. This is the opposite order adopted by jQuery.

```
$('li').each(function(ctx, idx) {
  $(ctx).css('color', 'red');
  console.log('Item number: ' + idx);
});
```

In jQuery the parameter order would be reversed:


```
$('li').each(function(idx, ctx) {
  $(ctx).css('color', 'red');
  console.log('Item number: ' + idx);
});
```

We not sure why John Resig chose to reverse the default order. ChocolateChipJS follows the normal order that JavaScript uses, so if you're used to jQuery's order, watch out for that.

The last parameter is optional, so if you just want to access a node and do something, you can code like this:


```
$('li').each(function(ctx) {
  $(ctx).css('color', 'red');
});
```

###prev

The method `prev` will return the previous sibling node. 

Markup:
```
<p>Some text</p>
<p id="message">More text here</p>
```

Script:
```
// Make the tag before color red:
$('#message').prev().css('color','red');
```

###next

The method `next` with return the next sibling node.

Markup:
```
<p id="message">More text here</p>
<p>Some text</p>
```

Script:
```
// Make the tag after color red:
$('#message').next().css('color','red');
```

###first

This method returns the first child element of a node.

Markup:
```
<ul>
  <li>One</li>
  <li>Two</li>
  <li>Three</li>
</ul>
```
Script:
```
// Make the first list item red:
$('ul').first().css('color','red')
```

###last

This method returns the last child element of a node.

Markup:
```
<ul>
  <li>One</li>
  <li>Two</li>
  <li>Three</li>
</ul>
```
Script:
```
// Make the last list item red:
$('ul').last().css('color','red')
```

###children

This method returns the child nodes of an element. If no selector is provided, it returns all child nodes, otherwise it returns the child nodes that match the selector.

```
// Make all list items blue:
$('ul').children().css('color', 'blue');

// Make selected list items red:
$('ul').children('.selected').css('color', 'red');
```

###parent

This method returns the parent of an element.

Markup:
```
<ul>
  <li>One</li>
  <li>Two</li>
  <li>Three</li>
</ul>
```

Script:
```
// Put red border on list items parent:
$('li').parent().css('border','solid 1px red');
```

###ancestor

This method returns the matching anchestor of an element. This will be the first ancestor matching the selector.

```
// Get the id of the ancestor article:
$('ul').ancestor('article')[0].id
```

###closest

This method is an alias for `ancestor` for people used to the jQuery method of the same name.

###siblings

This method returns the siblings of an element. If no arguments are passed, it will return all siblings. If a selector is supplied, it will return siblings that match the selector.

```
// Turn unselected list items blue:
$('li.selected').siblings().css('color','blue');


// Set all divs that are siblings of a paragraph
// to have red borders:
$('p').siblings('div').css('border', 'solid 1px red');
```

###unique

This method returns an array or collection of nodes without any duplicates. 

```
// Returns [1,2,3,4,5]
[1,2,2,2,3,3,4,5,5,5,5].unique();
```

###eq

This method is used on a collection to pluck out the node at the position indicated by the argument. It takes a zero-based integer. So 0 would get the first node, 1, the second, etc. By passing a negative number, you can pluck a node starting from the end of the collection. So -1 would be the last, -2, the next to last, etc.

```
$('li').eq(0); // get the first list item
$('li').eq(3); // get the fourth list item
$('li').eq('-1'); // get the last list item
$('li').eq('-2'); // get the next to last list itme
```

###index

This method will return the index of the element. This is zero-based, so and index value of 0 would be indicate the elements was the first, 1 would be the second, etc.

Markup:
```
<ul>
  <li>One</li>
  <li>Two</li>
  <li class='selected'>Three</li>
</ul>
```
```
$('.selected').index(); // returns 2
```

###is

This method will return the element that matches the condition provided as an argument. You can use any valide CSS selector, or a DOM node. Please note that this is not the same behavior as the jQuery function of the same name. jQuery only returns a boolean for each element, not the element.

```
// Only list items with a class of 'selected' will be red:
$('li').is('.selected').css('color','red');

// This is equivalent to the following in jQuery:
$('li').each(function(idx, ctx) {
  if ( $(ctx).is('.selected')) {
    $(ctx).css('color','red');
  }
});
```

###isnt

This method will return the element that does not match the condition provided as an argument. You can use any valide CSS selector, or a DOM node.

```
// Only list items that do not have a class of selected will be green:
$('li').isnt('.selected').css('color','green');
```
###has

This method returns all elements whose descendent matches the selector. You can use any valide CSS selector, or a DOM node.

```
 // Add class 'nav' to li items with aside:
$('li').has('aside').addClass('nav');
```
###hasnt

This method returns all elements that do not match the selector. You can use any valide CSS selector, or a DOM node.

```
 // Add class empty to all list items without an h3:
$('li).hasnt('h3').addClass('empty');
```
###hasClass

This method will return all elements that have the provided class.

```
 // Append to all list items with the class 'show-detail':
$('li').hasClass('show-detail').append('Gonna show something');
```

If you want to do a boolean test for the presence of a class, you can check to see if an element was returned. You can do that by reduce the array to a single element using the [0] index:

```
$('li').each(function(ctx, idx) {
   if ($(ctx).hasClass('show-detail')[0]) {
     // do something if it has the class
   } else {
     // Do something else.
   }
});
```

Or you could just slip down into normal ECMAScript 5 mode:

```
$('li').each(function(ctx, idx) {
   if (ctx.classList.contains('show-detail')) {
     // do something if it has the class
   } else {
     // Do something else.
   }
});
```


###hasAttr

This method will return the elements if they have the provided attribute.

```
// Append 'classified' to all list items with a class attribute:
$('li').hasAttr('class').append('classified');
```

##Manipulating the DOM

ChocolateChipJS has a number of methods that allow you to manipulate the DOM in various ways.

- $.make
- $()
- $.html
- $().html
- [].css
- [].attr
- [].removeAttr
- $().hasAttr
- [].prop
- [].addClass
- [].removeClass
- [].toggleClass
- $().hasClass
- [].dataset
- [].data
- [].removeData
- [].val
- [].enable
- [].disable
- [].show
- [].hide
- [].before
- [].after
- [].prepend
- [].append
- [].prependTo
- [].appendTo
- [].insert
- [].clone
- [].wrap
- [].unwrap
- [].remove
- [].empty
- $.replace

###$.make

This method will attempt to construct DOM nodes from the markup passed to it. The markup must be valid or the method will throw an exception. The nodes are returned in an array, so you can use any of ChocolateChipJS's collection methods.

```
// Create and empty list:
   var list = $.make('<ul class="list"></ul>');
   list.innerHTML =
   $.make('<li><h3>One</h3></li><li><h3>Two</h3></li><li><h3>Three</h3></li>');
```

###$()

This method, although generally used for finding a DOM node with a selector, can also be used to create DOM nodes. Pass it a string of valid markup and it will return nodes, just like `$.make`.

```
var button = $('<button>Click</button>');
```
###$.html

This method is an alias for `$.make`. Pass it a string of valide markup and it will return an array of nodes.

```
var button = $.html('<button>Click</button>');
```


###$().html

Unlike `$.html`, which creates nodes, this method will replace the contents of the target element(s) with the content provided. If no argument is provided, it will return the contents of the element, including nodes. If a string is provided as an argument, that string will replace the contents of the element. If the string is markup, it will be converted to nodes before inserting. If the string is empty (''), then the contents of the element will be replaced with an empty text node.

```
// Get all the items in the menu:
var contents = $('#menu').html();

// Empty the first item in the list:
$('li').eq(0).html('');

// Replace the content of all list items with text:
$('li').html('New Text Here!');

// Replace the content of all list items with new markup:
$('li').html('<h2>New List Title</h2>');
```

###css

This method allows you to set or retrieve the style of an element. It can accept to arguments: property and value. If no value is passed, the method attempts to retrieve the the CSS property of the first element matching the selector. If a property and value are provided, it will loop through all matching selectors applying that style. When passing property/value pairs, if it is a single pair, you can just pass them in as quoted strings separated by a comma. For more arguments, pass in a object literal defining the styles to apply. If you want the width or height of an element, use the $(selector).width() or $(selector).height() methods, as these return the values as straight integers with length identified. Otherwise, using $(selector).css('width'), etc., you'll need to pass the returned value through the parseInt method. See the examples below:

```
// Get the height of the first list item.
// returns the height with lent identifier: 100px
var height = $('li').css('height');

// Get the height of the third list item:
var height = $('li').eq(2).css('height');

// Get the body background color:
var backgroundColor = $.body.css('background-color');

// Set the background color on all list items:
$('li').css('background-color': 'gold');

// Set multiple properties on all list items:
$('li').css({'background-color': 'orange', 'color': 'red', 'font-weight': 'bold'});
```

When getting the CSS property of an element, it is not possible to chain any methods, as the value returned is the CSS, not the element. When apply styles, the elements are always returned, allowing you to chain other collection methods.

```
// Will throw and exception:
 $('li').css('height').append('Modified!');

 // This is chainable:
 $('li').css('height', '40px').append('Modified!');
```
###attr

This method gets or sets the attribute of elements. If only an attribute is provided, it will return the attribute value of the first element in the collection. If an attribute and value are provided, they will be set on all elements in the array.

```
// Get the checked state of the input in the first list item.
// (true or false): 
var checked = $('li').find('input').attr('checked'); 

// Set the checked state: 
var checked = $('li').eq(0).find('input').attr('checked', 'checked');

// Set the aria role of all buttons: 
$('button').attr('role', 'button');
```

###removeAttr

This method will remove the provided attribute from the elements while retuning the elements.

```
// Remove the attribute 'class' and color the list item red:
$('li').removeAttr('class').css('color','red');
```


###hasAttr

This method will return all elements that have the provided class.

```
 // Append to all list items with the class 'show-detail':
$('li').hasClass('show-detail').append('Gonna show something');
```

If you want to do a boolean test for the presence of a class, you can check to see if an element was returned. You can do that by reduce the array to a single element using the [0] index:

```
$('li').each(function(ctx, idx) {
   if ($(ctx).hasClass('show-detail')[0]) {
     // do something if it has the class
   } else {
     // Do something else.
   }
});
```

###prop

This is the same as the attr method described above.


###addClass

This method will add the provided class to every element in the array.

```
// Add the class 'nav' to every list item:
$('li').addClass('nav');
```


###removeClass

This method will remove the class from every element in the array.

```
// Remove the class 'nav' from every list item:
$('li').removeClass('nav');
```


###toggleClass

This method will toggle the class on all elements in the array. If the provided class is already present on an element, it will be removed. And if it is not on an element, it will be added.

```
// Toggle the class 'purchased' on all list items. 
// All items that already have that class will have it removed, 
// while all items that do not have it, will have it added: 

$('li').toggleClass('purchased');
```

###hasClass


This method will return all elements that have the provided class.

```
 // Append to all list items with the class 'show-detail':
$('li').hasClass('show-detail').append('Gonna show something');
```

If you want to do a boolean test for the presence of a class, you can check to see if an element was returned. You can do that by reduce the array to a single element using the [0] index:

```
$('li').each(function(ctx, idx) {
   if ($(ctx).hasClass('show-detail')[0]) {
     // do something if it has the class
   } else {
     // Do something else.
   }
});
```

###dataset

This method is used to get or set HTML5 data attributes on elements. You provide two arguments for setting a value: the attribute name and the value for it. The attribute provided will automatically have 'data-' prepended to it, so you just provide the name for the attribute and the value it should hold. The value must be a string. You could store not string data by converting it to a string. However, ChocolateChipJS provides a more efficient way to do this using the data method, which allows you to cache data objects in relation to an element.

To retrieve a dataset, just provide the name of the attribute as an argument.

```
// Set a data attribute.
// This will attrach the attribute like so: data-price='$2.00'.
$('li').eq(0).dataset('price', '$2.00');

// Get the data attribute:
var price = $('li').eq(0).dataset('price');

// Set the data attribute on all list items:
$('li').dataset('availability', 'in-stock');
```

When getting the data attribute on an array of elements, this method will return the value of the first element only:

```
var value = $('li').dataset('availability');
// This is the same as:
var value = $('li').eq(0).dataset('availability');
```


###data

This method will get and set data in relation to an element. Unlike the HTML5 data attribue, nothing is stored on the element. Instead the data is stored in a cache and tied to the element through its id. If the element has no id at the time the data is bound, the element is given a unique id to identify it in the cache.

```
$('li').on('singletap', function() { 
  // Store the list item title in the list's data cache: 
  $(this).parent().data('selectedItem', $(this).find(h3).text());
});
```

###removeData

This method will remove all stored data associated with an element.

```
// Remove data with the key "selected":
$('#myList').removeData("selected");

// Remove all data associated with the list:
$('#myList').removeData();
```

###val

This method will get or set the value of a form element. If no value is provided, it will return the element's value. If a value is provided, it will set that on the form element. When getting the value of an array of elements, this method will only return the value of the first element.

```
// Set the value of the input:
$('input[type=email]').val('me@me.com');
// Set the value of all text input to 'Empty!':
$('input[type=text]').val('Empty!');
```

```
// Get the value of the first text input:
var value = $('input[type=text]').val();
// or:
var value = $('input[type=text]').eq(0).val();
```

###enable

This method will remove any disabled attributes added by the disable method or from other user interaction.

```
$('button').enable();
```

###disable

This will set the disabled state on all elements in the array. It does this by adding the class 'disabled' to the element, the attribute 'disabled="true"', and the style 'cursor: default' so that there is no hover state for the element.

```
$('button').disable();
```


###show

This method will perform an animated show of the target elements. You can pass it the arguments 'fast' or 'slow', or an integer representing the speed in milliseconds. You optionally include a callback to execute when the animation is complete. The keyword 'this' will refer to the element being shown. If no argument is passed, the elements are shown immediately without any animation.

```
$('li').show('slow');
$('li').show('fast');
$('li').show(5000);
// Hide the elements and add the attribute hidden='totally':
$('li').show('fast', function() {
    $(this).attr('data-visible', 'totally');
});
$('li').show();
```

###hide

This method will perform an animated hide of the target elements. You can pass it the arguments 'fast' or 'slow', or an integer representing the speed in milliseconds. You optionally include a callback to execute when the animation is complete. The keyword 'this' will refer to the element being hidden. If no argument is passed, the elements are hidden immediately without any animation.

```
$('li').hide('slow');
$('li').hide('fast');
$('li').hide(5000);
// Hide the elements and add the attribute hidden='totally':
$('li').hide('fast', function() {
    $(this).attr('hidden', 'totally');
});
$('li').hide();
```

###before

This method will insert the provided content before the element upon which this method is executed. It can accept a node, an array of elements, or a string of markup.

When providing a string of markup, it will be created and inserted before each element. In the case of a DOM element or array of elements, it will only be inserted before the last element in the collection. This is because the nodes already exist in memory, so there is only one copy of them to insert.

```
// Insert before first list item:
$('li').eq(0).before('<li>New list item!</li>');

// Insert before every list itme:
$('li').before('<li>New list item!</li>');

var newListItems = $.make('<li>1</li><li>2</li><li>3</li>');
// Insert new list items before first list item:
$('li').eq(0).before(newListItems);

// Insert new list items before last list item:
$('li").before(newListItems);
```

###after

This method will insert the provided content before the element upon which this method is executed. It can accept a node, an array of elements, or a string of markup.

When providing a string of markup, it will be created and inserted after each element. In the case of a DOM element or array of elements, it will only be inserted after the last element in the collection. This is because the nodes already exist in memory, so there is only one copy of them to insert.

```
// Insert after first list item:
$('li').eq(0).after('<li>New list item!</li>');

// Insert before every list itme:
$('li').after('<li>New list item!</li>');

var newListItems = $.make('<li>1</li><li>2</li><li>3</li>');
// Insert new list items after first list item:
$('li').eq(0).after(newListItems);

// Insert new list items after last list item:
$('li").after(newListItems);
```

###prepend

This method will prepend the provided argument to the target element. If a string of text is provided, it will be prepended as a text node. If a string of markup is provided, it will be prepended as nodes. If an array of nodes are provided, they will be prepended in order.

```
// Prepend the text "Attention:":
$('h1').prepend('Attention: ');

// Prepend a new list item:
$('ul').prepend('<li>First Item</li>');
```


###append

This method will append the provided argument to the target element. If a string of text is provided, it will be appended as a text node. If a string of markup is provided, it will be appended as nodes. If an array of nodes are provided, they will be appended in order.

```
// Append the text ", etc.":
$('h1').append(', etc.');

// Append a new list item:
$('ul').append('<li>Last Item</li>');
```


###prependTo

This method will prepend the elements it is executed on to the selector provided as an argument. In the case of elements in the document, this will remove them from their current position and prepend them to the target element. If the elements are created in memory and not yet present in the document, they will simply be prepended to the target element.

```
// Remove all selected list items and
// prepend them to the selected items element:
$('li.selected').prependTo('#selectedItems');

// Prepend the new elements to #menu:
$.make('<li>Movies</li><li>Songs</li><li>Pictures</li>').prependTo('#menu');
```



###appendTo

This method will append the elements it is executed on to the selector provided as an argument. In the case of elements in the document, this will remove them from their current position and append them to the target element. If the elements are created in memory and not yet present in the document, they will simply be appended to the target element.

```
 // Remove all selected list items and
 // append them to the selected items element:
$('li.selected').appendTo('#selectedItems');

// Append the new elements to #menu:
$.make('<li>Movies</li><li>Songs</li><li>Pictures</li>').appendTo('#menu');
// or:
$('<li>Movies</li><li>Songs</li><li>Pictures</li>').appendTo('#menu');
```


###insert

This method allows you to insert content at a particular position in an element's collection of child nodes. Although there are methods to append or prepend an element, there is no straightforward way to insert something at position 5, etc. This method provides that functionality. It takes two parameters: the content to insert, and the position. If no position is supplied, the content is appended at the end of the container. If the container has no child elements, then, regardless of the position supplied, the content will simply be inserted as the first child of the element.

Position positions are 'first' or 'last' for appending to these position, or an integer value. This is one-based, so the first position will be 1, the second: 2, the third: 3, etc.

```
// Insert as last child of list item:
$('li').eq(2).insert('<aside><span class="nav"></span></aside>', 'last');
// or:
$('li').eq(2).insert('<aside><span class="nav"></span></aside>', 'last');

// Insert as first child of list item:
$('li').eq(2).insert('<aside><span class="nav"></span></aside>', 'first');
// or:
$('li').eq(2).insert('<aside><span class="nav"></span></aside>', 1);

// Insert as second child of list item:
$('li').eq(2).insert('<aside><span class="nav"></span></aside>', 2);

// Insert as third child of list item:
$('li').eq(2).insert('<aside><span class="nav"></span></aside>', 3);
```

If you provide an array of items and attempt to insert it into a single element, only the first item in the array will be inserted. If you instead insert the array of elements against another array of elements, the elements of the array will be inserted one each in every target element, matching each element in the array to the numerically corresponding element until there are no more elements in the array or no more elements in the target collection:

```
var nodes = $('<p>1</p><p>2</p><p>3</p>');

// Only '<p>1</p>' will be inserted.
$('li').eq(1).insert(nodes, 2);

// Nodes will be insert in order into each list item.
// If there are four list items, each one will a p tag in
// the order they are from the array:
 $('li').insert(nodes, 2);

// Possible results from the above:
<li>
   <h2>Title 1</h2>
   <p>1</p>
   <h3>Subtitle 1</h3>
   <p>Detail 1</p>
</li>
<li>
   <h2>Title 2</h2>
   <p>2</p>
   <h3>Subtitle 2</h3>
   <p>Detail 2</p>
</li>
<li>
   <h2>Title 3</h2>
   <p>3</p>
   <h3>Subtitle 3</h3>
   <p>Detail 3</p>
</li>
```



###clone

This method will create a clone of the element upon which it is executed. If a falsy value is passed as na argument, the clone will only be of the element. Otherwise the method will clone the element with its descendant nodes. Falsy values are false, undefined and null. Do not wrap them in quotes or they will be resolved to true. (Yeah, JavaScript is weird that way.) When cloning an element, any events that were directly bound to it will be not be present in the clone. This is where delegated events help.

```
// Create a clone of a list item and append it to the same list:
var clonedListItem = $('#list').find('li').eq(0).clone();
$('#list').append(clonedListItem);
```

```
 // Create a shallow clone of a list item:
var shallowClone = $('li').eq(0).clone(false);
```


###wrap

This method will wrap the element in the provided markup. This method expects a string of valid markup. It will cut the element from the page, wrap the provided markup around it and insert it back into its previous location.

```
// Wrap a list in a div:
$('myList').wrap('<div class='menu'></div>');

// Wrap all list icons in an anchor button:
$('li').find('.icon').wrap('<button></button>');
```



###unwrap

This method removes the parent of the element. It does this be clone the element, deleting its parent and insert the cloned element in the parents position. This will therefore remove any events bound to the element being unwrapped. If you need events for it, use a delegated event.

```
// Remove the parent of the list:
$('#myList').unwrap();
```


###remove

This method will remove the element from the document. Before doing so, it will unbind any events associated with it or its descendants.

```
// Remove the last list item from the list:
$('li').eq(-1).remove();
```

```
// Remove all list items with the class 'selected':
$('li.selected').remove();;
```



###empty

This method will empty an element, removing its child nodes.

```
// Empty the last list item and append new content:
$('li').eq(-1).empty().append('<h3>New Stuff Here!</h3>');
```

###$.replace

This method will replace one element with another. The first argument is the element that will be replaced, the second argument is the element to be replaced. The process of replacing will pluck the node replacing from its place in the document and inserted in the position of the node being replaced.

```
var oldTag = $('#oldTag');
var newTag = $('#newTag');
$.replace(oldTag, newTag);

```



##Reading DOM Properties

ChocolateChipJS provides the following methods to get various DOM properties.

- [].offset
- [].width
- [].height
- [].text
- [].html
- [].attr
- [].dataset
- [].data



###offset

This method will return the absolute coordinates of the first element in the collection. In other words, it returns an object with the top, left, bottom, and right of the element from the top left of the screen. For the width and height, including padding and borders, use the `$(selector).width()` and `$(selector).height()` methods described above. Since it is returning an object of values, you cannot chain other collection methods to it.

```
// Returns an object with top, left, bottom and right
var offset = $('.list').offset(); 
// Get just the top:
var top = $('.list').offset().top;
```


###width

`$(selector).width()` is a convenience method for getting the width on an element. It will return the width as an integer without the length identifier the `$(selector).css('width')` would. This works with DOM nodes. If you need the window width, use `window.innerWidth` Trying to get the width of the window object will return 'undefined'. In the case of a collection of multiple nodes, this method returns the width of only the first element in the collection.

```
// Get the width of the first list item:
var width = $('li').width();

// Try to get window width:
$(window).width() // returns undefined
// Get the window's width:
var windowWidth = window.innerWidth;
```


###height

`$(selector).height()` is a convenience method for getting the height on an element. It will return the height as an integer without the length identifier the `$(selector).css('height')` would. This works with DOM nodes. If you need the window height, use `window.innerHeight` Trying to get the height of the window object will return 'undefined'. In the case of a collection of multiple nodes, this method returns the height of only the first element in the collection.

```
// Get the height of the first list item:
var height = $('li').height();

// Try to get window height:
$(window).height() // returns undefined
// Get the window's height:
var windowHeight = window.innerHeight;
```


###text

This method allows you to set or retrieve the text of an element. To set text on an element, just provide a string to text as the argument. Be aware that this will replace everything in the element with the provided string. That means if the element has child nodes, these will be replaced as well, leaving only the string. If the method is executed without any argument, it returns the text content of the element. This includes the text of all child elements as well.

When executed on a collection, it will set the provided string on all elements, or return combined text of all elements.

```
// Set text on the first list item:
$('li').eq(0).text('New Text Here!');

// Set text on all list items:
$('li').text('New Text');

// Get text of first list item:
var text = $('li').eq(0).text();

// Get text of all list items:
var text = $('li').text();
```



###html

This method will replace the contents of the target element(s) with the content provided. If no argument is provided, it will return all the contents of the element, including nodes. If a string is provided as an argument, that string will replace the contents of the element. If the string is markup, it will be converted to nodes before inserting. If the string is empty (''), then the contents of the element will be replaced with an empty text node.

```
 // Get all the items in the menu:
var contents = $('#menu').html();

// Empty the first item in the list:
$('li').eq(0).html('');

// Replace the content of all list items with text:
$('li').html('New Text Here!');

// Replace the content of all list items with new markup:
$('li').html('<h2>New List Title</h2>');
```


###attr

This method gets or sets the attribute of elements. If only an attribute is provided, it will return the attribute value of the first element in the collection. If an attribute and value are provided, they will be set on all elements in the array.

```
// Get the checked state of the input in the first list item.
// (true or false): 
var checked = $('li').find('input').attr('checked'); 

// Set the checked state: 
var checked = $('li').eq(0).find('input').attr('checked', 'checked');

// Set the aria role of all buttons: 
$('button').attr('role', 'button');
```


###dataset

This method is used to get or set HTML5 data attributes on elements. You provide two arguments for setting a value: the attribute name and the value for it. The attribute provided will automatically have 'data-' prepended to it, so you just provide the name for the attribute and the value it should hold. The value must be a string. You could store not string data by converting it to a string. However, ChocolateChipJS provides a more efficient way to do this using the data method, which allows you to cache data objects in relation to an element.

To retrieve a dataset, just provide the name of the attribute as an argument.

```
// Set a data attribute.
// This will attrach the attribute like so: data-price='$2.00'.
$('li').eq(0).dataset('price', '$2.00');

// Get the data attribute:
var price = $('li').eq(0).dataset('price');

// Set the data attribute on all list items:
$('li').dataset('availability', 'in-stock');
```

When getting the data attribute on an array of elements, this method will return the value of the first element only:

```
var value = $('li').dataset('availability');
// This is the same as:
var value = $('li').eq(0).dataset('availability');
```


###data


This method will get and set data in relation to an element. Unlike the HTML5 data attribue, nothing is stored on the element. Instead the data is stored in a cache and tied to the element through its id. If the element has no id at the time the data is bound, the element is given a unique id to identify it in the cache.

```
$('li').on('singletap', function() { 
  // Store the list item title in the list's data cache: 
  $(this).parent().data('selectedItem', $(this).find(h3).text());
});
```


##Events

ChocolateChipJS has a number of function to help you handle events efficiently.

- [].bind
- [].unbind
- [].delegate
- [].undelegate
- [].on
- [].off
- [].trigger


###bind

This method binds events to nodes along with a callback. A third optional argument can set for the capture phase. By default it is false, but you can pass a boolean true value to change that behavior. In most cases you want the capture phase to resolve to false.

ChocolateChipJS uses its caching system to keep track of which events you’ve bound to an element to facility unbinding them at a later time. Please see the documentation for unbind for more information.

```
var doSomething = function() {
   console.log("I'm doing it now.");
};
$("#doIt").bind("click", doSomething);
// or:
$(".stop").bind("touchend", function() {
  console.log("Time to put an end to this!");
  // Delete the element
  this.remove();
});

// Bind a named callback:
var buttonInteraction = function() {
  // The keyword 'this' will be the button clicked in the callback:
  $(this).addClass('selected').css('border','solid 1px red');
};
// Bind event with buttonIneraction:
$('button').bind('singletap', buttonInteraction, false);
```

To get the event of the user interaction you can use any parameter inside the callback's parenthesis. This will give you access to the event object.

```
// We're using 'event' as the parameter,
// you can use whatever you want:
$('button').bind('singletap', function(event) {
// Log the event object:
console.dir(event);
// Get the name of the target element of the event:
console.log(event.target.nodeName);
});
```

Binding events to every element, particularly in a long list, is not very efficient. In those cases you should instead use event delegation. To see how that is implemented, look at the documentation for the "delegate" and "on" methods.


###unbind

This method will unbind and event from an element. When events are bound, memory is taken up by that. Unbinding events that are no longer needed will free up memory. In order to unbind an event you need to have acces to the element, the function, and the capture phase used, otherwise you will not be able to unbind it. Fortunately, ChocolateChipJS uses a caching system when bind events. This keeps a log of all events attached to an element. When you execute the unbind method on an element, the method searches the cache for the element and removes all events in the cache that match the parameters provided. If you only passed no arguments, all event handlers would be removed from the element. If you passed the only an event, all events of that type would be deleted, even if there were multiple instances of the event with different callbacks. If you named the callback which the event uses, you can pass the event and the callback name to only remove that event. Similarly, you can further distinguish which event your are removing by also providing the capture phase.

```
// Remove all events from the button:
$('#myButton').unbind();

// Remove all 'click' events:
$('#myButton').unbind('click');

// Remove the click event with the callback "doIt":
$('#myButton').unbind('click', 'doIt');

// Remove the click event with the callback "doIt",
// but only is the capture phase is true:
$('#myButton').unbind('click', 'doIt', 'true');
```

Deleting an element from the document without first unbinding events will lead to serious memory leaks, which can result in sluggish performance or a crash. When using ChocolateChipJS's 'remove' method, it automatically unbinds all events on an element and its descendants to prevent memory leaks.




###delegate

This method allows you to register an event listener at a higher level to trap user interaction on descendant elements. For example, registering a even on a list to capture user interaction with the list items. This is more memory efficient that attaching an event to every list item. This also allows you to add and remove items while still maintaining interactivity with the elements.

A common mistake with event delegation is to register them all on the body tag. This is also not very efficient because the events have to bubble all the way to the body tag before they can be trapped. Always try to register the delegates as close to their targets as possible, this will avoid a sluggish performance of event triggering.

The delegate function expects the following arguments: selector, event, callback, capturePhase. Selector is the target of the event. If no capturePhase is provided it will default to 'false'.

```
$('.list').delegate('li', 'singletap', function() {
  // This refers to the element receiving the single tap:
  $(this).addClass('selected');
  console.log($(this).text());
}, false);

$('.list).delegate('li', 'swiperight', function() {
  // Hide the list item if the user swipes right:
  $(this).hide('fast');
});
```

###undelegate

Like unbind, this method will remove the designated delegated events. It expects the following parameters: selector, event, callback, capturePhase. If no capturePhase if provided, it will default to false. If no event is provided, all events on the element will be removed. If you named the callback, you can provide its name for greater specificity.

```
// Add a delegate to the body tag:
var markSelect = function(item) {
  $(item).addClass('selected');
};
// $.eventEnd is alias to whatever event is supported by the device:
// mousedown, touchstart, MSPointerDown, etc.
$("body").delegate($.eventStart, markSelect);

// Remove the delegate:
$("body").undelegate($.eventStart);
```


###on

This is a shortcut for both bind and delegate, as such the parameter order is different. This bind and event it is the same as for bind, but if you want to create a delegate, then you use the parameter order of the delegate event. ChocolateChipJS examines the parameters you provide to determine whether you want to bind an event or create a delegate.

```
// Bind an event:
$('button').on('singletap', function() {
  $(this).addClass('selected');
  // Output the name of the button to the #result element:
  $('#result').html($(this).text());
});
```

To create a delegate, the parameter order is slightly different from the delegate method. You put the event first, followed by the element to listen for, followed by the callback and capture phase:

```
// Create a delegated event:
$('ul').on('singletap', 'li', function() {
  $(this).siblings().removeClass('selected');
  $(this).addClass('selected');
}, false);
```


###off

This is a shortcut for both unbind and undelegate. Like unbind and undelegate, it removes events based on the parameters you provide.

```
// Remove all events from the button:
$('#myButton').off();

// Remove all 'click' events:
$('#myButton').off('click');

// Remove the click event with the callback "doIt":
$('#myButton').off('click', 'doIt');

// Remove the click event with the callback "doIt",
// but only is the capture phase is true:
$('#myButton').off('click', 'doIt', 'true');

// Remove delegate for the callback is 'purchaseDecision':
$("body").undelegate($.eventStart, purchaseDecision);

// Remove delegate from list where the callback is 'listSelection':
$('#myList').undelegate('singletap', listSelection, false);
```


###trigger

This method will trigger the provided event on its target element. If the element does not have an event of the type being sent, nothing will happen. So, if you find that triggered events are not firing on an element, check to make sure you are triggering the correct event type. ChocolateChip-UI uses many types of events for capturing user interactions, such as mouse down, touch, single tap, double tap, swipe, etc.

```
// Trigger a singletap event on the back button of the current article:
$('nav.current').find('.back').trigger('singletap');
```




##Ajax

ChocolateChipJS provides a number of methods to enable various Ajax operations.

- $.ajax
- $.get
- $.getJSON
- $.post
- $.JSONP
- $.form2JSON



###$.ajax

This method allows you to execute an Ajax operation. You pass in an object literal of the properties you wish to execute. It expects at a minimal a URL to access. It has the foolowing default settings:

```
var settings = {
  type: 'GET',
  beforeSend: $.noop,
  success: $.noop,
  error: $.noop,
  context: null,
  async: true,
  timeout: 0
};
```

You can use the $.ajax method to retrieve HTML fragments to insert in your document:

```
$.ajax({
  url : "announcement.html",
  success : function(data) {
    // Insert the fragment into the page:
    $("#content").html(data);
  },
  error: function(data) {
    $("#content").html("<h4>There was an error while trying to get the file.</h4>");
  }
});
```

You can also request an external JSON file for user in your app:

```
$.ajax({
  url : "me.json",
  success : function(data) {
    // Before using a JSON object, you need to parse it.
    // Here we parse it and assign it to a variable:
    var me = JSON.parse(data);
    // Here we access the properties of the JSON object:
    $("#content").html(me.firstName + " " + me.lastName);
  },
  error: function(data) {
    content.html("<h4>There was an error while trying to get the file.</h4>");
  }
});
```

We can also do a post to a remote server:

```
$.ajax({
  url : "/path/to/controller",
  method: 'POST',
  headers: {
    "Content-Type" : "application/x-www-form-urlencoded",
    "async" : true,
    "Access-Control-Allow-Origin" : "*",
    "Accept" : "text/plain"
  },
  data: myData,
  success : mySuccessCallback,
  error: myErrorCallback
});
```

####Deferred Objects

Ajax requests return deferred objects. This allows you to take advantage of the powerful features of deferreds and promises and eliminate callback hell when dealing with complex Ajax dependencies.

If the Ajax request was for JSON or was a GET request, the first argument exposed to the successful deferred is the XHR.responseText. Otherwise it is the XHR.status, which lets you know whether the POST, PUT or DELETE was successful or not.

```
$.ajax({ url : "/data/me.json"})
  .done(function(me) {
    $("#content").html(me.firstName + " " + me.lastName);
  })
  .fail(function(status) {
    content.html("<h4>There was an error while trying to get the file:" + status + "</h4>");
  })
  // This executes whether a success or failure:
  .always(function() {
    console.log('An Ajax request was made to update the page.')
  })
});
```



###get

This is a short cut for doing a get request. It can handle the following parameters: url, data, success, dataType. At very least you can pass it the url and a function to execute on success.

```
// Get an html fragment and insert it in the document:
$.get('/data/suits.html', function(data) {
  $('#products').html(data);
});
```

Because $.get is just and alias for $.ajax, it returns a deferred object. This means you can use deferred methods with it:

```
// Get an html fragment and insert it in the document:
$.get('/data/suits.html')
.done(function(data) {
  $('#products').html(data);
})
.fail(function(status) {
  $('#errorMessage').text('There was and error: ' + status)
});
```

###$.getJSON

This is a shortcut for getting a JSON object from a server. It expects three parameters: url, data, and success. If no data is provided, it will execute with just the url and success callback.

```
$.getJSON('/data/deserts.json', function(deserts) {
  deserts.forEach(desert) {
    $('#deserts').append('<li>' + desert.name + '</li>');
  });
});
```

Like the $.ajax method, this also returns a deferred object:

```
$.getJSON('/data/deserts.json')
.done(function(deserts) {
  deserts.forEach(desert) {
    $('#deserts').append('<li>' + desert.name + '</li>');
  });
})
.fail(function() {
  alert('There was a problem getting the JSON!');
});
```


###$.post

This method is a shortcut for doing a post. It expects these parameters: url, data, success, dataType.

```
$.post("updateUser.php", 
  { "name": "Joe", "time": "10PM" }, 
  function() {
    console.log('The POST was successful.')
  },
  "json"
);
```

Or, with a deferred object:

```
$.post("updateUser.php", 
{ 
  "name": "Joe", 
  "time": "10PM" 
})
.done(function() {
  function() {
    console.log('The POST was successful.')
  }  
})
.fail(function() {
  console.log('There was a problem posting.')
});
```


###$.JSONP

This method allows you to perform requests from remote sites that support JSONP. It does this by sending a callback along with the request, hence the name JSON with Padding. It expects two arguments: a url and a callback to execute once the data has returned. Normally, the url should end with the phrase: &callback=?. However, there are some services out there that expect a slightly different ending. Please check the documentation for any API before trying to implement a JSONP request.

Because the data lives on a remote server, you'll need to read the documentation for the service to understand its format so that you can output it to your app.

$.JSONP has the following default settings, which you can override:

```
var settings = {
  url : null,
  callback: $.noop,
  callbackType : 'callback=?',
  timeout: null
};
```

Be aware that you have no control over when the remote server will respod to your JSONP request. It could take some time. If you would prefer to have more control over what happens, you can provide a timeout value great than 0. If the remote server has not responded by the time the timeout finishes, the request will be canceled.

Like normal Ajax requests, $.JSONP also returns a deferred object. This allows you to use the usal deferred method chaining with your JSONP request. When the deferred object is resolved, it returns the following values in order: data (the data retrieved, 'resolved' (a string), settings (the settings object used to initialize the request). When the deferred object is rejected, it returns the following values: 'timedout' (a string), settings (the settings object used to initialize the request).

```
$.JSONP({
  url: 'https://api.github.com/users/yui?callback=?', 
  callback: function(lib) {
    $('.list').append('<li><h3>The name of the library</h3><h4>' + lib.data.name + '</h4></li>');
  }
});

$.JSONP({
  url: 'http://www.geonames.org/postalCodeLookupJSON?postalcode=94102&country=US&callback=?', 
  callback: function(data){
    $('.list').append('<li><h3>My Location</h3><h4>' + data.postalcodes[0].adminName2 + ', ' + data.postalcodes[0].adminName1 + '</h4></li>');
  }
});
```

With timeout:

```
$.JSONP({
  url: 'http://www.geonames.org/postalCodeLookupJSON?postalcode=94102&country=US&callback=?', 
  callback: function(data){
    $('.list').append('<li><h3>My Location</h3><h4>' + data.postalcodes[0].adminName2 + ', ' + data.postalcodes[0].adminName1 + '</h4></li>');
  },
  // 5 second timeout:
  timeout: 5000
});
```

Deferred versions:

```
.JSONP({
  url: 'https://api.github.com/users/yui?callback=?'
})
.done(function(lib) {
  $('.list').append('<li><h3>The name of the library</h3><h4>' + lib.data.name + '</h4></li>');
});

.JSONP({
  url: 'https://api.github.com/users/yui?callback=?',
  timeout: 5000
})
.done(function(lib) {
  $('.list').append('<li><h3>The name of the library</h3><h4>' + lib.data.name + '</h4></li>');
})
.fail(function(msg, settings) {
  console.log(msg);
  console.log('There was a problem connecting to: ' + settings.url);
});
```



###$.form2JSON

This method converts form values into a JSON object. This can be converted to a string for sending to the server with a GET or POST request, or stored on the client side in localStorage or the client side SQLite database.

It expects two parameters:

selector: a valid selector for the form to be processed.
delimiter: a character to use as a delimiter for the marking of JSON member relations in the form element’s name values (see example below). This defaults to “.”, but you can use any other character that suits your purposes.
For this method to work properly you must name all form elements that you want to retrieve so that they match the structure of the resulting JSON object you would like. You use “.” to indicate the sub objects of the JSON object. For example, if your form was for signing up a user, you might have inputs with names such as:

```
<form id="newUser">
  <input type="text" name="newUser.name.first"></input>
  <input type="text" name="newUser.name.last"></input>
  <input type="text" name="newUser.address.street"></input>
  <input type="text" name="newUser.address.city"></input>
  <input type="text" name="newUser.address.state"></input>
  <input type="text" name="newUser.address.zip"></input>
  <input type="text" name="newUser.phone"></input>
  <input type="text" name="newUser.email"></input>
 </form>

{"newUser":
  {"name":
    {"first": "someValueHere"},
    {"last": "someValueHere"}
  },
  {"address":
    {"street": "someValueHere"},
    {"city": "someValueHere"},
    {"state": "someValueHere"},
    {"zip": "someValueHere"}
  },
  {"phone": "someValueHere" },
  {"email": "someValueHere"}
}
```

Disabled form elements or ones which have no value will be ignored.

If you need to create an array from something like a set of choices, you’ll need to marke the name with brackets “[]” to indicate that it’s an array:


```
<div>Favorite food</div>
<p>
  <label>Salad:</label>
  <input type="checkbox" name="user.favoriteFood[]" value="salad">
</p>
<p>
  <label>Pizza:</label>
  <input type="checkbox" name="user.favoriteFood[]" value="pizza">
</p>
<p>
  <label>Chicken:</label>
  <input type="checkbox" name="user.favoriteFood[]" value="chicken">
</p>
```


##Templates

ChocolateChipJS provides its own means of implementing templates. These can be string-based, or script tags or external files.

###$.templates

This is a cache for storing your templates. This makes it easy to retrieve your template for reuse later on.


```
$.template.myTemplate = '<li>Name: [[= data.name]]</li>';
```

Then you could just do this:

```
var parsedTemplate = $.template($.template.myTemplate);
$('#user').html(parsedTemplate(userInfo));
```
You could also use `$.template` to store parsed templates. Then you could use them at any time in your application:

```
$.template.myTemplate = $.template('<li>Name: [[= data.name]]</li>');

// Later:
$('#user').html($.template.myTemplate(userInfo));

```


###$.template

This method allows you to parse templates and render them with data. You can use it to store string templates, or parsed templates ready to pass data to.

ChocolateChipJS uses square brackets as delimiters.Matching sets of square brackets are used to mark off JavaScript code. To render a JavaScript variable, you use include the "=" sign as part of the first block of square brackets. Notice how the data object the template consumes is designated by the term `data`.


```
var myTemplate = '<li id="name">My Name: [[= data.name ]].</li>\
  <li>My age: [[= data.age ]].</li>\
  <li>Job: [[= data.job ]].</li>\
  <li>Current Salary: [[= data.salary ]].</li>\
  [[ for (var i = 0, l = 3; i < l; i++) { ]]\
     [[ console.log("'i' is: " + i); ]]\
  [[ } ]]';

var userInfo = {
  name: 'Wobba',
  age: 100,
  job: 'Rocket Scientist',
  salary: '$1,000,000,000'
};

// Parse the template:
var parsedTempl8 = $.template(myTemplate);

// Pass data to the template and insert it in the document:
$('#user').html(parsedTempl8(userInfo));
```

##Pub/Sub

Pub/sub is a useful programming pattern to help decouple code. This allows you to set up methods that "subscribe" to a topic, something like a radio channel. When you publish data to a topic, all methods that are subscribed to that topic will execute with that data. It thus provides an easy way to pass the same data to multiple functions without having to enclose them in one function.

When implementing a pub/sub pattern, be careful not to over use it, as it can sometimes be difficult to debug or run tests against.

###$.subscriptions


The is a cache for all subscriptions. If you unsubscribe for a topic, the subscription will be removed.

By convention, topics used for subscriptions use forward slashes to demark namespaces, something like a rest interface.

```
// Possible topics:
'user/new'
'user/current'
'purchases/songs'
'purchases/apps'
```


###$.subscribe

To subscribe to a topic, you pass two arguments, a topic and a callback to execute when data gets published. It's a good idea to put in some data checks to see what kind of data is being received. That way you can choose not to do anything, or to use only certain parts of the data. The type of data could be any valid JavaScript type: string, array, object, etc. It's up to you to figure out where this data that you publish comes from. It might reside on the server, it could be dynamically generated by the server. It might come from any number of possible sources: Ajax requests, Web services, etc.

Below is an example of how to subscribe to a topic:

```
var arraySubscriber = function( data ){
  $('.list').append('<li><h3>' + topic + '</h3><h4>' + data + '</h4></li>');
var newsSubscription = $.subscribe( 'news/update', arraySubscriber );
};
```



###$.publish

The $.publish() method lets you send data to all topics that are subscribed to a topic.

```
$.publish('news/update', 'The New York Stock Exchange rose an unprecedented 1000 points in just three minutes. Analysts and investors are confused and uncertain how to respond.');
```

###$.unsubscribe

After setting up a subscription to a topic, you may want to unsubscribe when certain conditions occur. You can do this with the $.unsubscribe() method. Just pass it the name to the subscription you set up, that would be one with a topic and callback as its arguments. See below:

```
$.unsubscribe(newsSubscription);
```

After unsubscribing from a topic, any further attempts to publish data to the method will do nothing.

```
var arraySubscriber = function(data) {
  $('.list').append('<li><h3>' + topic + '</h3><h4>' + data + '</h4></li>');
var newsSubscription = $.subscribe('news/update', arraySubscriber);
};
// This news item gets published:
$.publish( 'news/update', 'The New York Stock Exchange rose an unprecedented 1000 points in just three minutes. Analysts and investors are confused and uncertain how to respond.' );
$.unsubscribe(newsSubscription);
// Due to being unsubscribed above, this does nothing:
$.publish('news/update', 'We have nothing further to comment at this time.');
```



##Deferred Object

The Deferred object is a chainable utility object created by calling the `$.Deferred()` method. It can register multiple callbacks into callback queues, invoke callback queues, and relay the success or failure state of any synchronous or asynchronous function. This implementation is based on the version in jQuery 1.7. Please examine the example and unit tests to see how to use it.

The Deferred object is chainable, similar to the way ChocolateChip methods are chainable, but it has its own methods. After creating a Deferred object, you can use any of the methods below by either chaining directly from the object creation or saving the object in a variable and invoking one or more methods on that variable.

- $.Deferred
- done
- fail
- always
- pipe
- progress
- notify
- notifyWith
- state
- isResolved
- isRejected
- resolve
- resolveWith
- reject
- rejectWith
- then
- when


###$.Deferred

A constructor function that returns a chainable utility object with methods to register multiple callbacks into callback queues, invoke callback queues, and relay the success or failure state of any synchronous or asynchronous function.

The $.Deferred method can be passed an optional function, which is called just before the constructor returns and is passed the constructed deferred object as both the this object and as the first argument to the function. The called function can attach callbacks using `deferred.then()`, for example.

A Deferred object starts in the pending state. Any callbacks added to the object with `deferred.then()`, `deferred.always()`, `deferred.done()`, or `deferred.fail()` are queued to be executed later. Calling `deferred.resolve()` or `deferred.resolveWith()` transitions the Deferred into the resolved state and immediately executes any doneCallbacks that are set. Calling `deferred.reject()` or `deferred.rejectWith()` transitions the Deferred into the rejected state and immediately executes any failCallbacks that are set. Once the object has entered the resolved or rejected state, it stays in that state. Callbacks can still be added to the resolved or rejected Deferred — they will execute immediately.

Enhanced Callbacks with $.Deferred

In JavaScript it is common to invoke functions that optionally accept callbacks that are called within that function.

$.Deferred() introduces several enhancements to the way callbacks are managed and invoked. In particular, $.Deferred() provides flexible ways to provide multiple callbacks, and these callbacks can be invoked regardless of whether the original callback dispatch has already occurred.

One model for understanding Deferred is to think of it as a chain-aware function wrapper. The `deferred.then()`, `deferred.always()`, `deferred.done()`, and `deferred.fail()` methods specify the functions to be called and the deferred.resolve(args) or deferred.reject(args) methods "call" the functions with the arguments you supply. Once the Deferred has been resolved or rejected it stays in that state; a second call to `deferred.resolve()`, for example, is ignored. If more functions are added by `deferred.then()`, for example, after the Deferred is resolved, they are called immediately with the arguments previously provided.

```
var deferred = new $.Deferred();

// or without the new keyword:

var deferred = $.Deferred();
```
###done

Add handlers to be called when the Deferred object is resolved. The argument can be a function, or array of functions, that is called when the Deferred is resolved or rejected.

The `deferred.done()` method accepts one or more arguments, all of which can be either a single function or an array of functions. When the Deferred is resolved, the doneCallbacks are called. Callbacks are executed in the order they were added. Since `deferred.done()` returns the deferred object, other methods of the deferred object can be chained to this one, including additional `.done()` methods. When the Deferred is resolved, doneCallbacks are executed using the arguments provided to the resolve or resolveWith method call in the order they were added.


```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>deferred.done demo</title>
  <script src="chui/chocolatechip.js"></script>
</head>
<body>
 
<button>Go</button>
<p>Ready...</p>
 
<script>
// 3 functions to call when the Deferred object is resolved
function fn1() {
  $( "p" ).append( " 1 " );
}
function fn2() {
  $( "p" ).append( " 2 " );
}
function fn3( n ) {
  $( "p" ).append( n + " 3 " + n );
}
 
// Create a deferred object
var dfd = $.Deferred();
 
// Add handlers to be called when dfd is resolved
dfd
// .done() can take any number of functions or arrays of functions
  .done( [ fn1, fn2 ], fn3, [ fn2, fn1 ] )
// We can chain done methods, too
  .done(function( n ) {
    $( "p" ).append( n + " we're done." );
  });
 
// Resolve the Deferred object when the button is clicked
$( "button" ).on( "click", function() {
  dfd.resolve( "and" );
});
</script>
</body>
</html>
```

###fail

Add handlers to be called when the Deferred object is rejected. The argument can be a function, or array of functions, that is called when the Deferred is resolved or rejected.

The `deferred.fail()` method accepts one or more arguments, all of which can be either a single function or an array of functions. When the Deferred is rejected, the failCallbacks are called. Callbacks are executed in the order they were added. Since `deferred.fail()` returns the deferred object, other methods of the deferred object can be chained to this one, including additional `deferred.fail()` methods. The failCallbacks are executed using the arguments provided to the `deferred.reject()` or `deferred.rejectWith()` method call in the order they were added.

```
// Create a deferred object
var dfd = $.Deferred();

dfd.done(/* Do stuff when done */).fail(/* Do stuff when fail */);
```

###always

Add handlers to be called when the Deferred object is either resolved or rejected. The argument can be a function, or array of functions, that is called when the Deferred is resolved or rejected.

The argument can be either a single function or an array of functions. When the Deferred is resolved or rejected, the callbacks are called. Since `deferred.always()` returns the Deferred object, other methods of the Deferred object can be chained to this one, including additional `.always()` methods. When the Deferred is resolved or rejected, callbacks are executed in the order they were added, using the arguments provided to the resolve, reject, resolveWith or rejectWith 

```
// Create a deferred object
var dfd = $.Deferred();
dfd
  .done(/* Do stuff when done */)
  .fail(/* Do stuff when fail */)
  .always(/* This will always execute for done and fail */);
```

###pipe

Utility method to filter and/or chain Deferreds. It accepts two arguments. The first is `doneFilter` that is called when the Deferred is resolved. The second is `failFilter` that is called when the Deferred is rejected.

The `deferred.pipe()` method returns a new promise that filters the status and values of a deferred through a function. The `doneFilter` and `failFilter` functions filter the original Deferred's resolved / rejected status and values. If the filter function used is null, or not specified, the piped promise will be resolved or rejected with the same values as the original.

####Filter resolve example:

```
var defer = $.Deferred();
var filtered = defer.pipe(function( value ) {
  return value * 2;
});

defer.resolve( 5 );
filtered.done(function( value ) {
  alert( "Value is ( 2*5 = ) 10: " + value );
});  
```

####Filter reject example:

```
var defer = $.Deferred(),
filtered = defer.pipe( null, function( value ) {
  return value * 3;
});

defer.reject( 6 );
filtered.fail(function( value ) {
  alert( "Value is ( 3*6 = ) 18: " + value );
});
```

###progress

Add handlers to be called when the Deferred object generates progress notifications. It is executed from the object onto which the promise methods have to be attached.

The `deferred.promise()` method allows an asynchronous function to prevent other code from interfering with the progress or status of its internal request. The Promise exposes only the Deferred methods needed to attach additional handlers or determine the state (then, done, fail, always, pipe, progress, and state), but not ones that change the state (resolve, reject, notify, resolveWith, rejectWith, and notifyWith).

If target is provided, `deferred.promise()` will attach the methods onto it and then return this object rather than create a new one. This can be useful to attach the Promise behavior to an object that already exists.

If you are creating a Deferred, keep a reference to the Deferred so that it can be resolved or rejected at some point. Return only the Promise object via `deferred.promise()` so other code can register callbacks or inspect the current state.

```
var dfd = $.Deferred();
// Do some stuff with dfd, 
// then do something as it progresses:
dfd.progress(function(value1, value2) {
  equal(value1, param1, 'Should equal param1.');
  equal(value2, param2, 'Should equal param2.');
  return equal(this, context, 'Should be the context.');
});
```


###notify

Call the `progressCallbacks` on a Deferred object with the given args. Optional arguments are an object that is passed to the `progressCallbacks`.

Normally, only the creator of a Deferred should call this method; you can prevent other code from changing the Deferred's state or reporting status by returning a restricted Promise object through `deferred.promise()`.

When `deferred.notify` is called, any `progressCallbacks` added by `deferred.then` or `deferred.progress` are called. Callbacks are executed in the order they were added. Each callback is passed the args from the `deferred.notify()`. Any calls to `deferred.notify()` after a Deferred is resolved or rejected (or any `progressCallbacks` added after that) are ignored. For more information, see the documentation for Deferred object.


```
var context, def, param1, param2, progressCalled;
def = new $.Deferred();
context = new Array();
param1 = 'foo';
param2 = 'bar';
progressCalled = 0; 
def.progress(function(value1, value2) {
  progressCalled += 1;
  equal(value1, param1, 'Should equal param1.');
  equal(value2, param2, 'Should equal param2.');
  return equal(this, context, 'Should be the context.');
});
def.notifyWith(context, param1, param2);
equal(progressCalled, 2, 'Should equal 2.');
def.resolve();
def.notify();
```

###notifyWith

Call the `progressCallbacks` on a Deferred object with the given context and args.

Normally, only the creator of a Deferred should call this method; you can prevent other code from changing the Deferred's state or reporting status by returning a restricted Promise object through `deferred.promise()`.

When `deferred.notifyWith` is called, any `progressCallbacks` added by `deferred.then `or `deferred.progress` are called. Callbacks are executed in the order they were added. Each callback is passed the args from the `deferred.notifyWith()`. Any calls to `deferred.notifyWith()` after a Deferred is resolved or rejected (or any `progressCallbacks` added after that) are ignored. For more information, see the documentation for Deferred object.

See example for `notify` above.

###state

Determine the current state of a Deferred object.

"pending": The Deferred object is not yet in a completed state (neither "rejected" nor "resolved").
"resolved": The Deferred object is in the resolved state, meaning that either `deferred.resolve()` or `deferred.resolveWith()` has been called for the object and the doneCallbacks have been called (or are in the process of being called).
"rejected": The Deferred object is in the rejected state, meaning that either `deferred.reject()` or `deferred.rejectWith()` has been called for the object and the failCallbacks have been called (or are in the process of being called).
This method is primarily useful for debugging to determine, for example, whether a Deferred has already been resolved even though you are inside code that intended to reject it.

You can use this to check the state of the deferred object and take some action based on it. You might use a switch state to test for the states you are interested in.


```
var dfd = $.Deferred();

// Do some stuff with dfd.
// Log its state:
console.log(dfd.state());
```

###isResolved

`deferred.isResolved()` is used to determine whether a Deferred object has been resolved. This method does not accept any arguments. This is a convenience method for getting this value from the `state` method described above. 

Returns true if the Deferred object is in the resolved state, meaning that either `deferred.resolve()` or `deferred.resolveWith()` has been called for the object and the `doneCallbacks` have been called (or are in the process of being called).

Note that a Deferred object can be in one of three states: pending, resolved, or rejected; use `deferred.isRejected()` to determine whether the Deferred object is in the rejected state. These methods are primarily useful for debugging, for example to determine whether a Deferred has already been resolved even though you are inside code that intended to reject it.


###isRejected

`deferred.isRejected()` is used to determine whether a Deferred object has been rejected. This method does not accept any arguments. This is a convenience method for getting this value from the `state` method described above. 

Returns true if the Deferred object is in the rejected state, meaning that either `deferred.reject()` or `deferred.rejectWith()` has been called for the object and the failCallbacks have been called (or are in the process of being called).

Note that a Deferred object can be in one of three states: pending, resolved, or rejected; use `deferred.isResolved()` to determine whether the Deferred object is in the resolved state. These methods are primarily useful for debugging, for example to determine whether a Deferred has already been resolved even though you are inside code that intended to reject it.



###resolve

Resolve a Deferred object and call any `doneCallbacks` with the given args.

```
var dfd = $.Deferred();
// Do stuff with the deferred.
dfd.done(function() {/* Do stuff here */});
// Resolve the deferred, which will 
// cause dfd.done to execute:
dfd.resolve();
```

###resolveWith

Resolve a Deferred object and call any `doneCallbacks` with the given context and args. It accepts an optional array of arguments that are passed to the `doneCallbacks`.

Normally, only the creator of a Deferred should call this method; you can prevent other code from changing the Deferred's state by returning a restricted Promise object through `deferred.promise()`.

When the Deferred is rejected, any `doneCallbacks` added by `deferred.then` or `deferred.fail` are called. Callbacks are executed in the order they were added. Each callback is passed the args from the `deferred.reject()` call. Any `doneCallbacks` added after the Deferred enters the rejected state are executed immediately when they are added, using the arguments that were passed to the `deferred.reject()` call.

```
var context = ["one","two","three"];
var defer = new $.Deferred(); 
defer
  .done(function() {
    console.log('This has been resolved!');
    context.forEach(function(ctx) {
      console.log(ctx);
    });
  });
  .fail(function() {
    console.log('This has failed!');
  });

// Resolve the deferred and pass
// the array as the context:
defer.resolveWith(context);

// outputs:
// "This has been resolved!"
// "one"
// "two"
// "three"

```

###reject

Reject a Deferred object and call any `failCallbacks` with the given args. It accpets optional arguments that are passed to the `failCallbacks`.

Normally, only the creator of a Deferred should call this method; you can prevent other code from changing the Deferred's state by returning a restricted Promise object through `deferred.promise()`.

When the Deferred is rejected, any `failCallbacks` added by `deferred.then()` or `deferred.fail()` are called. Callbacks are executed in the order they were added. Each callback is passed the args from the `deferred.reject()` call. Any `failCallbacks` added after the Deferred enters the rejected state are executed immediately when they are added, using the arguments that were passed to the `deferred.reject()` call.

```
var context = ["one","two","three"];
var defer = new $.Deferred(); 
defer
  .done(function() {
    console.log('This has been resolved!');
  });
  .fail(function() {
    console.log('This was rejected!')
  });

// Reject the deferred:
defer.reject();

// outputs:
// "This was rejected!"

```

###rejectWith

Reject a Deferred object and call any `failCallbacks with` the given context and args. This works just like `deferred.resolveWith`, except that `failCallbacks` are executed instead of `doneCallbacks`.


```
var context = ["one","two","three"];
var defer = new $.Deferred(); 
defer
  .done(function() {
    console.log('This has been resolved!');
    context.forEach(function(ctx) {
      console.log(ctx);
    });
  });
  .fail(function() {
    console.log('This was rejected!');
    console.log('The array has ' + context.length + ' items.')
  });

// Resolve the deferred and pass
// the array as the context:
defer.rejectWith(context);

// outputs:
// "This was rejected"
// "The array has 3 items."
```

###then

Add handlers to be called when the Deferred object is resolved, rejected, or still in progress. It accepts the following arguments in order:

- doneCallbacks: A function, or array of functions, called when the Deferred is resolved.
- failCallbacks: A function, or array of functions, called when the Deferred is rejected.
- progressCallbacks: A function, or array of functions, called when the Deferred notifies progress.

Callbacks are executed in the order they were added. Since deferred.then returns a Promise, other methods of the Promise object can be chained to this one, including additional `deferred.then()` methods.

```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>deferred.then demo</title>
  <script src="chui/chocolatechip.js"></script>
</head>
<body>
 
<button>Filter Resolve</button>
<p></p>
 
<script>
var filterResolve = function() {
  var defer = $.Deferred(),
    filtered = defer.then(function( value ) {
      return value * 2;
    });
 
  defer.resolve( 5 );
  filtered.done(function( value ) {
    $( "p" ).html( "Value is ( 2*5 = ) 10: " + value );
  });
};
 
$( "button" ).on( "click", filterResolve );
</script>
 
</body>
</html>
```

###when

Provides a way to execute callback functions based on one or more objects, usually Deferred objects that represent asynchronous events. It accepsts one or more Deferred objects, or plain JavaScript objects.

If a single Deferred is passed to `$.when`, its Promise object (a subset of the Deferred methods) is returned by the method. Additional methods of the Promise object can be called to attach callbacks, such as `deferred.then`. When the Deferred is resolved or rejected, usually by the code that created the Deferred originally, the appropriate callbacks will be called.

If a single argument is passed to `$.when` and it is not a Deferred or a Promise, it will be treated as a resolved Deferred and any `doneCallbacks` attached will be executed immediately. The `doneCallbacks` are passed the original argument. In this case any `failCallbacks` you might set are never called since the Deferred is never rejected. For example:

```
$.when( { testing: 123 } ).done(function( x ) {
   alert( x.testing ); // Alerts "123"
});
```

In the case where multiple Deferred objects are passed to $.when, the method returns the Promise from a new "master" Deferred object that tracks the aggregate state of all the Deferreds it has been passed. The method will resolve its master Deferred as soon as all the Deferreds resolve, or reject the master Deferred as soon as one of the Deferreds is rejected. If the master Deferred is resolved, it is passed the resolved values of all the Deferreds that were passed to $.when.

In the multiple Deferreds case where one of the Deferreds is rejected, $.when immediately fires the `failCallbacks` for its master Deferred. Note that some of the Deferreds may still be unresolved at that point.

```
$.when(asyncEvent()).then(
  function(status){
    alert(status + ' Things are going well.');
    document.getElementById('foo').style.backgroundColor = 'green';
  },
  function(status){
    alert(status + ' You fail this time.');
    document.getElementById('foo').style.backgroundColor = 'red';
  }
);

// another example
function showDiv(){
  var dfd = $.Deferred();
  dfd.done(function(){
    document.getElementById('foo').style.display = 'block';
  });
  setTimeout(function() { dfd2.resolve() }, 2000);
  return dfd2.promise();
}
showDiv();
```

##Animation

###$.animate

This method allows you to create animations using CSS transitions. The method takes three arguments, an object literal of CSS values to animate, followed by a value for the duration in milliseconds, and an easing descriptor. If no easing is provided, it defaults to linear, if no duration is provided it defaults to half a second.

```
$("#animate").on('doubletap', function() {
  this.anim(
    {
      "-webkit-transform": "rotate3d(30, 150, 200, 180deg) scale(3) translate3d(-50%, -30%, 140%)",
      "opacity": .25,
      "-webkit-transform-style" : "preserve-3d",
      "-webkit-perspective": 500
    },
    2,
    "ease-in-out"
  );
});
```

You can include in the object an optional callback to execute when the animation ends:

```
function saySomething() {
  alert('The animation just ended.');
}

$("#animate").on("click", function() {
  this.anim(
    {
      "-webkit-transform": "rotate3d(30, 150, 200, 180deg) scale(3) translate3d(-50%, -30%, 140%)",
      "opacity": .25,
      "-webkit-transform-style" : "preserve-3d",
      "-webkit-perspective": 500,
      onEnd: saySomething 
    },
    2,
    "ease-in-out");
});
```



##Utilities

ChocolateChipJS has a number of useful utility properties and methods.

- $.version
- $.libraryName
- $.slice
- $.delay
- $.defer
- $.noop
- $.concat
- $.w
- $.isString
- $.isArray
- $.isFunction
- $.isObject
- $.isEmptyObject
- $.isNumber
- $.isInteger
- $.isFloat
- $.uuiNum
- $.makeUuid
- $.require
- "".camelize
- "".deCamelize
- "".capitalize
- "".capitalizeAll

###$.version

The current version of ChocolateChipJS.

###$.libraryName

If you're not sure whether ChocolateChipJS is running, examine this value. If it returns "ChocolateChip", you know what it is.

###$.slice

This is a shortcut for Array.prototype.slice. You use it to turn any array-like object into an array. This will work for node collections or the arguments object.

###$.delay

This method is used by $.defer to execute a function after the call stack is clear. See $.defer below.

###$.defer

This method will defer the execution of a function until the call stack is clear. This means that even though your function is defined before other code, it will not execute until all of the following code has executed.

```
$(document).ready(function() {
  $.defer(function() {
    console.log("This comes after Squawk!");
  });
  console.log('Squawk!');
  // Result: Squawk! This comes after Squawk!
});
```

###$.noop

This is a 'no operation performed' method the you can use when you need a placeholder for a callback. This gets used internally by ChUI.js for cases where it expects a callback but the user may not provide one.

```
// Attach and event that does nothing:
$('button').on('click', $.noop);
```

###$.concat

This method allows you to concatenate string or values using a method interface instead of the '+' operator.

```
$(function() {
  // Concatenate a series of arguments:
  var id = ' id="myButton"',
  class = ' class="button"',
  style = ' style="color: red;"';
  var button = $.concat('<button ', id, class, style, '>Button</button>');
  // Returns: '<button id="myButton" class="button" style="color: red;">Button</button>'

  // Concatenate an array of string values:
  var listIems = ['<li>One</li>','<li>Two</li>','<li>Three</li>','<li>Four</li>']
  var list = $.concat();
  // Returns: '<li>One</li><li>Two</li><li>Three</li><li>Four</li>'
});
```

###$.w

This method takes a space-delimited string of words and returns it as an array where the inidividual words are indeces.

```
var str = 'This is an example of a string'
var strArray = $.w(str) // Returns ['This','is','an','example','of','a','string']
```

###$.isString

This method will return true if the argument is a string.

```
$.isString("This is text"); // Will return 'true'
$.isString(1); // Will return 'false';
```


###$.isArray

This method will return true if the argument is an array.

```
$.isArray([1,2,3]); // Will return 'true'
```

###$.isFunction

This method will return true if the argument is a function.

```
$.isFunction($.noop); // Will return 'true'
```

###$.isObject

This method will return true if the argument is an object.

```
$.isObject({name: 'Joe Bodoni'}); // Will return 'true'
$.isObject(function() {return}); // Will return 'false'
```

###$.isEmptyObject

Returns true is the object is empty {}, otherwise it returns false.

```
var newObj = {};
$.isEmptyObject(newObj); // Will return 'true'
$.isEmptyObject(function() {return}); // Will return 'false'
$.isEmptyObject({name: 'Joe Bodoni'}) // Will return 'false'
```

###$.isNumber

This method will return true if the argument is a number.

```
$.isNumber(1); // Will return 'true'
$.isNumber(0.1); // Will return 'true'
$.isNumber('text'); // Will return 'false'
```

###$.isInteger

This method will return true if the argument is an integer.

```
$.isInteger(1); // Will return 'true'
// The following is a float, not an integer:
$.isInteger(1.1); // Will return 'false'
```

###$.isFloat

This method will return true if the argument is a float.

```
$.isFloat(0.1); // Will return 'true'
// This is an integer:
$.isFloat(1); // Will return 'false'
```

###$.uuiNum 

This method is used internally by ChocolateChip to create uuids for creating identifiers in its cache for events and data.

###$.makeUuid

Like the $.uuidNum method, this is used internally by ChocolateChip for creating unique identifiers for its caching system.

###$.require

This method will import a script into your app. You pass it the source for the script and an optional callback to execute when the script is fully parsed by the browser.

```
$.require('/scripts/libs/sillyputty.js', function() {
   var sp = new SillyPutty(); // Will not execute until sillyputty.js is fully loaded.
});
```

###"".camelize

This method turns a hyphenated string into a camel case string.

```
var camelized = $.camelize('background-color'); // Returns 'backgroundColor'
```

###"".deCamelize

This method converts a camel case string into a hyphenated string.

```
var hyphenated = $.deCamelize('backgroundColor'); // Returns 'background-color'
```

###"".capitalize

This method will capitalize the first letter of a string, as for the first word of a sentence.

```
var word = "chocolate";
word.capitalize(); // returns 'Chocolate'
```


###"".capitalizeAll

This method will capitalize every word in a string.

```
var name = $.capitalize("get out now"); // returns 'Get Out Now'
```

