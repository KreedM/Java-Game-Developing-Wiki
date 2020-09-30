To use your UI skin in libGDX, you must export a JSON file.

# Exporting a Skin JSON

* Go to *File >> Export*
* Choose an export path
* Select options and click *Export*

*Generate texture atlas* is necessary in most circumstances. If you pack your own atlas separately, for example combining game elements and UI in the same texture, you can uncheck *Generate texture atlas*. Be sure to include all the drawables used by the skin.

*Copy font files* is needed in almost all circumstances to ensure that the font files (bitmap or TTF) are available at run time. Uncheck this if you do not want to override existing files in a folder.

*Export with simple names* is a feature from libGDX 1.9.9 that simplifies the output of the names in the JSON. This is not compatible with older versions of libGDX. Similarly, *Exported JSON Format* can reduce or increase the verbosity of your JSON. Choose *JSON* for full JSON compliance. See the [wiki](https://github.com/libgdx/libgdx/wiki/Reading-and-writing-JSON#writing-object-graphs) for more details.

*Export colors as hexadecimal* is a convenience method to change the long form RGBA values to a single hex value.