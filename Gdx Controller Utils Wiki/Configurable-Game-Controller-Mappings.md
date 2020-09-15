Support configurable mappings for game controllers in your projects.

## Installation
Add the dependency to your core project: 

    compile "de.golfgl.gdxcontrollerutils:gdx-controllerutils-mapping:$cuversion"

## Usage
Make sure that you have read and understood [Documentation on libGDX' controller interface](https://github.com/libgdx/libgdx/wiki/Controllers). This extension provides a configurable mapping on top of that.

The main class of this extension is `ControllerMappings`. At the creation of your game, you have to instantiate this class (or a subclass of it). 

    public class MyGame extends Game {
        public ControllerMappings controllerMappings;

        @Override
        public void create() {
            //...
            controllerMappings = new MyControllerMappings();
        }
    }

    public class MyControllerMapping extends ControllerMappings {
        // we fill this soon
    }


### Defining the buttons and axis your game needs
Each game needs different controller buttons and axis. While retro games will work best with a D-Pad and up to four buttons, others will need two analog axis to work. You define the needed input types by adding `ConfiguredInput` objects to the mapping. `ConfiguredInput`'s constructor takes the type of the input (button, analog axis, digital axis or digital or analog axis) and a unique identification number: 

    public static final int BUTTON_JUMP = 0;
    new ConfiguredInput(ConfiguredInput.Type.button, BUTTON_JUMP)

Add the configured inputs by calling `addConfiguredInput()` of `ControllerMappings`. For example, the following calls placed in `MyControllerMapping`'s constructor define a good old NES-style gamepad (D-Pad, two action buttons, Start and Select): 

    public static final int BUTTON_JUMP = 0;
    public static final int BUTTON_FIRE = 1;
    public static final int AXIS_VERTICAL = 2;
    public static final int AXIS_HORIZONTAL = 3;
    public static final int BUTTON_START = 4;
    public static final int BUTTON_CANCEL = 5;

    public MyControllerMapping() {
        super();

        addConfiguredInput(new ConfiguredInput(ConfiguredInput.Type.button, BUTTON_JUMP));
        addConfiguredInput(new ConfiguredInput(ConfiguredInput.Type.button, BUTTON_FIRE));
        addConfiguredInput(new ConfiguredInput(ConfiguredInput.Type.button, BUTTON_START));
        addConfiguredInput(new ConfiguredInput(ConfiguredInput.Type.button, BUTTON_CANCEL));
        addConfiguredInput(new ConfiguredInput(ConfiguredInput.Type.axisDigital, AXIS_VERTICAL));
        addConfiguredInput(new ConfiguredInput(ConfiguredInput.Type.axisDigital, AXIS_HORIZONTAL));

        commitConfig();
    }

You must commit the configuration by calling `commitConfig()` before actually using it.

#### The different input types
As said before, `ControllerMappings` support four different controller input types. While `ConfiguredInput.Type.button` should be self-explanatory, difference between `axisDigital`, `axisAnalog` and `axis` is probably not at first glance. [Read the JavaDoc](https://github.com/MrStahlfelge/gdx-controllerutils/blob/master/core-mapping/src/de/golfgl/gdx/controllers/mapping/ConfiguredInput.java) for clarification.

### Define a default mapping
To define a default mapping, override `ControllerMappings`' `getDefaultMapping()` method:

    @Override
    public boolean getDefaultMapping(MappedInputs defaultMapping, Controller controller) {
        // see https://developer.android.com/reference/android/view/KeyEvent.html#KEYCODE_BUTTON_A
        boolean onAndroid = Gdx.app.getType() == Application.ApplicationType.Android;

        defaultMapping.putMapping(new MappedInput(AXIS_VERTICAL, new ControllerAxis(1)));
        defaultMapping.putMapping(new MappedInput(AXIS_HORIZONTAL, new ControllerAxis(0)));
        defaultMapping.putMapping(new MappedInput(BUTTON_JUMP, new ControllerButton(onAndroid ? 96 : 0)));
        defaultMapping.putMapping(new MappedInput(BUTTON_FIRE, new ControllerButton(onAndroid ? 97 : 1)));
        defaultMapping.putMapping(new MappedInput(BUTTON_START, new ControllerButton(onAndroid ? 108 : 9)));
        defaultMapping.putMapping(new MappedInput(BUTTON_CANCEL, new ControllerButton(onAndroid ? 4 : 8)));

        return true;
    }

If needed, you can query the name of the controller that was connected and is not already mapped from the `MappedInputs` object parameter.

#### Where to get a default mapping from?

Controller button codes are unfortunately platform dependant. That's why there is a difference between Android and GWT in the example above. You find the default button and axis mappings here:

* GWT/HTML5 and iOS: [W3C](https://w3c.github.io/gamepad/#remapping)
* Android: [Google](https://developer.android.com/training/game-controllers/controller-input) or [Amazon](https://developer.amazon.com/de/docs/fire-tv/game-controller-input.html#inputreference) (the constants are the same)
* Desktop: For Jamepad, see [axis enum](https://github.com/williamahartman/Jamepad/blob/master/src/com/studiohartman/jamepad/ControllerAxis.java) and [button enum](https://github.com/williamahartman/Jamepad/blob/master/src/com/studiohartman/jamepad/ControllerButton.java). For libGDX' default desktop controller implementation, there is no default so I would recommend you to use the Xbox Controller mapping as this is the most widespread mapping.

If you only use controller implementations from this library, you also have the possibility to query the default mapping through `AndvancedController.getMapping()`:

    @Override
    public boolean getDefaultMapping(MappedInputs defaultMapping, Controller controller) {
        ControllerMapping controllerMapping = ((AdvancedController) controller).getMapping();

        defaultMapping.putMapping(new MappedInput(AXIS_VERTICAL, new ControllerAxis(controllerMapping.axisLeftY)));
        defaultMapping.putMapping(new MappedInput(AXIS_HORIZONTAL, new ControllerAxis(controllerMapping.axisLeftX)));
        defaultMapping.putMapping(new MappedInput(BUTTON_JUMP, new ControllerButton(controllerMapping.buttonA)));
        ...
    }

### Let the user record his own mapping
The most important feature of a configurable Game Controller Mapping is the ability to let the user record his the inputs of his own controller. Use the `recordMapping()` method for it. It takes the `Controller` for which the mapping should be recorded and the id of the `ConfiguredInput` you want to record:

    controllerMappings.recordMapping(controller, AXIS_VERTICAL);

You can call this method in the `render()` or `act()` method of your mapping configuration dialog, but you should not call this method on every `render()` as it needs to poll all inputs of the given Controller. Wait 100ms or so.

`recordMappings` returns a `ControllerMappings.RecordResult` which informs you what happened:

* `recorded`: A mapping for the given `ConfiguredInput` was recorded
* `nothing_done`: Nothing was done (user did not press a button)
* `not_added`: User pressed a button that was not applicable for the given `ConfiguredInput`, or a mapping for the given input is already defined
* `need_second_button`: this is returned when your `ConfiguredInput` is an axis, but the user pressed a button. The pressed button is mapped to the positive axis value, now the negative axis value button must be defined
* `not_added_need_button`: this is returned when your `ConfiguredInput` is an axis and its positive axis value is already mapped to a button, but the user now did not pressed a button.

If you want to redefine a mapping, reset the old defined mapping by calling `resetMappings()`.

Some users might have a gamepad with accelerometer input or buttons/axis that are always reporting to be pressed. To ignore these buttons when you record your mappings, ask the user to not press any button and call `recordButtonsToIgnoreForMapping()` before your first call to `recordMappings()`.

You can find an **example mapping configuration dialog** for the NES controller on Gist: [GamePadMappingDialog.java](https://gist.github.com/MrStahlfelge/b7dcef41050bd40130f515f14cce44d3) or in the [SMC Platformer example project](https://github.com/MrStahlfelge/SMC-libgdx).

### Loading and saving user's recorded mappings
Of course, you want to save a recorded mapping so your players don't need to redefine it again every time they fire up the game. `ControllerMappings` provides `toJson()` and `fillFromJson(jsonValue)` methods for this.

### Polling a controller
For polling controllers respecting your mapping, use the `MappedController` instead of calling libGDX' `Controller` directly:

        MappedController mappedController = new MappedController(controller, controllerMappings);
        mappedController.isButtonPressed(BUTTON_JUMP);
        mappedController.getAxisValue(AXIS_HORIZONTAL);

### Event based controller input
For event based controller input recpecting your mapping, extend `MappedControllerAdapter` and add it as a listener to libGDX' `Controller` interface:

    Controllers.addListener(new MappedControllerAdapter(controllerMappings) {
            //override configuredXXX() methods here...
        });

### Map controller input to keyboard input
In many cases, especially if you do not use an analog axis, a game's controller input will give the player the very same actions than keyboard input. You already defined input listeners for keyboard actions, so you just want to map the controller input to keyboard keys. There is a convinience class for doing so: `ControllerToInputAdapter`.

The following code maps the NES-type controller defined above to keyboard actions:

        controllerToInputAdapter = new ControllerToInputAdapter(controllerMappings);
        controllerToInputAdapter.addButtonMapping(BUTTON_JUMP, Input.Keys.SPACE);
        controllerToInputAdapter.addButtonMapping(BUTTON_FIRE, Input.Keys.ALT);
        controllerToInputAdapter.addButtonMapping(BUTTON_START, Input.Keys.ENTER);
        controllerToInputAdapter.addButtonMapping(BUTTON_CANCEL, Input.Keys.ESCAPE);
        controllerToInputAdapter.addAxisMapping(AXIS_HORIZONTAL, Input.Keys.LEFT, Input.Keys.RIGHT);
        controllerToInputAdapter.addAxisMapping(AXIS_VERTICAL, Input.Keys.UP, Input.Keys.DOWN);
        controllerToInputAdapter.setInputListener(yourKeyboardInputListener);

On game start, bind it to all controllers by calling `Controllers.addListener(controllerMappings.controllerToInputAdapter);`

Then, on every applicable call to `Gdx.input.setInputprocessor`, make also a call to `controllerToInputAdapter.setInputprocessor`.

## Example project
Take a look at the [SMC Platformer example project](https://github.com/MrStahlfelge/SMC-libgdx) [Falling Lightblocks](https://github.com/MrStahlfelge/lightblocks) for sources using core-mapping.