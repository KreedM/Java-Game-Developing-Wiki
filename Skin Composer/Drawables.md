Drawables are the core of widget styles. These are usually basic PNG images, however they can be nine-patches, tinted/tiled variants, or completely generated from code. The Drawables dialog manages all of these.

# Importing PNG Files

Existing images and nine-patches can be imported directly into the editor. To create nine-patches, see the [guide on the Nine-Patch Editor](https://github.com/raeleus/skin-composer/wiki/Nine-Patches).

* Go to *Project >> Drawables*
* Click *Add*
* Select an image file or nine-patch file

The drawable will be listed in the catalog. Alternatively, you can drag and drop images from the OS directly into the Drawables window.

# Working with the Catalog

The catalog lists all of the drawables for the current skin. This list can be sorted to make finding specific drawables easier. Click the *Sort* select box to sort by alphabetical, reverse alphabetical, oldest modified date, or newest modified date.

Another way to find a specific drawable is to enable a filter. Click the filter button on the top left. You can disable specific types of drawables or use a name search. Use a [regular expression](https://www.rexegg.com/regex-quickstart.html) for even more precise control. Click *Apply*. A quicker way to apply a filter is to just start typing while viewing the catalog. To disable, click the filter button and select *Disable*.

The zoom slider controls how large the preview thumbnails are going to be. The default displays the graphic at its default size. Increased zoom fills the preview space and enlarges the preview area. Decreasing zoom all the way down enables a detail view which allows you to read the drawable names more easily.

# Choosing a Drawable Type

Basic images are limited in that they don't stretch very well. Skin Composer provides several options to stretch, tint, or render your drawables in different ways.

## Nine-Patches

See the [guide on the Nine-Patch Editor](https://github.com/raeleus/skin-composer/wiki/Nine-Patches). Nine-Patches have the advantage of being stretched without distortion. This is ideal for buttons and similar widgets.

## Tinted Drawables

Tinted drawables are colored versions of normal drawables or nine-patches. Add a drawable like normal, then click the color wheel on the drawable to make a tinted version. You'll be prompted to select a color from the color picker. You can also click on the color swatches button to select a color from the [Colors](https://github.com/raeleus/skin-composer/wiki/Colors) dialog instead. Any modification to the original color will be reflected with the tinted drawable. To change the properties of the resulting drawable, click the cog button.

## Tiled Drawables

Tiled drawables, when stretched, are repeated in a tiled pattern instead of stretching the pixels out. This is an important technique to use for progress bars, for example. Tiled drawables can also be tinted. Click the tile button on the drawable that you want to make into a tile. Enter a valid name, choose a color, and select a minimum size. Enter a minimum size of 0 if you want to allow the image to be crushed completely. To change the properties of the resulting drawable, click the cog button.

## Ten Patch Drawables

See the [guide on the Ten Patch Editor](https://github.com/raeleus/skin-composer/wiki/Ten-Patches). Ten Patches are an alternative to Nine-Patches with the added options of tiling content and multiple patch areas.

## Custom Drawables

Sometimes you want to create a drawable via code or you may have created your own drawable class in your project. To integrate these into your skin, you should use a custom drawable as a placeholder. Click the *Custom* button in the Drawables dialog. Enter a valid name and click *OK*. Unfortunately, Skin Composer can not render your custom drawable code. Instead you'll have a red box. Make sure to replace this drawable class before loading your skin in libGDX. You can also see the [custom classes](https://github.com/raeleus/skin-composer/wiki/Custom-Classes) option in Skin Composer.