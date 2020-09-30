![](https://ray3k.files.wordpress.com/2019/05/untitled-1.png)

# The Man Who Killed Hitler...

Progress Bars are easy to conceptualize, however they prove to be quite the task master when it comes to actually building them. Take it from the man who stalked the world's deadliest prey and lived to tell the tale... Follow this guide to learn several techniques to making successful progress bars for libGDX in Skin Composer.

## Rectangular Progress Bars
<img align="right" src="https://ray3k.files.wordpress.com/2019/05/rectangular-background.9.png"> <img align="right" src="https://ray3k.files.wordpress.com/2019/05/rectangular-knobbefore.png">Rectangular progress bars are your most basic design and are my recommendation for most uses. These consist of a single background graphic and an image for the field "knobBefore". The background can be a nine patch, but the knobBefore should be a plain, old, run-of-the-mill, normal image. The nine patch contents should be sized precisely to accommodate the height of the knobBefore image. Ideally, the knobBefore should be one pixel wide.

<img align="left" src="https://ray3k.files.wordpress.com/2019/05/ezgif.com-crop.gif">In Skin Composer, you can specify the minWidth of drawables. Remember to specify that the minWidth of your knobBefore is 0 to ensure that the bar will not show anything when the value is 0.<img align="right" src="https://ray3k.files.wordpress.com/2019/05/ezgif.com-crop-1.gif">

Pros: Easy to make and scales to any size as you see fit.

Cons: Looks very plain, rigid.

Rating: OKAY, YEAH!

## Nine Patch Progress Bars

<img align="right" src="https://ray3k.files.wordpress.com/2019/05/nine-patch-knobbefore.9-1.png"> <img align="right" src="https://ray3k.files.wordpress.com/2019/05/nine-patch-background.9-1.png">This technique is more of a cautionary tale. One might like the simplicity of the rectangular progress bar, but aspire to the greatness of the rounded progress bar. That's fine, but you'll notice using a normal graphic leads to distortion. <img align="left" src="https://ray3k.files.wordpress.com/2019/05/y8ff2o7k3k.gif">The solution may seem simple: make a nine patch for knobBefore. In theory, the ends are supposed to stay static while the center gets stretched. You forget you're using libGDX. libGDX's implementation of nine patch is not perfect. For instance, when you set your progress bar it will look all sorts of hideous. So then you'll want to try messing around with knobs and other hacky solutions. It doesn't work because I know from experience. Do not fall into the spiral trap of trying to make your knobBefore a nine patch. <img align="right" src="https://ray3k.files.wordpress.com/2019/05/ct4t8arq1w.gif"> 

Pros: None. Perhaps it should get an "A" for effort.

Cons: Completely unusable in every measure.

Rating: A flying, flipping, FAILURE

## Tiled Progress Bars

<img align="right" src="https://ray3k.files.wordpress.com/2019/05/tiled-knobbefore.png"> <img align="right" src="https://ray3k.files.wordpress.com/2019/05/tiled-background.png"> Now this is the juice. TiledDrawable is coming back and in a very big way. If you want to make a rounded progress bar, this is how to do it. Draw your background graphic like usual but at the size you want it to appear in your UI. Draw your knobBefore as if it was the background graphic, but what it would look like if it was completely full. <img align="left" src="https://ray3k.files.wordpress.com/2019/05/ezgif.com-crop-2.gif">It is very important that both of these images are exactly the same size and not nine patches.

<img align="right" src="https://ray3k.files.wordpress.com/2019/05/ay0d8h5hpb.gif">In Skin Composer, create a TiledDrawable for the knobBefore and ensure that it was a minWidth of 0. That's it. But why does it look so crummy in the preview? This technique has a major downside: it can not be scaled at all. It has to be sized to the precise size of the original graphic. So resizing the preview, it looks fantastic.<img align="right" src="https://ray3k.files.wordpress.com/2019/05/qiqailngd4.gif">

And in your code, you need to make sure to size it correctly too. Outstanding!

Pros: Looks the best just like a real progress bar should.

Cons: Can not be scaled and uses up more texture space.

Rating: Legendary!

# ...and then the Progress Bar

So maybe you killed Hitler years ago, but did that stop the war? The following are some advanced techniques to look into when you're eventually conscripted by the Canadian government to pursue and eradicate a plague-ridden monstrosity that threatens all of mankind as we know it. 

## Circular Progress Bars

There is no simple widget to make a circular progress bar, [so I made my own](https://ray3k.wordpress.com/cloud-form-ui-skin-for-libgdx/). This technique can be used to achieve a number of different effects, so it's best used to deconstruct and derive a solution pertinent to your use.

![](https://ray3k.files.wordpress.com/2019/05/ezgif.com-crop-3.gif)

## Ten Patch Progress Bars

Remember how I said to not use nine patches for your knobBefore graphic? Well, [Ten Patch](https://github.com/raeleus/TenPatch) is my answer to the shortcomings of vanilla libGDX. Notice that you can decrease the size of a TenPatchDrawable to 0 without any nasty effects. It even has options to tile if you so choose. It's basically the best solution if you don't mind using a third party library in your project. The editor is integrated into Skin Composer, check out the [instructions here](https://github.com/raeleus/skin-composer/wiki/Ten-Patches).

![](https://ray3k.files.wordpress.com/2019/05/ezgif.com-crop-4.gif)