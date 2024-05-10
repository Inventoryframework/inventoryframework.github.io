---
tags: [Item]
---

# DA_CoreItem
==- Data Asset Core Item
File location: Source\InventoryFrameworkPlugin\Public\Core\Items\DA_CoreItem.h
File Location: Source\InventoryFrameworkPlugin\Private\Core\Items\DA_CoreItem.cpp
==-

---

This is the data asset that is responsible for the base information for your items. I highly suggest looking at the <span style="color:violet">**DA_CoreItem.h**</span> file to see all the settings, functions and comments.

You can show and hide certain settings depending on your setup, and I use that a lot with the <span style="color:slateblue">**ItemType**</span> enum. Look at all the options, check out the meta properties (EditCondition and EditConditionHides) to see how it’s done, and you can then implement your own options and rulesets for their visibility.

Keep in mind, these settings are only HIDDEN, not disabled in the data asset editor, you can still access those variables from the data asset in blueprints and C++ and you have to remember that when making your functions and gameplay logic.
One of the most important settings to set up here is <span style="color:slateblue">**EItemType**</span> as many functions use this enum to do casts to the proper children of this asset. Whenever you make a new child, remember to go into the .cpp file and set the default value. You can see an example inside all the .cpp children of this.

If you are new to C++ or Unreal Engine, I suggest looking <a href="https://benui.ca/unreal/uproperty/" target="_blank">**here**</a> to find all the specifiers you can give each variable.

---
## Changing the Item Actor class
Currently your item actors must be a child of <span style="color:violet">**AA_ItemActor**</span>. If you want to change this, you can change it like so:
+++ From
```
UPROPERTY(EditAnywhere, BlueprintReadOnly, Category="CoreSettings")
TSoftClassPtr<AA_ItemActor> ItemActor = nullptr;
```
+++ To
```
UPROPERTY(EditAnywhere, BlueprintReadOnly, Category="CoreSettings")
TSoftClassPtr<AActor> ItemActor = nullptr;
```
AActor can be swapped out for any class, it does not need to be AActor, but it must be a child of AActor.
+++

---
## Custom shapes

This inventory system features container styles where items can be different lengths and different heights.
The way you define your shape is by adding the Custom Shape object to your <span style="color:slateblue">**ObjectsAndDrivers**</span> array and using the <span style="color:slateblue">**DisabledTiles**</span> array. Though this is not very pleasant to work with in its raw array format. It is recommended to use the [ItemEditor](https://inventoryframework.github.io/tools/itemeditor/), where you'll find a toolbox with a few tools inside of it, one being the shape editor. Which gives you a much better UI to work with.

---
## Asset verification

To aid quality assurance, the data asset will run custom data verification when the asset is saved.
The verification validates everything in the following process:
1. Use the <span style="color:slateblue">**ValidationClass**</span> in the <span style="color:slateblue">**DeveloperSettngs**</span> category to execute its <span style="color:brown">**VerifyData**</span> function. (Because we can't access a blueprint graph in each instance of a data asset, we do this to get around that limitation.)
2. <span style="color:slateblue">**ValidationClass**</span> -> <span style="color:brown">**VerifyData**</span> will run some code to check if any mistakes have been made. Since every project is different, it is suggested to create your own child of <span style="color:violet">**O_ItemAssetValidation**</span> and write your own code. To validate the data of object references, an interface function from the <span style="color:violet">**I_Validation.h**</span> interface also named <span style="color:brown">**VerifyData**</span> is called. You can see an example of this inside of <span style="color:violet">**O_BasicItemValidation**</span> and <span style="color:violet">**IO_Consumable**</span>
3. If any error messages are created during any of the <span style="color:brown">**VerifyData**</span> functions, you'll get a message at the bottom right stating the error message. If the <span style="color:slateblue">**ErrorMessages**</span> array is empty, the item passes the verification.

Do note, if an item does not pass the verification, it does NOT prevent you from playing the game.

## Where to store runtime logic

As the plugin has grown, more and more features have been added, blurring the definition of what an "item" is and where any gameplay logic should be stored.

All items are data assets. Then when the inventory is made, a struct is made, which contains all data that can be modified during runtime and this data is replicated under specific conditions.
- Data inside of data assets should NOT be modified during runtime. If you have 2 guns in your inventory, then change any data inside the asset, it'll change for both of the guns. But modifying the struct for a gun will only modify that specific struct.

The four primary options below leave you with many options to tackle where to store your data and logic. It's up to you to resolve which method is best for what you are trying to accomplish.

### GAS abilities
What I, and many others have been doing, is simply constructing one object, a GAS ability, and having that run the logic for, let's say, a gun item. Whenever you swap weapons, you'd just send an event to the ability and it would update what weapon is equipped.
This has many performance benefits, quality of life benefits, many more features, and much better networking options.
Overall, I would recommend using GAS over all other options whenever you can, but of course, not all use cases are the same. There is no ultimate solution that covers every scenario.

I personally recommend using <a href="https://www.unrealengine.com/marketplace/en-US/product/gas-companion" target="_blank">**GAS Companion**</a> and/or <a href="https://www.unrealengine.com/marketplace/en-US/product/gameplay-blueprint-attributes" target="_blank">**Gameplay Blueprint Attributes**</a> to help with your GAS implementation.

### Objects and drivers

https://inventoryframework.github.io/classes-and-settings/o_itemobjectandac_itemdriver/
Data assets have "objects" and "drivers", which I recommend you read the documentation for, but in essence, they are:
- Objects: Extra data you can add to any specific asset without creating a new parent. Essentially giving you "horizontal" hierarchy, just like how interfaces add functions to a class without reparenting it.
    - Example: You have a sword and a health potion asset. Both of these can be crafted, but you don't want to create a "Craftable" item asset to insert into their hierarchy. Not all swords and not all potions might be craftable. You can now add an object to any asset that has a recipe and have that object link to that recipe data asset. You can now ask your data asset if it is "craftable" without asking your entire recipe data base if it has a recipe for that item.
- Drivers: Actor components that can be created during runtime and are attached to the same actor that the inventory component lives on. These can run gameplay logic and destroyed during runtime, just like any other actor component. These are tied to objects so they can also be thought of as "horizontal" hierarchy.

### Item actor

Then there's the optional actor to spawn when the item is dropped, or in some cases equipped. These can be destroyed and created frequently, so it shouldn't really be trusted with a lot of gameplay logic. This should be treated as just the "visual representation" of your item.

### Item Struct Object

Finally, there's the "struct object". This is simply an optional object that is created when the item struct is created and are then tied together. Unlike the actor, this can be trusted with gameplay logic. It is only destroyed when either you explicitly destroy it or when the item itself is destroyed.
- Do note that this class is hard referenced by the item asset. Be mindful of your hard references.
- These are instance editable. Meaning that the same class can be used for each item asset, but you can modify the variables in the item asset. When the struct object is created, it'll inherit those settings automatically.
- There's two ways to initialize these objects. One is inside the data asset. This is the default object that will be used. But each struct instance can override either what class to use, or change the variables to more suit that specific instance.
    - I highly recommend that you do not start mixing the classes used these structs for a specific item. For exameple; two apples using a different object class can lead to a lot of confusion and messy code. I'm not going to put any restrictions on this, some projects might need this, but it's a consideration to keep in mind.
- Full replication support.
- Experimental: These objects can be serialized. They will only restore any variables using the `SaveGame` flag (in blueprints, it can be found in the `Advanced` section in the details panel when you have a variable selected). Since this is experimental, it is not recommended to use for production. The serialization function might change in a future update, leading to old save files not being compatible with the new function. It will also only save the players struct objects, not other actors struct objects, and that will likely stay like that. There are hundreds of more things to consider with serialization than just the inventory and there are better solutions on the marketplace from people who properly understand serialization.

#### When are they created and destroyed?
Struct objects are created during <span style="color:brown">**StartComponent**</span> and are tied to the item until the item is destroyed.
- The object will remain alive until the garbage collector kicks in, just like how widgets and many other objects work. You can use `IsDestroyed` to check if the object has been destroyed or not.

#### How does the networking work?
Objects are replicated to all clients that are relevant to the owner of the inventory component. But the item data is not replicated and even if it was, it should NOT be trusted on clients that are neither the owner of the inventory component or the server. This is because of how `IFP` optimizes networking, which can be read further about in the `Introduction` section.
- Experimental: These objects can declare their own replication condition, giving you a chance for massive networking improvements. This means you can have objects that only replicate to the owner, server only, or you can even disable replication.

#### Performance
Constructing these objects is cheap, but if you plan on using hundreds of these, the small cost can pile up. The main worry you might need to consider is the garbage collector. The developers of Hogwarts Legacy had this problem where they abused instanced objects that the garbage collector would cause hitches, though I am not sure as to how many objects they were making.
I suggest only making these objects for items that actually need it.