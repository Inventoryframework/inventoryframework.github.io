---
tags: [underrevision, tools]
---
# Productivity

---
## Inventory Helper
You will very likely be spending a good amount of time using this utility widget, so I suggest bookmarking it or having some way of quickly accessing it while you work.

Working with the raw container struct and the array of items is extremely cumbersome and not very efficient use of time. It also does not provide any sort of feedback of items that might be overlapping or have gone outside the bounds of the container.

This is why I've developed the Inventory Helper. This is a editor utility widget which attempts to mimick the final result you'd see in game and providing a place for you to create your own in-editor tools without going into C++.

It is **HIGHLY** advised to rarely, if ever, work with the raw container settings array. The only exception is class defaults. Use the inventory helper whenever you can.

Because of how many features this helper has, it becomes a lot to write down, so I've made a video (Not yet released) going over how to use the inventory helper and cover many of its features.

### The void system
What might confuse most is the "Void" widget, actor and data asset. Dragging and dropping an item onto the background will "pin" the item exactly where you dropped it. But the item data and its containers must be stored on an actor *somewhere* in the currently loaded level. The void actor is where that data is stored. This also has the benefit of acting as a proxy for moving an item from one actor to another.
You can select one actor, move an item into the void area, then select another actor and move the item into one of its containers.

There is one caviat though, the actor is reset when you open a new level. This is why there's a data asset holding a second copy of the container settings and storing some other data in there, so when you change levels the newly created item void actor can just open the data asset and retrieve your saved data from there. Meaning you can transport items from one level to another, including its containers.
If you are using source control, I suggest adding DA_ItemVoid to your ignore list, as it's data is not something you want to bother with.

==- Limitations
- The normal highlight widget is not supported. An alternate method is used inside the inventory helper.
- Item splitting is not supported.
- Only basic item collision is supported, no combining or stacking.
- The Item Void actor and data asset is not meant to be interacted with outside the inventory helper. If you wish to modify the Item Void regularily, consider modifying
- Generated item icons do not work. It is suggested opening your game, inspecting the item and saving the generated icon to disk, then setting it inside the data asset.
===

