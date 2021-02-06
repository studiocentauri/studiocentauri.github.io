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

Rigidbodies have a number of properties associated with them which impact on how the GameObjects will be reacting to various forces and motions.


