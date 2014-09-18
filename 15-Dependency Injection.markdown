#Dependency Injection

Dependency injection, or IOC (inversion of control), is a pattern for enabling  decoupled code. The code receiving the injection does not need to know about the actual code being injected. Once injected, you can use it inside as if it were part of the code.

In essence, you could implement a composite pattern function that could consume another funtion. This would be very simple dependency injection. But dependency inject gets most useful when it’s implemented with a container, as this enables an application to have dependency inject built in. Large frameworks, like Angularjs, provide an IOC container by default. And there are some smaller frameworks, like Somajs, that provide this as well. You may create an app that does not have any dependencies, or with many, that’s your choice.

ChocolateChip-UI does not have any built in support for dependency injection, but it is not that hard to implement the basics. That’s what we’ll do in this post. We’ll start with the basic shell for our dependency injector:

```
var Injector = function(fn) {
  // Store for dependencies
  //=======================
  this.dependencies = [];
};
```

To create a new IOC container we just use the "new" keyword with our constructor:

```
var app = new Injector();
```

For dependency injection to work, we need to map a string name to the dependency we wish to use. In our code we need to provide a way to map the string name to the actual dependency. The mapping will enable us to execute the dependency by using the named equivalent:


```
var Injector = function(fn) {
  // Store for dependencies
  //=======================
  this.dependencies = [];
};

// Register a string name to the dependency:
//==========================================
this.map = function(name, dependency) {
  this.dependencies[name] = dependency;
}
```

With the map function defined, we can now link a name to a dependency. Here is an example of a dependency and its mapping:


```
// Function that will be mapped:
var alertMessage = function(msg) {
  return 'This is the message: ' + msg;
}

// New Injector instance:
var app = new Injector();

// Map the depedency to a name:
app.map('Message', alertMessage);
```

Now that we have an array to store names mapped to dependencies, we need to create a "getter" method to retrieve the dependencies:

```
var Injector = function(fn) {
  // Store for dependencies
  //=======================
  this.dependencies = [];
};

// Register a string name to the dependency:
//==========================================
this.map = function(name, dependency) {
  this.dependencies[name] = dependency;
}

// Get a mapped dependency:
//=========================
this.get = function(dependencyArray) {
  var self = this;
  return dependencyArray.map(function(value) {
      return self.dependencies[value];
  });            
};
```

Now that we can map and retrieve the dependencies, we need to define a function on the injector that will process a callback and do the dependency injection. We'll call this function `run`:


```
var Injector = function(fn) {
  // Store for dependencies
  //=======================
  this.dependencies = [];
};

// Register a string name to the dependency:
//==========================================
this.map = function(name, dependency) {
  this.dependencies[name] = dependency;
}

// Get a mapped dependency:
//=========================
this.get = function(dependencyArray) {
  var self = this;
  return dependencyArray.map(function(value) {
      return self.dependencies[value];
  });            
};
/

// Define function to process a callback
// and inject the dependencies:
this.run = function(fn) {
  // Code here
};
```

This `run` function expects a callback as its argument. We now need a way of extracting the callback's arguments to identify its dependencies. Fortunately the Angularjs team came up with a nice regular expression to do that. We'll borrow just borrow it:


```
/^function\s*[^\(]*\(\s*([^\)]*)\)/m
```

This regular expression enables us to parse the callback that our injector will process and extra its arguments:


```
// Define function to process a callback
// and inject the dependencies:
this.run = function(fn) {
  var FN_ARGS = /^function\s*[^\(]*\(\s*([^\)]*)\)/m;
  // convert the provided callback to a string:
  var FN_STRING = fn.toString();
  // Extract the arguments from the string:
  var ARGS = FN_STRING.match(FN_ARGS)[1].split(',');
};
```

The variable `ARGS` could hold a single argument, or multiple. It doesn't matter:

```
// Define function to process a callback
// and inject the dependencies:
this.run = function(fn) {
  var FN_ARGS = /^function\s*[^\(]*\(\s*([^\)]*)\)/m;
  // convert the provided callback to a string:
  var FN_STRING = fn.toString();
  // Extract the arguments from the string:
  var ARGS = FN_STRING.match(FN_ARGS)[1].split(',');
};
ARGS = ARGS.map(function(value) {
  return value.trim();
});
```

With the arguments identified, we can map them against the array of stored dependencies to retrieve the correct ones. We then use the JavaScript `apply` method to execute our callback and pass the array of dependencies as its argument. This works because the apply method expects a single array-like object as its argument:

```
// Run the code with dependencies:
//================================
this.run = function(fn) {
  var FN_ARGS = /^function\s*[^\(]*\(\s*([^\)]*)\)/m;
  var FN_STRING = fn.toString();
  var ARGS = FN_STRING.match(FN_ARGS)[1].split(',');
  ARGS = ARGS.map(function(value) {
    return value.trim();
  });
  fn.apply(fn, this.get(ARGS));
};
```

That last line does the actuall injection of our dependencies into the function. Here's the complete code:


```
///////////////////////
// Dependency Injector:
///////////////////////
var Injector = function(fn) {
  // Store for dependencies
  //=======================
  this.dependencies = [];

  // Map a string name to a dependency:
  //===================================
  this.map = function(name, dependency) {
    this.dependencies[name] = dependency;
  }

  // Get a mapped dependency:
  //=========================
  this.get = function(dependencyArray) {
    var self = this;
    return dependencyArray.map(function(value) {
        return self.dependencies[value];
    });            
  };

  // Run the code with dependencies:
  //================================
  this.run = function(fn) {
    var FN_ARGS = /^function\s*[^\(]*\(\s*([^\)]*)\)/m;
    var FN_STRING = fn.toString();
    var ARGS = FN_STRING.match(FN_ARGS)[1].split(',');
    ARGS = ARGS.map(function(value) {
      return value.trim();
    });
    fn.apply(fn, this.get(ARGS));
  };
};
```

We'll add one more function. It doesn't add any utility but is purefly for semantics `this.injectInto`:


```
///////////////////////
// Dependency Injector:
///////////////////////
var Injector = function(fn) {
  // Store for dependencies
  //=======================
  this.dependencies = [];

  // Map a string name to a dependency:
  //===================================
  this.map = function(name, dependency) {
    this.dependencies[name] = dependency;
  }

  // Get a mapped dependency:
  //=========================
  this.get = function(dependencyArray) {
    var self = this;
    return dependencyArray.map(function(value) {
        return self.dependencies[value];
    });            
  };

  // Run the code with dependencies:
  //================================
  this.run = function(fn) {
    var FN_ARGS = /^function\s*[^\(]*\(\s*([^\)]*)\)/m;
    var FN_STRING = fn.toString();
    var ARGS = FN_STRING.match(FN_ARGS)[1].split(',');
    ARGS = ARGS.map(function(value) {
      return value.trim();
    });
    fn.apply(fn, this.get(ARGS));
  };

  // Semantic alias for "run" function
  // because dependencies are injected
  // into the argument passed to it.
  //==================================
  this.injectInto = function(fn) {
    return this.run(fn);
  };
};
```

We can now perform actual dependency injection. A function may have one or more dependencies.

```
// Function with dependency,
// this will be indicated
// when the mappping occurs:
//==========================
var MsgControllerOne = function (FirstMessage) {
  var list = $("&lt;ul&gt;&lt;/ul&gt;");
  list.addClass('list');
  $(list).append('&lt;p&gt;' + FirstMessage.announce() + '&lt;/li&gt;');
};

// Function with dependencies,
// these will be indicated
// when the mappping occurs:
//============================
var MsgContollerTwo = function(FirstMessage, BozoMessage, WobbaMessage) {
  var list = $('&lt;ul&gt;&lt;/ul&gt;');
  list.addClass('list');
  $(list).append('&lt;li&gt;' + FirstMessage.announce() + ' : ' + BozoMessage.announce() + '&lt;/li&gt;');
  $(list).append('&lt;li&gt;' + WobbaMessage ('Wobba was here!!!') + '&lt;/li&gt;');
};

// Function that will be injected:
var FirstMsg = {
  announce: function() {
    return 'This is the first message';
  }
};

// Function that will be injected:
var SecondMsg = {
  announce: function() {
    return 'Bozo the Clown was here!';
  }
};

// Function that will be injected:
var ThirdMsg = function(msg) {
  return 'This is the message: ' + msg;
};
```

Above are two functions, `msgControllerOne` and `msgContollerTwo`, and their dependencies are: `FirstMsg`, `SecondMsg`, and `ThirdMsg`. Our first controller has only one dependency, but the second has three. Now let's create the Injector instance and create the mappings:


```
// Create instance of dependency injector:
//========================================
var app = new Injector();

// Map dependencies to string names:
//==================================
app.map('FirstMessage', FirstMsg);
app.map('BozoMessage', SecondMsg);
app.map('WobbaMessage', ThirdMsg);
```

Now we can run the instance and see the injection take place:

```
////////////////////////////////////////
// Run instance and inject dependencies:
////////////////////////////////////////
app.run(function() {

  // Inject dependencies into functions.
  // This will execute the dependencies
  // and output their messages:
  //====================================
  app.injectInto(MsgControllerOne);
  app.injectInto(MsgContollerTwo);

});
```

This would result in the two controllers be executed with their dependencies, which would immediately output the following:


```
```

We could do more than simply use the IOC container for dependency injection. We can use it as the namespace for our app where we wire up events and evented mediators to handle user interaction with the UI. Let's add some markup and interactivity. We'll use ChocolateChip-UI's pub/sub function to create the data binding between an input element and a paragraph, which will be managed by a mediator:


```
<ul class="list">
   <li><input type="text"></li>
</ul>
<p id="result"></p>
```

```
////////////////////////////////////////
// Run instance and inject dependencies:
////////////////////////////////////////
app.run(function() {

  // Inject dependencies into functions:
  app.injectInto(MsgControllerOne);
  app.injectInto(MsgContollerTwo);

  var input = document.querySelector('input');
  var result = document.querySelector('#result');

  // Add event listener to publish updates:
  input.addEventListener('keydown', function(e) {
    $.publish('input-value', this.value);
  });

  // Define mediator to subscribe to updates:
  var Mediator = $.subscribe('input-value', function(e, value) {
    result.innerHTML = value;
  });
})
```

In our above example we've defined two controllers which we passed to the `infectInto` function of our app. We could also just as easily use anonymous functions as the argument of the `injectInto` and use our dependencies that way. Some people prefer this code style:


```
// Function that will be injected:
var FirstMsg = {
  announce: function() {
    return 'This is the first message';
  }
};

// Function that will be injected:
var SecondMsg = {
  announce: function() {
    return 'Bozo the Clown was here!';
  }
};

// Function that will be injected:
var ThirdMsg = function(msg) {
  return 'This is the message: ' + msg;
}

// Create instance of dependency injector:
//========================================
var app = new Injector();

// Map dependencies to string names:
//==================================
app.map('FirstMessage', FirstMsg);
app.map('BozoMessage', SecondMsg);
app.map('WobbaMessage', ThirdMsg);

////////////////////////////////////////
// Run instance and inject dependencies:
////////////////////////////////////////
app.run(function() {

  // Inject dependencies into functions:
  app.injectInto(function (FirstMessage) {
    var list = $("&lt;ul&gt;&lt;/ul&gt;");
    list.addClass('list');
    $(list).append('&lt;p&gt;' + FirstMessage.announce() + '&lt;/li&gt;');
  });
  app.injectInto(function(FirstMessage, BozoMessage, WobbaMessage) {
    var list = $('&lt;ul&gt;&lt;/ul&gt;');
    list.addClass('list');
    $(list).append('&lt;li&gt;' + FirstMessage.announce() + ' : ' + BozoMessage.announce() + '&lt;/li&gt;');
    $(list).append('&lt;li&gt;' + WobbaMessage ('Wobba was here!!!') + '&lt;/li&gt;');
  });

  var input = document.querySelector('input');
  var result = document.querySelector('#result');

  // Add event listener to publish updates:
  input.addEventListener('keydown', function(e) {
    $.publish('input-value', this.value);
  });

  // Define mediator to subscribe to updates:
  var Mediator = $.subscribe('input-value', function(e, value) {
    result.innerHTML = value;
  });
});
```

One last thing, if you have serious needs for dependency injection, you should look at using a library that has fully fleshed out support for it. This post is about showing how dependency injection works and how to set up the simplest possible implementation of an IOC container. We hope you found this insightful so that when you hear about dependency injection or use one of the dedicated solutions, you understand what is going on behind the scenes.






























