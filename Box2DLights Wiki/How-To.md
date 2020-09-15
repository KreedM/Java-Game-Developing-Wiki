This needs to be added to your project for integrating box2dlights

In ApplicationListener#create:

    rayHandler = new RayHandler(world);

You can disable shadows with:

    rayHandler.setShadows(false);

You can set the ambient light with
    rayHandler.setAmbientLight(R, G, B, Alfa);
    rayHandler.setBlurNum(3);

This creates a new white point light. RAY_NUM being the number of ray lights (e.g.: 4 is a simple star)

    new PointLight(rayHandler, RAYS_NUM, new Color(1,1,1,1), lightDistance, x, y);

In your render() loop after everything is drawn that you want to be lit:
 
    rayHandler.setCombinedMatrix(camera);
    rayHandler.updateAndRender();

Remember to dispose():

    rayHandler.dispose();

Examples of Box2d Lighting
* Sample code and showcase available on [LibGDX.info](https://libgdx.info/box2d-light-pooled-particle-effects-and-particleemitter2d/)
* For more details, check out this [example](https://github.com/libgdx/box2dlights/blob/master/test/tests/Box2dLightTest.java).