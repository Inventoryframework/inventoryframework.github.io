---
tags: [item]
---

# A_ItemPhysicalRepresentation
==- Actor Item Physical Representation
File location: InventoryFrameworkPlugin\Public\Core\Actors\Parents\A_ItemPhysicalRepresentation.h
File location: InventoryFrameworkPlugin\Private\Core\Actors\Parents\A_ItemPhysicalRepresentation.cpp
==- Blueprint Child: BP_SM_ItemPhysical
File location: Content\Core\ActorParents\BP_SM_ItemPhysical.uasset
===
---

This is your physical representation of the item. If you already have items/weapons of any sorts, you will want to reparent those classes to this.

This is not supposed to run important logic as these can get destroyed and created by the user. (for example equipping and unequipping)

In your data asset, you can access this class so whenever you drop the item out of the inventory or want to spawn it in some way, here is how you access the original pickup class.

It’s very important when making your items that have a physical representation in the world and can be picked up that the component on that actor references the data asset that references this physical actor. Otherwise when you drop the item, it’ll spawn the wrong class.

The setup for pickups goes like this: In your <span style="color:slateblue">**ContainerSettings**</span> index 0, you set <span style="color:slateblue">**ContainerType**</span> to <span style="color:slateblue">**ThisActor**</span> and item index 0 is the pickup data asset. There should be no other items in this items array.
Containers that might be attached to the pickup are added after and they can have Inventory or Equipment selected, but never <span style="color:slateblue">**CurrentItem**</span>.
