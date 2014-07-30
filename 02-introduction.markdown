#ChocolateChip-UI

##Introduction

###What is ChocolateChip-UI?

ChocolateChip-UI is a framework for building mobile Web apps using the popular jQuery JavaScript library. These can be run in mobile browsers, or delievered to app stores using Phonegap/Cordova. Specifically, ChocolateChip-UI is just the UI or interface of your app, what developers call the view. ChocolateChip-UI makes no assumptions about what your app's backend is. This means you can choose the backend you want for your app and what you need to implement the controllers and data models: Backbone, Knockout, Spine, Ember, Angularjs, JavaScriptMVC, CanJS, etc.

ChocolateChip-UI gives you the tools to create a consistent and satisfying user experience on three mobile platforms: Android, iOS and Windows Phone 8. It does this through the use of themes. To support a mobile platform is just a matter of switching out the theme.

####Clean, Semantic Markup

While we were in the planning stage, one of the things we wanted to accomplish was to make the framework really easy to understand. For this end we decided to go with a very lean and clean approach to defining the markup. Layouts and widgets are defined by simple, uncluttered, semantic HTML5 markup. ChocolateChip-UI recreates the UI of Android, iOS and Windows Phone 8 using just the following tags:

1. article
2. nav
3. section
4. div
5. aside
6. ul
7. li
8. h1 - h5
9. span
10. a

ChocolateChip-UI uses a small number of classes with these tags to define their styles and to identify their functionality to ChUI.js so that it can intialize them at load time.

####CSS3 Themes

The biggest challenge when trying to support more than one mobile platform is the differences between them in not only appearance, but also in user interaction. All platforms have ways of displaying data in organized structures and provide common patterns for user interaction through controls. However, each mobile platform strives to distinguish itself from its competitors by differenciating the visual appearance as much as possible. They also try to create quite different user experiences by making the interaction with the user interface unique to their platform. Although this is great for the users of the platforms, this is problematic for developers trying to build apps that work on them. 

There are two approaches generally taken when trying to resolve this problem. Create a one size fits all framework, where the app looks and behaves identically on all platforms. Or create a framework with a very limitted set of layouts and widgets that sort of look like the target platform through the use of themes. Unfortunately this always results in interfaces that are unconvincing and sometimes awkward and quirky. Users will notice the non-standard look and behavior and they may conclude that there is something wrong with the app because its interface doesn't look or behave properly.

We were not very happy with either of these approaches. We knew that having themes was the right approach, but we were not satisfied with anything we found. We therefore decided to take this approach a step further. We went over each platform, reading documentation, examining header files, really wrapping our heads around how they worked. We took to heart the UI/UX guidelines provided by each platform vendor and used these to determine what we needed to build. We compared our lists for each platform, we noted their similarities and differences, and came up with a master list that spanned all three platforms. Using the master list, we began creating diagrams of the markup needed to create the structures to represent them. We wanted the least amount of markup possible. We wanted the markup to represent data structures as faithfully as possible, eliminating markup that was purely a prop for a visual effect. We were fanatical about this. We wanted the DOM tree created by the markup to be as shallow as possible. This would allow for faster browser renderings. When this was completed, we moved on to the the next stage. 

ChocolateChip-UI themes are built using LESS. Each layout type and widget has its own LESS file. Also all colors are defined in a separate colors.less file. This means you can open that file up and modify the colors of your app for branding, then simply reprocess the LESS to get the new CSS for your app. 

The themes control everything regarding placement, animation, navigation and other visual user interactions. Imagine that you had started creating your app using the Android theme. Then later you realized you also wanted to make your app available to iPhone users. All you would have to do is change the reference of the link tag from Android to iOS. When the app loads, you will see your layouts and interactions change automatically to what an iPhone user would expect. The themes also support right-to-left languages, such as Arabic, Farsi, Urdu and Hebrew. Just add the attribute `dir="rtl"` to the HTML tag. When your app loads, everything will be switched around automatically.


###Why Use It?

There are many mobile Web app frameworks. Lots of them. Even so, we think ChocolateChip-UI offers something no other framework does. It enables you to create mobile Web apps with the look and feel of native apps. Other frameworks talk about having performance of native apps. We have that too. When you use ChocolateChip-UI for creating an app, your users will have no idea it is not native. Your apps will be snappy and responsive. You layouts and widgets will act the way users expect them to. And they will look fantastic. Your competitors will be wondering how you pulled it off. 

####Speed

ChocolateChip-UI achieves optimial performance in a number of ways. First of all, load times. The files for ChocolateChip-UI are just three: jQuery, ChUI.js and a theme. You can load jQuery from a CDN or use a minified version. ChUI.js minified is 42kb. The themes are: Android - 59kb, iOS - 49kb, Windows Phone 8 - 33kb. Another thing, the icons used by widgets on Android and iOS are in the CSS as SVG data urls, and for Windows Phone 8, we use the native icon font--Segoe UI Symbol--just as Microsoft does. This means for the framework you just have three files to load. If you lazy load your media data after the app loads, your app will be up and running quickly.

All animation in ChocolateChip-UI is accomplished through hardware-accelerated CSS. This means your users get to enjoy smooth interactions on today's advanced mobile devices.

Another problem that affects performance is the difference between mouse interaction and touch. ChocolateChip-UI has a gesture module that identifies when the user is on a desktop or mobile browser. ChocolateChip-UI abstracts these into a standard interface that works consistently for desktop, Android, iOS and Windows Phone 8. The following events are available:

- $.eventStart: mousedown/touchstart/MSPointerDown/pointerdown
- $.eventMove: mousemove/touchmove/MSPointerMove/pointermove
- $.eventEnd: mouseup/touchend/MSPointerUP/pointerup
- $.eventCancel: mousecancel/touchcancel/MSPointerCancel/pointercancel

Besides these, it also provides the following events:

- tap
- singletap
- doubletap
- longtap
- swipe
- swipeleft
- swiperight
- swipeup
- swipedown

These events work on desktop and mobile. We recommed the use of the singletap event instead of click or touchstart events. The reason is, if your content needs to be scrollable and contains items that are interactive with mousedown/touchstart/pointerdown events, when the user attempts to scroll, they may immediately trigger unintended interactions. This can make scrolling a page of interactive content very frustating. When you use a singletap event to register your interactions, there is a slight delay of 130 milliseconds. This gives the user enough time to touch interactive content and scroll the page without accidentally triggering an unintended interaction.

ChocolateChip-UI also has its own template engine. In performance tests, it is comparable to underscore or handlebars. You can use it to dynamically populate your views with data either local or through Ajax requests. 

####Full Set of Layouts and Widgets

Using standard HTML5 markup, ChocolateChip-UI provides a wide range of layout options to accomodate your needs. And because the layouts are created with HTML5 tags and CSS, you can easily add a class and define a new style to create the layout variation you need. 

ChocolateChip-UI provides the typical data lists which can be made into navigable lists, lists that display a detail about the chosen item, selection lists, deletable lists, as well as composite lists for complex layouts with columnar data. It also has a module for grid layouts implemented with the CSS3 flexible box model. 

ChocolateChip-UI offers navigation lists, tabbar interfaces, slide-out menus, slide-down sheets, toggle panels, paginated navigation, and carousels. For tablets, you can use the split-layout to create the popular master-detail layout. 

Widgets include buttons, back buttons, segemented controls, modal popups, popovers/spinners, busy indicators, switches, range controls, steppers and search boxes. Form nputs get styled appropriately for each platform. 

With ChocolateChip-UI you have everything you need to create a beautiful interface for your app without sweating the details. We do that for you so you can spend your precious time implementing the data services your app needs. 

ChocolateChip-UI was designed from the start to be resolution independent. The UI will always be rendered at the highest resolution available on the device. Its layouts are also responsive, they will resize automatically with orientation change and will expanding to fill the available screen real estate of all mobile devices. 

####Compatible with Other Libaries and Frameworks

You can use ChocolateChip-UI with a wide variety of other libraries and frameworks. ChocolateChip-UI is built on top of jQuery, so anything that is jQuery compatible should work as is. Before attempting to use any jQuery plugins with ChocolateChip-UI do check the plugin documentation to make sure that it works on mobile. 

There are many development frameworks that you can use to help you create organized and efficient code, such as Backbone, Knockout, Angularjs, Sammyjs, Ember, Somajs, Amplifyjs, Feathers, etc. 
