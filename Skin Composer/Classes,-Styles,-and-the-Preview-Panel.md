By following the previous guides, you should have a selection of [drawables](https://github.com/raeleus/skin-composer/wiki/Drawables), [colors](https://github.com/raeleus/skin-composer/wiki/Colors), and [fonts](https://github.com/raeleus/skin-composer/wiki/Fonts) at your disposal. You will now collect these assets together into styles and apply them to the various widget classes available in libGDX. The following steps detail this process in relation to creating a text button style.

# Creating a Style

* Select *TextButton* from the Class select box

* Click the plus button next to Styles

* Enter a valid name for the style

* Select a font by clicking the font field and choosing a font from the list

* Click the up field and select a drawable to render as the button's graphic

Styles can be accessed from libGDX via the name you specified above. Each class also has a "default" style. This has a special significance because these can be accessed from the skin by not passing in a style name. Fill the rest of the fields as you see fit. See the official [libGDX documentation](https://github.com/libgdx/libgdx/wiki/Scene2d.ui) to learn more about these fields and how these widgets operate.

# Working with the Preview

In order for the preview to work, you must fill all the mandatory fields as dictated by libGDX. These fields are highlighted in red within Skin Composer. Once filled, the preview panel allows you to interact with a widget as if you were in a real app. To modify the preview, edit the preview properties below. The available options depend on the class selected and reflect the options available when laying out the widget in libGDX.

# Additional Information

Styles can be duplicated, deleted, or renamed by clicking the associated buttons to the right of the Style select box. If you have widget classes that you've designed on your own, click the plus button next to *Class*. See the documentation on the [custom class feature](https://github.com/raeleus/skin-composer/wiki/Custom-Classes).