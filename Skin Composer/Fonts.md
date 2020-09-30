Fonts are used to display text components in your widget styles. Fonts can be created via the Bitmap Font, FreeType Font, or Image Font dialogs. You can also create your fonts in an external editor and import them into the Skin Composer.

# Create a Font

## Create a Bitmap Font

See the guide on [Creating Bitmap Fonts](https://github.com/raeleus/skin-composer/wiki/Creating-Bitmap-Fonts). The Bitmap Font editor has the advantage of making a clean font that is most compatible with LibGDX and with the least amount of setup.

## Create a FreeType Font

See the guide on [Creating FreeType Fonts](https://github.com/raeleus/skin-composer/wiki/Creating-FreeType-Fonts). FreeType fonts are dynamically generated at runtime and can be adjusted to match the scaling requirements of your game on the fly. They have the disadvantage of added complexity and incompatibility with the GWT/HTML5 backend.

## Create an Image Font

See the guide on [Creating Image Fonts](https://github.com/raeleus/skin-composer/wiki/Creating-Image-Fonts). Image fonts give you complete control of the appearance and style of your fonts. The added complexity of using an image editing software to design your font makes this not an ideal solution for most users. Also, kerning information from the source font is lost with this technique and may require manual tinkering to get right.

# Import a Font from an External Editor

Prepare your font in [Hiero](https://github.com/libgdx/libgdx/wiki/Hiero), [Littera](http://kvazars.com/littera/), or any editor capable of generating an AngelCode Bitmap Font. Save the font with the file extension *fnt* and ensure the associated PNG file is saved in the same folder and with the same name as the FNT file. Please consider that Hiero produces a font image with an unacceptable amount of white space and is highly not recommended for packing in your skin.

* Go to *Project >> Fonts*
* Click *Open FNT*
* Select the font file
* Click *OK* and then enter a name

Names must begin with a letter and must only contain letters, numbers, underscores, and hyphens.