The Bitmap Font editor has the advantage of making a clean font that is most compatible with LibGDX and with the least amount of setup. Although the Skin Composer Bitmap Font editor does not have as many effect options as Hiero, it produces a font image that is tightly packed and works much better in a LibGDX skin.

# Create a Bitmap Font

Bitmap Fonts are created from TTF files. You can download free fonts with permissible licenses from [Google Fonts](https://fonts.google.com/).

* Go to *Project >> Fonts*
* Click on *Create FNT*
* Click the *Browse* button for *Source TTF Path* and select the source font
* Click the *Browse* button for *Target FNT Path* and select the save path
* Choose a characters preset or type in the characters that you want to be generated in the bitmap font
* Choose the font settings that suit the appearance of the text that you desire

These are the same options used to create FreeType fonts. For more details on the options available, see the [libGDX wiki](https://github.com/libgdx/libgdx/wiki/Gdx-freetype#how-to-use-gdx-freetype-in-code).

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

# Saving Settings

You can save the settings you have made to apply to future fonts. Click *Save Settings* and choose a file path. You can then click *Load Settings* in the Bitmap Font or FreeType Font editors to load your settings.

# Exporting FNT

Click *Generate Font* to export your bitmap font FNT file. It is advised to keep these in a folder separate from your Skin Composer save path. Also, ensure that there are no images in the path that share the same name as your chosen FNT file.

After entering the FNT path, you will be asked to name the font as it will be used in your skin. Names must begin with a letter and must only contain letters, numbers, underscores, and hyphens.