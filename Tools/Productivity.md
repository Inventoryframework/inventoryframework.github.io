---

---
# Productivity

---
## Inventory Helper
You will very likely be spending a good amount of time using this utility widget, so I suggest bookmarking it or having some way of quickly accessing it while you work.

Working with the raw container struct and the array of items is extremely cumbersome and not very efficient use of time. It also does not provide any sort of feedback of items that might be overlapping or have gone outside the bounds of the container.

This is why I've developed the Inventory Helper. This is a editor utility widget which attempts to mimick the final result you'd see in game and providing a place for you to create your own in-editor tools without going into C++.

==- Limitations
- The normal highlight widget is not supported. An alternate method is used inside the inventory helper.
- Item splitting is not supported.
- Only basic item collision is supported, no combining or stacking.
- The Item Void actor is not meant to be interacted with outside the inventory helper. If you wish to modify the Item Void, you will need to do that through the inventory helper.
===

It is **HIGHLY** advised to rarely, if ever, work with the raw container settings array. The only exception is class defaults. Use the inventory helper whenever you can.

Because of how many features this helper has, it becomes a lot to write down, so I've made a video going over how to use the inventory helper and cover many of its features.

What might confuse most is the "Void" widget and actor. Dragging and dropping an item onto the background will "pin" the item exactly where you dropped it. But the item data and its containers must be stored *somewhere*