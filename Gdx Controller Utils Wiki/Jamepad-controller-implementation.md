Thanks to [born2snipe](https://github.com/born2snipe) for his work on this.

Just change the following lines in your build.gradle's desktop project to switch from libGDX controller implementation to [Jamepad](https://github.com/williamahartman/Jamepad):

        compile "com.badlogicgames.gdx:gdx-controllers-desktop:$gdxVersion"
        compile "com.badlogicgames.gdx:gdx-controllers-platform:$gdxVersion:natives-desktop"

to: 

        compile "de.golfgl.gdxcontrollerutils:gdx-controllers-jamepad:$cuversion"

This just affects the Desktop project. The implementation is compatible to the libGDX implementations, no code changes in your project needed.

In case your project does not already declare the Jitpack repository, you need to do this as well:

      repositories {
          ...
          maven { url "https://jitpack.io" }
      }

### Ups: 

* Hotplugging on Windows!
* A big controller mapping database which ensures your users probably won't need to remap their buttons
* Rumble/vibration supported (you need to add [gdx-controllers-advanced](https://github.com/MrStahlfelge/gdx-controllerutils/wiki/Advanced-Controller-interface) to your core project to use it) since 0.2.0
* implements [AdvancedController interface](https://github.com/MrStahlfelge/gdx-controllerutils/wiki/Advanced-Controller-interface)

### Downs:

* If a connected controller is not in the mapping database, it is not shown.
* D-Pad/POV values will be reported as buttons down/up. (Of course, it is just some piece of work to be done to map the D-Pad buttons to POV values)

### Work still to be done:
* Button to D-Pad mapping (can be done [like in GWT implementation](https://github.com/libgdx/libgdx/commit/66ac43392efc3c62bf6a598ddd95a8dc8038b1ed#diff-a57ca83c29806053f9a4f010adc1fbbd))