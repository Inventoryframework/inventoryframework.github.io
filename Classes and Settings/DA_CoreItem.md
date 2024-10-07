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
## Creating new items
<a href="https://youtu.be/GzxE6spwgCQ" target="_blank">**Video documentation**</a> (slightly outdated, for  example the content browser and item actor name have been changed, but majority of it is still relevant. The only thing that should be ignored is around `06:23`, I state that you should import the containers from the data asset into the item actor. This is no longer needed, only the first index needs to be setup.)

Creating new items is very simple.
To start off, you'll need to go into the engine and find your content browser, right click and go to **Inventory Framework -> Inventory Item Asset** and a pop up will appear asking you what parent your data asset should derive from.
You might have your own children or have removed some of the children that come with the asset, but whatever asset you choose must derive from <span style="color:violet">**DA_CoreItem**</span> at some point in its hierarchy.

I suggest creating a folder for your item and placing the data asset in there.

To create a blueprint actor for your item, you will have to create an actor who at some point in its hierarchy is a child of <span style="color:violet">**A_ItemActor**</span> (The blueprint child is called <span style="color:violet">**BP_ItemActor**</span>), then set that as your <span style="color:Slateblue">**ItemActor**</span>

The actor will automatically get the blueprint inventory component assigned to it. You will need to go into the component settings and change a few things.
1. Set <span style="color:Slateblue">**InventoryType**</span> to "**Pickup**"
2. Add one index to <span style="color:Slateblue">**ContainerSettings**</span> and set the <span style="color:Slateblue">**ContainerType**</span> to "**ThisActor**". This will then hide most of the settings except for the items array.
3. Your items array should only have one index, which will be your newly created data asset. All settings can be left to their default.
4. (Optional) If your item has containers associated with it, the data asset should have an array of <span style="color:Slateblue">**DefaultContainers**</span>. You will want to populate these with your containers and also assign the widget. Only available to supported data assets, such as <span style="color:violet">**IDA_Equippable**</span> and <span style="color:violet">**IDA_Weapon**</span>, reference them to create your own with container support. An example can be found in the example project with the gun and backpack item.

To have the asset manager pick up your data asset, it can not be identical to another data asset. (This includes the base file, so if all your variables are set to default, the asset will not get picked up.) Typically you want to immediately give your item a name and a developer image for the InventoryHelper.

---
### Creating new C++ children of DA_CoreItem

You might want to create your own category of items, and since the engine does not allow you to mix C++ and blueprint children of data assets, you are unfortunately forced to create C++ children.

I suggest installing the plugin directly to your project, and then creating new children inside the **Items** folder, which is found inside the plugins **Core** folder.

If you are creating a new category, you will also want to find the EItemType enum inside <span style="color:violet">**IFP_CoreData.h**</span> and create a new entry. Then inside your newly created .cpp file, override the constructor and set the type to be equal to this new category.
This is so functions inside of <span style="color:violet">**DA_CoreItem**</span> can perform the correct casts to retrieve data from its children.
You can find examples of this inside of the children of <span style="color:violet">**DA_CoreItem**</span> that come with the plugin.

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
The way you define your shape is by adding the Custom Shape trait to your <span style="color:slateblue">**TraitsAndComponents**</span> array and using the <span style="color:slateblue">**DisabledTiles**</span> array. Though this is not very pleasant to work with in its raw array format. It is recommended to use the [ItemEditor](https://inventoryframework.github.io/tools/itemeditor/), where you'll find a toolbox with a few tools inside of it, one being the shape editor. Which gives you a much better UI to work with.

---
## Asset verification

To aid quality assurance, the data asset will run custom data verification when the asset is saved.
The verification validates everything in the following process:
1. Use the <span style="color:slateblue">**ValidationClass**</span> in the <span style="color:slateblue">**DeveloperSettngs**</span> category to execute its <span style="color:brown">**VerifyData**</span> function. (Because we can't access a blueprint graph in each instance of a data asset, we do this to get around that limitation.)
2. <span style="color:slateblue">**ValidationClass**</span> -> <span style="color:brown">**VerifyData**</span> will run some code to check if any mistakes have been made. Since every project is different, it is suggested to create your own child of <span style="color:violet">**O_ItemAssetValidation**</span> and write your own code. To validate the data of trait references, an interface function from the <span style="color:violet">**I_Validation.h**</span> interface also named <span style="color:brown">**VerifyData**</span> is called. You can see an example of this inside of <span style="color:violet">**O_BasicItemValidation**</span> and <span style="color:violet">**IT_Consumable**</span>
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

- The only downside of GAS that I've found is that it is extremely difficult to have abilities be "instanced per item". As in, a new ability instance is created for each item instance without overriding a lot of C++ code and having a really good understanding of how GAS works under the hood. This type of logic can be achieved with the `ItemAbility` item component.

### Traits and Components

https://inventoryframework.github.io/classes-and-settings/o_itemobjectandac_itemdriver/
Data assets have "traits" and "components", which I recommend you read the documentation for, but in essence, they are:
- Traits: Extra data you can add to any specific asset without creating a new parent. Essentially giving you "horizontal" hierarchy, just like how interfaces add functions to a class without reparenting it.
    - Example: You have a sword and a health potion asset. Both of these can be crafted, but you don't want to create a "Craftable" item asset to insert into their hierarchy. Not all swords and not all potions might be craftable. You can now add an trait to any asset that has a recipe and have that trait link to that recipe data asset. You can now ask your data asset if it is "craftable" without asking your entire recipe data base if it has a recipe for that item.
- Components: Actor components that can be created during runtime and are attached to the same actor that the inventory component lives on. These can run gameplay logic and destroyed during runtime, just like any other actor component. These are tied to traits so they can also be thought of as "horizontal" hierarchy.

### Item actor

Then there's the optional actor to spawn when the item is dropped, or in some cases equipped. These can be destroyed and created frequently, so it shouldn't really be trusted with a lot of gameplay logic. This should be treated as just the "visual representation" of your item.

### Item Instance

`ItemInstance`'s are optional objects created when an item struct is created, and they're tied together. Unlike actors, `ItemInstance`'s can be trusted to handle gameplay logic.

- **Assigning an instance with an item**: There are two ways to assign an `ItemInstance`'s with an `ItemAsset`:
  1. **Data Asset Default**: The default instance is set in the data asset. This is the recommended way.
  2. **Override per Struct**: Individual struct instances can override variables for more customization. 
    - **Recommendation**: I highly recommend using the first method as that is soft-referenced.
  
- **Replication**: Fully supported and can be optimized with custom replication conditions.
- **Restrictions**: By default, assigning an `ItemInstance` in the item struct has 2 rules:
     1. "Wild" `ItemInstance`'s are not allowed. This refers to items which have no default `ItemInstance` assigned in the item asset, but you still want to add one to the item struct. It is highly advised to keep this rule enabled to prevent random items having random item instances as keeping track of this is extremely difficult in medium to large projects.

	 2. You are not allowed to select `ItemInstance`'s that do not match the same class that was assigned in the item asset. This is to prevent two identical items having two different ItemInstance classes, leading to odd behavior.*/

     - To disable either or both of these restrictions, modify <span style="color:violet">**FL_InventoryFramework**</span> -> <span style="color:brown">**ProcessContainerAndItemCustomizations**</span>
- **Serialization (Experimental)**: Item Instances can be serialized, saving only variables marked with the `SaveGame` flag. This feature is experimental and may change in future updates, so it's not recommended for production. I recommend going with a much more robust serialization system that covers much more than just the inventory.

---

#### Creation & Destruction

- **Creation**: Item Instances are created during `StartComponent` and remain tied to the item until the item is destroyed, unless you enable <span style="color:slateblue">**ConstructOnRequest**</span>. Then it will only be constructed when you call <span style="color:brown">**GetItemsInstance**</span>.
- **Destruction**: They stay alive until garbage collection activates, just like widgets. Use `IsDestroyed` to check their status.

---

#### Networking

- `ItemInstances`'s replicate to all clients relevant to the inventory owner. However, the item struct data itself isn't replicated and shouldn't be trusted on non-owner clients due to networking optimizations `IFP` enforces.
- `ItemInstances`'s can declare their own replication conditions, allowing for optimized network performance (e.g., replicating only to the owner or server-only).

- **Important**: If you move the inventory component to the player state for seamless travel, `ItemInstance`'s will replicate to every client. To avoid this, use replication conditions or consider not using `ItemInstance`'s, depending on your game's needs.


#### Performance
- Constructing these `ItemInstance`'s is cheap, but if you plan on using hundreds of these, the small cost can pile up. I highly recommend leaving the <span style="color:slateblue">**ConstructOnRequest**</span> option set to true. The main worry you might need to consider is the garbage collector. The developers of Hogwarts Legacy had this problem where they abused instanced objects that the garbage collector would cause hitches, though I am not sure as to how many objects they were making.
I suggest only making these instances for items that actually need it.

- If you have a pooling system, then I will continue recommending having <span style="color:slateblue">**ConstructOnRequest**</span> enabled, then overriding the <span style="color:brown">**RemoveObject**</span> function to no longer mark the object as garbage and move the `ItemInstance` into your pooling system, then modifying <span style="color:brown">**GetItemsInstance**</span> to access your pooling system.