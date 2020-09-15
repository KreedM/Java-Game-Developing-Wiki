In games where entities are constantly created and destroyed the [garbage collector](http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html) might become a problem. Imagine a space shooter where ships are firing projectiles at very high rates and explosions come and go rather fast. There is no way a phone can run such scenario without the occasional and unacceptable GC stall.

Luckily enough, Ashley has been designed with both ease of use and performance in mind. It features a very convenient [pooling system](http://en.wikipedia.org/wiki/Object_pool_pattern) which will solve this problem without too much hassle.

### PooledEngine

First of all, you have to use the [`PooledEngine`](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/core/PooledEngine.html) class instead of the regular `Engine` one. This class adds both entity and component pooling.

The `PooledEngine` constructor is the right place to pass in our pools configuration.

```java
PooledEngine engine = new PooledEngine(entityPoolInitialSize,
									   entityPoolMaxSize,
									   componentPoolInitialSize,
									   componentPoolMaxSize);
```

Each component class will have a dedicated pool with the specified configuration.

### Pooled entities

Instead of instantiating entity objects manually, we now have to do so through our`PooledEngine`.

```java
Entity entity = engine.createEntity();
```

Calling `Engine.removeEntity()` puts the entity back into the pool. In Ashley versions prior to 1.7.0, after returning an entity to the pool, its id will be set to `0`. Valid entities will always have a non zero id. **Entity UUIDs have been removed in versions 1.7.0 and forward**.

```java
if (entity.getId() > 0L) {
	// Entity is valid
}
```

### Pooled components

Your components now need to implement the `Poolable` interface in addition to `Component`, and have a default constructor and `reset()` method. This allows them to be created and to reset their state after they have been used.

```java
public class PositionComponent implements Component, Poolable {
	public float x = 0.0f;
	public float y = 0.0f;
	
	@Override
	public void reset() {
		x = 0.0f;
		y = 0.0f;
	}
}
```

Component instances are also obtained from the engine.

```java
PositionComponent position = engine.createComponent(PositionComponent.class);
```

Components will automatically be returned to their pools once they are removed from their entity. Similarly, entities are returned to the entity pool once they have been removed from the engine.

### Cleaning up

It is possible to remove all unused components and entities from their pools whenever you want to.

```java
engine.clearPools();
```

This will most likely result in a considerable amount of garbage for the collector to deal with. However, it might be the advisable way of doing things depending on the context. During gameplay, you might have been retrieving a lot of entities and components from the pools, which made them grow. Getting that memory back once a level has been completed would be a good idea if you need it for other purposes.

### Reflection and GWT

`PooledEngine` uses a `ReflectionPool` to efficiently manage memory. As specified in the [Libgdx reflection API documentation](https://github.com/libgdx/libgdx/wiki/Reflection), you need to tell GWT, which classes and/or packages should be made available for reflection via the `gwt.xml` files.

For a class:

```xml
<extend-configuration-property name="gdx.reflect.include" value="com.gdx.game.SomeClass" />
```

For a package:

```xml
<extend-configuration-property name="gdx.reflect.include" value="com.gdx.game.somepackage" />
```

Magic! Now the garbage collector will not bother you at all.