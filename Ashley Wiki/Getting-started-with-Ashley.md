The first thing you will want to do is to get Ashley into your project. You have a few options depending on your environment.

## Maven and Gradle

Ashley is available in Maven. The easiest way of going about this is to add the framework as a dependency of an existing [Maven](http://maven.apache.org/) or [Gradle](http://www.gradle.org/) project. There are two kinds of Ashley releases.

* **[Official](https://oss.sonatype.org/content/repositories/releases)**: well tested, stable packages that get released periodically. URL: `https://oss.sonatype.org/content/repositories/releases`
* **[Snapshots](https://oss.sonatype.org/content/repositories/snapshots)**: the result of nightly builds, they are more likely to break in unexpected ways. URL: `https://oss.sonatype.org/content/repositories/snapshots`

### Maven

First, you need to add the desired repository to your `pom.xml` file. Change the URL depending on whether you want releases or snapshots.
```xml
	<repositories>
		<repository>
			<id>sonatype</id>
			<name>Sonatype</name>
			<url>https://oss.sonatype.org/content/repositories/releases</url>
		</repository>
	</repositories>
```
Then you need to add the Ashley dependency.
```xml
	<dependency>
		<groupId>com.badlogicgames.ashley</groupId>
		<artifactId>ashley</artifactId>
		<version>1.7.3</version>
	</dependency>
```
### Gradle

Add the Maven repositories to your `build.gradle` file.
```groovy
    repositories {
        maven { url "https://oss.sonatype.org/content/repositories/snapshots/" }
        maven { url "https://oss.sonatype.org/content/repositories/releases/" }
    }
```
Add the dependency to the `dependencies` element.
```groovy
    dependencies {
        compile "com.badlogicgames.ashley:ashley:1.7.3"
    }
```

### Nightly builds

If you want the bleeding edge version of Ashley, you can get the nightly builds by using the dependency: `com.badlogicgames.ashley:ashley:1.7.4-SNAPSHOT`.

## Working from sources

If you want the most bleeding edge version of Ashley or prefer to make your own modifications, working from sources might be the best option. Follow these steps.

1. Install [Gradle](http://www.gradle.org/downloads)
2. Clone the Git repository `git clone git@github.com:libgdx/ashley.git`
3. Import the `core` and `tests` projects into the IDE of your choice. Using the command line is also an option.
4. Now you can either:
  * Run the `uploadArchives` task and generate three jar files: bytecode, sources and javadocs. They will be in your local Maven repository.
  * Add the `ashley` project as a dependency to yours.

## Using Ashley in a Libgdx project

If you are creating a new Libgdx project from scratch, use the [gdx-setup](http://bitly.com/1i3C7i3) tool and tick the Ashley checkbox.

![](http://i.imgur.com/Dp3bb9P.png)

On the other hand, if you already have a Libgdx project and want to use Ashley you need to add the appropriate repository to the `build.gradle' file as we saw in the previous section. Additionally, you will need to add the following dependencies to your projects.
```groovy
	project(":core") {
		dependencies {
			compile "com.badlogicgames.ashley:ashley:1.7.3"
		}
	}

	project(":android") {
		dependencies {
			compile "com.badlogicgames.ashley:ashley:1.7.3"
		}
	}

	project(":html") {
		dependencies {
			compile "com.badlogicgames.ashley:ashley:1.7.3:sources"
		}
	}
```

If you are targeting the browser, do not forget to update your `GdxDefinition.gwt.xml` and `GdxDefinitionSuperdev.gwt.xml` files with the proper inheritance element.

```xml
<inherits name='com.badlogic.ashley_gwt' />
```