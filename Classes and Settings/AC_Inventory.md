---
tags: [InventoryComponent]
---
# AC_Inventory
==- Actor Component Inventory
File Location: Source\InventoryFrameworkPlugin\Public\Core\Components\AC_Inventory.h
File Location: Source\InventoryFrameworkPlugin\Private\Core\Components\AC_Inventory.cpp
==- Blueprint Child: BP_AC_Inventory
File Location: Content\Core\Components\BP_AC_Inventory.uasset
==-

---

This is where the vast majority of the logic is handled and starts. You have full control of when the component activates by calling the <span style="color:brown">**StartComponent**</span> function. When called, the component initializes the containers and the items inside or the pickup if the owning actor is a pickup. This does NOT prepare the widgets versions of the containers and the items.

To create the widget version of a container and its items, call <span style="color:brown">**BindContainerWithWidget**</span> and hook in the container struct and the container widget. This should give you full control of when you want to construct a container.
Some examples would be when the player opens their inventory, but you don’t want to load all the items' attachment widgets. You could then load the attachment widgets when the player inspects the item. For inventory systems that can get massive with a lot of items inside of it with attachment widgets associated with it, this should massively improve performance and memory usage if used correctly.

For pickups, I recommend calling <span style="color:brown">**StartComponent**</span> on either <span style="color:brown">**BeginPlay**</span> or when the player is close to the item and has line of sight of the item, as items that have a spawn chance associated with it need that spawn chance rolled.
The item might also have items attached to it and be visible, such as a sight on a gun, which also might have a spawn chance and you don’t want that sight to be visible if it failed its spawn chance.

---
## Settings
The first category is <span style="color:green">**Settings**</span> and most of these are instance editable, so you’ll be spending most of your time with these settings for your item actors.

<span style="color:slateblue">**ContainerSettings**</span> is where the majority of the work is done. I recommend going into <span style="color:violet">**IFP_CoreData.h**</span> and find <span style="color:slateblue">**FS_ContainerSettings**</span>  and looking at the comments for everything as there are some variables that aren’t instance editable.

---
## User Settings
The second category is <span style="color:green">**UserSettings**</span> and these are settings that are meant to be used by the player during runtime. It’s up to your settings system to update these if you want the player to be able to change these settings during runtime.
The input system is extremely basic and is meant to be replaced. They are never used at a C++ level, so your designers can replace it easily.
<span style="color:slateblue">**RotateKey**</span> : While dragging an item, the player might want to rotate an item. When they press this key, it’ll rotate.
<span style="color:slateblue">**SplitKey**</span> : While dragging a stackable item, when they drop the item onto a valid tile, they can split an item into two stacks.

---
## Color settings
Third category is <span style="color:Slateblue">**ColorSettings**</span>  and this is where most of the colors used by the system are dictated. This is set in here so all the widgets can fetch colors easily and so designers can extend any color settings (for example color blind settings) to the user settings. If you want to add any color settings, this place is the preferred place. Item rarity colors are handled in here.

---
## Network Queue
The component has an array called <span style="color:Slateblue">**NetworkQueue**</span>, these are items that are waiting for a server RPC to finish. It is up to you to call <span style="color:brown">**C_AddItemToNetworkQueue**</span> and <span style="color:brown">**C_RemoveItemFromNetworkQueue**</span>. This will automatically call <span style="color:brown">**ItemAddedToNetworkQueue**</span> or <span style="color:brown">**ItemRemovedFromNetworkQueue**</span>.

If the item widget is available, it'll also call: 
<span style="color:violet">**W_InventoryItem**</span> -> <span style="color:brown">**ParentItemAddedToNetworkQueue**</span> / <span style="color:brown">**ParentItemRemovedFromNetworkQueue**</span>

These events are where your designers hook in any logic that alerts the player the item is pending some networking event. This is most often seen as making the item's icon gray-scale or flashing the icon.

You will want to implement some way of preventing players from interacting with items that are pending a network event, either by making the widget uninteractable or through code. You can check if an item is in the queue by using:
<span style="color:violet">**FL_InventoryFramework.h**</span> -> <span style="color:brown">**IsItemInNetworkQueue**</span>

Containers can also be added to the network queue, though for now it is only used for <span style="color:brown">**AdjustContainerSize**</span>. It has all the same functions as the item network queue, but with the word "Item" replaced with "Container".
It is highly recommended to "soft lock" players from spamming <span style="color:brown">**AdjustContainerSize**</span> as it can get very heavy for network traffic.

The Network Queue system is only relevant for clients, it's never used in single player or listen server scenarios.

---
## Infinite containers
Containers can be infinite in either the X or Y direction, the system tries to dynamically resize the container whenever an item is moved, added or removed from it.

---
## States
The containers can be in one of three states during development.
- The "raw" state is where <span style="color:slateblue">**UniqueID**</span>'s are not set and containers <span style="color:slateblue">**BelongsToItem**</span> directions are direct index directions and all object references are null. The  component is designed to be in this state for when you call the <span style="color:brown">**StartComponent**</span> function. This is where all the data is "human readable", but most functions won't work.
- The "Editor" state is where <span style="color:slateblue">**Unique**</span>'s have been initialized and <span style="color:slateblue">**BelongsToItem**</span> are set to the items and containers <span style="color:slateblue">**UniqueID**</span>'s and some object references might be valid. This state is meant for development and it might make some data not human-readable, and it'll mean most of your functions will work as if you are in a valid game session. Many tools depend on the component being in this state, such as the  InventoryHelper.
- Finally there's the "Initialized" state. This simply means the gameplay logic has called the <span style="color:brown">**StartComponent**</span> function and you are inside a gameplay session. The component should never be put into this state while you're outside of a game session.

You can use <span style="color:brown">**GetComponentState**</span> to resolve what state it is in, and look inside that function to better understand what conditions must be met for each state.

There is a function called <span style="color:brown">**ConvertToRawState**</span> which will prepare all containers for a gameplay session. Ideally, this function should not be called during a gameplay session as it's a waste of CPU time. All container data should already be prepped for gameplay.
There is also another function called <span style="color:brown">**ConvertFromRawStateToEditorState**</span> which preps all data to be usable for editor tools.

There is a editor utility widget called "EUW_EditorStateChecker" which simply checks a list of actors and their inventory components and checks the components state, then return a list of all actors that aren't prepped for gameplay. The component also gives you a message on its BeginPlay if it's state was still in the Editor state.