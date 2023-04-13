---
order: 9
icon: comment-discussion
tags: 
---

# F.A.Q

---
### Q: Can the system be used for a more traditional inventory system where all items are 1x1?
A: Of course! Containers have styles, and the List style already converts all items into 1x1.

---
### Q: Is there a crafting system implemented?
A: No. This is an inventory system, not an inventory and crafting system. I know many assets on the marketplace already include a crafting system, but crafting systems are very genre dependent and often behave completely differently. I don’t think I can implement any sort of abstract base that can adapt to all scenarios. I feel like most people will either just not use the system I implement or remove it, and the time I spend implementing it will just raise the price.
At the end of the day, crafting systems just require item data and some form of a “recipe”. It should not be difficult to create your crafting system with the item data system this asset uses and the functions the inventory component has.
I might change my mind someday and attempt to implement some sort of abstract base, similar to what this plugin attempts to do for inventory systems. But as of right now, there are no plans to implement anything relating to a crafting system.

---
### Q: Will stats/attributes or ability systems of any kind be added?
A: No. I believe most people should be using the Gameplay Ability System framework as it is becoming the industry standard or something similar (I personally suggest <a href="https://www.unrealengine.com/marketplace/en-US/product/gas-companion" target="_blank">**GAS Companion**</a> from the marketplace). A lot of people also already have a stat/attribute or ability system in place and I do not want to step on their toes.

---
### Q: Will I be adding any art, meshes or animations to the asset?
A: No, with the exception of some basic things like the Tile Icon and some other things, just enough so you don’t get any compile errors and can interact with the system before you implement your art.

---
### Q: Does the plugin feature an interaction system?
A: This plugin does not provide a fleshed-out interaction system for you, but an interface system is integrated in the demo's as that's the foundation of every interaction system I've seen. This is done so your project does not build a dependancy on my interaction system if you ever decide this plugin does not fit your project. The interaction system is designed to be replaced, as most people already have an interaction system already in place.

---
### Q: Will I be adding X feature/system? For example item weight.
A: Only if it’s a highly requested feature, only then will I either implement it into the base asset itself or make a youtube tutorial on how to implement it yourself. You can track what is being worked on and what will be worked on in the future by going to the Trello link at the top of the page.
