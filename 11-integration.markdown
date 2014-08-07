#ChocolateChip-UI

##Integration

###Just the View

ChocolateChip-UI was designed to work easily with any backend and supporting frameworks. ChocolateChip-UI is just your app's view. You can use whatever you want to handle the data model, controllers, routing, and data binding.

####Single Page Web App

ChocolateChip-UI is not intended to rendering multi-page websites. It was created for use with single page apps. ChocolateChip-UI allows you to break your app down into logical structures that work and feel like normal native software.

###Knockout

[http://knockoutjs.com](http://knockoutjs.com)

The KnockoutJS library provides you with a way to implement the Model View View-Model pattern. Knockout uses declarative bindings between your data and the UI. You need to create a view model of your data. Then bind the view to your view model with declarative statements in the view's markup. If your needs are simple, you may find Knockout's two-way data-binding is all you need to implement your app.

In the following example, we define a View Model that points to the data we want to display. We need to bind the View Model to the View. And we need to put some declarative bindings on the View.


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
  <link rel="stylesheet" href="../chui/chui-ios-3.5.4.css">
  <script src="http://code.jquery.com/jquery-2.0.3.min.js"></script>
  <script src="../chui/knockout-3.1.0.js"></script>
  <script src="../chui/chui-3.5.4.js"></script>
  <script>
    $(function() {
      var music = [
        {
          title: 'Radioactive',
          artist: 'Imagine Dragons',
          detail: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea.',
          imgPath: '../images/music/Imagine Dragons.jpg'
        },
        {
          title: 'The Hurry and the Harm',
          artist: 'Hurt',
          detail: 'Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo.',
          imgPath: '../images/music/Hurry and Harm.jpg'
        },
        {
          title: 'Permanent',
          artist: 'David Cook',
          detail: 'Nemo enim ipsam voluptatem quia voluptas sit aspernatur aut odit aut fugit, sed quia consequuntur magni dolores eos qui ratione voluptatem sequi nesciunt. Neque porro quisquam est, qui dolorem ipsum quia dolor sit amet.',
          imgPath: '../images/music/Permanent.jpg'
        },
        {
          title: 'This Kiss',
          artist: 'Kiss',
          detail: 'At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias excepturi sint occaecati cupiditate non provident.',
          imgPath: '../images/music/Kiss.jpg'
        },
        {
          title: 'Yeah Yeah',
          artist: 'Willy Moon',
          detail: 'Temporibus autem quibusdam et aut officiis debitis aut rerum necessitatibus saepe eveniet ut et voluptates repudiandae sint et molestiae non recusandae. Itaque earum rerum hic tenetur a sapiente delectus, ut aut reiciendis voluptatibus.',
          imgPath: '../images/music/Willy Moon.jpg'
        }
      ];

      function MusicViewModel() {
        var self = this;
        self.data = music;
      }
      ko.applyBindings(MusicViewModel);

    });
  </script>
</head>
<body>
  <nav>
    <h1>Composite List</h1>
  </nav>
  <article id='main'>
    <section>
      <h2>Music</h2>
      <ul class='list' data-bind='foreach: data'>
        <li class='comp'>
          <aside>
            <img title='Imagine Dragons' data-bind='attr: {src: imgPath}' height="80px">
          </aside>
          <div>
            <h3 data-bind='text: title'></h3>
            <h4 data-bind='text: artist'></h4>
            <p data-bind='text: detail'></p>
          </div>
          <aside>
            <span class='show-detail'></span>
          </aside>
        </li>
      </ul>        
    </section>
  </article>
</body>
</html>
```

This is of course very basic. If you already know KnockoutJS, you know can take this to the next level. Otherwise, visit the site to [learn more](http://knockoutjs.com).

###Backbone

[http://http://backbonejs.org](http://http://backbonejs.org)

If you know Backbone, you already know what you need to do. Create a model, make a collection based on the above Knockout example, define an underscore template on the ChocolateChip-UI example, etc. There are all kinds of plugins to add things like data binding, etc.

Backbone is a complex framework that takes time to learn. The following example is not best practices. There's no real templates, no data binding, etc. It's just here to give you an idea of how it is possible to use ChocolateChip-UI as the themes and templates for you Backbone app.


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
  <link rel="stylesheet" href="../chui/chui-ios-3.5.4.css">
  <script src="http://code.jquery.com/jquery-2.0.3.min.js"></script>
  <script src="../chui/underscore-min.js"></script>
  <script src="../chui/backbone-min.js"></script>
  <script src="../chui/chui-3.5.4.js"></script>
  <script>
    $(function() {
      var music = [
        {
          title: 'Radioactive',
          artist: 'Imagine Dragons',
          detail: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea.',
          imgPath: '../images/music/Imagine Dragons.jpg'
        },
        {
          title: 'The Hurry and the Harm',
          artist: 'Hurt',
          detail: 'Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo.',
          imgPath: '../images/music/Hurry and Harm.jpg'
        },
        {
          title: 'Permanent',
          artist: 'David Cook',
          detail: 'Nemo enim ipsam voluptatem quia voluptas sit aspernatur aut odit aut fugit, sed quia consequuntur magni dolores eos qui ratione voluptatem sequi nesciunt. Neque porro quisquam est, qui dolorem ipsum quia dolor sit amet.',
          imgPath: '../images/music/Permanent.jpg'
        },
        {
          title: 'This Kiss',
          artist: 'Kiss',
          detail: 'At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias excepturi sint occaecati cupiditate non provident.',
          imgPath: '../images/music/Kiss.jpg'
        },
        {
          title: 'Yeah Yeah',
          artist: 'Willy Moon',
          detail: 'Temporibus autem quibusdam et aut officiis debitis aut rerum necessitatibus saepe eveniet ut et voluptates repudiandae sint et molestiae non recusandae. Itaque earum rerum hic tenetur a sapiente delectus, ut aut reiciendis voluptatibus.',
          imgPath: '../images/music/Willy Moon.jpg'
        }
      ];

      var SongModel = Backbone.Model.extend({});

      var SongCollection = Backbone.Collection.extend({ 
        model: SongModel
      });

      var songs = new SongCollection(music);
      
      $('.list').empty();
      songs.models.forEach(function(ctx) {
        $('.list').append('<li class="comp"><aside><img height="80" src="'+ctx['attributes'].imgPath+'"></aside><div><h3>'+ctx['attributes'].title+'</h3><h4>'+ctx['attributes'].artist+'</h4><p>'+ctx['attributes'].detail+'</p></div><aside><span class="show-detail"></span></aside></li>')
      });
    });
  </script>
</head>
<body>
  <nav>
    <h1>Composite List</h1>
  </nav>
  <article id='main'>
    <section>
      <h2>Music</h2>
      <ul class='list'>
      </ul>        
    </section>
  </article>
</body>
</html>
```
###Angular

[https://angularjs.org](https://angularjs.org)

AngularJS is a comprehensive framework that provides a solid Model View Control pattern for building complex and robust apps. It's also very flexible in that it can be integrated with other frameworks. Of course, for Angular users it would be best if there was an Angular version of ChocolateChip-UI where the widgets were Angular directives. At the moment we are serious looking into creating an Angular version of ChocolateChip-UI.

Using Angular's declarative properties and directives, you can easily use it with ChocolateChip-UI:

```
<!DOCTYPE html>
<html ng-app="musicApp">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="msapplication-tap-highlight" content="no">
  <title>ChocolateChip-UI iOS</title>
  <link rel="stylesheet" href="../chui/chui-ios-3.5.4.css">
  <script src="http://code.jquery.com/jquery-2.0.3.min.js"></script>
  <script src="../chui/angular.js"></script>
  <script src="../chui/chui-3.5.4.js"></script>
  <script>
    var music = [
      {
        title: 'Radioactive',
        artist: 'Imagine Dragons',
        detail: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea.',
        imgPath: '../images/music/Imagine Dragons.jpg'
      },
      {
        title: 'The Hurry and the Harm',
        artist: 'Hurt',
        detail: 'Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo.',
        imgPath: '../images/music/Hurry and Harm.jpg'
      },
      {
        title: 'Permanent',
        artist: 'David Cook',
        detail: 'Nemo enim ipsam voluptatem quia voluptas sit aspernatur aut odit aut fugit, sed quia consequuntur magni dolores eos qui ratione voluptatem sequi nesciunt. Neque porro quisquam est, qui dolorem ipsum quia dolor sit amet.',
        imgPath: '../images/music/Permanent.jpg'
      },
      {
        title: 'This Kiss',
        artist: 'Kiss',
        detail: 'At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias excepturi sint occaecati cupiditate non provident.',
        imgPath: '../images/music/Kiss.jpg'
      },
      {
        title: 'Yeah Yeah',
        artist: 'Willy Moon',
        detail: 'Temporibus autem quibusdam et aut officiis debitis aut rerum necessitatibus saepe eveniet ut et voluptates repudiandae sint et molestiae non recusandae. Itaque earum rerum hic tenetur a sapiente delectus, ut aut reiciendis voluptatibus.',
        imgPath: '../images/music/Willy Moon.jpg'
      }
    ];
    angular.module("musicApp", [])
        .controller("musicCtrl", function ($scope) {
            $scope.songs = music;
        });
  </script>
</head>
<body>
  <nav>
    <h1>Composite List</h1>
  </nav>
  <article id='main'>
    <section>
      <h2>Music</h2>
      <ul class='list' ng-controller="musicCtrl">
        <li class='comp' ng-repeat="song in songs">
          <aside>
            <img title='{{ song.title }}' src="{{ song.imgPath }}" height="80px">
          </aside>
          <div>
            <h3>{{ song.title }}</h3>
            <h4>{{ song.artist }}</h4>
            <p>{{ song.detail }}</p>
          </div>
          <aside>
            <span class='show-detail'></span>
          </aside>
        </li>
      </ul>
    </section>
    </article>
</body>
</html>
```


If you want to be able to use ChocolateChip-UI's gestures as directives, you can import the following module into your app, then inject the gestures you wish to use in your app's module. We use the prefix "cc" for ChocolateChip to distinguish these directives from others:


```
///////////////////////////////////////////////////
// Create directives for ChocolateChip-UI gestures.
// Iterate over array of avaialable events:
///////////////////////////////////////////////////
['tap', 'singletap', 'longtap', 'doubletap', 'swipe', 'swipeleft', 
'swiperight', 'swipeup', 'swipedown'].forEach(function(gesture) {

  // Camel Case each directive:
  //===========================
  var ccGesture = 'cc' + (gesture.charAt(0).toUpperCase() + gesture.slice(1));

  // Create module for each directive:
  //==================================
  angular.module(ccGesture,[]).directive(ccGesture, ['$parse',
    function($parse) {
      return {
        // Restrict to attribute:
        restrict: 'A',
        compile: function ($element, attr) {
          // Define the attribute for the gesture:
          var fn = $parse(attr[ccGesture]);
          return function ngEventHandler(scope, element) {
            // Register event for directive:
            $(element).on(gesture, function (event) {
              scope.$apply(function () {
                fn(scope, {$event: event});
              });
            });
          };
        }
      };
    }
  ]);
});
```


With the above module imported into your app you can inject the gestures as follows:


```
var app = angular.module('MyApp', ['ccSingletap', 'ccDoubletap', 'ccSwipe']);
```

With these injected into your app, you can then use them like normal Angularjs event directives:

```
<li ng-repeat="item in list" cc-singletap='respondToSingleTap(item)' cc-swipe='respondeToSwipe(item)'></li>
```

You could then define the method on your controller's scope:

```
app
.controller('MyCtrl', function($scope){
  $scope.respondToSingleTap = function(item) {
    alert('You single tapped on: ' + item)
  };
  $scope.respondToSwipe = function(item) {
    alert('You swiped on: ' + item)
  };
});
```


###Phonegap

[http://phonegap.com](http://phonegap.com)

Phonegap allows you to wrap your mobile Web app with native code so that your Web app becomes a native app. After creating an empty Phonegap/Cordova project, you need to add ChocolateChip-UI. It goes in the platform-specific www folder. You'll need the theme and the ChUI.js file, as well as any media you need. Phonegap will render your HTML, CSS and JavaScript in a Webview for each platform you target. Just use the appropriate CSS theme for each platform. It's that straightforward.

Please visit [http://phonegap.com](http://phonegap.com) or [https://cordova.apache.org](https://cordova.apache.org) for more specific information on creating native wrappers for Web apps.

Here you can see a typical iOS Phonegap project in Xcode. Notice the www folder towards the top in the file explorer on the right. You can see the `chui` folder and the index file, which is our Web app. The rest is the Phonegap generated structure to turn this into a standalone native iOS app. Phonegap does something similar for other platforms as well.

![Phonegap](images/integration/phonegap.png)

