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

<!---

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
-->

## Body Type (Modes)

Rigidbody is attached to a GameObject so as to have the impact of Physics on that GameObject. In a game scene, one may want a GameObject to behave in various ways and in terms of how Physics have impact on them, there are modes that are needed to be defined for a Rigidbody. 

* __StaticBody__ bodies are not controlled by the `Physics Engine`. They get involved in collisions but do not move in response to them, but can impart motion to the colliding bodies. 

* __KinematicBody__ bodies are also not controlled by the `Physics Engine`. They can detect collision and respond accordingly and also they are required to be controlled by the user via a code, as they do not have impact of Physics on them. 

* __RigidBody( Dynamic Body)__ bodies are directly controlled by the `Physics Engine` and are very less dependent on the user controlling them. The `Physics Engine` control them on the basis of the set of properties defined associated with them like mass, drag etc. In one sense, one may interpret a Rigidbody as a ragdoll which is not under the user's control but is impacted by the Physics of the current game scene. This mode( or body type) is default to a Rigidbody component. 


![Die Brick](/img/learn/brick.png)

In the above scene of a game (in which we have to break all the bricks using that ball), the bricks and walls (not visible in the image) are static bodies, the paddle is a kinematic body and the ball is a rigid (dynamic) body. They all have some scripts(codes) associated with them so as to make the game more interactive. 


## Implementation 

For a proper implementaion of a rigibody, one needs to add/remove various properties assoiciated with it. Now to alter this property, one may either do it directly with the help of a game engine or one may write a code for this. Usually codes are preferred for detecting collisions, simulating transistions from one state to another and fixing some parameters. Altering the properties of a rigidbody can directly be done from the game engine interface. 

![Ball Script](/img/learn/ball_script.png)  |  ![Paddle Script](/img/learn/paddle_script.png)  | 


The above 2 screenshots show the scripts associated with the ball and the paddle in the Die Brick game respectively. The __Scripting__ tells the game engine what to do when something happens. In the case of Kinematic bodies, scripting is very much important as the user is needed to control them and thus it is needed to instruct the game engine how the user will be giving input to the Kinematic body. 


#### References 

__Godot__ :  
* [RigidBody - Godot Documentation](https://docs.godotengine.org/en/stable/tutorials/physics/rigid_body.html)
* [RigidBody2D - Godot Documentation](https://docs.godotengine.org/en/stable/tutorials/physics/physics_introduction.html#rigidbody2d)

__Unity__ : 
* [RigidBody - Unity Documentation](https://docs.unity3d.com/Manual/RigidbodiesOverview.html) 
* [RigidBody2D - Unity Documentation](https://docs.unity3d.com/Manual/class-Rigidbody2D.html)

<!--- asfasd -->