#ChocolateChip-UI

##Themes

ChocolateChip-UI uses themes as a convenient way to handle the differences between mobile operating systems. The theme defines the look, the layout and the animations that make each operating system unique. Users expect their apps to conform to the conventions of the platform on their devices. The themes try to make this as convincing as possible.

Currently there are three themes: Android, iOS and Windows Phone 8. Possible future additions are Firefox OS and Tizen. 

###How They Work

The themes are defined using the LESS CSS preprocessor. In the source code for ChocolateChip-UI is a folder called themes. Inside you'll find android, ios and win. These three folders contain all the LESS files that create each theme. Every widget and layout type has its own module of LESS. The themes share a few modules, such as grids and mixins. They also share certain standard ways of rendering navbars, articles, toolbars and split-layouts. These enable the themes to fill the mobile screen and act like a single view. 


####Android

General description of Anrdoid theme: flat, mostly right angles.

#####buttons 

Buttons are retangular. They have a lighter border across the top and a darker border on the bottom with a one pixel shadow. Action buttons look exactly like regular buttons.

#####segmented control

These are rectangular with the buttons rendered like regular Android buttons described above.

#####inputs

These are rendered in a unique way. The bottom has a border and on the sides a border ascends just 4 pixels to create an upward facing bracket structure.

#####navigation list

On Android a navigation list does not show indicators the way iOS does. In fact, it is up to the user to tap to see if a list item is navigable or not.

#####back button

Android uses a chevron pointing left (right with right-to-left alphabets). It uses no text, the way iOS does. Instead it just displays the apps icon to the right of the back button. Tapping the app icon will also send the user to the previous article.

By default, ChocolateChip-UI provides a default icon image for back buttons. It's a green droid icon. You can override this like so:


```
.back::after, .backTo::after {
  background-images: transparent url(path/to/android/app/icon.png) !important;
}
```

#####tab bar

On Anrdoid tab bars are display across the top of the app, under the navbar. Also the buttons have text but do not ever show icons. Icons in tab bars is an iOS-specific convention and should be avoided on Android.

#####busy control (activy indicator)

The busy control on Anrdoid is a ring with concentric gradients spinning in opposite directions. The tint can be customized for the color scheme of your app.

#####popover (spinners)

Android has popovers called spinners. These are drop down menus that provide users with controls to interact with the contents of the current screen or to perform some other operations. On iOS popovers have titles, but they never do on Android. On Android, the elements that will display a popover have a special trangular marker. That is how the user knows that the element will reveal a spinner. ChocolateChip-UI allows you to indicate this spinner marker using the class `show-popover`. On other platforms (iOS and Windows Phone 8) this indicator will not be displayed.

#####popup

These are just fancy alert boxes. When one appears, a partially opaque screen mask is paced behind it to prevent the user from interacting with thing behind the popup. 

#####range control (seek bar)

On Android the range control or seek bar has a very distinctive look. The colors are set using the LESS @appTint value. So if you change the tint of your app, this will get updated automatically.

#####switch

On Android switches are flat, rectangular controls. For ChocolateChip-UI, we've eliminated the On/Off labels to make it easier for localization. When the user taps or swipes on them, the switch thumb sides to the opposite state.

#####select box (picker)

ChocolateChip-UI uses the HTML select box with options to replicate the Android spinner. When the user taps a select box, the browser actually displays a standard Android spinner with radio buttons next to each item to show which one is selected. Using a seclect box in this way is a convenient and consistent way for you to provide your users with things like dates, times, values, etc.



####iOS

Starting at vesion 7, iOS has a flatter, more translucent appearance where interface elements are deemphesized over content.

#####buttons

In general, iOS buttons have no borders or background colors, just text. They appear like labels. They usually are colored with the app tint through the LESS @appTint value.

#####segmented control

Segmented buttons are semi-translucent with rounded corners on the ends. The selected button is a solid color which is the app tint.

#####inputs

Inputs are completely transparent, no background or border. The only indication that they exist is their placeholder text.

#####navigation list

Navigation list items has a chevron pointing in the direction the navigation will lead. By default these point to the right, but with right-to-left alphabets these point left.

#####back button

The back button displays a chevron along with the title of the previous article to let the user know where they will return to. Back buttons have no borders or background colors. The color of the text and chevron are control by the LESS @appTint value. For right-to-left alphabets the back button gets render on the opposite side. On Android and Windows Phone 8, the label value is ignored.

#####tab bar

On iOS the tab bar is always on the bottom and its buttons contain icons and text. The color of the icon is controls by the LESS @appTint value.

#####busy control

The busy indicator is an animated SVG , it can be colored as needed to match the app branding.

#####popover

On iOS these are drop down menus with a pointer near were they appeared from. They have a title at the top and contain a list of actionable items that affect the content currently on the screen. The title is not displayed when these are rendered for Android and Windows Phone 8.

#####popup

This is equivalent to the normal modal popup or alert box on iOS.

#####range control

The range control is a normal range input styled to look like the native control. The color of the progress bar is determined by the LESS @appTint value.

#####switch

Switches are rounded rectangles. The thumb is also round. Tapping or swiping causes the thumb to animate to the opposite state. The switch's track changes color as this happens.

#####select box (picker)

Using an HTML select box with options looks like a normal form input. But when the user taps it, the browser pops up from the bottom a native picker control populated with the content of the select box. Selecting from the picker will set the value on the select box. The visible picker also has previous and next buttons for navigating to other form elements on the screen.


####Windows Phone

The Windows 8 and Windows Phone 8 is the most extreme, flat, minimalistic design. You'll either love it or hate. Everything is two dimensional. There's no depth, no drop shadows. There are some z-index-based flipping involved in certain transitions. But these too look like a stiff sheet of something flipping. Even though they are turning on their z-index, they still feel two-dimensional. For the Windows 8 theme Microsoft makes exentsive use of their own SegoeUI Symbol font for system icons. The ChocolateChip-UI theme uses that same font for its icons as well.

#####buttons

Buttons are just a three pixel thick border around the button label, forming a rectangle.

#####segmented control

A segmented control is just a set of rectangular buttons side-by-side.

#####inputs

All inputs have three pixel borders around them like buttons. They do have a white background on the dark theme and placeholder text. 

#####navigation list

Like Android, Windows Phone 8 does not use indicators to show that a list item will navigate elsewhere. However, the Windows 8 theme does use a slightly desaturated version of the text color for navigable items as a subtle way to make them stand out from normal text. The Microsoft approach is to make subtle hints that something is different to encourange the user to explore.

#####back button

The back button is just a circle with an arrow pointing to the left. On right-to-left alphabets this will be on the opposite side of the navbar pointing to the right.

#####tab bar

Like Android, Windows 8 tab bars are display at the top, right below the navbar. Also like Android, the tab bar buttons do not use icons/images ever. They are plain text labels.

#####busy control

For Windows 8 ChocolateChip-UI uses the HTML5 progress bar, which it will render as the native looking Windows progress bar. This can be colored to better match your branding.

#####popover

ChocolateChip-UI's popovers are equivalent to Windows 8 flyouts. They have no title, just the actionable items.

#####popup

Popups are styled to resemble Windows 8 message dialogs. These stretch from edge to edge.

#####range control

The HTML5 range input gets styled automatically as a native slider control. The left colored progress slider can be colored to match your branding.

#####switch

The ChocolateChip-UI switches are styled to resemble the rectangular switch of Windows 8. When the user taps or swipes, the thumb animates to the opposite state.

#####select box (picker)

The HTML select box gets rendered as a Windows 8 drop-down list. It is also equalivalent to a list box;

###Icons

Because of differences between mobile platforms, each theme handles them differently.


####Android

Android uses background images to render icons. ChocolateChip-UI prefers to use SVG for background images, but you could also use pngs. The usual approach is to use a pseudo element, such as `::before` or `::after`: 


```
.myButton::after {
  content: '';
  display: block;
  height: 35px;
  width: 35px;
}
```

This will be the piece you will use to display the icon. Then you need to include a definition for the background image:


```
background-image: url("/images/mySpecialIcon.png");
background-position: 50% 50%;
background-size: 80% 80%;
background-repeat: no-repeat;
```

That's the simplest possible way to style an icon. Depending own how the pseudo-element is positioned, you may need to add some margins to get it in the right place.

If you want your icons to be more flexible, you could use SVG. This is a great choice where you might need to change the color of the icons. SVG images are text-based. You can open them up in a text editor. To change a color, just look for the property `fill="red"`. The value will be it's default color. Change that to whatever you need.

When you use external images for icon, there is a penalty for load time. You could instead convert your pngs into 64 bit data urls. 

```
.myIcon {
   background-image: url(data:image/png; base64, iVBORw0KGgoAAAANSUhEUgAfSkekdL3S3iksdui#SDFweDESSejTF);
}
```

There are websites and programs that will convert pngs into 64 bit format. 

You can use a similar approach for SVG, but instead of converting it to 64 bit, you just render it as a string. To do this you need to open the SVG in a text editor and remove all carriage returns and tags from the markup. 

Here's a typical SVG:


```
<?xml version="1.0" encoding="utf-8"?>
   <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
   <svg version="1.1" id="Layer_2" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 48 48" xml:space="preserve">
   <polygon fill='red' points="35,25 17,11 17,39 "/>
</svg> 
```

To use this as a background image you only need the markup of the svg tag:


```
<svg version="1.1" id="Layer_2" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 48 48" xml:space="preserve">
   <polygon fill='red' points="35,25 17,11 17,39 "/>
</svg> 
```

Notice that we have on the polygon tag a fill attribute of 'red'. We could change this to any hex or rgb/rgba value to change the color of this image. To use this as a data url we need to delete line endings and tabs:


```
<svg version="1.1" id="Layer_2" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 48 48" xml:space="preserve"><polygon fill='red' points="35,25 17,11 17,39 "/></svg> 
```

Using this, we can define an icon background image like this:


```
.myButton::after {
  content: '';
  display: block;
  height: 35px;
  width: 35px;
  background-image: url('data:image/svg+xml;utf8,'<svg version="1.1" id="Layer_2" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 48 48" xml:space="preserve"><polygon fill='red' points="35,25 17,11 17,39 "/></svg>');
  background-position: 50% 50%;
  background-size: 80% 80%;
  background-repeat: no-repeat;
}
```

Because this is SVG, it can scale to any size. And we can easily change the color of the icon by changing the value of the fill attribute.




####iOS

For iOS, you can use the exact same approach as defined for Android. You can also choose to define the icon as an image mask instead of a background image. The advantage this provides is that you can leave your icon image defined with a shade of black, then control the color of the icon by defining the background color on the element that the mask image is defined on.

Unfortunatley, Android support for image masks is unreliable, so using a background image is best. On iOS image masks are a long establish approach for icons and always work reliably.

Using image masks is identical to using background images. You just need to change references to background images to mask images. On iOS you need to include the `-webkit-` prefix:



```
.myButton::after {
  content: '';
  display: block;
  height: 35px;
  width: 35px;
  -webkit-mask-image: url('data:image/svg+xml;utf8,'<svg version="1.1" id="Layer_2" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 48 48" xml:space="preserve"><polygon fill='#000000' points="35,25 17,11 17,39 "/></svg>');
  -webkit-mask-position: 50% 50%;
  -webkit-mask-size: 80% 80%;
  -webkit-mask-repeat: no-repeat;
  background-color: red;
}
```

####Windows Phone 8

On Windows 8 and Windows Phone 8 the preferred way to exihibit an icon is to use a pseudo element with a font glyph. Windows Phone 8 comes with the SegoeUI Symbol font installed, which is used by the operating system to display all the icons that the system uses. You can also access that font to show its icons in the Windows version of your app.


###Colors

All themes have a colors.less file in which variables are defined that set the colors used in the theme. If you open a colors.less file, you find something like this:


```
@appBkColor: #f2f0f0;
@appTint: #81cfeb;
@buttonBorderColor: #cbcbcb;
@buttonTopColor: #e5e5e5;
@buttonTextColor: #000;
@buttonTextHoverColor: #fff;
@buttonBoxShadow: #666;
@buttonHoverColor: lighten(@appTint, 10%);
@backButtonColor: #000;
@backButtonHoverColor: #fff;
@switchBorder: #979797;
@switchBkgnd: #fafafa;
@switchThumbBkgnd: #cecece;
@switchThumbBorder: #919191;
@switchOnTop: lighten(@appTint, 12%);
@switchOnColor: darken(@appTint, 30%);
@navBkgnd: #e2e2e2;
@toolbarBkgnd: @navBkgnd;
@tabbarBkgnd: @navBkgnd;
@navBorderColor: #acacac; 
@listBorderColor: #b6b6b6;
@listTitleColor: #4d4d4d;
```


ChocolateChip-UI uses these LESS variables to define the colors that make up a theme. The first color is for the overall background of your app: @appBkColor. Changing this to black would automatically change the background of the them to black. The next color is @appTint. This is used to define the primary color a theme uses for making things standout. By carefully customizing the color values you can create a color scheme for your app's branding needs.

If you do start customizing the color scheme, make sure you test it for all the layout types and widgets. Otherwise you might miss something that will popup and surprise or annoy you or your users. 

If you make changes to the LESS files, you'll need to rebuild the CSS using either Gulp or Grunt:

```
gulp themes
```

Or:


```
grunt themes
```



##Customizing and Branding

Nobody wants an app that looks generic, like the mobile device's system preferences. You need to distinguish your app from others through careful use of color, icons, etc. Some companies already have in place strict branding guidelines. You can meet your branding needs by customizing the colors.less file as described above. You could also do something interesting with background images on the app itself or on its navbars. If you have special typography needs, please note that mobile devices have a fixed set of fonts. You can of course use the @font-face method to include your own fonts for your app. You could also simply convert a title or section of text into outlines in Illustrator, then export them as SVG. You can put SVG inline in ChocolateChip-UI just like any other markup. And SVG will scale smoothly to whatever size you need. Or you could convert your font into an SVG font.

To customize the colors of the default themes is very easy. You just need to open up the colors.less file for the theme and change a few color values. Then, from the terminal, run `gulp themes` to rebuild the themes. 

###Android

For Anroid, you can choose a primary color, or even a secondary color to change the character of your theme. Be sure to consult someone with an understanding of color theory before getting too wild or you may wind up with something garish. Here are the values you can play with:


```
@primaryColor: #007aff;
//@primaryColor: #2a36b1;

@secondaryColor: @primaryColor;
//@secondaryColor: #e65100;

@navBkgnd: #eaeaea;
//@navBkgnd: @primaryColor;

@navbarTextColor: #2d2d2d;
//@navbarTextColor: contrast(@primaryColor);
```

You can change the primary color to whatever you want. By default the secondary color is also set to the primary color, but you could set it to something else for some contrast. If you want more color in your app, you can cahnge the color of the navbars and toolbars to the primary color by uncommenting the second line. If you do, you'll also need to uncomment the line for navbarTextColor. The `contrast()` function will make sure that the text on title and buttons in navbars and toolbars are readable, regardless of the background color of the navbars/toolbars.

###iOS

Like Android, you can change the primary color for iOS to whatever you want. You can also add a secondary color by uncommenting the second line for the secondary color and changing it to what you want. Like Android, you can also uncomment the second line for navColor, which will color the navbars and toolbars of your app. If you do, also uncomment the second line for barButtonTextColor, this will make sure the text and buttons are standout from the background color in the navbars and toolbars. You can also uncomment any of the other values below to adjust the theme to your needs:



```
@primaryColor: #007aff;
//@primaryColor: #880e4f;

@secondaryColor: @primaryColor;
//@secondaryColor: #0277bd;

@primaryColorHighlight: contrast(@primaryColor);

@primaryColorText: @primaryColor;
//@primaryColorText: @primaryColorHighlight;

@navColor: #f7f7f7;
//@navColor: @primaryColor;

@barButtonTextColor: @primaryColorText;
//@barButtonTextColor: @primaryColorHighlight;

@switchOnColor: #6ee102;
//@switchOnColor: @secondaryColor;

@actionButton: #585d63;
//@actionButton: @secondaryColor;

@listNavIndicator: #c7c7cc;
//@listNavIndicator: @secondaryColor;

@tabbarButton: #929292;
//@tabbarButton: #aaaaaa;

@tabbarButtonSelected: @primaryColor;
//@tabbarButtonSelected: contrast(@navColor);
```



###Windows Phone

The Windows theme works a bit differently than Android or iOS. You can change the primary color to whatever you need. If you want the color to dominate your app, you can uncomment the second line for appBkColor, navBkgnd and toolbarBkgnd. If you will, you might also play witht he color for listBkColor to create some contrast. If you do, make sure that text in the lists are readable. You may need to adjust the following values for your lists: listTitleColor, listSubtitleColor, listDetialColor, listAsideColor.



```
@primaryColor: #007aff;
//@primaryColor: #2a36b1;

@appBkColor: #000;
//@appBkColor: @primaryColor;

@navBkgnd: #000;
//@navBkgnd: @primaryColor;

@toolbarBkgnd: lighten(@appBkColor, 10%);
//@toolbarBkgnd: lighten(@primaryColor, 10%);

@listBkColor: #000;
//@listBkColor: #fff;
```


###Switching Themes

ChocolateChip-UI was designed to allow you to write one app with data and user interactions which you can deploy to iOS, Android and Windows Phone 8. The only difference between these platforms is the CSS file: chui.css. When the user requests your app from the server, you can determine which browser and operating system they are on and provide them the appropriate theme. Because this is done on the server before the page is sent, the user sees only the appropriate theme load.

We present two ways to do user agent sniffing on the server: with PHP and NodeJS. If you have any experience writing back end code for your server, you should be able to figure out how to do this for your platform based on the examples here. Please note that these are very basic tests for the user agent. There are many solid solutions available for server-side user agent sniffing. You just need to find out what is available for your setup.

####PHP


```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <title>Demo</title>
  <script src="/choco/scripts/chocolatechip.js"></script>
  <script src="/choco/scripts/iscroll.js"></script>
  <?php
    $agent = $_SERVER['HTTP_USER_AGENT'];
    $browser = null;
    $agent = $_SERVER['HTTP_USER_AGENT'];
    $browser = null;

    if (preg_match("/android/i", $agent))
      {
       echo '<link rel="stylesheet" href="../c3/chui/chui.android-3.0.css">';
      }
      elseif (preg_match("/Trident/i", $agent) && (preg_match("/MSIE 10/i", $agent) || pre_match("/MSIE 11/i", $agent))
      {
       echo '<link rel="stylesheet" href="../chui/chui.win-3.0.css">';
      }
      elseif (preg_match("/Apple/i", $agent))
      {
       echo '<link rel="stylesheet" href="../chui/chui.ios-3.0.css">';
      }
       echo '<link rel="stylesheet" href="../chui/chui.ios-3.0.css">';          {
     }
  ?>
</head>
```


####NodeJS

The following example uses the Express framework for NodeJS, with the EJS template engine. To get the user agent on the page being rendered is easy with Express. When handling the routing for the page, grab the user agent from the headers object and attach it to the response object being sent to your page. This will make the user agent available to the page during rendering.


```
exports.demo = function(req, res){
  res.render('demo', { title: 'ChocolateChip-UI', userAgent: req.headers['user-agent'] });
};
```

Here we access the user agent string sent to the page in the userAgent variable. Based on whether we detect Android, iOS or Windows Phone, we output the appropriate stylesheet link:


```
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="utf-8">
   <meta name="viewport" content="initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
   <meta name="apple-mobile-web-app-capable" content="yes">
   <meta name="msapplication-tap-highlight" content="no">
   <title>Demo</title>
   <!-- Examine header userAgent to decide which stylesheet to use -->
   <% if (/android/i.test(userAgent) || /chrome/i.test(userAgent)) { %>
   <link rel="stylesheet" href="/chui/chui.android-3.0.css">
   <% } else if (/safari/i.test(userAgent)) { %>
   <link rel="stylesheet" href="/chui/chui.ios-3.0.css">
   <% } else if (/trident/i.test(userAgent)) { %>
   <link rel="stylesheet" href="/chui/chui.win-3.0.css">
   <% } %>
</head>
```

Most servers have frameworks to enable more sophisticated userAgent sniffing.

###OS-Specific Classes

When testing your app on multiple devices, you may find that you need to make some style adjustments for one operating system. ChocolateChip-UI makes these easy by automatically putting the following classes on the body tag when your document loads: isiOS, isAndroid and isWin. This allows you to write OS-specific styles in your app that will only be rendered when your app is on that OS.

`isiOS` will render for both iOS and desktop Safari. `isAndroid` will render for Android and desktop Chrome. `isWin` will render for Windows Phone 8 and IE10-11.

In the demo page we use the following code to style icons with or without border radii, depending on the operating system:


```
/* Border Radius for each OS: */
.isiOS .icon {
   /* rounded corners for iOS */
   border-radius: 10px;
}
.isAndroid .icon {
   /* circular for Android */
   border-radius: 50%;
}
.isWin .icon {
   /* square for Windows Phone 8 */
   border-radius: 0;
}
```

On Android in particular there are many differencec in rendering between the native Android browser and the mobile Chrome browser. If you find and issue with how something is rendering with one of these, you can use a class to target them to address the problem:

```
.isNativeAndroidBrowser .button {
  border: solid 1px red;
}
.isChrome .button {
  border: solid 1px blue;
}
```



