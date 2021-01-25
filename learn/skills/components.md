---
layout: skill-details
kaka: "Fresh recruits need to know these common tactics before they can start their training."
title: "Components"
---

Entities and Components are the most popular ways to model game logic. Here we'll explain the concept from the point of view of 
Unreal and Unity. Other engines will work on similar lines. Note that code segments are for illustration purposes only and should 
be treated as simplified, pseudo-code.

### Overview

A *component* is a collection of data or behaviour. A collection of these components defines a *game object* or *entity*. Exact 
definitions might vary across engines, but the following diagram illustrates the general concept.

![components.png](/img/learn/components.png)

Consider two entities in your game, `Player` and `Tree`. Both of these entities have a position in the world, so we attach a 
`Position` component to them which tracks their coordinates. On the other hand, only the `Player` entity needs to respond to 
input, so we attach an `Input` component to it. This component contains the logic for reacting to keyboard or mouse input.

As you might have guessed, in the model described above the entities themselves are not very different. They're just boxes that 
you can put components in. It is the combination of these components that makes the entity work as intended. Just attaching the 
`Input` component to a tree will allow you to move it in-game!

### Unity Implementation

This notion of an entity is represented by a `GameObject` in Unity. There are a ton of built-in components that you can attach 
to these objects, and you can even script your own with C#. Every Unity object has a `Transform` component by default that tracks 
position, rotation and scale. However, for the sake of continuity, we will use our own `Position` component.

```cs
public class Input : MonoBehaviour {
    // This function is called each frame
    void Update () {
        ...
        position = GetComponent<Position>(); // Get this object's Position component
        position.x += distance_to_move_in_the_x_direction_this_frame; // Modify that component
        ...
    }
}
```

**More Info:** [GameObjects - Unity Documentation](https://docs.unity3d.com/Manual/GameObjects.html)

### Unreal Implementation

Unreal has a very different approach to this. Entities here are typical C++ classes, organized in an object-oriented pattern. 
Consider the following classes.

```cpp
class Position { float x, y, z };

class Input {
    void BindKeyboard(/* arguments */);
}

class Object; // Base class for all objects

class Actor : public Object { // An Actor is an Object that has a position in the game world
    Position position;
}

class Pawn : public Actor { // A Pawn is an Actor that can be controlled
    Input input;
}
```

With these classes that Unreal provides, we can now declare our objects as follows.

```cpp
class Player : public Pawn { // Player inherits Pawn because it takes input
    Player() {
        input.BindKeyboard(OnKeyboardInput);
    }

    OnKeyboardInput() {
        position.x += distance_to_move_in_the_x_direction_this_frame;
    }
};
```

Modifying position here is a little indirect. You need to tell the input component "Hey, call the `OnKeyboardInput` function 
whenever the user presses the keyboard". We do this by using the inherited `input` member variable from the `Pawn` class. The 
`OnKeyboardInput` then modifies the inherited `position` member variable from the `Actor` class.

The `Tree` class is much simpler, just inherit the `Actor` class because the tree will not take any input.

```cpp
class Tree : public Actor {};
```

**More Info:** [Player Input and Pawns - Unreal Documentation](https://docs.unrealengine.com/en-US/ProgrammingAndScripting/ProgrammingWithCPP/CPPTutorials/PlayerInput/index.html)

### Bonus Content: Entity-Component-System

A more general approach introduces two new constructs: *systems* and *events*. In the ECS pattern, components only hold data. 
Therefore, entities (along with the components that are attached to them), act as a blueprint of all the data that is associated 
with this entity. *Events* provide a mechanism to initiate interaction between entities, and *systems* contain the logic to 
modify the involved entities. This is how the input example will work with this pattern.

```cpp
class Position { float x, y, z; };
class Input {};
```

The input component here has no functionality, it is not supposed to. It just acts as a marker to tag entities that can be 
controlled.

We will then create two entities: player and tree. The `EntityRegistry` is a special class that the engine would provide. This class handles creation, deletion, modification and searching for entities.
```cpp
int main(void)
{
    ...

    Entity player = EntityRegistry.Create();
    player.AddComponent<Position>();
    player.AddComponent<Input>();

    Entity tree = EntityRegistry.Create();
    tree.AddComponent<Position>();

    ...
}
```

Finally, to complete the set up, we create a system to handle events.
```cpp
class PlayerInputSystem {
    void OnKeyboardInput(KeyboardEvent e) {
        Entity[] entities = EntityRegistry.Find<Input, Position>(); // Find all entities that have both Input and Position
        for (Entity entity : entities) {
            Position position = entity.GetComponent<Position>();
            position.x += distance_to_move_in_the_x_direction_this_frame;
        }
    }
}
```

Now when the engine detects a keyboard input, it will fire the `KeyboardEvent`. This event will be handled by `OnKeyboardInput` 
from the `PlayerInputSystem`. This handler with iterate through each active entity in the world that has both `Input` and 
`Position` components, modifying their position components as required. Note that the tree entity will be skipped, as it has only 
`Position`.

This separation of data and logic is what makes the ECS pattern flexible. Unity's new DOTS stack is based on this pattern, so is 
our own game engine, Lotus.

**More Info:** [Entity Component System - Wikipedia](https://en.wikipedia.org/wiki/Entity_component_system)
