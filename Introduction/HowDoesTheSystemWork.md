---
tags: [inventorycomponent]
---

# How does the system work?

---
## Design structure
With how strict the performance requirements were for this plugin, I’ve had to design this inventory system slightly differently from other systems you might find either on the marketplace or Youtube tutorials.
The biggest thing to keep in mind is that this system does NOT rely on widgets being valid or created to keep track of its data.

This is to achieve five things:
1. This greatly speeds up widget construction since all the data they need (for example where an item is inside the container and so forth) is already prepped.
2. Allows you to remove widgets from RAM and still have the necessary data to find out important things like looting an item and finding a free space for that item.
3. Since widgets have way fewer references, there is less work to be done to manage widgets getting garbage collected properly.
4. Designers are able to create new widgets and prototype much faster since all the data they need about containers and items is always available from the component.
5. You have complete control over when you want to construct these widgets, meaning you can reduce lag spikes when starting your game or even speed up the time it takes for your players to load a level or area.

<span style="color:slateblue">**ContainerSettings**</span> is an array inside of <span style="color:violet">**AC_Inventory.h**</span> and it contains all the settings your designers will use and some settings only the programmers will use, and with all the settings combined you can figure out everything about that container.

When creating a widget for a container, the widget uses a <span style="color:slateblue">**UniqueID**</span> to find out what container it is representing.

Items differentiate themselves from each other by using a <span style="color:slateblue">**UniqueID**</span> as items can have the same name, same data asset and so forth. A component can not give items or containers the same <span style="color:slateblue">**UniqueID**</span> twice. This is essential for functions to work properly.

The component is designed to live on individual actors, not the player controller. Though there is no reason why it can't live on the controller, but I won't be actively testing it that way.

---
## How are the infinite items inside of items achieved?

TLDR; Containers have coordinates explaining what item they belong to. The array is simply containers and those items have an array of items. The only thing the compoonent needs to know whether a container is owned by the component or owned by an item is by checking <span style="color:slateblue">**FS_Container**</span>**.**<span style="color:slateblue">**BelongsToItem**</span>.

I recommend reading the code comment for the container struct and then looking at <span style="color:violet">**AC_Inventory.h**</span> -> <span style="color:brown">**GetItemsContainers**</span> for an example.