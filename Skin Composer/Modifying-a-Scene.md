# About

You've designed your layout in Scene Composer and you've exported it successfully via JSON. This cleans up the code significantly, but the UI doesn't actually do any work. To do that, you need Listeners and you need to apply them to the widgets already in the Stage. This can be done via the findActor() method.

# Setup

This technique will not work if you do not name any of your widgets. In Scene Composer, name the widgets you wish to access before you export the JSON. For example, I might name one of my TextButtons "start-button" because that will be the button that starts the game.

Ensure that you are loading your JSON scene via the SceneComposerStageBuilder as seen in [the example](https://github.com/raeleus/stripe/blob/master/test/src/com/ray3k/stripe/test/SceneComposerTest.java).

# The example explained

The following creates our Stage, Skin, and builder:

```java
stage = new Stage(new ScreenViewport());
Gdx.input.setInputProcessor(stage);
skin = new Skin(Gdx.files.internal("metal-ui.json"));
        
SceneComposerStageBuilder builder = new SceneComposerStageBuilder();
builder.build(stage, skin, Gdx.files.internal("test-layout.json"));
```

Adjust the Skin path and builder path as necessary.

***

The following code finds a TextButton named "test-button":

```java
TextButton textButton = stage.getRoot().findActor("test-button");
```

You can make any sort of modification to this button now that you have a reference to it. Use the typical class methods of the widget to change the values, appearance, etc.

***

Most commonly, you'll want to add a ChangeListener to detect when the button is clicked. The following simply prints a "Test Successful" message to the console:

```java
textButton.addListener(new ChangeListener() {
    @Override
    public void changed(ChangeEvent event, Actor actor) {
        System.out.println("Test Successful");
    }
});
```

***

That is essentially it. As said previously, it can make more sense to just write all the code out manually instead of exporting from Scene Composer. It is up to you to determine if the complexity of your layout requires one technique or another.

# Next steps

If you need a recap on how to layout widgets in Scene Composer, please see the [first guide](https://github.com/raeleus/skin-composer/wiki/Scene-Composer).

If you want the full details on how to export your Scene in a variety of ways, see the [previous guide](https://github.com/raeleus/skin-composer/wiki/Exporting-a-Scene).