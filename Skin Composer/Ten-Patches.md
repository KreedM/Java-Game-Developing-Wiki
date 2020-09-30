TenPatch is an alternative to libGDX's Nine-Patch implementation. It adds the options of tiling and multiple stretch areas. It also eliminates errors when shrinking the drawable to sizes smaller than the original image size. To use TenPatch in your projects, you must import the library via Gradle. Editing Ten Patch Drawables is integrated directly in Skin Composer. As the TenPatch spec is completely exported in JSON, there is no need for additional boiler plate code to make it compatible with your game. It just works. See more about [TenPatch here](https://github.com/raeleus/TenPatch).

# Designing Ten Patch Drawables

## Workflow

The workflow for integrating Ten Patches into your skins is as follows:

* Open Project >> Drawables.
* Add a regular PNG or Nine-Patch PNG to your drawables list.
* Press the "10" button on the drawable.
* Edit the stretch and content areas.
* Enable tiling, change the tint, and modify the minimum size if desired.
* Click "OK". Close the Drawables dialog.
* Use the Ten Patch Drawable as you would any other kind of Drawable.
* Export your skin and ensure your libGDX project's root build.gradle has the following:
```
allprojects {
    ...
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```

Add the dependency to the core project:

```
project(":core") {
    apply plugin: "java"


    dependencies {
        ...
        compile 'com.github.raeleus.TenPatch:tenpatch:2.0'
    }
}
```

## Editor

The Ten Patch editor is a powerful tool that has some hidden techniques to increase your productivity.

* The switches in the top left control the stretch/contents mode and horizontal/vertical mode. To quickly switch between horizontal and vertical, right click on the editor pane. To switch betwen stretch and contents, double right click.
* To zoom in, click and drag the slider in the lower right of the editor. You can also use the scroll wheel while the mouse is over the editor.
* To pan the editor view, right click and drag the editor window.
* To reset the view, press the reset button next to the zoom slider.
* Press gridline buttons to activate a light or dark pixel-sized gridline overlay.
* While in Stretch mode, click and drag to draw a new stretch area. This denotes an area that will shrink or stretch as the drawable is resized. You can have multiple stretch areas in one Ten Patch. When the drawable is used in an interface, resizing will be distributed across all the stretch areas based on the ratio of their relative sizes.
* You can modify an existing stretchable area by clicking and dragging an existing handle. To erase a stretch area, shrink the stretch area to a size of 0. To combine two stretch areas, extend the stretch area of one so that it touch another.
* In Content mode, click and drag the existing handles to modify where the content of a drawable can be drawn. Note that you may only specify one content area.
* You can load the patch settings from an existing nine-patch file by clicking "Load Patches". This will overwrite any stretch/content areas you have defined.
* Click "Save to File" to save the Ten Patch to a ".9.png" file for future import. Note that a Ten Patch with multiple stretch areas can not be loaded in libGDX this way. It also does not preserve any other special settings. 
* Press "Clear" to reset everything.

### Options
Ten Patches have various options that can be configured from within the editor or through code.

* The Tile option allows the texture to repeat within the stretch areas instead of simply stretching. It's a great choice for images with patterns.
* The color option allows you to tint the drawable. This is the equivalent of creating a TintedDrawable.
* The MinWidth and MinHeight indicate what the smallest size this drawable can be shrunken to. Change the MinWidth or MinHeight to 0 if you are implementing the Ten Patch in a progress bar, for example
* Setting the MinWidth and MinHeight to -1 in the editor leaves the minimum size to the original size of the image.

### Preview
The preview area is a helpful feature to see how your drawable is going to look like when you stretch and deform its bounds.

* Mouse over the preview. Click and drag any of the handles to deform the drawable.
* Click the "Content" select box to choose a type of content to test. "Text" is a word-wrapped label. "Color" is a single rectangle grown to the full size of the content area. "Drawable" allows you to place an existing drawable into the contents area.
* Click the "Content" color wheel to change the color of the contents.
* Click the "Background" color wheel to change the rendering background of the preview.

## Export
Export your skin as you normally would. If you were to inspect the contents of the skin.json, you'll see that a section of the JSON is dedicated to Ten Patches. The default skin loader of libGDX will search for the TenPatchDrawable class on your path. As long as you have modified your build.gradle as detailed above, this process will happen seamlessly in the background.