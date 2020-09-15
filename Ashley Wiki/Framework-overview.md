Ashley offers a very simple and easy to understand API. Here is an UML class diagram that will help you understand Ashley's architecture.

![](http://i.imgur.com/rkUM4VD.png)

* **[Entity](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/core/Entity.html)**: simple containers of components.
* **[Component](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/core/Component.html)**: interface for your game Components to implement, they are supposed to be bags of data with no logic whatsoever.
* **[ComponentMapper](https://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/core/ComponentMapper.html)**: provides super fast component retrieval of entities.
* **[Family](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/core/Family.html)**: used to represent a set of entities with a specific collection of components.
* **[Engine](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/core/Engine.html)**: main class of the framework, manages all entities, systems and listeners.
* **[EntitySystem](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/core/EntitySystem.html)**: allows you to implement game logic that may operate on entities of a given family.
* **[EntityListener](http://libgdx.badlogicgames.com/ashley/docs/com/badlogic/ashley/core/EntityListener.html)**: can be used to be notified of entity related events.