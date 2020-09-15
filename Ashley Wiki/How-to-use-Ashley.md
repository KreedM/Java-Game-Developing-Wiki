1. [Engine](#engine)
2. [Entities](#entities)
3. [Components](#components)
4. [Retrieving components with ComponentMapper](#retrieving-components-with-componentmapper)
5. [Entity families](#entity-families)
6. [Entity Systems](#entity-systems)
7. [Entity Events](#entity-events)
8. [Updating the engine](#updating-the-engine)
9. [Special considerations](#special-considerations)

One of Ashley's main advantages is its clean API, which makes it easy to use the right way and hard to break it. Let us take a look at how to use it, step by step. Bear in mind this is just a guide, for a complete API reference, please refer to the [javadocs](http://libgdx.badlogicgames.com/ashley/docs/).

First, make sure you understand what an [[Entity System|About Entity Systems]] is.

### Engine

The [`Engine`](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/core/Engine.html) class is the center of the framework. Typically, you would have only one instance per application.

```java
Engine engine = new Engine();
```

You will learn more about the operations you can do with `Engine` throughout this article.

### Entities

In Ashley, [entities](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/core/Entity.html) are simple bags of components.

```java
Entity entity = new Entity();
```

Entities need to be explicitly added to the engine so as to be processed by systems although, be careful not to add the same entity to an engine twice as that will throw an exception.

```java
engine.addEntity(entity);
```

Removing entities from the engine is rather simple.

```java
engine.removeEntity(entity);
```

### Components

[Components](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/core/Component.html) are meant to be data bags and nothing more. All logic should be placed in entity systems. To define a new component, you only need to implement the `Component` interface.

In the following example we define two components: `PositionComponent` and `VelocityComponent`.

```java
public class PositionComponent implements Component {
	public float x = 0.0f;
	public float y = 0.0f;
}

public class VelocityComponent implements Component {
	public float x = 0.0f;
	public float y = 0.0f;
}
```

The collection of components an entity has will determine its behaviour. Adding components to an entity is dead easy.

```java
entity.add(new PositionComponent());
entity.add(new VelocityComponent());
```

However, mind that Entities can only hold one component instance of each class. So, if you try to add two instances of the same class, the second one will replace the old one.

You can remove components from entities at any time.

```java
entity.remove(PositionComponent.class);
entity.removeAll();
```

Retrieving components can be done as follows.

```java
PositionComponent position = entity.getComponent(PositionComponent.class);
VelocityComponent velocity = entity.getComponent(VelocityComponent.class);
```

However, this runs in `O(logn)`. For awesome and fast `O(1)` component retrieval you should be using [`ComponentMapper`](#retrieving-components-with-componentmapper).

You can get them all at once by doing this.

```java
ImmutableArray<Component> components = entity.getComponents();
```

The [`ImmutableArray`](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/utils/ImmutableArray.html) class lets you access the contents of the underlying array but it disallows any modifications to the actual array. You can iterate over the components and even modify them. However, it is impossible to remove or add new components this way.

You can iterate over an `ImmutableArray<Component>` either with a classic loop.

```java
for (int i = 0; i < components.size(); ++i) {
	Component c = components.get(i);
	...
}
```

Or a *for each* approach.

```java
for (Component c : components) {
	...
}
```

### Retrieving components with ComponentMapper

The [`ComponentMapper`](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/core/ComponentMapper.html) class offers lightning fast component retrieval from entities. You should have one instance per component class you need access to. Conveniently, you can use the same instance to access the same component type for different entities.

```java
ComponentMapper<PositionComponent> pm = ComponentMapper.getFor(PositionComponent.class);
ComponentMapper<VelocityComponent> vm = ComponentMapper.getFor(VelocityComponent.class);

...

PositionComponent position = pm.get(entity);
VelocityComponent velocity = vm.get(entity);
```

If you dislike the idea of having to instantiate a `ComponentMapper` to efficiently retrieve components, you can always have a class that keeps all of them in one place.

```java
public class Mappers {
	public static final ComponentMapper<PositionComponent> position = ComponentMapper.getFor(PositionComponent.class);
	public static final ComponentMapper<VelocityComponent> velocity = ComponentMapper.getFor(VelocityComponent.class);
	...
}

PositionComponent pos = Mappers.position.get(entity);
VelocityComponent vel = Mappers.velocity.get(entity);
```

### Entity families

Entities with the same set of components can be grouped in [`Family`](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/core/Family.html) objects. You can obtain a `Family` by specifying the list of component classes the entities belonging to said family must possess. This should satisfy most of your entity classification needs.

```java
Family family = Family.all(PositionComponent.class, VelocityComponent.class).get();
```

A higher level of granularity can be achieved when it comes to grouping entities in families. You can specify:

* A set of components the entity must have.
* A set of components of which the entity must have at least one.
* A set of components the entity cannot have.

Imagine we want to group all entities that should be rendered. It certainly must have a position and either a texture or a particle system. Additionally, we need to make sure it is not invisible. These constraints can easily be represented the following way.

```java
Family family = Family.all(PositionComponent.class)
					  .one(TextureComponent.class, ParticleComponent.class)
					  .exclude(InvisibleComponent.class)
					  .get();
```

The `Engine` has the capability of providing the full collection of entities that match a specific family.

```java
ImmutableArray<Entity> entities = engine.getEntitiesFor(family);
```

Iterating over them is really easy.

```java
for (Entity entity : entities) { ... }
```

### Entity Systems

[`EntitySystem`](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/core/EntitySystem.html) derived classes contain the logic that process our game entities. These are the methods you can override.

```java
public abstract class EntitySystem {
	public EntitySystem();
	public EntitySystem(int priority);
	public void addedToEngine(Engine engine);
	public void removedFromEngine(Engine engine);
	public void update(float deltaTime);
	public boolean checkProcessing();
	public void setProcessing(boolean processing);
}
```

For instance, let us say we want to change our entities' position according to their velocity. We can create a `MovementSystem` that takes care of it.

```java
public class MovementSystem extends EntitySystem {
	private ImmutableArray<Entity> entities;

	private ComponentMapper<PositionComponent> pm = ComponentMapper.getFor(PositionComponent.class);
	private ComponentMapper<VelocityComponent> vm = ComponentMapper.getFor(VelocityComponent.class);

	public MovementSystem() {}

	public void addedToEngine(Engine engine) {
		entities = engine.getEntitiesFor(Family.all(PositionComponent.class, VelocityComponent.class).get());
	}
	
	public void update(float deltaTime) {
		for (int i = 0; i < entities.size(); ++i) {
			Entity entity = entities.get(i);
			PositionComponent position = pm.get(entity);
			VelocityComponent velocity = vm.get(entity);
			
			position.x += velocity.x * deltaTime;
			position.y += velocity.y * deltaTime;
		}
	}
}
```

`Engine` will update all the registered entity systems every frame according to their priority order. The lower the priority level, the sooner the system will be updated. Keep in mind that you cannot change the priority of the system once its been added to the engine. Do the following to register a system with the engine.

```java
MovementSystem movementSystem = new MovementSystem();
engine.addSystem(movementSystem);
```

Anytime you want, you can unregister a system from the engine.

```java
engine.removeSystem(movementSystem);
```

A system can be retrieved from an engine reference by its class.

```java
MovementSystem movementSystem = engine.getSystem(MovementSystem.class);
```

You can enable/disable systems whenever you want. So, if what you need is to temporarily pause a system, you do not really need to keep registering and unregistering it. When a system is disabled, it will not be updated by the engine.

```java
movementSystem.setProcessing(false);
``` 

Ashley comes with a few [[Built-in Entity Systems|built-in entity systems]] that might make your life easier and avoid some boilerplate code.

### Entity events

By implementing the [`EntityListener`](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/core/EntityListener.html) interface you can easily be notified of entity related events.

```java
public interface EntityListener {
	public void entityAdded(Entity entity);
	public void entityRemoved(Entity entity);
}
```

If you register your listener with the engine, you will be notified every time an entity is added or deleted.

```java
engine.addEntityListener(listener);
```

You can also remove a listener from the engine.

```java
engine.removeEntityListener(listener);
```

However, you might be interested in knowing when entities enter or leave a particular family. This can be useful to keep track of when components of a particular type are added to or removed from entities. For example, you may want to know whenever an entity with a `PhysicsComponent` is removed from the engine so you can destroy the physics body from the Box2D `World`. In this case, you will need to pass in the family to the `addEntityListener()` method.

```java
Family family = Family.all(PhysicsComponent).get();
engine.addEntityListener(family, listener);
```

Note that if the component was removed from the entity (but the entity remains in the engine), the listener will only be called after the component is already removed (see issues [#186](https://github.com/libgdx/ashley/issues/186), [#188](https://github.com/libgdx/ashley/issues/188) and [#193](https://github.com/libgdx/ashley/issues/193))

You can control the order in which the listeners are notified of entity events by specifying a priority when registering them with the engine. Lower priority means the listener will be notified earlier.

```java
engine.addEntityListener(familyA, 0, listenerA);
engine.addEntityListener(familyB, 1, listenerB);
engine.addEntityListener(0, listenerC);
```

### Updating the engine

Everything is in place to start ticking the engine. You should be calling `Engine.update()` once per frame with the elapsed time since the last frame. This will trigger an update on all the systems according to their priority order and thus, our game logic will work its magic.

```java
engine.update(deltaTime);
```

### Special considerations

Here are a couple of details on how Ashley deals with special situations you might want to know about.

* Entity removal requests will be delayed if issued while an entity system is being updated. This is because removing it immediately would affect family iteration. Outstanding removal operations will be carried out as soon as the next entity system finishes updating.
* Component additions and removals will always take effect immediately. However, listeners, including family membership, will only be notified after the next system update finishes. This is for the same reasons as entity removals.