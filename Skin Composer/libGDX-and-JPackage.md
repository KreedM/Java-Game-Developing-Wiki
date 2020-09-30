The days of expecting your user base to have Java installed on their computers is long over. Even in its heyday, it was never known if your client has the latest JRE or even if they knew how to configure it correctly. It is now expected to deliver your apps with the targeted JDK bundled with an executable. Thankfully, the Java people have created [JPackage](https://jdk.java.net/jpackage/) (formerly JavaPackager and JPackager) to meet this need. This is a modern replacement for [Packr](https://github.com/libgdx/packr) that is compatible with the latest versions of OpenJDK. This guide will show you how to install JPackage and integrate it into your libGDX game via Gradle. These steps are subject to change as JPackage is an early access tool.

# Download JPackage

* Download JPackage from [java.net](https://jdk.java.net/jpackage/) for your OS.
* Unzip the files. For convenience, place the files in your game's desktop project: `<project name>/desktop/jpackage/`

# Add the Gradle Tasks

The following tasks need to be added to the build.gradle file in your desktop project:

* Near the top of your build.gradle, define the following variable:
```java
def osName = System.getProperty('os.name').toLowerCase(Locale.ROOT)
```

* jpackage initiates the command to build your desktop game and run JPackage. Notice the optional commands to add a splash screen to your game and to run LWJGL3 on Mac. jpackage depends on createRuntime to create a minified JDK runtime. Modify as necessary:
```java
// creates a slim JDK runtime for distribution
task createRuntime(type: Exec) {
    doFirst() {
        project.delete("${buildDir}/runtime")
    }
    String runtimePath = "${buildDir}/runtime"

    workingDir project.projectDir
    commandLine = [
            "${project.projectDir}/jpackage/bin/jlink",
            '-p', "${project.projectDir}/jpackage/jmods",
            '--add-modules', 'java.base,java.desktop,jdk.unsupported',
            '--strip-debug',
            '--no-header-files',
            '--no-man-pages',
            '--strip-native-commands',
            "--vm=server",
            "--compress=2",
            '--output', runtimePath
    ]
}

// creates application bundle (executable + runtime)
task jpackage(type: Exec, dependsOn: dist) {
    doFirst() {
        project.delete("${buildDir}/distribution/$project.appName")
    }
    dependsOn createRuntime
    workingDir project.projectDir
    def commands = [
            "${project.projectDir}/jpackage/bin/jpackage",
            '--output', "${buildDir}/distribution",
            '--input', "${buildDir}/libs",
            '--name', project.appName,
            '--main-class', project.mainClassName,
            '--main-jar', jar.archiveFile.get().asFile.getName(),
            '--runtime-image', "${buildDir}/runtime"
    ]
    
    if (osName.contains('windows')) {
        commands << '--icon'
        commands << "${project.projectDir}/logo.ico"
        commands << '--java-options'
        commands << "-splash:splash.png"
    } else if (osName.contains('linux')) {
        commands << '--icon'
        commands << "${project.projectDir}/logo.png"
        commands << '--java-options'
        commands << "-splash:splash.png"
    } else if (osName.contains('mac')) {
        commands << '--icon'
        commands << "${project.projectDir}/logo.icns"
        commands << '--java-options'
        commands << "-XstartOnFirstThread"
    }
    
    commandLine = commands
}
```

# Distributing Your App

You must create appropriate icon files for your app in order for it to build properly. Use your favorite image editing software to create .ico, .png, and .icns for your appropriate OS. Otherwise you must remove the options to bundle icons with your executables.

* Copy your icon files to the root folder of your desktop project.
* Run Gradle task jpackage.
* Copy your splash.png file to `<project name>/desktop/build/distribution/<project name>/app`
* Test your bundle by running the executable file in `<project name>/desktop/build/distribution/<project name>`
* Zip the folder `<project name>/desktop/build/distribution/<project name>` and distribute to your audience.

# Pros and Cons

* You must be running the target OS to create an associated executable. For example, to create a Mac .app bundle, you must be running a Mac OSX computer.
* This technique has less false positives in virus scanning software compared to Packr.
* JPackage allows you to specify an executable icon for Windows while Packr does not.
* JDK compatibility with java versions up to 13 in JPackage. Packr only supports up to 8.
* Packr may make a marginally smaller package size if you aggressively minimize all classes.

# Source

* See a working example in Skin Composer: https://github.com/raeleus/skin-composer/blob/master/desktop/build.gradle