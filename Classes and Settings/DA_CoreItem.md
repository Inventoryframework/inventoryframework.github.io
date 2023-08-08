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
## Changing the Physical Actor class
Currently your item actors must be a child of <span style="color:violet">**A_ItemPhysicalPresentation**</span>. If you want to change this, you can change it like so:
+++ From
```
UPROPERTY(EditAnywhere, BlueprintReadOnly, Category="CoreSettings")
TSoftClassPtr<AA_ItemPhysicalRepresentation> PhysicalActor = nullptr;
```
+++ To
```
UPROPERTY(EditAnywhere, BlueprintReadOnly, Category="CoreSettings")
TSoftClassPtr<AActor> PhysicalActor = nullptr;
```
AActor can be swapped out for any class, it does not need to be AActor, but it must be a child of AActor.
+++

---
## Custom shapes

This inventory system features container styles where items can be different lengths and different heights.
The way you define your shape is by adding the Custom Shape object to your <span style="color:slateblue">**ObjectsAndDrivers**</span> array and using the <span style="color:slateblue">**DisabledTiles**</span> array. Though this is not very pleasant to work with in its raw array format. It is recommended to use the [ItemEditor](https://inventoryframework.github.io/tools/itemeditor/), where you'll find a toolbox with a few tools inside of it, one being the shape editor. Which gives you a much better UI to work with.

---
## Asset verification

To aid quality assurance, the data asset has a "Verify Data" button at the top of the asset.
The verification validates everything in the following process:
1. Use the <span style="color:slateblue">**ValidationClass**</span> in the <span style="color:slateblue">**DeveloperSettngs**</span> category to execute its <span style="color:brown">**VerifyData**</span> function. (Because we can't use Blueprint data assets, this step is used to get around that limitation.)
2. <span style="color:slateblue">**ValidationClass**</span> -> <span style="color:brown">**VerifyData**</span> will run some code to check if any mistakes have been made. Since every project is different, it is suggested to create your own child of <span style="color:violet">**O_ItemAssetValidation**</span> and write your own code. To validate the data of object references, an interface function from the <span style="color:violet">**I_Validation.h**</span> interface also named <span style="color:brown">**VerifyData**</span> is called. You can see an example of this inside of <span style="color:violet">**O_BasicItemValidation**</span> and <span style="color:violet">**IO_Consumable**</span>
3. If any error messages are created during any of the <span style="color:brown">**VerifyData**</span> functions, you'll get a message at the bottom right stating the error message. If the <span style="color:slateblue">**ErrorMessages**</span> array is empty, the item passes the verification.

Do note, if an item does not pass the verification, it does NOT prevent you from playing the game.