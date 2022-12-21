---
tags: [Item]
---

# DA_CoreItem
==- Data Asset Core Item
File location: Source\InventoryFrameworkPlugin\Private\Core\Items\DA_CoreItem.h
File Location: Source\InventoryFrameworkPlugin\Public\Core\Items\DA_CoreItem.cpp
==-

---

This is the data asset that is responsible for the base information for your items. I highly suggest looking at the <span style="color:violet">DA_CoreItem.h</span> file to see all the settings, functions and comments.

You can show and hide certain settings depending on your setup, and I use that a lot with the <span style="color:slateblue">ItemType</span> enum. Look at all the options, check out the meta properties (EditCondition and EditConditionHides) to see how it’s done, and you can then implement your own options and rulesets for their visibility.

Keep in mind, these settings are only HIDDEN, not disabled in the data asset editor, you can still access those variables from the data asset in blueprints and C++ and you have to remember that when making your functions and gameplay logic.
One of the most important settings to set up here is <span style="color:slateblue">EItemType</span> as many functions use this enum to do casts to the proper children of this asset. Whenever you make a new child, remember to go into the .cpp file and set the default value. You can see an example inside all the .cpp children of this.

Currently your item actors must be a child of A_ItemPhysicalPresentation. If you want to change this, you can change it like so:
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
+++