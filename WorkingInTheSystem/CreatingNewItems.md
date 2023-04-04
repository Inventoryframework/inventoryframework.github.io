# Creating new items

Creating new items is very simple.
To start off, you'll need to go into the engine and find your content browser, right click and go to **Miscellaneous -> Data Asset** and a pop up will appear asking you what parent your data asset should derive from.
You might have your own children or have removed some of the children that come with the asset, but whatever asset you choose must derive from <span style="color:violet">**DA_CoreItem**</span> at some point in its hierarchy.

I suggest creating a folder for your item and placing the data asset in there.

To create a blueprint actor for your item, you will have to create an actor who at some point in its hierarchy is a child of <span style="color:violet">**BP_SM_ItemPhysical**</span>, then set that as your <span style="color:Slateblue">**PhysicalActor**</span>

The actor will automatically get the blueprint inventory component assigned to it. You will need to go into the component settings and change a few things.
1. Set <span style="color:Slateblue">**InventoryType**</span> to "**Pickup**"
2. Add one index to <span style="color:Slateblue">**ContainerSettings**</span> and set the <span style="color:Slateblue">**ContainerType**</span> to "**ThisActor**". This will then hide most of the settings except for the items array.
3. Your items array should only have one index, which will be your newly created data asset. All settings can be left to their default.
4. (Optional) If your item has containers associated with it, the data asset should have a <span style="color:Slateblue">**AttachmentSettings**</span> with an array of <span style="color:Slateblue">**DefaultContainers**</span>. You will want to import these default containers into the blueprint actors <span style="color:Slateblue">**ContainerSettings**</span>, while leaving the first <span style="color:Slateblue">**ContainerSettings**</span> array index (0) and the first item array index (0) as the item this actor is representing. An example can be found in the example project with the gun and backpack item.

To have the asset manager pick up your data asset, it can not be identical to another data asset. (This includes the base file, so if all your variables are set to default, the asset will not get picked up.) Typically you want to immediately give your item a name and a developer image for the InventoryHelper.

---
## Creating new C++ children of DA_CoreItem

You might want to create your own category of items, and since the engine does not allow you to mix C++ and blueprint children of data assets, you are unfortunately forced to create C++ children.

I suggest installing the plugin directly to your project, and then creating new children inside the **Items** folder, which is found inside the plugins **Core** folder.

If you are creating a new category, you will also want to find the EItemType enum inside <span style="color:violet">**IFP_CoreData.h**</span> and create a new entry. Then inside your newly created .cpp file, override the constructor and set the type to be equal to this new category.
This is so functions inside of <span style="color:violet">**DA_CoreItem**</span> can perform the correct casts to retrieve data from its children.
You can find examples of this inside of the children of <span style="color:violet">**DA_CoreItem**</span> that come with the plugin.