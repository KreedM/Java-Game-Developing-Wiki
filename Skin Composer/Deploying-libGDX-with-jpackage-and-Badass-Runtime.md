The days of expecting your user base to have Java installed on their computers is long over. Even in its heyday, it was never known if your client has the latest JRE or even if they knew how to configure it correctly. It is now expected to deliver your apps with the targeted JDK bundled with an executable. Thankfully, the Java people have created JPackage (formerly JavaPackager and JPackager) to meet this need. This is a modern alternative for Packr available since JDK 14. The resulting package will be minimized in size, ready for distribution to your users. This guide will show you how to integrate JPackage into your libGDX game via Gradle.

## Download OpenJDK 14 or later

This technique requires JDK 14 or later as jpackage is a recent java feature. Your project does not need to have a source level of 14, however.

* Download OpenJDK and install: https://adoptopenjdk.net/
* _Alternative_: Install OpenJDK via IntelliJ Idea. File >> Project Structure >> SDKs. Click the plus sign and click Download JDK. Select AdoptOpenJDK as the vendor and choose the most recent Java version. Note where it is installed.

## Add Badass Runtime to your project

Badass Runtime is a Gradle plugin that automatically configures and implements JPackage. It makes packaging your app very convenient from the IDE and the command line. However, it will require some modifications to your build.gradle.

* Upgrade Gradle to 6.4
    * In your root project, open gradle/wrapper/gradle-wrapper.properties
    * Change the distributionUrl line to `distributionUrl=https\://services.gradle.org/distributions/gradle-6.4-bin.zip`
* Modify the desktop build.gradle
    * open desktop/build.gradle
    * To enable Badass Runtime, add the plugins line at the top:
```groovy
plugins { id 'org.beryx.runtime' version '1.8.4' }
```
* Badass expects there to be a mainClassName to be defined. Simply modify 
```groovy
project.ext.mainClassName = "com.mygdx.game.desktop.DesktopLauncher"
```
to 
```groovy
mainClassName = "com.mygdx.game.desktop.DesktopLauncher"
```
* To add OS specific instructions, we need to determine the OS of the system. Add the following after the main class line:
```groovy
def osName = System.getProperty('os.name').toLowerCase(Locale.ROOT)
```
* Badass depends on the application plugin, causing a name conflict with the "run" task. Rename your run task to something cheeky like "runGame" instead.
* Badass expects the distributable jar to be exported to "lib" instead of "libs". Inside of the "dist" task, add the following line: 
```groovy
destinationDirectory = file("$buildDir/lib")
```
* To make the packaging process more convenient, add the following line after the dist task to make jpackage update your game's build first: 
```groovy
jpackageImage.dependsOn dist
```
* The runtime block configures jpackage for minimization and various other options. Add the following at the end of the build.gradle:
```groovy
runtime {
    options = ['--strip-debug',
               '--compress', '2',
               '--no-header-files',
               '--no-man-pages',
               '--strip-native-commands',
               '--vm', 'server']
    modules = ['java.base' ,
               'java.desktop',
               'jdk.unsupported']
    distDir = file(buildDir)

    jpackage {
        //jpackageHome = '/usr/lib/jvm/open-jdk'
        mainJar = dist.archiveFileName.get()
        if (osName.contains('windows')) {
            imageOptions = ["--icon", file("../icons/icon.ico")]
        } else if (osName.contains('linux')) {
            imageOptions = ["--icon", file("../icons/icon.png")]
        } else if (osName.contains('mac')) {
            imageOptions = ["--icon", file("../icons/icon.icns")]
        }
    }
}
```
* In your project root, create a new folder called "icons". Save your application icons here: icon.ico (Windows), icon.png (Linux), icon.icns (Mac)
* Run the desktop:jpackageImage task. You have created a native distribution matching your host OS in desktop/build/jpackage. Repeat this task in each OS you want to support.
* Note: If Badass does not detect the JDK 14 installation, you may need to configure the jpackageHome value in the jpackage block. Uncomment the line and point it to the path of your installation.
* See more configuration options here: https://badass-runtime-plugin.beryx.org/releases/latest/  
See official jpackage documentation and features here: https://openjdk.java.net/jeps/343

## Pros and Cons in relation to Packr
👎 You must be running the target OS to create an associated executable. For example, to create a Mac .app bundle, you must be running a Mac OSX computer.  
👍 Packr has no option for an application icon in Windows. Hacking the EXE to add an icon results in a false-positive detection in various virus scanners. jpackage has no such issue.  
👎 Packr can make slightly smaller packages if you pass the correct settings.  
👍 Packr only supports up to JDK 8. You have the freedom, nae, the duty to use the latest version of Java with jpackage.  
👎 jpackage does not make you toast. Packr doesn't either, but it has a fashionable, hipster name with a missing "e" like Grindr.  
👍 Suave developers prefer the slow, clean drag of jpackage. The crisp taste goes down smooth and filters out the additives. Try it today!

## Source
See an example of the desktop build.gradle here: https://gist.github.com/raeleus/8adc0f1d6c662da8dc01c73738ccdf0b