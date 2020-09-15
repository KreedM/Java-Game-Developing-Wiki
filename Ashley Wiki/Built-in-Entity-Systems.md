Ashley comes with a few Entity Systems that will spare you from typing boilerplate code.

1. [IteratingSystem](#iteratingsystem) 
2. [IntervalSystem](#intervalsystem)
3. [IntervalIteratingSystem](#intervaliteratingsystem)
4. [SortedIteratingSystem](#sortediteratingsystem)

### IteratingSystem

Most of the time, you will only need to iterate over a family of entities. In that case you can just extend the [`IteratingSystem`](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/systems/IteratingSystem.html) class and override its `processEntity()` method instead.

```java
public class MovementSystem extends IteratingSystem {
	private ComponentMapper<PositionComponent> pm = ComponentMapper.getFor(PositionComponent.class);
	private ComponentMapper<VelocityComponent> vm = ComponentMapper.getFor(VelocityComponent.class);

	public MovementSystem() {
		super(Family.all(PositionComponent.class, VelocityComponent.class).get());
	}

	public void processEntity(Entity entity, float deltaTime) {
		PositionComponent position = pm.get(entity);
		VelocityComponent velocity = vm.get(entity);
			
		position.x += velocity.x * deltaTime;
		position.y += velocity.y * deltaTime;
	}
}
```

### IntervalSystem

Sometimes you might want to make a system not run every tick, but at constant intervals. Rather than messing about with `setProcessing()` you can extend the [`IntervalSystem`](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/systems/IntervalSystem.html) class and implement its `updateInterval()` method.

```java
public class MovementSystem extends IntervalSystem {
    private ComponentMapper<PositionComponent> pm = ComponentMapper.getFor(PositionComponent.class);
    private ComponentMapper<VelocityComponent> vm = ComponentMapper.getFor(VelocityComponent.class);
    ImmutableArray<Entity> entities;

    public MovementSystem (float interval){
        super(interval);
    }

    @Override
    public void addedToEngine(Engine engine) {
        entities = engine.getEntitiesFor(Family.all(PositionComponent.class, VelocityComponent.class).get());
    }

    @Override
    protected void updateInterval() {
        for (Entity entity : entities) {
            PositionComponent position = pm.get(entity);
            VelocityComponent velocity = vm.get(entity);

            float interval = getInterval();
            position.x += velocity.x * interval;
            position.y += velocity.y * interval;
        }
    }
}
```

### IntervalIteratingSystem

It is also possible to have the best of both worlds (`IteratingSystem` and `IntervalSystem`) thanks to [`IntervalIteratingSystem`](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/systems/IntervalIteratingSystem.html). In this case, you will need to implement its `processEntity(Entity)` method.

### SortedIteratingSystem

Whenever you need to iterate over a family of entities in a specific order, [`SortedIteratingSystem`](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/systems/SortedIteratingSystem.html) will be your friend. You simply need to specify the family a comparator and implement the `processEntity()` method.

Let's say we're writing a `RenderingSystem` that processes entities with a `RenderableComponent` and a `PositionComponent`. The `z` value of the position will determine the order in which entities are processed, the system will sort them for you automatically.


```java
public class RenderingSystem extends SortedIteratingSystem {
	private ComponentMapper<RenderableComponent> rm;

	public RenderingSystem() {
		super(Family.all(RenderableComponent.class, PositionComponent.class).get(), new ZComparator());

		rm = ComponentMapper.getFor(RenderableComponent.class);
	}

	protected void processEntity(Entity entity, float deltaTime) {
		// Render the entity
	}

	private static class ZComparator implements Comparator<Entity> {
		private ComponentMapper<PositionComponent> pm = ComponentMapper.getFor(PositionComponent.class);
		
		@Override
		public int compare(Entity e1, Entity e2) {
			return (int)Math.signum(pm.get(e1).z - pm.get(e2).z);
		}
	}
}
```