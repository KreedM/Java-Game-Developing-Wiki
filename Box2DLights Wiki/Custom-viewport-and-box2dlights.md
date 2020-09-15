Support for custom viewports, e.g. resolution independend viewports is available only since box2dlights 1.3, if you are using older version, please update to at least 1.3 to enable support.

For the correct viewport rendering, if not using default values, you should set it manually:

`
rayHandler.useCustomViewport(x, y, width, height);
`

Where the x, y, width and height are the values of your custom viewport.
For correct rendering after the window is resized or viewport sizes changed, you should call this method manually again. E.g. inside of `resize(width, height)` method.

When no more needed, it could be reset back to default:

`
rayHandler.useDefaultViewport();
`
