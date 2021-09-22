# W Project

In this new project I want to create a fully playable and functional level with a few spell/power mechanics, AI's with ability to damage the player, main menu, pause menu, HUD, and an fully designed level (*for creating the level environments, objects and textures I am going to use AssetForge, which is a very helpfull app where you can build 3d models and basically everything with building blocks*). The goal of this project is to learn how to work with behavior trees which allows design an AI behavior, to create an fully animated character (*not just walking, running and jumping but also different animations for different spells, damage animation, death animation and others*), and most importantly to design and create an game level. The game I'm using as a reference is The Legend of Zelda Link's Awakening (2019 remake). An avdenture with fixed or top-down camera, minimalistic and simple look with all environments and textures in one similar/lore friendly style which creates a recognizable and unique design.
I have already finished implementing the character in the project with several animations, 3 spells with animations, Health and Mana HUD which have tied variables and actually react to damage or heal spell.
I'll start with a gameplay demo and after with screenshots of blueprints.


Right now I have only 3 spells, one is heal spell, another fireball spell and the last is a passive ability which is mana regeneration. Also, I created the health and mana bar which are connected to each other. Both health and mana represent variables with value equal to 1.0, everytime the player uses the heal spell, the mana bar decreases with 0.15 and health bar increases with 0.2, I didn't make the numbers equal because other spells require mana as well so it needs some balance. 

# Heal Spell

One of the most important variable in the whole heall spell logic is the CastDelay boolean variable. This variable is used in animation blueprint and basically represent a state in which the character stops and plays the spell casting animation. By default this variable is set to false. The first branches and nodes check if the mana is bigger than 0 and if it is true then it makes the character to stop. The Spawn Emmiter at Location node spawns an special effect/FX effect at any given location, in my case the location is the Cleric_Staff (*can be find in character skeletal mesh*), after this the CastDelay is set to true which means that character enters the casting state for 1.5 seconds and after the animation we regain the control and movement over the character.
![1](https://user-images.githubusercontent.com/90534698/134270353-d1558662-fcdd-49ea-8a84-15bd4e87a89c.png)
![2](https://user-images.githubusercontent.com/90534698/134270397-4a99865a-75db-47da-9ac5-2936d603cffd.png)

# Mana Regeneration

Here are 2 variables that make the whole logic work. Mana rate means that every x seconds the logic will fire, mana regen variable mean how many mana will regenerate (*in my case Mana rate is set at 2 and mana regen at 0.03 which means that every 2 seconds mana increases by 0.03 where mana is at the 1.0 value*). Obviously, all this work only when the mana is less than 1.0.
![3](https://user-images.githubusercontent.com/90534698/134272348-baebe5f2-280a-434b-b128-e2905a6adb7d.png)

# Fireball

For this spell the logic is pretty similar at the beginning. The exception is that we need to create a new boolean variable similar to CastDelay but this time it will store another stance with another animation. Till the true CastingStaffC and the delay everything is similar to heal spell. This time we need to actually spawn an object which will fly from the character and hopefully will look as a fireball. To do this I used the node SpawnActor with class FireProjectile. This class is just an actor blueprint with a sphere and particle system.
![4](https://user-images.githubusercontent.com/90534698/134273421-8805fa02-c413-4f01-993a-64d0e20b15fa.png)
Now, the location must be the ending of the wizard staff, I can try and use the node GetSocketLocation like in heal spell but there is a problem in this case. The fireball is an object which spawns at given coordinates, the problem here is that the staff is always in motion, it has an animation when the character is running. So, everytime we fire the fireball the casting animation will fire fine but the fireball itself will spawn at different locations, sometimes lower than the staff position, sometimes above the staff. To avoid this I used GetActorTransform (*actor being ThirdPersonCharacter which is the wizard*), BreakTransform and MakeTransform. What actually these nodes do is that we take the character coordinates (*I didn't use the getactorlocation because the actor can be in motion and the coordinates will be different everytime*)

