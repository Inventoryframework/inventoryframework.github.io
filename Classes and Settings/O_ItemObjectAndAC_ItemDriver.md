---
tags: [item]
---

# O_ItemObject and AC_ItemDriver
==- Object Item
File Location: Source\InventoryFrameworkPlugin\Public\Core\Objects\Parents\O_ItemObject.h
File Location: Source\InventoryFrameworkPlugin\Private\Core\Objects\Parents\O_ItemObject.cpp
==- Blueprint Child: BP_O_ItemObject
File Location: Content\Core\ActorParents\BP_O_ItemObject.uasset
==-

==- Actor Component Item Driver
File Location: Source\InventoryFrameworkPlugin\Public\Core\Components\AC_ItemDriver.h
File Location: Source\InventoryFrameworkPlugin\Private\Core\Components\AC_ItemDriver.cpp
==- Blueprint Child: BP_AC_ItemDriver
File Location: Content\Core\ActorParents\BP_AC_ItemDriver.uasset
==-
---

## Objects
These are pure <span style="color:violet">**UObjects**</span> which are accessible from the items data asset. Each object's variables can be modified per data asset, allowing you to add any variables or functions to any data asset, regardless of their parent data asset.

You can then retrieve this data by using the <span style="color:brown">**GetObjectsByTag**</span>/<span style="color:brown">**GetObjectsByClass**</span>. The class version will automatically cast to the correct blueprint. For the tag version, you manually have to cast to the object.

There are two types of objects, pure objects, which are data-only, then there are objects designed to go alongside a driver.

---
## Data-only objects
While you can add those variables to the data asset itself, some people do not want to go into C++ OR there is some data that can't be categorised to an item category. For example; quest data. Any item can be a part of a quest, but you might not want create a data asset parent dedicated to a quest category.

These objects can also run functions, but share the same limitations as data assets functions. These functions can not have delays, timers, async tasks or RPC’s. Do not write any data to any variables belonging to the Object as every item with the same data asset shares the same Object. A unique Object is NOT created per item.

!!!Important
- Whenever possible, use soft references and try to avoid hard references. Item data bases are one of the fastest ways of creating a nasty web of hard references and before you know it, the majority of your game is loaded at times where you don't want it to be. These objects are potentially the biggest culprit in this system to create that kind of nasty web of hard references.
!!!

---
## Drivers
These are actor components that are meant to drive any gameplay logic related to an item, such as equipping an item, consuming an item, creating a widget and so forth.
These can be assigned to an item through any object that is a child of <span style="color:violet">**O_ItemObject.h**</span>. Every driver needs an object to work, hence why there's a documentation page for each individual class, except for this class. Drivers are tied to objects. But every object does not need a driver.

These components can also be viewed as a proxy between the inventory system and any other system. These <span style="color:violet">**ItemObject**</span> and <span style="color:violet">**ItemDrivers**</span> have been designed to be extremely abstract, so you can modify them however you want without it affecting how the inventory works. There's very few aspects of the inventory that depend on this system.

They work just like any other actor component, they can have delays, timers, RPC's and so forth. They also have methods of following the item it is bound to where ever it goes and retrieving the item data.

<span style="color:violet">**ItemDrivers**</span> have three replication methods; <span style="color:slateblue">**Server**</span>, <span style="color:slateblue">**Client**</span> and <span style="color:slateblue">**Both**</span>. Server <span style="color:violet">**ItemDriver**</span>'s only exists on the server, client <span style="color:violet">**ItemDriver**</span>'s only exists on the client, and <span style="color:slateblue">**Both**</span> are created on the server and replicated to the client. You’ll most likely be using <span style="color:slateblue">**Both**</span> most of the time. 
Clients are able to request the creation of an <span style="color:violet">**ItemDriver**</span> and pass in a tag, which you can associate with a function. This prevents those pesky situations where you are working on client code, for example a widget, and are trying to activate a function inside an item driver. 
The function <span style="color:brown">**GetItemDriver**</span> will create the driver for you, but if you are on the client and trying to create an <span style="color:violet">**ItemDriver**</span> set to <span style="color:slateblue">**Both**</span>, the function won’t be able to return it, because it has to send an RPC to the server which is not instant. 

This function allows you to pass in a <span style="color:slateblue">**tag**</span> which will trigger any function you have associated with the tag once the <span style="color:violet">**ItemDriver**</span> is replicated. Look at the code comment for <span style="color:violet">**AC_ItemDriver.h**</span> -> <span style="color:brown">**ActivateEvent**</span>.

### Notes:

- Item drivers should be destroyed after their logic is complete, especially if it’s from a stackable item. The reason being that it’s really hard or almost impossible to keep track of the stack it came from as that stack can be put into other stacks, split or fully used.

- You must always keep in mind if the item driver is running logic that’s on a timer,  you need to be aware that the item the driver belongs to might become invalid at any point. There is always the chance the item that owns the driver might get destroyed during the timer logic. I also recommend “locking” the player from interacting with the item during this timer or having some sort of failsafe if the player manipulates the item in some way while the timer is still running (For example; a gun playing its pickup animation and the player drops it mid-animation)

- The properties inside of `FDriverPayload` struct are completely unused by the plugin and is meant to be modified for each projects' needs.

---
## Tips and tricks

![](/pictures/BadObjectClassNames.png)
By default, your objects will display their default class name in the drop down menu.


![](/pictures/GoodObjectClassNames.png)
You can change the name they are given inside this drop down menu by following the next steps.

-![](/pictures/ClassNameExample.png)
By going into your Class Settings (1) you can change the class name (2). The drop down menu will now display whatever text you put in here.