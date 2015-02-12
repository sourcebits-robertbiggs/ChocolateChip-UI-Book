#Making Hybrid Apps for Android, iOS and Windows Phone

##Create a Hybrid iOS App

There are many options for building hybrid apps: Phonegap, Adobe Cordova, Sencha Touch, KendoUI, Appgyver, Ionic, etc. These give you access to the underlying operating system calls or to underlying hardware functionality. However, you do not really need any of these if you have created a typical Web app with ChocolateChip-UI and you want to deploy it as a native app. All you need is the bare minimal code to launch a Web view and load your Web app as local resources.

In this chapter we will see how simple it is to create a WebView wrapper for a Web app that can be deployed to Android, iOS and Windows Phone. To make this easier, we're starting with the Web app, Vino, that we built in the previous chapter. You can get the app [resources here on Github](https://github.com/chocolatechipui/Vino). 

Although Apple recently annouced the availability of `WKWebView`, it is not fully flushed out, as it suffers certain limitations when trying to load local resources, etc., and it is only available on iOS8. In general UIWebViews have a bad rap for being slower than the default browser engine powering mobile Safari. However, in the case of a Web app created with ChocolateChip-UI, this is not really a problem. That is because ChocolateChip-UI does not depend on the power and speed of JavaScript to animate the interface. All the movement, animations and transitions are executed with hardware acceleration.
One small thing, because this is for iOS, we are going to use Xcode. This means you do need to be on a Mac to make this work. If you are on Windows and you have access to Xamarin, you could probably create a similar solution with it and use their cloud testing for iOS device support, however that is beyond the scope of this article.
The images for this article are from Xcode 6.1.1. You can download this project from Github.

##A View is a View

To make this happen, we need a view control into which we will load a UIWebView or chromeless browser. To do this, open up Xcode and create a new project.
From the File menu choose: `New -> Project…`. This will open the new project panel. Make sure you are on the iOS section in the left side panel and are on Application. On the right you should see the icons for Master-Detail, Page-Based Application and Single View Application. Choose Single View Application and click the Next button.


![](images/hybrid-apps/ios/Single-View-App.png)

This will take you to a screen where you can provide a name for your new project. We’re going to name ours “Vino”. Our company name is Sourcebits, so we’ll put that as the organization name. Make sure that for Language you have Swift selected. Finally, for Devices, by default it shows for iPhone. This is really your choice. If you only want to target the iPhone, leave it at that, or perhaps you want to create a tablet app, in which case you can choose iPad. Or you might want to target both iPhone and iPad, in which case you would choose Universal. Because ChocolateChip-UI is responsive, it will scale to fit any screen size, so the call for Device is your choice. For this example we’re going to leave it on iPhone and continue.



![](images/hybrid-apps/ios/Name-the-project.png)

After naming your project, you will be prompted to save it somewhere. Save it to wherever makes sense for you. After saving your project, it will open in Xcode revealing all the default files, code, etc.



![](images/hybrid-apps/ios/project screen.png)

On the left you should see a file exporer that shows the folder structure and files that comprise your app. If it is not visible, click on the Folder icon in the top left of the project window. Since our project is named Vino, you should see a folder named Vino, folled by Vino Tests and Products. The Vino folder has the following files:

- AppDelegate.swift
- ViewController.swift
- Main.storyboard
- Images.xcassets
- LaunchScreen.xib
- Supporting Files

We’ll only be working with the `ViewController.swift`, `Main.storyboard` and `Images.xcassets`. We won’t actually need the `LaunchScreen.xib` since we will provide a graphic launch screen in `Images.xcassets`, so that file can be deleted.

##Add Web App to Project

We now can add any existing, standalone Web app to our Xcode project. You literally do this by dragging the folder with your Web app on to the top left of your project in the File viewer.



![](images/hybrid-apps/ios/Add Website.png)

As soon as your let go of the Web app folder over the project, a dialog will pop up. The defaults are fine. You want the folder and its contents copied to your project. Just hit “Finished”.



![](images/hybrid-apps/ios/Website added.png)

You’ll now see that the folder has been added, along with all it’s content, at the root level of your project. This is good. You’ll be able to access this with some code later on.



![](images/hybrid-apps/ios/Add Webapp Popup.png)

##Add a UIWebView

Select Main.storyboard in the left hand file viewer. You should see View Controller Scene. If it is not open, open it so that you can see its contents. It will have View Controller, First Responder, and Exit. We are only concerned with View Controller. Open it. In the bottom you will see an icon for View. Select this and your view will be highlighted in the main screen area.



![](images/hybrid-apps/ios/SelectMainStoryboard-1.png)

This is the view into which we want to load a UIWebView. To do so, we need to first add it to the View structure. In the far right column on the bottom, make sure you have selected the Show the Object library (third round icon from the left).



![](images/hybrid-apps/ios/SelectMainStoryboard-icon-selected-2.png)

This will display a number of default object types avialable in Xcode for use in apps. We want the Web View, so scroll down until you see it. Drag it onto the highlighted area of your view. You will see a UIWebView object appear. Let go so that it is dropped inside the View space. You should now see the Web View object inside the View in the View Controll Scene to the left of the view.



![](images/hybrid-apps/ios/Find WebView.png)

We need to do one last thing to set the correct dimensions for the Web View. With the Main.storyboard selected, and the Web View selected in the View Controller Scene column, look over at the top right of Xcode. You should see these controls. If not, make sure the right column is showing by selecting the right column icon in the top right of the Xcode window (see image) and that the Size Inspector icon is selected in the right column (see image). Set your values to x: 0, y: 20, width: 600, height: 580. The default measurements would be 600 x 600, but because we want our app to site below the iOS status bar, we set the top as 20 pints, and therefore subtract 20 from the total height. Points are resolution independent measurements that iOS uses when drawing to the screen.



![](images/hybrid-apps/ios/constraint-icons.png)

##Creating an IBOutlet for the Web View

In Cocoa, the underlying framework for iOS, an `IBOutlet` is the means by which we get a reference to an instance of an interface object. We therefore need to first add a code snippet to the ViewController.swift file. Select it. Before we can use the WebView, we need to tell Xcode to add the WebKit framework to our project when it compiles it. Otherwise, trying to access a Web View when the support for it is not present would generate and error:
Update the ViewController.swift file like this by adding import WebKit after the import UIKit line:

```
import UIKit
import WebKit
```

Then add the following line of code as the first line of the ViewController class:

```
 class ViewController: UIViewController {

  @IBOutlet var webView: UIWebView!

  override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view, typically from a nib.
  }

  override func didReceiveMemoryWarning() {
    super.didReceiveMemoryWarning()
    // Dispose of any resources that can be recreated.
  }
}
```

The value webView which follows the keyword var is the name by which we will refer to our Web View when we want to load an html file in it.

Now go back and select the `Main.storyboard` file in the file viewer on the left. Then select the View Controller in the View Controller Scene column. Notice that three icons appear above the view controller in the center area. The first should alread be selected, otherwise select it now. Then, hold down the control key and drag the selected, round icon above the view controll down onto the highlighted UIWebView area. When you let go, you should see a small menu pop up with two options, view and webView. Choose webView. This refers to the line where we named the reference for our WebView in `ViewController.swift`. The Web View is now completely hooked up with the IBOutlet code in the ViewController.



![](images/hybrid-apps/ios/CreateIBOutlet.png)

##Loading HTML in the Web View

If we ran our project, it would open in the simulator. But because there is no code to load anything into the Web View, you would just see a blank screen. We need to add some code to the `ViewController.swift` file to open our Web app in the Web view.

We need to define the path to our Web app. In this case, because our Web app is in the directory Website, we need to indicate that we want to load the file index.html in that folder, then we need to create an URL object and a request object which we will pass to our Web view to load.Using the pathForResource method enables us to tell the Webview that the file we want to load is local.
Please note, if you named the folder of your Web app something other than Website as I have, you will need to provide that name to inDirectory for Xcode to know where to look for the local file. Similarly, if you named your app file something other than “index.html”, you’ll need to use that as the first value passed to pathForResource. Xcode doesn’t care what you name the Web app folder or the main HTML file. You just need to provide it with the names you’ve chosen so that it can find them. I’ve used Website and index.html because they are generic and easy to reuse in other projects.

```
override func viewDidLoad() {
  super.viewDidLoad()

  // Define location of local HTML file:
  let html = NSBundle.mainBundle().pathForResource("index.html", ofType: "", inDirectory: "Website")

  // Create an URL object with the above path:
  let url = NSURL.fileURLWithPath(html!) // The html cannot be nil.

  // Create a request object with the url object:
  let request = NSURLRequest(URL: url!) // The URL cannot be nil too.

  // Tell the Web view to load the url request object:
  self.webView.loadRequest(request)
}
```

Our ViewController.swift file should now look like this:

```
import UIKit
import WebKit

class ViewController: UIViewController {

  @IBOutlet var webView: UIWebView!

  override func viewDidLoad() {
    super.viewDidLoad()

    // Define location of local HTML file:
    let html = NSBundle.mainBundle().pathForResource("index.html", ofType: "", inDirectory: "Website")

    // Create an URL object with the above path:
    let url = NSURL.fileURLWithPath(html!)

    // Create a request object with the url object:
    let request = NSURLRequest(URL: url!)

    // Tell the Web view to load the url request object:
    self.webView.loadRequest(request)
  }
}
```

With this, we can now build and run our hybird app either in the simulator on on a device and see our local Web app load. When you run the app, you might notice that the Web View doesn’t quite fit the device screen properly. That’s because we just tossed the Web View into the view without any regard for Cocoa’s builtin layout features. See how our app is too wide for the screen?



![](images/hybrid-apps/ios/app-without-constraints.png)

##Auto Layout Constraints

Xcode has a feature called auto layout that can make our Web View fit any screen, filling all available space. To do that, we need to go back and select the main.storyboard in Xcode’s File Viewer, then select the Web View in the View Controller Scene column. This will make the Web View highlighted light blue. Below the highlight Web View, on the lower right of Xcode’s windows, you will see a set of four icons:



![](images/hybrid-apps/ios/Show-constraints-in-scene.png)

If you click on the second icon from the left, a popup will appear. It will look like this:



![](images/hybrid-apps/ios/Constraints-popup-2.png)

Notice that at the top there is a small square with four dotted red lines leading to four text boxes. You need to click on each dotted red line around that square. Doing so will make it solid red. When done, look like this:



![](images/hybrid-apps/ios/selected contrainsts-red.png)

You should see a button is now available at the bottom of this constraints popup that says “Add Constraints”. Click this. After this you will notice a new item appears in the View Controller Scene Column. Right beneath the Web View, there is an item call Constraints. Click on it to open it. There you will see the four constraints on the Web View that you just created:



![](images/hybrid-apps/ios/Select a constraint in scene.png)

However, these are not quite right yet, probably the default values are not going to be right. To make sure that the Web Views constraints work as we want them to, select the Web View, then go back down to the set of icons on the lower right. But this time click on the third icon from the left (image). A menu will pop up in which you should see a choice that says “Reset to Suggested Constraints”. Choose that.



![](images/hybrid-apps/ios/reset-constrainst-2.png)

This will set your restraints so that you Web View now always fills the screen in portrait on landscape and on any device screen size.
Having done this, you should now rebuild your app in Xcode. When it launches in the simulator you should now have a hybrid app that knows where the bound of your device’s screen are and which can readjust itself when the device rotates. Congratulations!



![](images/hybrid-apps/ios/Correct Constrains in App.png)

If you want to deploy to an actual iPhone you’ll need to have an Apple developer account and set your phone up for provisioning. You can find out more on how to do this on the Apple developer site.



##Creating an Hybrid Android App

Previously we looked at how to make a hybrid app for iOS using Xcode. Now we’ll see how to do the same thing for Android. We’re targeting Android 4.x. For this we are going to use Google’s new developer IDE, Android Studio. After you’d downloaded and installed Android Studio, you are almost ready to go. To use Android Studio, you will need to have the Android SDK install. The first time you open it, it should offer you the ability to download the SDK, otherwise you may need to do that separately. You can get the SDK here.

Once you have both Android Studio and the Android SDK installed, you can open Android Studio. When you open Android Studio, you will be presented with the following dialog:



![](images/hybrid-apps/android/OpenAndroid-Studio.png)

You want to select: “Start a new Android Studio project”. After selecting that, you will be presented with another screen where you name your project. A standard Android project expects a URL based domain identify, just like Xcode did. And you need to indicate where you want the project to reside:



![](images/hybrid-apps/android/name-the-project.png)

After hitting “Next”, you will be presented with another screen. Select Phone and Table and the minimum SDK that you wish to support.



![](images/hybrid-apps/android/choose-form-factor.png)

For the purposes of this tutorial, this is fine, hit “Next”. This will present us with a screen from which we can choose activity type for our app. By default, “Blank Activity” is selected, and that is what we want.



![](images/hybrid-apps/android/choose-app-type.png)

Hitting “Next” will take you to the following screen. These are default names for the files that Android Studio is about to create. Accept these defaults and hit “Finish”.



![](images/hybrid-apps/android/options-for-new-file.png)

As Android Studio sets up your project, it will present you with the following dialog. Choose “Allow”:



![](images/hybrid-apps/android/Incoming-network-permissions.png)

Now you should find yourself looking at your new Android project:



![](images/hybrid-apps/android/new-project.png)

Now we need to open open the project so that we have the entire project visible. By default when Android Studio shows a project, it show it in simplified Android structure. We, however, need to see the whole project. We get to the whole project by clicking in the upper right dropdown and selecting the first option, which is “Project”:



![](images/hybrid-apps/android/select-project-from-menu.png)

Now you’ll see all the files and folders in your project in the file column:



![](images/hybrid-apps/android/project-files.png)

In the project file column we need to select the `AndroidManifest.xml` file, which is in `Vino/app/src/main`. See the image above for details. After selecting it, usually with a double click, it will appear in the area to the right of the column:



![](images/hybrid-apps/android/Android-Manifest.png)

Right after the closing `</application>` tag we need to add the following:

```
<uses-permission android:name="android.permission.INTERNET" />
```

This will allow our hybrid app to access the Internet. Save the file.
Adding a WebView
Next we want to open the MainActivity Java file. This is were we will put the Java code to create a WebView. You’ll find it at `app/src/java/your-app-name`. In this case, since we named our app “Vino”, this will be: `app/src/java/com`.sourcebits.vino:



![](images/hybrid-apps/android/MainActivity.png)

Looking at the image above, you can see there code to create a menu and menu items. We do not need this. We don’t want an Android action bar across the top. We want our app to be full screen. We can therefore delete the code for the following two functions: `onCreateOptionsMenu` & `onOptionsItemSelected`.
Next we need to make a change the the MainActivity method. At the moment it extends `ActionBarActivity`, we need to change that to Activity. Next, expand the “import” section at the top of the `MainActivity` file and add this:

```
import android.view.Window;
import android.view.WindowManager;
import android.webkit.*;
```

Your file should not look like this:



![](images/hybrid-apps/android/MainActivity-complete.png)

We now need to add some code to the `onCreate` method. We need to tell Android that we want a full screen app:

```
/* Make the app full screen*/
requestWindowFeature(Window.FEATURE_NO_TITLE);
getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN, WindowManager.LayoutParams.FLAG_FULLSCREEN);
Then we need to create an instance of a WebView:
/* Find WebView element by its id*/
WebView webView = (WebView) findViewById(R.id.webView);
webView.setWebChromeClient(new WebChromeClient());
Next we need to create a Settings object for our WebView:
/* Create new settings for our WebView element */
WebSettings webSettings = webView.getSettings();
Using the Settings object, we can define other settings for our WebView, such as allowing the execution of JavaScript, localStorage and a url to load:
/* Enable JavaScript */
webSettings.setJavaScriptEnabled(true);

/* Enable localStorage */
webSettings.setDomStorageEnabled(true);

/* Set scrollbars to be inside the app */
webView.setScrollBarStyle(View.SCROLLBARS_INSIDE_OVERLAY);

/* Define path to your local Web assets */
/* Using the name "android_asset" means that
the system will look for a file named "assets"
at app/src/main, the same folder this file is in. */
webView.loadUrl("file:///android_asset/index.html");
```

Here is what our MainActivity file should look like:



![](images/hybrid-apps/android/Completed-MainActivity.png)

Go over to the Project column and select the folder main at `app/src/main`. Then right click and choose “New”. That last line is really important: webView.loadUrl. This tells our app where to look for the local HTML file to load. The protocol file indicates it’s in our app bundle. We named the directory `android_asset`. This is a default name that Android uses. It resolves this to a folder named assets.



![](images/hybrid-apps/android/new-folder-for-assets.png)

When a dialog pops up asking for a name, name it “assets”. You’ll now see the assets folder as the first item in `app/src/main`. This “assets” folder will hold the Web app. Just copy all the files from you Web app, HTML, CSS, JavaScript and images, into this folder. For this example I’m going to copy the contents of the Android theme of the Vino app into this folder.



![](images/hybrid-apps/android/activity_main.png)

After selecting the `activity_main.xml` file you will see this:



![](images/hybrid-apps/android/Layout-main.png)

You need to select the “Text” tab to see the xml for this file. You will then see this:

```
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
android:paddingRight="@dimen/activity_horizontal_margin"
android:paddingTop="@dimen/activity_vertical_margin"
android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">

<TextView android:text="@string/hello_world" android:layout_width="wrap_content"
    android:layout_height="wrap_content" />

</RelativeLayout>
```

You do not need the TextView tag, so delete it. Instead we need to change this file to match this:

```
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
  xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
  android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
  android:paddingRight="@dimen/activity_horizontal_margin"
  android:paddingTop="@dimen/activity_vertical_margin"
  android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">

  <WebView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/webView"
    android:layout_gravity="center_vertical" />

</RelativeLayout>
```

And finally we need to open the file `app/src/main/res/dimens.xml` file. This sets the padding arround an app. In this case we do not want any padding, so we need to set all its values from 16dp to 0dp:

```
<resources>
    <!-- Default screen margins, per the Android Design guidelines. -->
    <dimen name="activity_horizontal_margin">16dp</dimen>
    <dimen name="activity_vertical_margin">16dp</dimen>
</resources>
Change this to:
<resources>
    <!-- Default screen margins, per the Android Design guidelines. -->
    <dimen name="activity_horizontal_margin">0dp</dimen>
    <dimen name="activity_vertical_margin">0dp</dimen>
</resources>
```

With that, you’re ready to build. Hit Android Studio’s green Run button. You get the following dialog:



![](images/hybrid-apps/android/choose-device.png)

This dialog alows you to deploy to a tether Android device, if you have it set up for development. Otherwise, the default will load in a simulator. Hit OK and you should see this:



![](images/hybrid-apps/android/Hybrid App Running.png)

And that’s it. You’ve created a hybrid app with ChocolateChip-UI, or any other Web app you’ve come up with.



##Creating a Windows Phone App


In this post we will look at how to create a simple hybrid app with a Web view for Windows Phone 8. All you need is [Visual Studio Community Edition](http://www.visualstudio.com/products/visual-studio-community-vs). This is a powerful and free version of Visual Studio. You will need to download this version to follow this tutorial.

Although Apple generally gets the most praise for the power and features that its mobile Safari browser offer for the development of HTML5 Web and hybrid apps, Microsoft has taken this a step further by building support for Web apps right into the SDK for Windows Phone 8. That means, when you open up Visual Studio Community Edition you will have the choice of creating an HTML5 app for Windows Phone 8.



![](images/hybrid-apps/wp8/New Project Menu.png)


This will present you with the following choices:



![](images/hybrid-apps/wp8/New Project.png)

In the above screen, you want to choose Visual C#/Store WebView App (Windows Phone)

In this project, in order to convert this into our HTML5 app, we are only concerned with the contents of the HTML folder, which is show below in a red rectangle:




![](images/hybrid-apps/wp8/Project.png)

Open the folder and you’ll see and index.html file and a folder of CSS:




![](images/hybrid-apps/wp8/HTML Assets.png)

You can delete these. Then go to your Web app, select the contents and drag them to this empty HTML folder in your Visual Studio project. Visual Studio will import them into your project for you:



![](images/hybrid-apps/wp8/Web App in place.png)

And really, that’s it. You can now run the project using the built in simulator and you will see your Windows Phone 8 hybrid app up and running:




![](images/hybrid-apps/wp8/Run App.png)

After clicking run, we get our hybrid app running in the Windows Phone simulator:



![](images/hybrid-apps/wp8/Running App.png)

You can also click on the Run Button and get a dropdown menu to choose what device to emulate. Or, if you have a Windows phone tethered to your computer, you can deploy directly to it from this menu. After this you just need to provide icons and a start up screen for your app.
