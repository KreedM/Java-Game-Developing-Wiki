# About

Scene Composer is a great way to prototype your Scene2D layouts and see your Skin in action. It is a WYSIWYG editor that can export JSON or Java files to be loaded directly in your game. As a module of Skin Composer, the layout saves with your project so you can continue where you left off at any time.

# How to design a scene

First you must develop a Skin that implements all the widgets you plan to use in your layout. You can't use a widget if it doesn't have a valid style. Do this how you would usually develop a Skin in Scene Composer.

In your Skin Composer window, Go to Project >> Scene Composer. In the "Properties" area, click "Add Table". Select a Table size of your choosing. You can set the properties of the Table by clicking each button and modifying the options.

Selecting a cell can be done in one of two ways. The path listed at the bottom of the window is a snapshot of where you are at in the Actor tree. You can click "Select Child" and click a button to modify that cell. The alternative is to click the preview area at the top to select a cell. Boxes are drawn around each selectable child. Click one of the cells to select it.

With a cell selected, you have a number of layout options available. Please consult the [libGDX wiki](https://github.com/libgdx/libgdx/wiki/Scene2d.ui) on these details. These options largely correlate to what is available via code. You can add or duplicate cells from this view as well. Most importantly, you can click "Set Widget" to add a widget to your cell. Select the type you want to add from the list. Options with incomplete styles are disabled.

With the widget selected, you now can modify its specific properties. Once again, these correlate to the options you would typically find available in the widget class. As you make changes, the preview is updated. Some advanced options are omitted for simplicity/brevity and are left to you to [implement in code](https://github.com/raeleus/skin-composer/wiki/Modifying-a-Scene). Widgets with text can use escaped characters such as `\n` which are automatically parsed in the preview. Unicode character notation is allowed as well, so long as the Skin font supports it: `\u2022` for a • as an example. It's important to set the "Name" of the widget if you do wish to access it via code later. If you no longer want this widget in the cell, click "Delete". If this is a "Layout" type of widget, you have options to nest even more widgets within it.

Once you are done setting the widget options and want to select another cell, you can click the parent Table in the path below. If you want to navigate directly through the preview, you can click outside of the selection in the preview to go up one level. Continue to do this until the Table is selected. Proceed to fill out the cells from here as you see fit.

To finally test your layout, go to View >> Live. This disables the click navigation and you can interact with the scene as if it was a real UI. The "Outline" view is helpful to see the debug lines of your cells and components. If you want to start from scratch, click Scene >> Clear. Undo and Redo functionality is available as well. To return to your Skin, go to File >> Quit.

# Next steps

This is enough for many developers as they are using Scene Composer only as a preview for their Skin. They prefer writing the code for their UI's manually. This is my suggestion to new developers to learn as well. However, some users with simple layouts may prefer to use Scene Composer to directly edit their menus and HUD's. This is facilitated by the various [export options](https://github.com/raeleus/skin-composer/wiki/Exporting-a-Scene).

Once you have loaded a scene in your game, you'll want to be able to add listeners to your widgets and add advanced options. Follow the guide on [modifying a scene](https://github.com/raeleus/skin-composer/wiki/Modifying-a-Scene).