---
layout: post
title: Summer Game Jam Highlights - Bouncer 3D
subtitle: "A quick review of the design and the creation process"
date: 2021-10-11
author: "Abhishek Pardhi"
# header: "/img/kaka.png"
---

*We recently conducted the Summer Game Jam 2021. This article presents Abhishek's experience and design choices making his game, Bouncer3D, which you can play [here](https://abhishek-pardhi.itch.io/bouncer-3d).*

## **Initial Design**

It all started one day when I was playing this game called *Mini-Militia*; I started thinking about what this game would look like if it were a 3d game instead of 2D, and the bullets which we shoot bounces off the walls of the map and can also damage the Player who shoots those bullets!!. So when the **Summer Game Jam** started, I thought of implementing this idea into my game which I would name **BOUNCER 3D**.

![concept](https://img.itch.zone/aW1nLzY2NDgzMTgucG5n/315x250%23c/KqFFuq.png)

## **Development Process**

At first, I thought of making it a multiplayer game, but since I was left with only three days to complete my project, I had to drop it and make it **Human vs. AI**. Then I started making a map (which has to have a closed surface so that bullets don’t get thrown out like an astronaut lost in deep space :p) that now looks like an Icosphere and then implemented basic movement of the Player around the map. I added Bullets to my game and Particle System to have an explosion effect whenever the bullet collides with some object. Then I employed enemies into my game and added AI behaviour(like following the Player & shooting at the Player) to them. To make it a bit more challenging, I set the spawn rate of enemies to continuously increase until it reaches a value of 7 spawns/second (chaos indeed!). After a bit of thinking, I decided to make the score equal to the time survived by the Player in seconds. To give my game a final touch, I added a few scenes like the main menu, pause menu, etc., and spooky music to the main menu scene.

![development](https://img.itch.zone/aW1hZ2UvMTE0NTU4Ny82NjQ4Njg1LnBuZw==/original/ImwaPs.png)

## **Rules of the game (In Mobile version):**
- You can move anywhere inside the icosphere using the joystick.
- You can shoot as many balls as you want and reloading takes 2 seconds.
- Balls shot by you/enemy bounces off the walls 3 times before they get destroyed at 4th collision with the wall.
- Balls shot by you can damage(=10hp) to you as well and same goes for enemeies.
- You have max. 100hp while enemies have max. 20hp.
- Rate of spawn of enemies increases with time until it reaches 7 per second.
- Your score=number of seconds you survived.
- Lastly, play the game in landscape mode!

## **Conclusion**
In the end, I had with me my first **3d FPS Shooter Survival** game made by me as an individual. Overall, it was a fascinating and incredible learning experience while using the Unity game engine to make this game.

![conclusion](https://img.itch.zone/aW1hZ2UvMTE0NTU4Ny82NjQ4Njg4LnBuZw==/original/XhxvKJ.png)

PS: This game which I made for the Summer Game Jam 2021, is a prototype. I might complete this project later on if I get time and interest in it. Until then, **Enjoy!!!**
