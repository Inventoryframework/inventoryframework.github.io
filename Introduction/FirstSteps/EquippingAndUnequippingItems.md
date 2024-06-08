---
order: 20
tags: [item]
REVISION NOTES: Paragraph explaining OT_Equipment might be worth explaining in here now because we have the HTML blueprint graphs.
---

# Equipping and unequipping items

!!!Important
This documentation covers the default equipment system. In some cases, you might want to implement your own or use a hybrid solution, which is covered more at the bottom of the page.
!!!

[Deep dive video](https://youtu.be/Tb7FwW3ri6k)

---
A big problem I try to solve with this system is that I did not want to assume anything about how your actors' meshes are set up, how your equipment setup looked like and I did not want to make a base player or actor blueprint.
There are two main reasons behind this decision: 
First is this allows you complete control over what items are attached to (for example if you have a horse and want to give it armor, but the component is only setup on the player) and secondly is that people might already have a player blueprint hierarchy setup and I did not want to mess with that and some people might not want to use my hierarchy setup.

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

Blueprint's are about 11 times more expensive to spawn than meshes, but are the most customizable. I recommend staying away from using Blueprint's as your item representation whenever possible. For multiplayer, meshes are encouraged even more as they are very cheap to replicate.

==- Profiler
![](/pictures/Profiler_BlueprintEquipment.png)

![](/pictures/Profiler_StaticMeshEquipment.png)

Blueprints took 1.5ms, while static mesh only took 0.131ms
===

For skeletal and static meshes, you gain access to most settings you would have in the details panel while editing them in a blueprint

==- Notes regarding Skeletal and Static Meshes
The default material section is not available (I assume this is an engine bug?). If you wish to change the default materials, go into Rendering -> Override Materials
===

==- Notes for optimal multiplayer experience
The way Unreal handles replication for attached actors is not the most elegant. When another player becomes relevant for another client, not all attached actors will get replicated within the same frame, and they might not even replicate in the order that you want them to. Leading to odd snapping or pop-in. This could be awkward if you have naked characters and then the clothing around them are blueprints.

This is another reason why it's suggested to stay away from blueprints for multiplayer, as with meshes, since they get attached to the actor itself, when it comes time to replicate it theres no snapping or pop-in. All components on an actor are replicated within the same frame.

There is always a chance a packet will be dropped, which is again why you should refrain from blueprints as the engine does a very good job ensuring every mesh component is replicated. For every blueprint, you're adding another actor, and another equipment manager component, and more RPC's and more RepNotify's and each of those have a chance to get dropped.
===

These optimizations of course only matter depending on the scope of your game. This more or less serves as an idea of what to focus on during the optimization phase of your game and what you might have to look for.

---
## Events
There are two events that can activate when equipping and unequipping an item occurs.
1. The first is the component's <span style="color:brown">**ItemEquipped**</span> and <span style="color:brown">**ItemUnequipped**</span> delegate. This is only called when an item is added or removed from a slot. This is only called on the actor that the inventory component the slot belongs to, so equipping a weapon as a player will have the delegate call on the player, not the weapon.
2. Two interface functions also called <span style="color:brown">**ItemEquipped**</span> and <span style="color:brown">**ItemUnequipped**</span> from <span style="color:violet">**I_Inventory**</span> are called whenever the equipment system spawns or updates the status of an item. For example, equipping a gun so it spawns, then holstering it and unholstering it.
    - If your settings have the item set as a blueprint, these events are called on the spawned blueprint. For example, equipping a weapon as the player will have the interface functions called on the weapon, not the player. The inverse of point 1.
    - If your settings have the item set as a static mesh or skeletal mesh, the interface functions are called on the player.

---
## Notes on general equipment system design
- Equipping and unequipping items can get messy, most importantly it can get messy when players start spamming the system with animations, either by quickly equipping and unequipping an item really quickly or equipping multiple items, all trying to activate montages which will interrupt each other.
In the demo, items do not play an animation when equipped or unequipped. But they do play an animation when you holster or unholster an item. If you're in multiplayer, the item will also be added to the network queue to prevent clients from spamming the server with RPC's.
By default, there is no failsafe if an animation is interrupted as that is something most designers want to implement their own system into. Some designers might want players to be able to cancel animations or cancel the equip if it was interrupted. It is up to you to implement any sort of failsafe if players are finding ways to manipulate this animation cancelling in ways you don't like.

- If you're going to have items visible on your character, it is recommended to have at least one setup for every item that instantly attaches the item to the desired location and mesh. This is so when you load a save, you won't get several animations playing at the same time and overlapping each other.

---
## Why are all my components getting renamed?
The system automatically tries to keep all components attached to an actor with unique names. Including attached actors. This is because of how the  preview system works. The preview system attempts to reconstruct the actor it's previewing without fully cloning it. There are ways of perfectly cloning an actor, but those are very expensive and come with their own set of problems. The preview system only looks at the visible components on the actor, including attached actors, and reconstructs it. But problems occur if two components have the same name from 2 different attached actors.
When the preview system reconstructs the actor, it finds out what component it was attached to, but it needs to know which component that is on itself, it can't use the object reference. This means that if two components have the same name, it'll mess up which component it should attach to. The simple fix to this is to make sure all components have a unique name. The engine already handles this if you attach a component to an actor and that actor has a component with the same name, but it does not do this for attached actors.

But to simplify debugging, only a underscore and a random number is added to the end of it. The  rest of the name is untouched.

---
## Where are the items being created?
A separate actor component called <span style="color:violet">**BP_AC_EquipmentManager**</span> is created and attached to the owning actor to handle the creation of items. This is handled this way because of replication. Before the system was replicated, this component didn't even exist and the system worked just like it does now.

This component is responsible for ensuring items are replicated in the correct order and attaching things in the correct order, in case an item got replicated before the item it is attached to is replicated.

---
## Common issues
Physics is typically the main culprit for the equipment system breaking. If an item or components have physics on by default, they will instantly detach the moment they are initialized. Breaking the entire equipment system. Physics will also cause issues with generated item icons, as that system relies on the attachment hierarchy being correct and detaching things due to physics will break it.

---
## Custom equipment system
It's fairly simple to completely replace the equipment system or use a hybrid solution.
To understand the beginning of the chain, the equipment system begins ideally on the actor on the Inventory -> ItemEquipped delegate. This is where you'd inject any modifications to the base behavior of the equipment system.

In some cases, it's simpler to just swap out a mesh with a new one. For example, the player might have a default jacket if no other jacket is equipped. In this case, it's much simpler to just use a custom equipment object which holds that other mesh and swap the default jacket mesh with the new one.

It all comes down to the complexity of your game. The default equipment system tries to cover **a lot** of different and complex scenarios, thus it has become big and can be overcomplicated depending on the simplicity of your project.