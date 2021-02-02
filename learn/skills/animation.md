---
layout: skill-details
kaka: "Lorem ipsum deets"
title: "Animation"
---

Animation for games happens in two steps. First, you create motion for your character in a software like Blender (called "animation clips" here). Second, you import these clips into your game engine and add some logic. This defines circumstances for 
these animations clips to be played, because they must react to the player's actions. We will only deal with the second step in 
this article, and assume that you already have a few animation clips imported into your engine.

### State Machines

Animation logic uses the general concept of a [state machine](https://en.wikipedia.org/wiki/Finite-state_machine). As the name 
implies, we create a set of states that our character can be in, we define animation clips that should be played in each state, 
and finally we define some conditions that allow transitions between states.

![run.png](/img/learn/animation-run.png)

Consider the simple two-state situation shown above. There are three things to point out here: states, transitions and loops.

- **States:** The idle and the running state are a logical way to define what all can the player do. They each carry an animation 
clip that is played when the player is in that state.
- **Transitions:** There is a *bidirectional* transition here: the player can go from idle to run and from run to idle. A simple 
condition triggers these transitions: is the player pressing 'W'?
- **Loops:** What if the player holds 'W' for a while? The run animation clip is finite, but we can ask the engine to loop that 
clip as long as 'W' is held. Similarly, the idle animation also loops (denoted by the blue symbol here).

Here is another example. This time we're looking at a jump animation.

![jump.png](/img/learn/animation-jump.png)

We have two key differences here. 

- **Transitions:** They're *unidirectional* now. A player cannot go from the falling clip to the anticipation clip, that would 
just not make sense in the real world.
- **Loops:** Not all states are looping. They just play their clips and move on to the next state.

The anticipation and the jump are deterministic, till the player reaches the heighest point. These clips can be played just once, 
moving on to the next state. On the other hand, the player's fall time is not deterministic, as they may be jumping on 
level ground or falling from a cliff. Therefore the fall animation should loop.

### Implementation

Usually these state machines are created using a GUI provided by your engine. A separate asset is created, and is associated with 
game objects by attaching a component.

![unity.png](/img/learn/animation-unity.png)

The above screenshot shows how the animation states look like in Unity. The parameters on the left define variables that you 
can modify in code. Transitions between states will use these parameters.

### References

- **Unity:** [John Lemon's Haunted Jaunt](https://learn.unity.com/tutorial/the-player-character-part-1?uv=2020.2&projectId=5caf65ddedbc2a08d53c7acb#5caf8edcedbc2a0c0aee464e)
- **Unreal:** [Overview of State Machines](https://docs.unrealengine.com/en-US/AnimatingObjects/SkeletalMeshAnimation/StateMachines/Overview/index.html)
