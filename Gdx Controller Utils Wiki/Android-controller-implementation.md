This is very similar to the libGDX' official Android controllers implementation, however, there are some minor additions:

* Fix for Crash on FireTVs (from PR [5657](https://github.com/libgdx/libgdx/pull/5657))
* Configuration for getting all controller button events (from PR [5702](https://github.com/libgdx/libgdx/pull/5702))
* Implements [Advanced controller interface](https://github.com/MrStahlfelge/gdx-controllerutils/wiki/Advanced-Controller-interface) and supports vibration (since v0.4.0)


You can use it by just changing the following lines in your build.gradle's Android project:

        compile "com.badlogicgames.gdx:gdx-controllers-android:$gdxVersion"

to: 

        compile "de.golfgl.gdxcontrollerutils:gdx-controllers-android:$cuversion"

## Attention
* Use cuversion 1.0.1 for libGDX versions 1.9.10 or lower
* Use cuversion 2.0.0 or higher for libGDX version 1.9.11 and newer