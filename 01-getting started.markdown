#ChocolateChip-UI

##Getting Started

###Setup

ChocolateChip-UI is a Web app framework. As such it needs to run in a mobile browser. ChocolateChip-UI is an open source project available on Github: [https://github.com/sourcebitsllc/chocolatechip-ui](https://github.com/sourcebitsllc/chocolatechip-ui). You can download it in two ways: download a zip or clone it to your desktop. Cloning the repository gives you a duplicate of the original repository that you can work with. If you clone it, you can send us a pull request if you find a bug and fix it, improve a feature, or add a new one.

If you do not want to have to build ChocolateChip-UI from source code, you can just open the folder `dist`. This contains pre-compiled versions of the JavaScript and themes you need to make an app with ChocolateChip-UI. Otherwise, please continue ahead to learn how to build ChocolateChip-UI from scratch.

ChocolateChip-UI uses Node modules to enable building the framework. ChocolateChip-UI has no dependency on Nodejs for running, so you do not need the modules in your project.

####Installing Nodejs

First go to the [Node](http://nodejs.org) site. You'll see an install button on the page. This will install the correct version of Node if you are on a Mac or Windows PC. Please note that if you are on a Windows 8 device, this will not work with WinRT. Furthermore, for Windows 8, you must use the browser on the desktop, not in the Modern UI Start Screen.

After Node is finished installing, you need to install the modules that Node will need to build the framework. Using the terminal, change to the folder where ChocolateChip-UI is. You can do that by typing <code>cd</code> into the terminal, leaving a space after it, then dragging the folder to the terminal. When you see the path to the folder appear after the cd command, hit return. You will now be inside that folder. That is where you need to be to run the install scripts.

On the Mac, in same terminal window where you used the cd command to get to your ChocolateChip-UI folder, run the following command:

```
sudo npm install
```

On Windows, open the command prompt and run this:

```
npm install
```

When you hit Return (Mac), the terminal will ask you for your user password. Enter it and hit return. On Windows, enter the command and hit Enter. In both cases you will see a list of messages output to the terminal. This install reads ChocolateChip-UI's package.json file to see what version of modules the framework needs. These  dependencies are put in a folder labeled "node_modules". If there are no errors, you're ready to setup a task manager for building ChocolateChip-UI.

ChocolateChip-UI uses [Gulpjs](http://gulpjs.com) as its task manager. If you are familiar with Gruntjs, this is very similar. Although Grunt has been around longer and more developers are familiar with it, we like Gulp because of its fast build times and minimalistic code. Although we used to support Gruntjs, since Gulp was easier to maintain and faster, we decided to drop support for Gruntjs.

####Install and Run Gulp

To use Gulp, you need to first install it globally so that it is available from the terminal/command prompt. On the Mac, run this:

```
sudo npm install -g gulp
```

On windows, run this:

```
npm install -g gulp
```


Once this install is done, you're ready to build ChocolateChip-UI. The simplest way to build ChocolateChip-UI is to just runt `gulp` in the terminal. That will output the framework along with examples and demos for Android, iOS and Windows Phone 8. The Android examples can be tested in the Chrome desktop browser, the iOS examples, in the Safari desktop browser, and the Windows Phone 8 examples in IE 10 or 11.

After running the Gulp script, you will find the following new folders in your ChocolateChip-UI folder:

- chui/
- data/
- demo/
- examples-android/
- examples-ios/
- examples-win/
- images/
- node_modules/


If you're starting a new project, you only need the files in the **chui** folder. To create a custom build with just those files, you can run any of these tasks:


``` 
// Build only ChUI.js and the Android theme:
gulp js android

```

```
// Build only ChUI.js and the iOS theme:
gulp js ios
```

```
// Build only ChUI.js and the Windows Phone 8 theme:
gulp js win
```


Or if you want all three themes with ChUI.js:


```
gulp chui
```

Running these commands will give you the normal and mininfied version of the framework. While in development, you probably want to use the unminified version so you can troubleshoot errors. Please do use the minified versions in production.

You can also create a platform-specific version with examples. Run one of the following commands:


```
// Build only ChUI.js and the Android examples:
gulp android_examples
```

```
// Build only ChUI.js and the iOS examples:
gulp ios_examples
```

```
// Build only ChUI.js and the Windows Phone 8 examples:
gulp win_examples
```

ChocolateChip-UI supports right-to-left alphabets, such as Arabic, Farsi, Urdu and Hebrew. To enable it all you have to do is put the property `dir="rtl"` in the document's html tag. ChocolateChip-UI will automatically render your documents for correct right-to-left interfaces. In the source code there are examples of right-to-left layouts and widgets using Arabic. You pass Gulp a flag `--dir rtl` so that it builds out these examples. Run this in your terminal:

```
gulp --dir rtl
```

This will produce the following folders:

- chui
- data
- images
- rtl-demo
- rtl-examples-android
- rtl-examples-ios
- rtl-examples-win
- rtl-images

You can also use the rtl flag on any of the OS-specific builds:


```
gulp android_examples --dir rtl
```



```
gulp ios_examples --dir rtl
```



```
gulp win_examples --dir rtl
```



###Watching Changes

If you want to customize a theme for branding purposes, you can ask Gulp to watch the files for changes. As soon as you save a change to the LESS files, Gulp will rebuild the theme. Similarly, if you make a change to the source files for ChUI.js and save, Gulp will rebuild the ChUI.js file for you as well. The easiest way to enable this is to just run the Gulp watch task:


```
gulp watch
```

If you want, you can run the build and watch scripts at the same time. These will watch only the designated theme source files:


```
gulp js android && gulp watch_android
```

```
gulp js ios && gulp watch_ios
```

```
gulp js win && gulp watch_win
```

###Building with ChocolateChipJS

ChocolateChip-UI is built to run on jQuery, the popular and well-know JavaScript library. However, ChocolateChip-UI can optionally run on ChocolateChipJS. This is a mobile-first JavaScript library written to be small and fast, as much as 30% faster than jQuery. If you find that with jQuery your app's interactions feel sluggish, you might want to try using ChocolateChipJS. It's easy to build out examples of ChocolateChip-UI using ChocolateChipJS. Just pass Gulp the flag: --chocolatechipjs:


```
// Build everything, including examples with ChocolateChipJS instead of jQuery:
gulp --chocolatechipjs

// Build only the Android examples and demos with ChocolateChipJS instead of jQuery:
gulp android_examples --chocolatechipjs

// Build only the iOS examples and demos with ChocolateChipJS instead of jQuery:
gulp ios_examples --chocolatechipjs

// Build only the Windows examples and demos with ChocolateChipJS instead of jQuery:
gulp win_examples --chocolatechipjs

// Build the right-to-left version with ChocolateChipJS:
gulp --dir rtl --chocolatechipjs

// Build only the Android right-to-left examples with ChocolateChipJS:
gulp android_examples --dir rtl

// Build only the iOS right-to-left examples with ChocolateChipJS:
gulp ios_examples --dir rtl

// Build only the Windows right-to-left examples with ChocolateChipJS:
gulp win_examples --dir rtl
```