# Scene2D Primer
In this tutorial, we are going to learn the basics of Scene2D, Skin, Drawable, and Table layout.

## Stage and Actors
A Stage holds all of the elements of our interface. With the analogy of a play at the theater, we will call these elements Actors. Each Actor takes a position on the stage as X and Y coordinates with position (0,0) being the bottom left corner of the stage. Every Actor has its own width, height, rotation, and other various properties. These properties can be changed during the course of your "play". We can actually implement this most basic usage of Scene2D as the framework for our entire game. However, most developers use Scene2D for UI exclusively. Widgets, like TextButton for example, are built on top of the Actor class. This can result in some confusion as you'll see down the line.

## Drawables
Widgets, like Button, Slider, ProgressBar and more, are largely composed of Drawable instances. [Drawable](https://github.com/raeleus/skin-composer/wiki/Drawables) is a kind of class that knows how to render itself onto the screen. The most basic of these is the TextureRegionDrawable which is just a static image loaded from a TextureAtlas. You can stretch this image to whatever size you need, but you'll notice that the edges start to get blurry. A better kind of Drawable is the [NinePatchDrawable](https://github.com/raeleus/skin-composer/wiki/Nine-Patches). This preserves the edge details as you manipulate it. There are many variations of Drawable to meet different needs.

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-24.png)

## Styles and Skins
Drawable instances, among other things, are collected and applied to a widget via a Style. Style is a very basic class unique to each widget you use. It is necessary to create a Style before you construct a widget. For instance, a TextButton would require a TextButtonStyle. This can be done programmatically or loaded from file. Skin makes this process much easier as it helps organize all styles and assets related to your UI. Typically, you'll load a Skin from a JSON text file. Sample skins can be found [here](https://ray3k.wordpress.com/artwork/).

## Stage Example
The following is an example of a single TextButton added directly to the Stage. Ignore the boiler plate code for now. This example requires the [Metal UI skin](https://ray3k.wordpress.com/metal-ui-skin-for-libgdx/). Ensure that it is copied to your assets folder.
```java
package com.ray3k.demonstration;

import com.badlogic.gdx.ApplicationAdapter;
import com.badlogic.gdx.Gdx;
import com.badlogic.gdx.graphics.GL20;
import com.badlogic.gdx.scenes.scene2d.Stage;
import com.badlogic.gdx.scenes.scene2d.ui.Skin;
import com.badlogic.gdx.scenes.scene2d.ui.TextButton;
import com.badlogic.gdx.utils.Align;
import com.badlogic.gdx.utils.viewport.ScreenViewport;

public class Core extends ApplicationAdapter {
    private Skin skin;
    private Stage stage;
    
    @Override
    public void create () {
        skin = new Skin(Gdx.files.internal("metalui/metal-ui.json"));
        
        stage = new Stage(new ScreenViewport());
        Gdx.input.setInputProcessor(stage);
    
        //Begin layout
        TextButton textButton = new TextButton("Hello", skin);
        stage.addActor(textButton);
    }
    
    @Override
    public void render () {
        Gdx.gl.glClearColor(.9f, .9f, .9f, 1);
        Gdx.gl.glClear(GL20.GL_COLOR_BUFFER_BIT);
        stage.act();
        stage.draw();
    }
    
    @Override
    public void resize(int width, int height) {
        stage.getViewport().update(width, height, true);
    }
    
    @Override
    public void dispose () {
        skin.dispose();
        stage.dispose();
    }
}
```
![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-01.png)

To reposition it the middle of the screen, you can change the code to the following:
```java
...
        //Begin layout
        TextButton textButton = new TextButton("Hello", skin);
        //Put the button in the middle of the screen
        textButton.setPosition(stage.getWidth() / 2, stage.getHeight() / 2, Align.center);
        stage.addActor(textButton);
...
```
![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-02.png)

Things get a little hairy when you try position multiple widgets:
```java
...
        //Begin layout
        TextButton textButton = new TextButton("Hello", skin);
        textButton.setPosition(stage.getWidth() / 4, stage.getHeight() / 2, Align.center);
        stage.addActor(textButton);
    
        textButton = new TextButton("Hello", skin);
        textButton.setPosition(stage.getWidth() / 4 * 2, stage.getHeight() / 2, Align.center);
        stage.addActor(textButton);
    
        textButton = new TextButton("Hello", skin);
        textButton.setPosition(stage.getWidth() / 4 * 3, stage.getHeight() / 2, Align.center);
        stage.addActor(textButton);
...
```
![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-03.png)

## Table Layout

**The above example is the wrong way to do UI.** Manually setting coordinates gets complicated pretty quickly when you start adding more elements. You would also have to reposition every widget manually if the screen size changes. Thankfully we have Table layout to help us out. Table simplifies spacing and alignment. Think of an Excel spreadsheet with rows and columns. Each item added to this "spreadsheet" is automatically positioned relative to other items.

A root Table is necessary to arrange your content. This Table will cover the entire screen and all widgets will be added to it:
```java
package com.ray3k.demonstration;

import com.badlogic.gdx.ApplicationAdapter;
import com.badlogic.gdx.Gdx;
import com.badlogic.gdx.graphics.GL20;
import com.badlogic.gdx.scenes.scene2d.Stage;
import com.badlogic.gdx.scenes.scene2d.ui.Skin;
import com.badlogic.gdx.scenes.scene2d.ui.Table;
import com.badlogic.gdx.utils.viewport.ScreenViewport;

public class Core extends ApplicationAdapter {
    private Skin skin;
    private Stage stage;
    
    @Override
    public void create () {
        skin = new Skin(Gdx.files.internal("metalui/metal-ui.json"));
        
        stage = new Stage(new ScreenViewport());
        Gdx.input.setInputProcessor(stage);
    
        Table root = new Table();
        root.setFillParent(true);
        stage.addActor(root);
        
        //Begin layout
    }
    
    @Override
    public void render () {
        Gdx.gl.glClearColor(.9f, .9f, .9f, 1);
        Gdx.gl.glClear(GL20.GL_COLOR_BUFFER_BIT);
        stage.act();
        stage.draw();
    }
    
    @Override
    public void resize(int width, int height) {
        stage.getViewport().update(width, height, true);
    }
    
    @Override
    public void dispose () {
        skin.dispose();
        stage.dispose();
    }
}
```

### Adding Content

To add content, we need to simply use the Table#add() method. Be careful to use add() and not addActor(). addActor() is a carry over from the Actor class that you are not meant to use:
```java
...
        //Begin layout
        TextButton textButton = new TextButton("Hello", skin);
        root.add(textButton);
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-04.png)

You can add more content to the row by using Table#add() again. To go down to a new row, use Table#row():
```java
...
        //Begin layout
        TextButton textButton = new TextButton("Hello", skin);
        root.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
        
        //start the next row.
        root.row();
        textButton = new TextButton("Goodbye", skin);
        root.add(textButton);
    
        textButton = new TextButton("Goodbye", skin);
        root.add(textButton);
    
        textButton = new TextButton("Goodbye", skin);
        root.add(textButton);
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-05.png)

### Sizing Content

The default behavior of Table is to size all children to their minimum size. You must remember that when you use Table, you no longer set the dimensions of the widget directly. You set the size of the cell, then the cell sizes the contents. Do not use setWidth() or setHeight().
```java
...
        //Begin layout
        TextButton textButton = new TextButton("Hello", skin);
        root.add(textButton).width(120);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton).width(120);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton).width(120);
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-06.png)

Normally, you wouldn't want to manually set the size of widgets. That doesn't allow your UI to adapt to different screen sizes. Instead, you can have the children grow to the available space:
```java
...
        //Begin layout
        TextButton textButton = new TextButton("Hello", skin);
        root.add(textButton).growX();
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton).growX();
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton).growX();
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-07.png)

Grow comes in three different varieties. growX() is horizontal space, growY() is vertical space, and grow() is both. Additionally, grow is actually two separate commands: fill and expand. Cell#expand() tells the cell to take up as much available space as possible. If you want to just evenly space the buttons across the screen, you can do the following:
```java
...
        //Begin layout
        TextButton textButton = new TextButton("Hello", skin);
        root.add(textButton).expand();
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton).expand();
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton).expand();
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-23.png)

If there are more than one cell with expand activated, they will split the space equally. Cell#fill() tells the contents of the cell to fill the entire size of the cell. These methods come in the x/y flavors as well and can be used in any combination:
```java
...
        //Begin layout
        TextButton textButton = new TextButton("Hello", skin);
        root.add(textButton).grow();
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton).expandX();
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton).expandX().expandY().fillX().fillY();
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-08.png)

### Alignment
Table automatically aligns all children to the center of the cell. If you want to change the alignment, use the top(), left(), bottom(), or right() methods.
```java
...
        //Begin layout
        TextButton textButton = new TextButton("Hello there pardner!", skin);
        root.add(textButton).top();
    
        textButton = new TextButton("Hello\nmy good friend\nfrom soccer practice\nwho is nice", skin);
        root.add(textButton);
    
        textButton = new TextButton("Hello my neighbor", skin);
        root.add(textButton).bottom();
        
        root.row();
        textButton = new TextButton("Hello", skin);
        root.add(textButton).right();
        
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton).left();
    
        root.row();
        textButton = new TextButton("Hello", skin);
        root.add(textButton).left();
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton).right();
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-09.png)

This makes sense when you have multiple cells with content of varying sizes. What if you have a single cell that you want to align to the left?
```java
...
        //Begin layout
        TextButton textButton = new TextButton("Hello", skin);
        root.add(textButton).left();
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-10.png)

The above code does not appear to work. You have to expand the cell first, then you can align as you see fit:
```java
...
        //Begin layout
        TextButton textButton = new TextButton("Hello", skin);
        root.add(textButton).expandX().left();
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-11.png)

### Spacing
The confusing thing with Table is that there are two ways to specify spacing between cells. You can use pad() or space(). The difference is that padding adds up. If you pad two buttons by 10, the total distance between the two is 20. If you set space to 10 for both of them, the total distance is 10. In addition to that, you can pad or space a specific edge of the cell: padLeft(), padRight(), padTop(), padBottom(), spaceLeft(), spaceRight(), spaceTop(), spaceBottom().
```java
...
        //Begin layout
        TextButton textButton = new TextButton("Hello", skin);
        root.add(textButton).space(10);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton).space(10);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton).space(10);
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-12.png)

```java
...
        //Begin layout
        TextButton textButton = new TextButton("Hello", skin);
        root.add(textButton).pad(10);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton).pad(10);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton).pad(10);
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-13.png)

If you just want padding around your table, there is a simple method for that.

```java
...
        //Begin layout
        root.pad(50);
        TextButton textButton = new TextButton("Hello", skin);
        root.add(textButton).grow();
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-15.png)

### Convenience Methods
Java is already verbose, but having to set the same properties for every cell is too much. Table#defaults() is the answer for this. Any properties applied to the defaults cell is applied to every new cell created. The defaults can then be cleared to return to regular behavior.
```java
...
        //Begin layout
        root.defaults().grow();
        TextButton textButton = new TextButton("Hello", skin);
        root.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
        
        root.row();
        root.defaults().reset();
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-14.png)

An easy technique to copy widget sizes is to use the uniform() method. This is great for making buttons look even without explicitly setting their size or calling grow() on all of them. You can even do this with an empty cell to keep spacing even.
```java
...
        //Begin layout
        TextButton textButton = new TextButton("A very long button! Indeed!", skin);
        root.add(textButton).uniform().fill();
        
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton).uniform().fill();
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-16.png)

Sometimes you don't want to use the exact same size of another cell, but a percentage of it. The Value class can calculate this for you. This can be passed directly to the cell methods where you would normally provide a number.
```java
...
        //Begin layout
        TextButton bigButton = new TextButton("Hello\nGood day\nTop of the morning\nGreetings\nHola\nSalutations", skin);
        root.add(bigButton);
        
        TextButton textButton = new TextButton("Hello", skin);
        root.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton).height(Value.percentHeight(.5f, bigButton));
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-17.png)

I dislike having a ton of variables to manage. You can name a widget with a String value instead. This is helpful when working across multiple classes.
```java
...
        //Begin layout
        TextButton textButton = new TextButton("Hello", skin);
        textButton.setName("firstButton");
        root.add(textButton);
        
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
        
        textButton = stage.getRoot().findActor("firstButton");
        textButton.setText("I changed the text of the first button.");
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-18.png)

### Advanced Layout
So far this tutorial has covered very simple layouts and that is typically recommended for regular menus. However, you may want to explore more complex layouts for HUD's or utilities. You might want to have a title span over two columns of buttons for example. This can be achieved with Cell#colspan().
```java
...
        //Begin layout
        TextButton textButton = new TextButton("Hello", skin);
        root.add(textButton).colspan(2).fill();
        
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
        
        root.row();
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-19.png)

What if you wanted a cell to span multiple rows? Unfortunately, this is not a feature of table. You'll want to use nested tables to achieve that effect.
```java
...
        //Begin layout
        TextButton textButton = new TextButton("Hello", skin);
        root.add(textButton).fill();
        
        Table table = new Table();
        root.add(table);
        
        textButton = new TextButton("Hello", skin);
        table.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        table.add(textButton);
        
        table.row();
        textButton = new TextButton("Hello", skin);
        table.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        table.add(textButton);
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-20.png)

In fact, you want to use nested tables whenever a group of widgets would impact the flow of other widgets in the Table.
```java
...
        //Begin layout
        root.defaults().space(10).fillX();
        TextButton textButton = new TextButton("Hello", skin);
        root.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
        
        root.row();
        Table table = new Table();
        root.add(table).colspan(2);
        
        table.defaults().space(10);
        textButton = new TextButton("Hello", skin);
        table.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        table.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        table.add(textButton);
        
        root.row();
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
    
        textButton = new TextButton("Hello", skin);
        root.add(textButton);
...
```

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-21.png)

## Further Steps
This tutorial is not a comprehensive guide to Scene2D.UI. The goal was to introduce basic layout techniques and enough background to allow you to follow along in the rest of this series. Please continue with the next chapter, [From the Ground Up 01 - Buttons](https://github.com/raeleus/skin-composer/wiki/From-the-Ground-Up-01:-Buttons).

### Practice
Experimentation is critical when learning Scene2D.UI. Try to make the following layout on your own using what you've learned from the lesson.

![](https://ray3k.files.wordpress.com/2019/10/from-the-ground-up-00-22.png)

### [Read the official libGDX wiki on Table layout](https://github.com/libgdx/libgdx/wiki/Table)
