# About

So you've designed your layout in Scene Composer and now you want to see it in your game. There are several options available to facilitate this. The "Java" option saves the scenegraph to a java file directly in your project. The "Copy to Clipboard" option copies the data and allows you to paste fully qualified code into an existing file. The "JSON" option serializes the data to either be imported back into Scene Composer or incorporated in your Stage via the Stripe library. Keep in mind, Scene Composer is not intended to replace learning to write proper code. Writing direct code will always be the most flexible and robust option. Nevertheless, if you're in a rush, you can continue below.

# How to export

The first step is to make sure that the skin is available in your project. From the main window of Skin Composer, click File >> Export and choose the path to the "assets" folder of your GDX project.

## Java

Open Scene Composer. With your layout complete, go to File >> Settings. Change the Package and Class to match the settings in your GDX project. If you take the default GDX settings for example, the package would be "com.mygdx.game" and the class would be "MyGdxGame". The Skin Path should be the relative path from your assets folder to the Skin you exported above (ie. skins/myskin.json). The Background Color should be set to a color of your choosing.

Close the settings window. Go to File >> Export. Choose "Save to Java" and select the path. Note that this will overwrite the file already at this location. This file will typically run on its without much modification if you set the settings correctly. You are free to add Listeners or whatever code you wish directly to the file.

## Clipboard

With your complete layout loaded, go to File >> Export. Click on "Copy to Clipboard". Open your GDX project and find an appropriate space to paste your stage code and paste it. The code pastes with fully qualified paths, so you'll have to clean it up quite a bit.

## JSON

JSON is the cleanest way to export your code, though it takes some work to setup correctly. You must implement the Stripe project in your build.gradle. Follow the instructions [on the Stripe page](https://github.com/raeleus/stripe/blob/master/README.md). Then, in Scene Composer, go to File >> Export. Click "Save JSON". Save the JSON file to the assets folder of your GDX project. See [the example code](https://github.com/raeleus/stripe/blob/master/test/src/com/ray3k/stripe/test/SceneComposerTest.java) to load and implement the scene in your game using the SceneComposerStageBuilder.

## Loading a JSON in Scene Composer

You can load exported JSON files back into Scene Composer, enabling you to edit multiple layouts for a project. Go to File >> Import. Choose the file you wish to load. Please take care to load JSON files that were designed for your existing Skin. Layouts referring to non-existent styles and assets will have their associated widgets disabled.

# Next Steps

If you need a recap on how to layout widgets in Scene Composer, please see the [previous guide](https://github.com/raeleus/skin-composer/wiki/Scene-Composer).

Using the SceneComposerStageBuilder to load your layout from JSON can be convenient. However, Scene Composer lacks a lot of the features available through code. Check the [next guide](https://github.com/raeleus/skin-composer/wiki/Modifying-a-Scene) on modifying a scene to implement Listeners and various other niceties.