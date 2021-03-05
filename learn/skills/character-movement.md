---
layout: skill-details
kaka: "Lorem ipsum deets"
title: "Character Movement"
---

One of the ways you can influence the `Objects` in a game is by moving them. Moving an object is a primitive form of Object manipulation you can see in Games. This is usually done in scripts attached to Objects. Movement is seen throughout games such as a moving Player character, fired bullets from a gun,etc.
   
### Overview 
The movement of `Objects` can be done in one of two ways
1. By changing position(`Transform` manipulation)
2. By adding Force or Velocity(`Rigidbody` Manipulation)

The following examples are in relation to `Unity` engine. The implementation in other engines is more or less the same following the same priniciples.

### Transform Manipulation
Movement of an `Object` by using its Transform involves directly changing the position of the object. The position can be changed by the push of a button as :-

1. Changing the position by some vector each time `Space` is Pressed.
```cs
void Update()
{
    ...
    // Check if Space button is Just Pressed down
    if(Input.GetKeyDown(KeyCode.Space))
    {
        // transform.position is a Vector3 and gives the object's position
        transform.position = transform.position + Vector3(0, 0, 10);
    }
    ...
}
```
2. Changing the position when `Space` is held.
```cs
// Setup Constants
private float velocity = 5.0f;
private Vector3 moveDirection = Vector3(0,0,10);

void Update()
{
    ...
    // Checks if the space button is held down
    if(Input.GetKey(KeyCode.Space))
    {
        transform.position += moveDirection * veclocity;
    }
    ...
}
```

3. Constantly changing the position.
```cs
private float velocity = 5.0f;
private Vector3 moveDirection = Vector3(0,0,10);

void Update()
{
    ...
    // Statement is not inside any if statement
    transform.position += moveDirection * veclocity;
    ...
}
```
#### **Note**:-
Movement by manipulation of `Transform` component is done in cases where collision are not applied. Example:- Objects revolving around other Objects.

### Rigidbody Manipulation
Manipulating the `Rigidbody` component involves one of two approaches:-
1. Setting the velocity.
2. Applying Force

#### 1. Setting the Velocity:-
This is done in cases such as Player movement and moving Vehicles. This involves the use of applying some Acceleration.

1. Changing velocity when `W` is pressed.
```cs
Rigidbody rb;

void FixedUpdate()
{
    ...
    // Store value of current velocity
    Vector3 velocity=rb.velocity;
    if(Input.GetKey(KeyCode.W))
    {
        // Change that stored value
        velocity += Vector3(0, 0, 1);
    }
    // Set new velocity
    rb.velocity = velocity;
    ...
}
```

2. Applying acceleration when `W` is pressed.
```cs
Rigidbody rb;
private float acceleration = 2;

void FixedUpdate()
{
    ...
    Vector3 velocity=rb.velocity;
    if(Input.GetKey(KeyCode.W))
    {
        velocity += Vector3(0, 0, 1) * acceleration;
    }
    rb.velocity = velocity;
    ...
}
```

#### 2. Applying Force:-
This is done in cases such as Jumping and Firing Projectiles. The Force is applied as an Impusle for simulating motion. 

1. Adding force when `Space` is pressed.
```cs
Rigidbody rb;
private float jumpForce;

void FixedUpdate()
{
    ...
    if(Input.GetKey(KeyCode.Space))
    {
        // Applies impulsive force
        // Function is as AddForce(Vector3 dir,ForceMode mode);
        // Applies force in direction "dir"
       rb.AddForce(Vector3(0,1,0) * jumpForce, ForceMode.Impulse);
    }
    ...
}
```

2. Add Gravition force constantly
```cs
Rigidbody rb;
private const float accGravity = 9.81f;

void FixedUpdate()
{
    ...
    // Applies acceleration force
    rb.AddForce(Vector3(0,-1,0) * accGravity, ForceMode.Acceleration);
    ...
}
```

#### **Note**:-
Movement by manipulation of `Rigidbody` component is done in cases where collision are applied. Example:- Player Jump.

### Bonus Content: Frame vs Physics Frame
In both the above cases of `Transform` and `Rigidbody` manipulation we used 2 different functions:-
1. `Update`
2. `FixedUpdate`

These names are `Unity` specific but the logic is same everywhere.
- `Update` is called every game loop and may have different time gap between calls.
- `FixedUpdate` is called at regular intervals of time. This is when Physics calculations take place.

### Bonus Content: DeltaTime vs FixedDeltaTime
In these examples the change in position and velocity are changed every `Update` or `FixedUpdate` call. Thus the changes is Frame dependent. One example may be of changing position of a player.

```cs
private float moveSpeed=5.0f;

void Update()
{
    ...
    // GetMoveDirection returns the direction to move in based on some inputs
    Vector3 moveDir=GetMoveDirection();
    transform.position += moveDir * moveSpeed;    
    ...
}
```

OR

```cs
private float acceleration=2.5f;

void FixedUpdate()
{
    ...
    Vector3 moveDir = GetMoveDirection();
    Vector3 velocity = rb.velocity;
    velocity += moveDir * acceleration;
    rb.velocity = rb.velocity;
    ...
}
```

These frame calls are dependent on the hardware of the user may lead to different behaviour for different users. For a uniform behaviour, we need to make these changes time dependent. This is where `deltaTime` is used.
- `deltaTime` is the time gap between successive `Update` calls.
- `fixedDeltaTime` is the time gap between successive `FixedUpdate` calls.

We can multiply these changes by `deltaTime` and `fixedDeltaTime` to make the changes dependent on the time gap between function calls leading to a uniform experience.

```cs
private float moveSpeed=5.0f;

void Update()
{
    ...
    Vector3 moveDir=GetMoveDirection();
    // transform.position += moveDir * moveSpeed;
    transform.position += moveDir * moveSpeed * Time.deltaTime; // multiply deltaTime 
    ...
}
```

OR

```cs
private float acceleration=2.5f;

void FixedUpdate()
{
    ...
    Vector3 moveDir = GetMoveDirection();
    Vector3 velocity = rb.velocity;
    // velocity += moveDir * acceleration;
    velocity += moveDir * acceleration * Time.fixedDeltaTime; // multiply fixedDeltaTime
    rb.velocity = rb.velocity;
    ...
}
```

The above examples are the most general forms of movement you can see in a game.

### References
1. [Transform.position - Unity Documentation](https://docs.unity3d.com/ScriptReference/Transform-position.html)
2. [Rigidbody.velocity - Unity Documentation](https://docs.unity3d.com/ScriptReference/Rigidbody-velocity.html)
3. [Rigidbody.AddForce - Unity Documentation](https://docs.unity3d.com/ScriptReference/Rigidbody.AddForce.html)
4. [ForceMode - Unity Documentation](https://docs.unity3d.com/ScriptReference/ForceMode.html)
5. [Update - Unity Documentaion](https://docs.unity3d.com/ScriptReference/MonoBehaviour.Update.html)
6. [FixedUpdate - Unity Documentaion](https://docs.unity3d.com/ScriptReference/MonoBehaviour.FixedUpdate.html)
7. [Time.deltaTime - Unity Documentaion](https://docs.unity3d.com/ScriptReference/Time-deltaTime.html)
8. [Time.fixedDeltaTime - Unity Documentaion](https://docs.unity3d.com/ScriptReference/Time-fixedDeltaTime.html)