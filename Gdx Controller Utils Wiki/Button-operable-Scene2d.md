Key and button supporting Stage with focusable Actors for Scene2d. This is also usable in games without Game Controller support.

## Installation
Add the dependency to your core project: 

    compile "de.golfgl.gdxcontrollerutils:gdx-controllerutils-scene2d:$cuversion"

For GWT, of course the source is needed in the HTML project:

    compile "de.golfgl.gdxcontrollerutils:gdx-controllerutils-scene2d:$cuversion:sources"

and the inheritance in gwt.xml:

    <inherits name="de.golfgl.gdx.controllers.controller_scene2d" />

## Usage

Despite its name, you can use `ControllerMenuStage` and the other classes even without game controllers, but to make your game's GUI fully keyboard operable.

### ControllerMenuStage
Use `ControllerMenuStage` instead of `Stage` in your game. Every Actor that should be operable by buttons must be posted to the Stage by calling `addFocusableActor`:

        stage.addActor(playButton);
        stage.addFocusableActor(playButton);

The initially focused Actor must be set by calling

        stage.setFocusedActor(playButton);

That's all. If the focused actor is not highlighted according to the skin's mouse over style, make sure that the actor is layed out correct before calling `setFocusedActor`, typically by calling `validate()` on a surrounding Table.

#### More options
You can also set an actor that is automatically hit when Escape (or back on Android) is pressed:

        stage.setEscapeActor(exitButton);

Check out the other `set` methods as well to change behaviour. If you need even more control, you can override most methods in `ControllerMenuStage` to adjust its behaviour to your needs. Interesting methods for this are `isXxxKeycode` and `onFocusLost`/`onFocusGained`. See below for some more options.

### ControllerMenuDialog
`ControllerMenuDialog` is a dialog adjusted to use with `ControllerMenuStage`: it automatically focuses its first button when shown, and restores the focus to the last Actor of the parent stage when hidden.

When you add own buttons to the dialog without calling the `button()` methods, make sure to add it to `buttonsToAdd` array. All Actors listed in this array will be posted as focusable Actors to the Stage when ControllerMenuDialog is shown.

### Other Actors
controller-scene2d comes with actors that can be used with `ControllerMenuStage`. You can also get an idea of how controller-scene2d can be adjusted to your needs by looking into the sources.

#### ControllerScrollPane
`ControllerScrollablePane` is a ScrollPane that scrolls on arrow keys or d-pad buttons. The next focusable Actor gets the focus only if the ScrollPane cannot scroll any more.

#### ControllerSlider
`ControllerSlider` extends `Slider`. Its value is changed on arrow keys or d-pad buttons.

#### ControllerList
`ControllerList` extends Scene2d's `List`. On arrow keys or d-pad button the selected item is changed.

### Adjusting the behaviour

Most methods in `ControllerMenuStage` are protected and thus overridable by you.

There are some predefined interfaces you can implement on Actors to adjust behaviour based on the focused Actor: 

* `IControllerActable` to adjust behaviour when return key/default controller button was pressed
* `IControllerScrollable` to adjust behaviour when arrow keys/controller direction buttons were pressed
* `IControllerManageFocus` to adjust determination of next Actor to focus (since 2.1.0)

## Example project
core-scene2d is shown with sources in [raeleus' Shadow Walker UI demo](https://github.com/raeleus/Shadow-Walker-UI).