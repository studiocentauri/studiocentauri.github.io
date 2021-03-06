---
layout: skill-details
title: "Prefabs"
---

As the scale of your game increases, it becomes more and more difficult to track all the `Objects` or `Entities` being used. There may also be situations where you need you multiple copies of a same `Object`. Example:- Coins in a Level,Bullets fired from a Gun,etc. One solution to these problems is to use `Prefabs`.

### Overview
Prefabs can be understood as templates or blueprints of a `Object` that can be used in other parts of a game. This template is stored on your System as an `Asset` file which can later be used wherever required.

Prefabs greatly improve the game development process by speeding up Development time and helps in Debugging. To further explain this process lets take the exams of Coins in a game. Lets say you have a Game Level where you need to place 20 coins throughout the level. Your initial approach would be to:
1. Create a Coin Objects
2. Make Multiple Copies of that Object
3. Place them throughout the level

Now if some situation arrives like you need to change the color of the Coin, you would need to individually change the color of every coin Object in your Level. This process can be alleviated by using Prefabs. Now your approach would be:
1. Create a Coin Object
2. Save it as a Prefab
3. Place the Prefab throughout the Scene

Now if you want to change the color of the Coin, you only need to change it in the Coin Prefab Asset file. Making this change in the Asset would automatically translate to all the Instances of the Prefab.Thus the color of the Coin would change in all the Coins.

Prefabs can also be helpful in Debugging your Game. When you want to apply a Patch or Fix a Bug which affects some Objects you only need to apply it to the Prefab Assets instead of all the Objects. This new fix would be automatically available to all the Instances. 

These `Prefab` Asset files can be used in a Game in multiple ways:-
- Creation and Editing of Prefabs
- Manual Placement in Scenes
- Initialisation during Runtime
- Overriding Prefab Data
- Create Prefab Variants

### Creation and Editing of Prefabs
Prefab creation is a `Engine` process. The end result is same in all cases with the Creation of an Asset file where all the data is stored. The Prefabs consists of:-
- All the Components of an Object
- All the Child Objects and their Components
- All the data fields related to these Components

This Prefab can later be edited as based on the User needs. This is done in a `Prefab` editor in `Unity` or a `Scene` editor in `Godot`.


### Manual Placement in Scenes
Prefabs can be manually placed in your scenes to be used.(Ex- Placing coins throughout a Map) This is done in the Engine Editor by placing the Prefab in your Scene and Making Copies of them as you see fit.

### Initialisation during Runtime
Prefabs can be intialised in during Runtime based on user Input or Some Game Logic.
(Ex- Bullets initialised when a Gun is Fired)

Lets take two examples of Prefab Initialisation.These are explained in context of `Unity` engine.
- Bullet being Fired when Space is Pressed
```cs
GameObject bulletObject; // Stores Bullet Prefab Reference

void Update()
{
  ...
  // Checks if Space Key is Just Pressed down
  if(Input.GetKeyDown(KeyCode.Space))
  {
    // Spawns a bulletObject as (1,1,1)
    Initialise(bulletObject, Vector3(1,1,1));
  }
  ...
}
```
- Spawn a Enemy at a Random Position at Regular Intervals

```cs
GameObject enemyObject; //Stores Enemy Prefab Reference

void Update()
{
  ...
  // Calculate Time
  float timeLeft = GetTimeLeft();
  // Checks if no time is left
  if(timeLeft<=0)
  {
    // Get Random Spawn Position
    Vector3 spawnPosition = GetRandomPosition();
    // Spawns Enemy at said Random Position
    Initialise(enemyObject, spawnPosition);
    // Reset timer
    timeLeft = ResetTimer();
  }
  ...
}
```

### Overriding Prefab Data
All the instances of a Prefab have the same properties and component values as the Original Asset. You can also change them to suit your own needs. By overriding some value in an Instance you define that value as different.
- Lets take a Example of a Sword as :-

|     Item     |  Value   |
|--------------|----------|
| Sword Prefab | Attack=5 |
| Instance 1   | Attack=5 |
| Instance 2   | Attack=5 | 

- If we change the Attack value in the Prefab to 6, the value would change as:- 

|     Item     |  Value   |
|--------------|----------|
| Sword Prefab | Attack=6 |
| Instance 1   | Attack=6 |
| Instance 2   | Attack=6 |

- Now if we override the attack value in `Instance 1` to 8 we get values as:-

|     Item     |  Value   |
|--------------|----------|
| Sword Prefab | Attack=6 |
| Instance 1   | Attack=8 |
| Instance 2   | Attack=6 |

- If we change the attack value to 10 in the Prefab,we get values as:-

|     Item     |  Value    |
|--------------|-----------|
| Sword Prefab | Attack=10 |
| Instance 1   | Attack=8  |
| Instance 2   | Attack=10 |

- Thus the overriden Instance's value would no longer change with the Prefab. This can be fixed by reverting the changed field. Thus the values would finally be:-

|     Item     |  Value    |
|--------------|-----------|
| Sword Prefab | Attack=10 |
| Instance 1   | Attack=10 |
| Instance 2   | Attack=10 |


### Create Prefab Variants
Prefab Overriding works well when you have a small number of Objects to work with. If the number of Objects increase, using Prefab Variants would provide better results. To understand this, lets take the example of a `Health Potion`.
- A `Health Potion` heals the user by a certain amount when used.
- You can have Multiple Types of Prefabs as:
  - Small Potion (heals 5 HP)
  - Normal Potion (heals 15 HP)
  - Large Potion (heals 50 HP)
- All of these items have the same logic with parameter changes. Also all of these can also have multiple instances in multiple parts of the Game
- Thus they can be used as:
  - A Health Potion Object saved as Prefab
  - 3 Prefab Variants of this Health Potion as
    - Small
    - Normal
    - Large
  - These variants are themselves stored as Asset files and can be used in your Game
- Now if you want to use a Particle Effect on your Health Potions, you only need to Add it to the Original Prefab
  - This change would be added to the all the Variants of the prefabs
  - This would also be added to all the instances of all the Prefabs

### Bonus Content: Nested Prefabs
Prefabs can also be nested inside other prefabs. These can be used for dividing the development process into smaller chunks.Lets take the example of an RPG game with enemy characters with weapons. The features of the game are:-
- There are Enemy character which can be found in various places around the world.(Ex - Raider)
- Each Enemy also has items such as
  - Weapons (Ex-Sword, Shield, Axe)
  - Armor (Ex-Helmet, Leather Armor)
  - Other (Ex-Potion, Coins)
- This type Equipment can also be used by other Enemies (Ex-Guard) or by the Player.

Thus the prefabs that need to be created are :-
  - Player
  - Raider
  - Guard 
  - Sword
  - Shield
  - Axe
  - Helmet
  - Leather Armor
  - Potion
  - Coin

These prefabs can be created individually during the development process and can be put together in the form of prefabs.
These prefabs are finally assembled into Objects in context to the Game such as:-

```
Prefabs
│   Coin
│   Potion
|   Leather Armor   
|   Hemlet
|   Axe
|   Shield
|   Sword
└───Raider
    │   Sword
    |   Leather Armor
    |   5 Coins
└───Guard
    │   Sword
    │   Shield
    │   Helmet
    |   Leather Armor
    │   1 Potion
    |   10 Coins
└───Player
    │   Axe
    │   Shield
    │   Helmet
    |   Leather Armor
    │   3 Potion
    |   25 Coins
```

### Bonus Content: Parent-Child Relationship
As in the case of Nested prefabs we came to know that Objects and made child of Other Objects. This forms a parent-child relationship between objects. This involves interaction between the two Object i.e. Parent and Child.
The Child inherits multiple properties from its parents such as:
- Position
- Rotation
- Scale
- Colliders


### Refereneces

1. [Prefabs - Unity Documentation](https://docs.unity3d.com/Manual/Prefabs.html)
2. [Scenes - Godot Documentation](https://docs.godotengine.org/en/stable/getting_started/step_by_step/scenes_and_nodes.html)
3. [Blueprint Class - Unreal Documentation](https://docs.unrealengine.com/en-US/ProgrammingAndScripting/Blueprints/index.html) 