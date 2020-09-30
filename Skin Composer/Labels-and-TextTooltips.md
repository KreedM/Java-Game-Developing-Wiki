Labels make working with fonts easier within your UI's. They give you options for color, alignment, and wrapping. These are great for simple text in your menus or for stylized titles. You can extend the functionality of a simple label via the TypingLabel third party library.

# LabelStyle

Labels take a [BitmapFont](https://github.com/raeleus/skin-composer/wiki/Fonts) and an optional Color and background [Drawable](https://github.com/raeleus/skin-composer/wiki/Drawables). For your basic labels, design your font with a white fill. This way you can tint the label via a style or through code. Background is seldom used, however consider using it if you want a label to stand out like a special heading.

If you want to apply an underline to a font, a great technique for that is to set an underline 9patch to the background.

Choose a color for your style and make a new style for every predetermined color you wish to use. Or, you can leave the color field blank. This allows you to dynamically color the label via the Color Markup Language as seen below.

# Label Layout

Don't confuse label alignment with cell alignment. These features will work in tandem to achieve different effects. For example, the cell is aligned to the right.

Now the text is aligned to the right.

To use multiple colors in the same label, use the [Color Markup Language](https://github.com/libgdx/libgdx/wiki/Color-Markup-Language) in the String text.

## Ellipses and Word Wrap

You need to design your UI's to accommodate different screen resolutions and allow for independent resizing of widgets. That means your labels are going to need to resize too. If you do nothing, they will run off the screen or overlap other widgets. An easy solution is to just cut off the text and use ellipses to indicate that there is more text to be read.

Ellipses are fine, but that insufficient for important data that the user needs to see. You can automatically wrap to new lines via the word wrap feature.

# TextTooltip

TextTooltipStyle has very simple requirements. All you need to do is provide a valid LabelStyle and provide a background Drawable. An optional parameter is the wrapWidth. Set a width to automatically make text go to the next line, or set it to 0 for no wrap.

You don't add the TextTooltip to the Table or the Stage. Instead, you add it like an EventListener to your widgets. When you mouse over, it fades in as you would expect.

# TypingLabel

TypingLabel lets you achieve animated effects while keeping the same functionality as a typical Label. It's compatible across all backends. You must add the dependency to your project.

See the [TypingLabel](https://github.com/rafaskb/typing-label) documentation for more details.