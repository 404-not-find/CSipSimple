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
You'll need following package **you may need to adapt depending to your distribution packages names** subversion git quilt unzip wget swig2.0 python make yasm You also have to install the android NDK and the android SDK. I do not details here how to install android NDK and SDK. Please read Android documentations about that : * Android NDK doc * Android SDK doc To install correctly the android NDK, please add the NDK to your path. I usually recommend to add these lines to your .bashrc : export ANDROID_NDK=/_path_to/android-ndk-linux/ export ANDROID_SDK=/_path_to/android-sdk-linux/ export PATH=$PATH:$ANDROID_SDK/tools:$ANDROID_SDK/platform-tools:$ANDROID_NDK Anyway the naming of ANDROID_NDK export is important in the build of one part of the application. You can also do that only at build time, but if you use to develop for android, it's a good idea to do that.
