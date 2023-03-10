---
order: 20
tags: [item]
REVISION NOTES: Paragraph explaining OT_Equipment might be worth explaining in here now because we have the HTML blueprint graphs.
---

# Equipping and unequipping items

---
A big problem I try to solve with this system is that I did not want to assume anything about how your actors' meshes are set up, how your equipment setup looked like and I did not want to make a base player or actor blueprint.
There are two main reasons behind this decision: 
First is this allows you complete control over what items are attached to (for example if you have a horse and want to give it armor, but the component is only setup on the player) and secondly is that people might already have a player blueprint hierarchy setup and I did not want to mess with that and some people might not want to use my hierarchy setup.

Your container's compatibility settings dictate what is allowed in them. If you only want helmets to go into container X, then your helmet items should use <span style="color:slateblue">**EquipmentSlot**</span> (Rename one of the options to Helmet) and your container should only allow Helmets. You can also of course rename one of the <span style="color:slateblue">**EquipmentType**</span> enum entries to “Head” and make your helmet item be of <span style="color:slateblue">**EquipmentType**</span> “Head” and then all items that might not be a helmet, but are a head item like glasses can go into that container.

The logic that runs the equipping and unequipping logic is stored in a object and item driver called <span style="color:violet">**IO_Equipment**</span> and <span style="color:violet">**ID_Equipment**</span> (if you are unfamiliar with the [item driver system](https://inventoryframework.github.io/classes-and-settings/o_itemobjectandac_itemdriver/), I suggest getting familiar with it first) and they use a struct called <span style="color:slateblue">**EquipmentData**</span>, this struct attempts to solve most issues when it comes to having multiple skeletons, meshes or complex equipment logic, such as having a gun attached to a backpack which is attached to the player character and that gun has a sight attached to it or FPS shooter setups where you have a local arms skeleton and then a full body skeleton. This struct also attempts to keep the system away from hard coding any types of “main weapon” and “secondary weapon” or anything of the sort. I’ve found those types of setups  often become cumbersome when a designer wants to add or remove these types of things.

Keeping the equipping system inside of a item driver provides a modular way for you to allow any item you want to be equippable, or trigger special animations when its added to an items container. It also allows you to easily implement your own system if you want without rewriting C++ code or affecting how the inventory system works at a fundamental level.

During my research, I found an astonishing amount of different workflows and processes. It was very daunting to try and solve all of them in one go, and maybe my system isn’t perfect, but it tries to solve as many as possible at once, while still being flexible and adjustable.

I recommend reading the comments in the <span style="color:slateblue">**EquipmentData**</span> struct (Found in <span style="color:violet">**IFP_CoreData.h**</span>) and going through <span style="color:violet">**ID_Equipment**</span> (which is an item driver found in the plugins folder Core -> ActorParents -> DriversAndObjects -> Equipment.) and look at the logic in there to get a better understanding of how the system is using this struct. I would explain it here, but it’s so long and extensive that explaining it in just text and pictures would take too long.
This driver attempts to copy the system that is used in GAS with its <span style="color:brown">**PlayMontageAndWait**</span> function, which creates an object that handles the animation, but I wanted to keep mine inside of Blueprints.

---
## Optimizations
For weapons and equippables, I've implemented three methods of creating your equipment mesh (Item reperesentation):
1. Blueprint, this allows you to simply use any blueprint you've made as the item representation.
2. Skeletal mesh
3. Static mesh

Blueprint's are obviously the most expensive, but are the most customizable. I recommend staying away from using Blueprint's as your item representation whenever possible.

For skeletal and static meshes, you gain access to most settings you would have in the details panel while editing them in a blueprint

==- Notes regarding Skeletal and Static Meshes
The default material section is not available (I assume this is an engine bug?). If you wish to change the default materials, go into Rendering -> Override Materials
===

---
# Notes on general equipment system design
- Equipping and unequipping items can get messy, most importantly it can get messy when players start spamming the system with animations, either by quickly equipping and unequipping an item really quickly or equipping multiple items, all trying to activate montages which will interrupt each other.
In the demo, items do not play an animation when equipped or unequipped. But they do play an animation when you holster or unholster an item. If you're in multiplayer, the item will also be added to the network queue to prevent clients from spamming the server with RPC's.
By default, there is no failsafe if an animation is interrupted as that is something most designers want to implement their own system into. Some designers might want players to be able to cancel animations or cancel the equip if it was interrupted. It is up to you to implement any sort of failsafe if players are finding ways to manipulate this animation cancelling in ways you don't like.

- If you're going to have items visible on your character, it is recommended to have at least one setup for every item that instantly attaches the item to the desired location and mesh. This is so when you load a save, you won't get several animations playing at the same time and overlapping each other.