IMPORTANT
==========
For any question on how to build PLEASE join the development group Also, if you build CSipSimple, you should first read the Licensing wiki page<br>

Introduction
==========
The application has two distinct parts : * The pjsip dynamic library part that produce a .so (to be more precise a .so by android target). * The java application that produce a .apk (and can be installed on your android device).<br><br>
If you are a java developer and you are not interested in building the sip stack, there is an alternate - not easy to maintain - way to build by getting auto-generated swig java class and the dynamic library from nightly build website. Please search the dev google group to get info about this method that is not recommended.<br><br>
Some very important note : CSipSimple is released under GPL license. Be really aware about what does that mean before developing. Read Licensing wiki page.<br><br>
So if you plan to improve CSipSimple and redistribute it, you MUST release your source code. As we (CSipSimple developers) are open to any contribution, I'd advise you to directly contribute the CSipSimple project. All contributions are welcome !!!<br><br>

System and Software Requirements
==========
For Linux
_________
You'll need following package **you may need to adapt depending to your distribution packages names** `subversion git quilt unzip wget swig2.0 python make yasm` You also have to install the android NDK and the android SDK. I do not details here how to install android NDK and SDK. Please read Android documentations about that : * Android NDK doc * Android SDK doc To install correctly the android NDK, please add the NDK to your path. I usually recommend to add these lines to your .bashrc : `export ANDROID_NDK=/_path_to/android-ndk-linux/ export ANDROID_SDK=/_path_to/android-sdk-linux/ export PATH=$PATH:$ANDROID_SDK/tools:$ANDROID_SDK/platform-tools:$ANDROID_NDK` Anyway the naming of ANDROID_NDK export is important in the build of one part of the application. You can also do that only at build time, but if you use to develop for android, it's a good idea to do that.

Don't forget to install SDK and NDK dependencies as well. For example, on ubuntu 64bits system you will need ia32-libs for both NDK and SDK.


For MacOSX
==========
Thx to Benjamin and Magnus for the contrib on these instructions
_______________________________________________________________

For main instructions it's almost the same as for Linux.<br>
Packages are a little bit more complicated to get however. The two more difficult to get are quilt and swig2.0.<br>
**DUE TO A BUG IN MACPORT with quilt 0.6 -- DON'T USE THE FOLLOWING.**
To install quilt you should install MacPorts first (http://www.macports.org). Installation instructions<br>

If you work behind a proxy follow these instructions additionally MacPorts behind proxy<br>
And then install quilt using : sudo port selfupdate sudo port install quilt<br>
### Instead build it from source

Download quilt from latest source available here : http://download.savannah.gnu.org/releases/quilt/ And then execute          `./configure # optionally add prefix here to be able to install in your env make make install # optionally do that in sudo if target is read only for the current user`  <br>
To install swig 2.0 : download from source available here : http://www.swig.org/download.html And then execute  `./configure # optionally add prefix here to be able to install in your env make make install # optionally do that in sudo if target is read only for the current user`  Finally, since csipsimple make file has some crappy hard coded link to swig tool for now, you have to add a symbolic link to allow your system to recognize swig2.0 to be the same than swig. The faster way is probably to make a symlink:  `sudo ln -s <path to swig> /opt/local/bin/swig2.0` <br>
Other packages (subversion git unzip wget python make) should be easier to install but are also required on Mac.<br>
Continue instructions of Linux build. It should go the same way once packages are installed.<br>


For Windows
==========
I don't not explain how to build for Windows because if you really want to develop you probably don't and shouldn't use Windows. However it should be possible to build it for Windows too. To do so, you must understand Makefile syntax and have a look on commands to run to build the project. It's most about checkouting other projects using git, getting zip files from elsewhere, applying patches, and generating a swig interface. Maybe using cygwin it's easier.

So, in this case the easiest way is to use the virtual machine as described in following chapter.<br>


With a virtual machine
====================
If you want to have an environment with everything required for CSipSimple you can use a Virtual Machine. Thanks to Dennis work, there is one ready for direct use :

CSipSimple-CompilerVM.vmdk

You can get root privilege with sudo command. The password is equals to the username, which should be "user".

Using, this machine, you should still read the following step for your complete understand of how things work, but don't need to run the commands since everything was already done. The step about how to update may be necessary to read if you want to be able to update to latest source code.<br>

## Finalize your SDK installation
Launch android sdk manager to download and install the SDK of versions listed as target in files : CSipSimple project.properties and ABS project.properties.<br>

## Checkout source code


We assume here you are in your folder where you put dev with a shell.<br>
Start with a checkout of source code  <br>

## Checkout CSipSimple incl. all svn dependancies
Checkout CSipSimple incl. all svn dependancies
svn checkout http://csipsimple.googlecode.com/svn/trunk/ CSipSimple-trunk  `It will create a folder CSipSimple-trunk with the following folder structure containing all source code and SVN dependencies: * ActionBarSherlock: is a backward compatible way to use Fragments and Action Bar pattern of android newer versions * CSipSimple: The CSipSimple-App source code * CSipSimpleBranded * CSipSimpleCodecG729 * CSipSimpleCodecPack * CSipSimpleVideoPlugin`


## Build native library
Go into source folder `cd CSipSimple-trunk/CSipSimple`
**Launch make to build the native part of the library and dependancies `make` If needed build video support libary (Optional) `make VideoLibs` If needed build CodecPackLibs (Optional) ` make CodecPackLibs` If needed build CodecG729 (Optional) `make CodecG729` Note that you can use make -j4 to make things go faster if your computer allow that**


If you get a problem at this point, it could be because there is a missing package on your system install. Since at this point it will do git, swig2.0, wget, unzip, quilt... there is chance one of these apps is missing. If so, try to get that installed on your system first. About swig you may have to build and install it by yourself because version 2.0 is recent.<br>

If everything goes well, congrats, you have created the native libraries for CSipSimple (it will also produce native libs for plugins apps).<br>
## Without building the native library

**Alternate way -- most developers should ignore this section**
This method is not officially supported. However it's helpful if you have not knowledge in native development or GNU toolchains<br>

The native library build produce two things : * swig auto generated java/jni bindings class. * dynamic native library .so files To get the java/jni bindings class get the archive file here :<br>
org.tar.gz and unpack it to have the org.pjsip package in the src folder of CSipSimple.<br>

To get the .so files, download latest nightly build from nightly build website and unpack the apk file (apk files are just zip files). **Then get lib/ARCH/.so files and copy it into CSipSimple libs/ARCH/.so**

Build apk
===========

There is several ways to build the apk once the native library is built. You can either use an IDE that integrates android development toolchains (described in details for Eclipse) or you can use Ant (for command line environments).<br>

## Setup/import application in your IDE (Optional)
You can now open the projects with Eclipse or your preferred java IDE that is able to build a regular android application.<br>
Import the projects into Eclipse as an Android projects (right click > import > Android project from source) and select the folder CSipSimple-trunk.<br>

Pick at least **SipHome and ActionBarSherlock. Add CSipSimpleXXX-projects** if needed


Then when ADT plugin will ask you which android version you'd like to link on, you should check the "project.properties" file of each project you are importing (do that both for CSipSimple and ActionBarSherlock). You can have a look to online files here `CSipSimple project.properties and ABS project.properties`. Obviously you need these versions to be installed by Android SDK Manager if not already done.<br>

Don't be worried about the fact it's high values! It does not mean that your app will not run with older android versions. Actually, it will be able to run on android 1.6! MinSdk version and targetVersion are not linked. Backward compatibility is ensured by code not by the lib you link on while building.<br>

Finally, you need to add the project dependencies to "SipHome", which is at least "ActionBarSherlock" and optional native lib projects. In Eclipse: right-click on "SipHome"-> Properties -> Java Build Path -> Projects.<br>

If you encounter some problems about override of methods, ensure that your Eclipse env setup projects for "java 1.6" or upper. -You should also check that per project if necessary- <br>
You should now be able to launch the application as an Android application.<br>

## Using ant (Optional)

This is particularly useful for a pure command line environment. It's used on the automatic nightly build server for example.<br>

Directory structure: * ./ActionBarSherlock * ./CSipSimple <br>
Steps: **Install ant** Setup SDK and NDK path (see above) **(if needed) Built native lib part: in ./CSipSimple execute: make** in ./ActionBarSherlock execute:`android update project -p ./`  **in ./CSipSimple execute: `android update project -p ./`**

Setup code signing for .apk **(if needed) `Create keystore: keytool`**  **Configure code signing in ant.properties** in ./CSipSimple execute: ant release * ./CSipSimple/bin/CSipSimple-release.apk


You have to create your own ant script based integrating ant scripts from the SDK. You can get inspired of what is produced by command line tool of the sdk http://developer.android.com/guide/developing/projects/projects-cmdline.html.


Some interesting tutorial to follow : http://www.androidengineer.com/2010/06/using-ant-to-automate-building-android.html.


You can also add additional ant task such as the one made for automatic revision in code number of the manifest.


Updates
===========

If you would like to update your checkout of CSipSimple-trunk to the current SVN revision use the provided **make tasks**! This command will remove all applied quilt patches from SVN dependencies, update the SVN dependencies and CSipSimple-repository, and re-apply the new patches.All native libraries must be re-build `make update && make`<br>

If you want to automate the build of the native part as well as the build of the android project, you can do that by installing CDT on eclipse and then on the android project do : New Project->Other->C++->Convert to C++ and tells it's a makefile project.<br>

## Fast way to develop a GPL branded version

BuildGplBrandedVersion<br>

Integrate an existing pjsip module
=================================

IntegrateExistingPjsipModule

Build with pjsip 1.x
=================================

OldWayToBuild wiki page gives for reference the old way to build the library on older csipsimple versions. It's not supported anymore in development process.



