Image fonts give you complete control of the appearance and style of your fonts. The added complexity of using an image editing software to design your font makes this not an ideal solution for most users. Also, kerning information from the source font is lost with this technique and may require manual tinkering to get right.

# Create an Image Font

* Go to *Project >> Font*
* Click on *Create from Image*
* Choose a characters preset or type in the characters that you want to be generated in the bitmap font
* Click *Copy*

This copies a string of text to be pasted into the text tool of an image editor of your choosing. From there, you can modify the characters as you see fit, adding outlines, shadows, gradients, etc. Export the resulting image as a PNG, then load it in the Image Font editor by clicking *browse*. To ensure that your characters are split appropriately, ensure that there is at least a one pixel gap of complete transparency between each glyph of your image.

# Foreign Language Characters

To support foreign language characters, it is advised to use the character text file technique. This file can be created in any text editor to define what characters you want to include in your bitmap font. Follow the steps below to use Notepad on Windows:

* Open Notepad
* Type in all the characters you want to include in your font
* Go to *File >> Save As...*
* Choose save path and change the encoding listed below to *UTF-8*
* Click *Save*
* In Skin Composer's Bitmap Font editor, choose *Load from file (UTF-8)* from the *Characters* select box
* Find and select the text file that you created in the previous step

Please note that you also have to use a TTF that supports all of the characters that you defined in the text file.

# Adjusting the Output

The image font process is complicated and wrought with potential error. See the preview text area below. If the characters are incorrectly displayed, this may indicate that the automatic character detection is failing. Increase or decrease the *Gap Detection Size* and check if all the characters are being detected. If your font has large gaps within its characters, such as the quote character, you should draw some pixels in the image to fill the gap if all else fails. Click *View Characters* to see the temporary folder of all the split characters.

Kerning is the space between each character. Increase or decrease this value if the text looks oddly spaced. Kerning pairs allows individual pairs of characters to be uniquely spaced. This is helpful when it comes to characters with common spacing errors such as with A's, I's L's and O's. Click the *Kerning Pairs* button to activate this feature.

Line height is the vertical space between each line. The baseline is the measurement from the absolute top of the characters to the bottom where the characters rest. Adjust these in tandem to get appropriate vertical spacing.

Space width is the width of a single space in pixels. Tab spacing is calculated as a multiple of the space width.

# Saving

Click *Save Settings* to save the Image Font settings to be loaded later. Otherwise, select a *Save File* path and click *Generate Font*. You will also be prompted to name the font. Names must begin with a letter and must only contain letters, numbers, underscores, and hyphens.

The font may still not be spaced as you wish. You can open the exported FNT file in a text editor and make modifications to the values manually. See the [AngelCode Bitmap Font documentation](http://www.angelcode.com/products/bmfont/doc/file_format.html).