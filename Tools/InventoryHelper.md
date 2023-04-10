---
tags: [tools]
---
# Inventory Helper
<a href="https://youtu.be/5CsMMlH9w2Y" target="_blank">**Video documentation**</a>


---
You will very likely be spending a good amount of time using this utility widget, so I suggest bookmarking it or having some way of quickly accessing it while you work.

Working with the raw container struct and the array of items is extremely cumbersome and not very efficient use of time. It also does not provide any sort of feedback of items that might be overlapping or have gone outside the bounds of the container.

This is why I've developed the Inventory Helper. This is a editor utility widget which attempts to mimick the final result you'd see in game and providing a place for you to create your own in-editor tools without going into C++.

It is **HIGHLY** advised to rarely, if ever, work with the raw container settings array. The only exception is class defaults. Use the inventory helper whenever you can.

Because of how many features this helper has, it becomes a lot to write down, so I've made a video (Not yet released) going over how to use the inventory helper and cover many of its features.

---
### The void system
What might confuse most is the "Void" widget and actor. Dragging and dropping an item onto the background will "pin" the item exactly where you dropped it. But the item data and its containers must be stored on an actor *somewhere* in the currently loaded level. The void actor is where that data is stored. This also has the benefit of acting as a proxy for moving an item from one actor to another.
You can select one actor, move an item into the void area, then select another actor and move the item into one of its containers.

==- Limitations
- It is NOT recommended to edit the <span style="color:slateblue">**Container Settings**</span> through the <span style="color:slateblue">**SelectedComponent**</span> widget. The reason being is that the <span style="color:brown">**OnPropertyChanged**</span> event does not return which index has been modified. Meaning it is not possible to resolve which container or which item was modified, which means that every container widget and item widget must be updated. Only modify the container settings inside this widget if you really have to.
- The normal highlight widget is not supported. An alternate method is used instead.
- Item splitting is not supported.
- Only basic item collision is supported, no combining or stacking.
- The Item Void actor is not meant to be interacted with outside the inventory helper. If you wish to modify the Item Void regularily, consider modifying the InventoryHelper and have the tool modify it for you.
- <a href="https://trello.com/c/SE98T1XT/64-generated-item-icons-for-editor-utility-widgets" target="_blank">**Generated item icons do not work**</a> . It is suggested to open your game, inspecting the item and saving the generated icon to disk, then setting it inside the data asset.
===