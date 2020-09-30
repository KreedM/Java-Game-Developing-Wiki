Adding custom classes to your project allows you to interact with widgets beyond libGDX's defaults.

# Adding a Custom Class

* Click the plus button next to Class
* Enter the fully qualified name of the class you are adding
* Enter the display name for convenience

If your class depends on Scene2D.UI widgets to operate, check *Declare after UI classes*. If your class implements Drawable and you intend on using it throughout the skin classes, uncheck *Declare after UI classes*. Click *OK*. The name of your class now appears in the *Class* select box.

# Adding Properties to a Custom Class

Properties of a custom class must be the same as the fields that are defined in your code. Your class must also have a no-argument constructor in order for libGDX to deserialize the object. See more details on the [libGDX wiki](https://github.com/libgdx/libgdx/wiki/Reading-and-writing-JSON).

* Click the plus sign in the lower right of the *Style Properties* pane
* Type the name of the field exactly as it is entered in your code
* Choose a property type that best suits the data you are trying to represent

The property type determines what kind of widget will appear in the *Style Properties* panel. A *Color* widget will allow you to select an existing color from your *Colors* list for example. Repeat this process for every property you wish to modify with your styles.

# Filling the Properties of the Custom Style

With the design complete, you may create new styles and fill the properties as you would with the normal Scene2D.UI widgets. As you fill the properties, a rudimentary preview is shown detailing the individual elements you have selected. It is not possible for Skin Composer to render your custom designed widgets. With these steps complete, you will now be able to retrieve your custom class from your skin in code.