FreeType fonts are dynamically generated at runtime and can be adjusted to match the scaling requirements of your game on the fly. They have the disadvantage of added complexity and incompatibility with the GWT/HTML5 backend. FreeType fonts can be implemented in your code or loaded from a skin file with a custom serializer.

# Create a FreeType Font Placeholder

If you want to create your FreeType fonts in code but also integrate them into your skin file, it will be necessary to add a placeholder.

* Go to *Project >> Fonts*
* Click on *FreeType font*
* Enter a name for the font to be used throughout the skin
* Select a suitable preview font
* Click *Generate Font*

This font will now be selected in your styles. You can add your own preview fonts by clicking on *Open Preview Folder* and pasting TTF files here. You will need to substitute these fonts in your code. See the steps below.

# Replacing a Placeholder Font in libGDX

The following code snippet details how to replace a FreeType font placeholder that you've selected:

```java
skin = new Skin();

FreeTypeFontGenerator generator = new FreeTypeFontGenerator(Gdx.files.internal("some-font.ttf"));
FreeTypeFontParameter parameter = new FreeTypeFontParameter();
parameter.size = 16;
parameter.color = Color.RED;
BitmapFont font = generator.generateFont(parameter);
skin.add("font", font);

skin.addRegions(new TextureAtlas("Skin-Name.atlas"));
skin.load(Gdx.files.internal("skin-name.json"));
```

The trick is to create your FreeType font first, then load the textures and styles from your Skin.

# Using a Custom Serializer

It may be more appropriate to design your FreeType font in Skin Composer so it will appear exactly as you specify in the editor. libGDX does not have this functionality built in, so you'll have to implement a custom serializer to load the font.

* Go to *Project >> Fonts*
* Click on *FreeType font*
* Enter a name for the font to be used throughout the skin
* Check mark *Use custom serializer and integrate TTF in Skin JSON*
* Click *More Info...*
* Click *Copy FreeTypeSkin code to clipboard*
* Paste the code in the appropriate section of your code

The sample code recognizes FreeType fonts specified in skin files, as they are formatted in Skin Composer, and builds them on the fly. The sample code is repeated here for clarity:

```java
            skin = new FreeTypeSkin(Gdx.files.internal("skin-name.json"));
```

You'll notice that this code will not function until you implement the Stripe Widgets project. Please follow the instructions in the [README](https://github.com/raeleus/stripe/blob/master/README.md) to add it to your build.gradle.

If you use an asset manager to load your skin, you'll have to take a different tack. If you click *Copy FreeTypeSkinLoader code to clipboard*, you'll have the following to paste into your project:

```java
            AssetManager assetManager = new AssetManager();
            assetManager.setLoader(Skin.class, new FreeTypeSkinLoader(assetManager.getFileHandleResolver()));
            assetManager.load("skin-name.json", Skin.class);
```

After your assetManager is done loading, you can access the skin like normal.

```java
            Skin skin = assetManager.get("skin-name.json);
```

# Designing a FreeType Font

FreeType Fonts are created from TTF files. You can download free fonts with permissible licenses from [Google Fonts](https://fonts.google.com/).

* Go to *Project >> Fonts*
* Click on *FreeType Font*
* Enter a name for the font to be used throughout the skin
* Check mark *Use custom serializer and integrate TTF in Skin JSON*
* Click the *Browse* button for *TTF Path* and select the source font
* Choose a characters preset or type in the characters that you want to be generated in the bitmap font
* Choose the font settings that suit the appearance of the text that you desire

For more details on the options available, see the [libGDX wiki](https://github.com/libgdx/libgdx/wiki/Gdx-freetype#how-to-use-gdx-freetype-in-code).

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

# Saving Font

Click *Generate Font* to use the font throughout your skin. If you wish to make changes to this font in the future, click the settings button for the specific font in the Fonts dialog.