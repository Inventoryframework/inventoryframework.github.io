---
order: 9
icon: comment-discussion
tags: [underrevision]
---

# F.A.Q

!!!danger
Under revision
!!!

### Q: Can the system be used for a more traditional inventory system where all items are 1x1?
A: Of course! The demos even include traditional inventory systems. There’s a lot of math and nested for loops being done with the idea of things being in different dimensions (also one of the main reasons why a lot of the components functions are in C++, as blueprints aren’t the best at math and nested for loops). But since all that math and nested for loops is being handled in C++, from my performance measurements there’s virtually no performance impact, even with massive items. With that in mind, this system should handle 1x1 items with ease.

### Q: Is there a crafting system implemented?
A: No. This is an inventory system, not an inventory and crafting system. I know many assets on the marketplace already include a crafting system, but crafting systems are very genre dependent and often behave completely differently. I don’t think I can implement any sort of abstract base that can adapt to all scenarios. I feel like most people will either just not use the system I implement or remove it, and the time I spend implementing it will just raise the price.
At the end of the day, crafting systems just require item data and some form of a “recipe”. It should not be difficult to create your crafting system with the item data system this asset uses and the functions the inventory component has.
I might change my mind someday and attempt to implement some sort of abstract base, similar to what this plugin attempts to do for inventory systems.

### Q: Will stats/attributes or ability systems of any kind be added?
A: No. I believe most people should be using the Gameplay Ability System framework or something similar and there are a few ways to implement that system (I personally suggest <a href="https://www.unrealengine.com/marketplace/en-US/product/gas-companion" target="_blank">**GAS Companion**</a> from the marketplace). Some people also already have a stat/attribute or ability system in place and I do not want to step on their toes.

### Q: How do you add more variables to items and create new item types?
A: (Insert video tutorial here)

### Q: Will I be adding any art, meshes or animations to the asset?
A: No, with the exception of some basic things like the Tile Icon and some other things, just enough so you don’t get any compile errors and can interact with the system before you implement your art.
I dislike assets that sell themselves as one thing, but then start padding the asset with things that aren’t relevant to what the asset is advertising, which just raises the price and most people just end up removing those extra assets anyways. This is an inventory system and will always be just an inventory system.

### Q: Does the plugin feature an interaction system?
A: This plugin does not provide a fleshed-out interaction system for you, but an interface system is integrated in the demo's as that's the foundation of nearly every interaction system I've seen. This is done so it's more flexible to a project's interaction system. Some use on-click, some use linetrace or something else, but at the heart of most of them is an interface handling the interaction. This is designed to be replaced, as most people already have an interaction system already in place.

### Q: Will I be adding X feature/system? For example item weight.
A: Only if it’s a highly requested feature, only then will I either implement it into the base asset itself or make a youtube tutorial on how to implement it yourself.
