title: Trying to Run React Native Android on Windows
date: 2015-09-15 14:20:00
tags: []
---

React Native for Android was released yesterday. As soon as I spotted the release I wanted to give it a whirl on my work PC. I did the npm install and got started right away, only to find that the init command was failing. I expected this because officially React Native isn't supported on windows but there is this master tracking issue for this in Github =>  https://github.com/facebook/react-native/issues/2693 ... anyway.. so I got an error:  

    events.js:141      throw er; // Unhandled 'error' event      ^Error: spawn npm ENOENT    at exports._errnoException (util.js:837:11)    at Process.ChildProcess._handle.onexit (internal/child_process.js:178:32)    at onErrorNT (internal/child_process.js:344:16)    at doNTCallback2 (node.js:429:9)    at process._tickCallback (node.js:343:17)    at Function.Module.runMain (module.js:477:11)    at startup (node.js:117:18)    at node.js:951:3

Not being one to give up right away I wanted to fix it. I noticed from the error message that the spawn of npm using ('child_process').spawn was causing the error. Luckily, npm contains several modules that can be a drop-in replacement for this and I just grabbed the module [cross-spawn](https://www.npmjs.com/package/cross-spawn). I needed to fix this inside the react-native-cli/index.js file.  

This fixed this first error. Next, I encountered an error where the script could not find my Android SDK. This is pretty much expected because on my machine I'm running Android Studio so it is the only place that knows about the SDK.  The quick fix here is to find the path to the Android SDK and add that to a new environment variable ANDROID_HOME.  I also needed to make sure that my JAVA_HOME environment variable was correct so that the build process would be able to find it.  

Awesome, now I'm nearly there. I started my JS Server to watch for changes, I'm using Console 2 and Git Bash here  

    $ react-native start

Then in another console tab I run:  

    $ react-native run-android

Result:  
It works, but not really.  

<div class="separator" style="clear: both; text-align: center;">[![](http://3.bp.blogspot.com/-DVqOO3REEQY/Vfhf8E5nZxI/AAAAAAAASZc/EdEpHiH3OXs/s320/Screenshot_20150915-140937.png)](http://3.bp.blogspot.com/-DVqOO3REEQY/Vfhf8E5nZxI/AAAAAAAASZc/EdEpHiH3OXs/s1600/Screenshot_20150915-140937.png)</div>

<div>  
First it seems to think that I'm not running my JS server, but then it kicks in to fetch the file changes. </div>

<div class="separator" style="clear: both; text-align: center;">[![](http://3.bp.blogspot.com/-0Kk23HNkt3A/VfhgAcdfCuI/AAAAAAAASZk/ctaW5qagi00/s320/Screenshot_20150915-140945.png)](http://3.bp.blogspot.com/-0Kk23HNkt3A/VfhgAcdfCuI/AAAAAAAASZk/ctaW5qagi00/s1600/Screenshot_20150915-140945.png)</div>

<div>This gets my hopes up. but then goes back to the red screen of fail.  Why did it do this?!!?  Well it seems that there may be an underlying issue with the way that the packager works on Windows based on the error that the packager spits out. I'm guessing that there is a simple thing to get this to work, but for now I'm moving back to OSX to do React Native.  

</div>