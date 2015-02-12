#Mediators, Pub/Sub and Data Binding

Mediators are a really useful coding pattern to help organize and maintain a UI. A mediator is an object that represents another object. This object could be anything. When using a mediator with a user interface, a mediator usually represents the visual aspect of some interactive part of the interace. That way, if the user makes a choice while interacting with the UI, the mediator becomes aware and does something, such as updating the model, or persisting the model, or acquires other data relevant to the user's choice.

The role of a mediator is to keep your view and your model completely separated. With proper mediators, the model and view don't need to know anything about each other. 

ChocolateChip-UI provides a very efficient way for mediators to communicate with the model and view without having to know about either. This can be done using the built-in `pub/sub` methods. Your models subscribe to broadcasts and publish broadcasts, and your view publishes broadcasts whenever the user interacts with it. The mediator can subscribe or publish as well, allowing it to update the view or interact with the model through the publication of broadcasts. At the moment this may not seem very useful and perhaps even a bit confusing, so we'll introduce some examples of how to set up a mediator with `pub/sub` broadcasts.

Here's the markup for our example. We want to bind the `h3` in the second list item to the input in the first list item, so that as the user enters data in the text input, the `h3` gets updated automatically.

```
<nav class="current">
  <h1>Data Binding</h1>
</nav>
<article id="main" class="current">
  <section>
    <ul class="list">
      <li><lable>Enter data:</lable> 
        <input id='input-source' type='text'>
      </li>
      <li>
        <h3 id='input-output'></h3>
      </li>
    </ul>
  </section>
</article>
```

We could make this markup work with an event handler on the input. However, this is spaghetti code. If we had lots of form elements, this would get complicated. And each time we wanted to make a change to our UI, we'd need to update our input event listeners. This would lead to maintenance issues with complex UIs. Here's an example of an event handler that will update the h3 when the user inputs text:

```
$('#input-source').on('input', function(e) {
  $('input-output').text($(this).val());
});
```

This works, it's simple, it's clear what typing in the input will do, but the two are now tightly coupled. Using a mediator with pub/sub we can separate the two. Of course, it's more code. If your app only does this in a few places, it might not be worth your trouble implementing mediators. But if you have a more complex app, you'll want to give mediators a try. Now let's set up a mediator. First we need to change the role of the input event listener. Instead of directly updating the h3, it's going to publish a broadcast with its changed data. Any number of objects can subscribe to this broadcast and respond to it differently according to their needs. Besides publishing a broadcast, we also send the value of the input as the second parameter of the broadcast.


```
$('#input-source').on('input', function(e) {
  $.publish('input-value-update', $(this).val());
});
```

Now that the input can publish a broadcast, we can create a mediator to listen for it:

```
var H3Mediator = $.subscribe('input-value-update', function(event, value) {
  $('input-output').text(value);
});
```


Using the above mediator pattern, we could create as many mediators as we want, thousands even, without the input event handler needing to know that they exist. And we can modify the mediators or remove them without needing to update the event handler. Hopefully, this makes it clear why this pattern is so userful in app development. Imagine you had a lot of widgets on a page. Some widgets might need to be updated when the user interacted with others. Without mediators and publications, the widgets would need to know about each other and their code would be tangled together. With mediators and publications, no widget needs to know about any other widget. If you create your mediators with proper checks, you would be able to add and remove widgets without at problem.

For most inputs, the `input` event is sufficient to capture any changes in their values. However, select boxes are different. Let's take a look at how to handle a select box.


```
<ul class="list">
  <li>
    <select name="choices" id="select-source">
      <option value="First Choice">First Choice</option>
      <option value="Second Choice">Second Choice</option>
      <option value="Third Choice">Third Choice</option>
    </select>
  </li>
</ul>
<p id="select-value"></p>
```

Because this is a select box, we need to use the `change` event to capture the value the user has chosen:

```
$('#select-source').on('change', function(e) {
  $.publish('select-change', $(this).val());
});
```

Then we can wire up a mediator to update the paragraph tag:


```
var SelectBoxMediator = $.subscribe('select-change', function(event, choice) {
  $('#select-value').text(choice);
});
```


This works when the user makes a choice, but at load time the paragraph would not show any value. That's because the event only fires when there's a change in the select box. We can get around this problem with select boxes by immediately publishing a broadcast with the current value of the select box at load time. This needs to come after the mediator that subscribes to the broadcast, otherwise the broadcast will be published before the mediator has subscribed to it:

```
$.publish('select-change', $('#select-source').val());
```

Now let's setup a range input. We want to capture the value whenever the user drags the range input's thumb, so we'll define a mediator to handle this:


```
<ul class="list">
  <li>
    <input type="range" id="range">
  </li>
</ul>
<p id="range-output"></p>
```

First we need to define an event handler to publish the current value of the range input, then we need to capture that and ouput it to the screen with a mediator. We also need to publish what the default value of the range input is at the time the page loads: 


```
$('#range').on('input', function(e) {
  $.publish('range-value', $(this).val());
});

var RangeInputMediator = $.subscribe('range-value', function(event, value) {
  $('#range-output').text(value);
});

// Publish default range value at load time:
$.publish('range-value', $('#range').val());
```

##Data Binding

As of version 3.7.0, ChocolateChip-UI includes a simple method, `$.UIBindData()`, for implementing data binding between form inputs and other parts of the UI. You designate the binding relationship using two attributes: `data-controller` and `data-model`. If you give a `data-controller` the same value as a `data-model` and then run `$.UIBindData()`, this method will scan the document and establish the binding between them. That's all there is to do. Although this is technically only one-way data binding, you can set up a mediator with a broadcast for a `data-controller` and model to easily establish a two-way data binding relationship. The use of simple, declarative markup and ChocolateChip-UI's `$.UIBindData` method result in the creation of mediators and publications without you having to write any support code. Take a look at this markup:


```
<ul class="list">
  <li><lable>Enter data: </lable> 
    <input id='myText' data-controller='input-value' type='text'></li>
  <li>
    <h3 data-model='input-value'></h3>
  </li>
</ul>
```

Because the data-controller and data-model both have the same value of `input-value`, to establish the binding so that when the user enters text in the input, the h3 updates automatically, we just need to execute ChocolateChip-UI's data binder:

```
$.UIBindData();
```

This registers a delegated event for the input and publishes the data which the mediator consumes and uses to update the h3. If we wanted, we could put the same data-model on other elements so that they all updated with the input value at the same time. Or we could put the same data-controller on different inputs so that any single one of them would update the h3. 

As mentioned, this is all just one-way data binding. If we want a model to also be updated by the changed value of the input, or the input's value to be updated if some other action update's it's model, we can do that with a mediator. First we'll need to define a model that will handle our data:

```
var InputTextModel = function() {
  // Data is a simple string.
  // We make it private so that
  // it is not accessible outside
  // this model:
  var private_data = '';

  // We deine a subsription to the input,
  // and use that to update the private data:
  $.subscribe('data-binding-input-value', function(event, data) {
    private_data = data;
  });

  // We define a method to 
  // get the private data:
  this.get = function() {
    return private_data;
  }
};

// After defining the model, 
// we initialize it:
var myText = new InputTextModel();
```

In the above code we define our model and include a subscriber that will update its data when an input controller broadcasts its data. By instantiating an new instance, we can access this model in real time through the browser console. Just make sure this block of code is outside the jQuery document ready function. After entering text in the input, you'll be able to enter `myText.get()` in the console and see that the model now has the same value as the input and h3. If we use this code with our example of the data bound text input and h3, when the user enters text in the input, the above model will capture that broadcast and update its data.

For any serious data acquisition it would be a good idea to include some kind of data validation before attaching the received data to the model. For this simple example we will not bother.

Another thing we can do is add a setter to our model. We want to be able to update the value of our model at any time in our app's life cycle. At the same time, if that happens, we also want that update to be published to our input and h3. This will effectively give us two data binding. Here's how we inplement it:



```
var InputTextModel = function() {
  // Data is a simple string.
  // We make it private so that
  // it is not accessible outside
  // this model:
  var private_data = '';

  // We deine a subsription to the input,
  // and use that to update the private data:
  $.subscribe('data-binding-input-value', function(event, data) {
    private_data = data;
  });

  // We define a method to 
  // get the private data:
  this.get = function() {
    return private_data;
  }

  // Define a setter. This will
  // publish a two broadcasts:
  // one for the text input
  // and one for the h3.
  this.set = function(data) {
    $.publish('data-binding-input-value', data);
    $.publish('update-input-value', data);
  }
};

// After defining the model, 
// we initialize it:
var myText = new InputTextModel();
```

We'll define a mediator to subscribe to our publication and then update the text input:


```
var updateInputText = $.subscribe('update-input-value', function(event, data) {
  $('#myText').val(data);
});
```

With the above code defined, out previous example of a data bound text input and h3 are also bound to `InputTextModel`. If we open the browser's console and do the following:

```
myText.set('This is the new input value.');
```

We would see that the input and the h3 automatically update to the model's new value. In this example we've bound a text input to and h3 to a model, and we also bound the model back to the text input and h3. A change in the value of one will cause a change in the value of the other. 

If your needs are not too complicated, it's easy to use ChocolateChip-UI's `$.BindData` method with mediators to create some useful one and two-way data binding. Most of the time you will probably not need two-way data binding. If your app does happen to require a lot of two-way data binding, you might want to consider using a more robust framework that supports that, such as Knockout, Angularjs, or others. Do be aware that when you create many two-way data binding relationships with such frameworks, your app will most likely experience a noticeable slowdown on mobile devices, resulting in sluggish performance. ChocolateChip-UI's approach to data binding reduces memory consumption and the resulting mobile Web app slowdown. Only implement data binding where absolutely necessary.




