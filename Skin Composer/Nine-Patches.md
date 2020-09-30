Nine-patches are a type of image that allows stretching along the horizontal and vertical axes without artifacts. This is achieved by denoting stretchable areas of repeating pixels so that corner graphics remain static. These are basically PNG files with a one pixel border around the edge filled with either black or transparent pixels where the patch areas should begin or end. This is a technique popularized by the Android operating system. libGDX's implementation is limited in that you can only specify one stretchable area per axis. Nine-patches are extremely well suited for UI elements and is used extensively throughout Skin Composer in UI design.

You can use any nine-patch editor as you see fit in conjunction with Skin Composer. However, Skin Composer's nine-patch editor works intuitively and can help improve your workflow. The following steps will describe how to take an existing button graphic and transform it into a stretchable nine-patch with content padding.

# Creating a Nine-Patch

* Go to *Project >> Drawables*
* Click on *Create 9-Patch*
* Click *Load Image* and select your source image
* Click *Auto Patches* to estimate where the stretchable areas are supposed to be
* Drag the horizontal and vertical handles to fine tune the stretchable areas
* Resize the preview graphic to notice any scaling artifacts
* Flip the *Padding* switch on the top left to *Content*
* Adjust the content padding handles to maximize the usable area inside the graphic
* Use the *Content* select box to see how different types of content will layout in the graphic

You can also use the number spinners to manually change the content or padding values at any time. The zoom slider enlarges the graphic so you can see the individual pixels better. Use the grid buttons to add a visual pixel grid to the screen. Click and drag the screen to pan your view. Click the reset view button to return the pan and zoom to their defaults.

# Saving and Batch Operations

Click *Save* and select a save file path for the nine-patch. If you have more than one image that needs the same nine-patch settings as this image, such as multiple states for the same button, use the batch option instead. Click *Batch Apply to File(s)*. Select multiple images at once and click *Open*. The current patch settings will be applied to each file automatically and named accordingly.

# Loading images

An easier technique to load an image is to drag and drop a PNG file from the operating system into the window. These can be plain PNG's or existing nine patch files. If you are working on one image and need the patch settings from another image to be applied, use the *Load Patches From File* button.