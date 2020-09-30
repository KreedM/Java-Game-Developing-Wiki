TextField is a great way to get String input from the user like a typical form. Tab traversals work and it can be set to behave like a password field as well. TextArea is the multi-line sibling to TextField.

# TextFieldStyle

TextFields require a font and a "fontColor" to represent the text that the user types. There is also a separate field called "messageFont". Consider this a way to show hint text in the TextField when the user hasn't entered anything into the box. Typically, you'll choose a light color for the "messageFontColor" and a darker, more vivid color for the "fontColor".

You should set a background to represent the box for the TextField. Users often associate white boxes with a dark outline as something you can type in. Make sure to accommodate for some padding to ensure that the text doesn't seem squished against the border in your [NinePatch](https://github.com/raeleus/skin-composer/wiki/Nine-Patches).

When the user clicks in your TextField, this sets the Stage's keyboard focus to the widget and activates the "focusedBackground". This can have a brighter border to make it stand out from the rest of the fields in your scene. Or you can simply not specify it and it will always show the default Drawable background. Another optional background is the "disabledBackground". This is usually a washed out, gray fill to indicate that the field can not be typed in. Don't forget to also set corresponding colors for "focusedFontColor" and "disabledFontColor".

The "cursor" field represents the insertion point that shows the user's place in the text. This works best with a single pixel width line. The height will automatically adjust to the height of the font. You can experiment with different sized cursors, though it will be challenging to get it to be placed exactly where you want it to go.

To indicate what characters the user has highlighted, you need to specify the "selection" field. This is drawn behind the text, so you can feel free to use an opaque drawable. A 1x1 pixel of a vibrant color that contrasts with the background and the text is usually enough unless you want to draw something ornate.

TextFieldStyle can be applied to both TextField and TextArea. You just have to make sure that your TextField Drawable can be stretched horizontally and vertically.

# Layout

TextFields have a default preferred width of 150. Make sure to specify a width if you want a certain size.

You can do some text manipulations to the text entered in the field.

Password mode hides the text you type with a character of your choosing. This is usually the asterisk or the bullet symbol if you chose to add that to your font's supported characters.

If you want to toy around with the default behaviors, you can change the key repeat time. See the static fields "keyRepeatTime" and "keyRepeatInitialTime".

Using TextArea is nearly identical.

TextFields are automatically programmed to traverse to the next field to the right and below. If you don't like this order, you can override this functionality.

You can simply disable focusTraversal if you don't need it.