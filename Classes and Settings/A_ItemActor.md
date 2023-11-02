---
tags: [item]
---

# A_ItemActor
==- C++ Item Actor
File location: InventoryFrameworkPlugin\Public\Core\Actors\Parents\A_ItemActor.h
File location: InventoryFrameworkPlugin\Private\Core\Actors\Parents\A_ItemActor.cpp
==- Blueprint Child: BP_ItemActor
File location: Content\Core\ActorParents\BP_ItemActor.uasset
===
---

This is your physical representation of the item. If you already have items/weapons of any sorts, you will want to reparent those classes to this.

This is not supposed to run important logic as these can get destroyed and created by the user. (for example equipping and unequipping)

In your data asset, you can access this class so whenever you drop the item out of the inventory or want to spawn it in some way, here is how you access the original pickup class.

It’s very important when making your items that have a physical representation in the world and can be picked up that the component on that actor references the data asset that references this physical actor. Otherwise when you drop the item, it’ll spawn the wrong class.

The setup for pickups goes like this: In your <span style="color:slateblue">**ContainerSettings**</span> index 0, you set <span style="color:slateblue">**ContainerType**</span> to <span style="color:slateblue">**ThisActor**</span> and item index 0 is the pickup data asset. There should be no other items in this items array.
Containers that might be attached to the pickup are added after and they can have Inventory or Equipment selected, but never <span style="color:slateblue">**CurrentItem**</span>.

---
# Custom Blueprint Children
The <span style="color:violet">**BP_ItemActor**</span> is a optional parent in your hierarchy. It has some code that you will want to copy over to any parents that you have if you wish to make your own, primarily the BeginPlay, Equip and Unequip delegates, and Destroyed event.

The C++ inventory compoonent will automatically get added to your actor, but the base C++ version should not be used. You want to click on the InventoryComponent in your components list and set ComponentClass to BP_AC_Inventory (Or your own child of the inventory component if you've made one)
