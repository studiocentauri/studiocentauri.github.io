---
layout: skill-details
kaka: "After learning about GameObjects and their components, it is time to discuss about the impact of Physics on these GameObjects"
title: "Rigidbody"
---

In Game Development, Physics is one of the most important factors that are needed to be kept in mind while deciding the gameobjects' actions in any scene. These actions include collisions, restricted movements, velocities/accelerations of moving objects, etc. We will be discussing aspects of all these in this section. 

## Overview
A *Rigidbidy* is the main component that enables physical behaviour for a GameObject. Rigidbodies have a little different interpretations in Godot and Unity, but they do tend to indicate the same things. The key difference of the interpretations comes in due to different handling of collisions in them. We will learn about this further in this section.
A Rigidbody can be understood as a component of a GameObject that allows it to be controlled by the Physics engine. In its maiden form, Rigidbody is like a football in a 3-D space which is acted upon by gravity, air drag, wind and all such external forces. However, one can manipulate the constants associated with these features and also treat a Rigidbody as a ragdoll by giving its controls to the player. 

As it can be seen that Physics Engine play a very important role in controlling Rigidbody, it is now important to understand how a Physics Engine work.
The `Physics Engine` is the software component that performs the physics simulation. It replicates the dynamic behaviour of a rigidbody in a game `scene`  by receiving the specifications of a rigidbody. It is responsible for carrying out only numerical calculations related to various events. These calculations are done by using the *Newtonian Mechanics* i.e., the Newton's Laws of Motion. 

A brief overview of the working of a `Physics Engine` is given in the following diagram. 

![Physics Engine Overview](/img/learn/physics_engine.png)

Rigidbodies have a number of properties associated with them which impact on how the GameObjects will be reacting to various forces and motions

## Godot Implementation

In Godot, we can develop both 2D and 3D games. Meaning of the term RigiBody does not change in them but the implementation may differ. 

#### 2D Games
For developing 2D games, we create the world of our game in `Node2D` node. Then, we add some _GameObject_ in our scene. For these, we have 3 Physics Bodies termed as `PhysicsBody2D`.  

* __StaticBody2D__ bodies are not controlled by the `Physics Engine`. They get involved in collisions but do not move in response to them, but can impart motion to the colliding bodies. 

* __KinematicBody2D__ bodies are also not controlled by the `Physics Engine`. They can detect collision and respond accordingly and also they are required to be controlled by the user via a code, as they do not have impact of Physics on them. 

* __RigidBody2D__ bodies are controlled by the `Physics Engine` and the user does not have a direct control over them. Although the beharvious of these bodies can be altered by changing the Physical properties in the _Inspector_. 
RigidBodies also have modes in Godot i.e., _Static_, _Rigid_, _Kinematic_ and _Character_ (similar to a typical `RigidBody2D` body expect it cannot rotate). We can do it by using enumerations - 

```
enum Mode : 
- MODE_RIGID = 0  // Body behaves like a typical RigidBody2D. This is the default mode
- MODE_STATIC = 1   // Body behaves like a StaticBody2D. 
- MODE_CHARACTER= 2   // Similar to MODE_RIGID except the body now cannot rotate.
- MODE_KINEMATIC = 3  // Body behaves like a KinematicBody2D.
```
Apart from using RigidBodies like a ragdoll which are handled by the `Physics Engine` alone, the user can also control RigidBodies via a code written in the script. For doing this we use, `_integrate_forces()` callback. 

```
extend RigidBody2D

var thrust
var torque

func _integrate_forces(state):
    ...
    if Input.is_action_pressed(input):
         applied_force = thrust.rotated(rotation)
    ...

    var rotation_dir = 0
    if Input.is_action_pressed(input):
        rotation_dir = +1 or -1
    
    applied_torque = rotation_dir*torque
    ...
    
```

#### 3D Games


