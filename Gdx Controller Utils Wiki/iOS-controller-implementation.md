To use the controller implementation in your iOS project, add the dependency to your build.gradle file's ios section:

        compile "de.golfgl.gdxcontrollerutils:gdx-controllers-iosrvm:$cuversion"


Change your IOSLauncher to use the initialize the iOS Controller implementation. It is important to initialize it not before your game's create() method is called, because Gdx.app reference might not be set. On the other hand, you must initialize it before your first call to `Controllers.getControllers`. So best place is a overriden create method:

    GdxGame game = new GdxGame() {
        @Override
        public void create() {
            IosControllerManager.initializeIosControllers();
            super.create();
        }
    }
    return new IOSApplication(game, config);

When done, MFI controllers will work in your game.

### Adding support for iCade controllers
MFI controllers are a good thing, however, most players don't own an MFI controller. There's a semi-standard available for using normal Bluetooth controllers on iOS that are supported by 8Bitdo and iPega devices: the [iCade mode](https://www.raywenderlich.com/2958-adding-icade-support-to-your-game). Basically, controllers in iCade mode pretend to be a Bluetooth keyboard and send key events. The iOS controller implementation can handle these events, but you need some more changes in your ios project. That's why iCade support is not enabled by default.

To enable iCade controller support, add the following line after calling `initializeIosControllers()`:

    IosControllerManager.enableICade((IOSApplication) Gdx.app).getUIViewController(), Selector.register("keyPress:"));

This will call some iOS methods that will send all iCade key events to your game's view controller's `keyPress` selector. The normal libGDX backend's view controller does not have such a selector. From 1.9.10 on, you can add it by defining a new `MyUIViewController` class:

```java
public class MyUIViewController extends IOSGraphics.IOSUIViewController {
    protected MyUIViewController(IOSApplication app, IOSGraphics graphics) {
        super(app, graphics);
    }


    @Callback
    @BindSelector("keyPress:")
    @TypeEncoding("v@:@:@")
    public static void keyPress(UIViewController self, Selector sel, UIKeyCommand sender) {
        IosControllerManager.keyPress(sender);
    }
}
``` 

and inject it in your `IOSLauncher`'s `createApplication()` method:

        IOSApplication app = new IOSApplication(game, config) {
            @Override
            protected IOSGraphics.IOSUIViewController createUIViewController(IOSGraphics graphics) {
                return new MyUIViewController(this, graphics);
            }
        };
        return app;

Prior to 1.9.10, that's not possible, you can [use an own iOS backend build](https://github.com/MrStahlfelge/gdx-backends/)

That's a lot of effort, but you'll have a unique selling point for your game.