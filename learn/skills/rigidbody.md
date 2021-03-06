---
layout: skill-details
kaka: "After learning about GameObjects and their components, it is time to discuss about the impact of Physics on these GameObjects"
title: "Rigid Bodies"
---

Physics simulation is an integral part of most games. It defines how your objects will interact with the scene, including collisions, restricted movement and acceleration of moving objects. We'll cover how these aspects are represented and implemented in this article.

### Overview
A *rigidbidy* is any object that takes part in the physics simulation. Such objects are usually marked by a `RigidBody` component. For example, consider a football in a 3D space — a lot of external forces like gravity, air drag and wind act on this ball. The game developer can specify associated constants and parameters, like the acceleration due to gravity. The *physics system* is the software component that performs the actual calculations and applies forces based on the specified parameters. These calculations are usually *Newtonian*, i.e., based on Newton's Laws of Motion.

### Body Types

We can control the extent to which an object is affected by physics and classify objects into three broad categories:
- **Static:** These are not moved by physics at all but will participate in collisions. Usually used for making walls and other components of your scene that will not move.
- **Kinematic:** Similar to static, but used when objects should be moved manually. This includes movement driven by animation, updating transforms through code, etc.
- **Rigid:** Full physics simulation. These will participate in collisions and will be affected by them. Can only be controlled by applying forces to them.

![Die Brick](/img/learn/brick.png)

In the game above (break all the bricks using the ball), the bricks and screen boundaries are static bodies: they do not move but participate in collisions. The paddle is a kinematic body: we don't want gravity or the ball to affect its position. The paddle is explicitly controlled by the player. Finally, the ball is a full rigidbody: we want it to bounce off when it hits a surface.

### Implementation

The game engine's UI provides ways to add a rigid body component or modify its parameters. The following pseudo-code illustrates how the ball and paddle scripts would look like.

```cpp
class Ball : RigidBody {
    // Do this once when the game starts...
    void Start() {
        // Turn off gravity, the ball should simply reflect
        // Note that most parameters like this can be changed in code or in the engine UI
        this.gravity = 0;
    }

    // Do this each frame...
    void Update() {
        // Iterate over all bodies that collided with this ball this frame
        for body in colliding_bodies {
            // Destroy the brick if hit
            if body.is_brick() {
                body.destroy()
            }
        }
    }
}

class Paddle : KinematicBody {
    // Since this is kinematic we don't need to explicitly turn off gravity
    // Just process input each frame
    void Update() {
        if input_key_pressed {
            this.transform.position.x += distance_to_move_this_frame
        }
    }
}

// A brick has no functionality other than just staying there
class Brick : StaticBody {}
```

### Colliders

One last thing that we have to consider while working with physics is the concept of a collider. We've been saying that the physics 
system is incharge of collision calculations, but that would mean it would have to perform intersection calculations on shapes as 
complex as highly detailed characters. From a performance point of view, that's going to be nightmare, considering most of the time 
we don't really care about collisions that precise anyway.

Enter colliders. We simply assign a simpler shape (collider) to the complex character, and that shape is what's used by the physics system. Choosing the right combination of colliders is important to make your game feel right. If your character's hands are not covered with a collider, they'll go through walls. If the "hit box" is too large, bullets will hit your character even when it looks like they shouldn't.

![colliders.png](/img/learn/rigidbody_colliders.png)

The above image illustrates a *compound* setup, that is, we're using multiple basic shapes (capsules, boxes) to create a collider 
for the character. The green wireframe representation is what will be used by the physics system, and it is clear that this is much simpler than the actual character.


### Bonus Content: Game Loops

![Physics Engine Overview](/img/learn/physics_engine.png)

To understand physics, we first need the concept of a *game loop*. Every game engine implements a loop: process player actions, simulate physics, update what's on the screen and repeat. One iteration of this loop is what we call a *frame* (the fps from "60 fps" is the frequency of iteration of this loop)<sup>1</sup>. The `Update()` method in Unity is used to define the code you want to execute each frame inside this game loop. If the player hits something, that hit is queued and processed in the next frame by the physics system. Moving the football towards the ground a little bit every frame is how the physics system will implement "gravity".

*1. This is not strictly necessary and depends on how the game engine is implemented. Physics and rendering might have completely different loops that somehow communicate with each other. However, we will define our game loops this way for simplicity unless otherwise specified.*

### References

- [RigidBody - Godot Documentation](https://docs.godotengine.org/en/stable/tutorials/physics/rigid_body.html)
- [RigidBody2D - Godot Documentation](https://docs.godotengine.org/en/stable/tutorials/physics/physics_introduction.html#rigidbody2d)
- [RigidBody - Unity Documentation](https://docs.unity3d.com/Manual/RigidbodiesOverview.html) 
- [RigidBody2D - Unity Documentation](https://docs.unity3d.com/Manual/class-Rigidbody2D.html)
