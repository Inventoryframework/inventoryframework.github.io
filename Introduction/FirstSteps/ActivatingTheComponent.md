---
order: 30
tags: [inventorycomponent]
---

# Activating the component

---
## Starting the component
Starting the component is very simple, just call <span style="color:violet">**AC_Inventory**</span> -> <span style="color:brown">**StartComponent**</span>.
The bigger challenge will be deciding when you want to call this function. Typically this can be called at <span style="color:brown">**BeginPlay**</span> or when the player gets close to the actor. This will depend on some factors, the biggest being if any item inside the component will be visually present AND if they have a spawn chance. For example; a gun with a sight on it and the sight has a spawn chance. The sight's spawn chance might fail, and you might want the sight to get removed off the gun. Another example would be an NPC that is wearing some armor that has a spawn chance. If the armor fails its spawn chance, you'll probably want to modify some attributes and remove the armor.

This type of logic is very project dependant and depends on if the inventory component is eating up any impactful performance. I suggest leaving the <span style="color:brown">**StartComponent**</span> function on <span style="color:brown">**BeginPlay**</span>, and when it's time to optimize your games performance, then you start evaluating if calling <span style="color:brown">**StartComponent**</span> is more worthwhile being called at different times.

The order of the items inside every container is very important. If you hover over an items <span style="color:slateblue">**TileIndex**</span> you'll see your options. If an item is set to finding the first tile available or a random tile, then another item attempts to spawn on top of that item, it'll fail. There is a delegate which will fire whenever an item fails to spawn.

---
## Stopping the component
This function will remove all widgets and object references. Under no circumstances should this function be called while the player is actively interacting with the component.
Here's a list of scenarios where you want to refrain from calling <span style="color:brown">**StopComponent**</span>.
1. The player will be interacting with the component within a short period  of time, perhaps within 10 minutes.
2. If the player will be interacting with the component again, if the component has a lot of items or a lot of containers and it causes a noticable hitch when starting the component, it might be more efficient to keep the component active.
3. The component has items that have a spawn chance. Every time the component starts, it'll roll these spawn chances.
4. In a multiplayer scenario, you'll be eating a potentially large RPC call every time you start the component. If you stop the component and the player interacts with this component again, the <span style="color:brown">**StartComponent**</span> function will call a RPC that might be quite large.

The component can not be stopped while its <span style="color:slateblue">**Listeners**</span> array is not empty, which is used for multiplayer.

If your games memory usage is tight, it might be worthwhile trying to call <span style="color:brown">**StopComponent**</span>, but the above points must be kept in mind.

What might be worthwhile for you to optimize would be to try and clean up any widgets and object references the component is holding onto. That is most often the root of any performance issues after activating a component with a lot of items.
