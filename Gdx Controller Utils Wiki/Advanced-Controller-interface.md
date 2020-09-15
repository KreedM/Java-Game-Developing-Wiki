The `AdvancedController` interface gives you access to controller features that are not accessible by libGDX' core `Controller` interface. This way, you can query a button mapping for the controller or get game controllers to vibrate. And there are more features available. 

### Installation

Add the dependency to the core project:

        compile "de.golfgl.gdxcontrollerutils:gdx-controllers-advanced:$cuversion:sources"

And of course, you need to add the sources to your HTML project:

        compile "de.golfgl.gdxcontrollerutils:gdx-controllers-advanced:$cuversion:sources"


### Features
The following features are available through the advanced interface. See the [JavaDocs](https://github.com/MrStahlfelge/gdx-controllerutils/blob/master/core-advcontrollers/src/com/badlogic/gdx/controllers/AdvancedController.java) for further documentation.

Check if the returned controller is implementing the inteface before using it:

```java
if (Controllers.getControllers().get(0) instanceof AdvancedController {
    // you are safe to use the new methods here
}
```
#### Button mapping

    ControllerMapping getMapping();

Returns an immutable `ControllerMapping` object holding constants for the axis and button indices of the underlying platform, implementation or controller. You can query the state of Button A independant of the platform and connected controller type with the following code: 

    controller.getButton(controller.getMapping().buttonA));

Note that some buttons or axis can be set to `ControllerMapping.UNDEFINED`.

#### Query available features

    int getMinButtonIndex();
    int getMaxButtonIndex();
    int getAxisCount();
    int getPovCount();
    boolean canVibrate();

#### Vibration

Not supported on iOS, on HTML5 only supported on Chrome.

    boolean isVibrating();
    void startVibration(int milliseconds, float strength);
    void cancelVibration();

#### Connection state

    String getUniqueId();

If more than one controller of the same type is connected, this unique ID helps you identifying a certain controller instance.

    boolean isConnected();

If you hold references to a controller in your game, you might want to know if this controller is already disconnected without having to look it up in the controllers array.

#### Player Index

Some controllers can show the player index (like the Wii Remotes or Xbox controllers can). Currently only supported on iOS.

    boolean supportsPlayerIndex();
    int getPlayerIndex();
    void setPlayerIndex(int index);
