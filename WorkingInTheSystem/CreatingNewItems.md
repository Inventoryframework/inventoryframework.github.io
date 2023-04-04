# Creating new items

Creating new items is very simple.
To start off, you'll need to go into the engine and find your content browser, right click and go to **Miscellaneous -> Data Asset** and a pop up will appear asking you what parent your data asset should derive from.
You might have your own children or have removed some of the children that come with the asset, but whatever asset you choose must derive from <span style="color:violet">**DA_CoreItem**</span> at some point in its hierarchy.

I suggest creating a folder for your item and placing the data asset in there.

To create a blueprint actor for your item, you will have to create an actor who at some point in its hierarchy is a child of <span style="color:violet">**BP_SM_ItemPhysical**</span>.

To have the asset manager pick up your data asset, it can not be identical to another data asset. (This includes the base file, so if all your variables are set to default, the asset will not get picked up.) Typically you want to immediately give your item a name and a developer image for the InventoryHelper.

---
## Creating new C++ children of DA_CoreItem

You might want to create your own category of items, and since the engine does not allow you to mix C++ and blueprint children of data assets, you are unfortunately forced to create C++ children.

I suggest installing the plugin directly to your project, and then creating new children inside the **Items** folder, which is found inside the plugins **Core** folder.

If you are creating a new category, you will also want to find the EItemType enum inside <span style="color:violet">**IFP_CoreData.h**</span> and create a new entry. Then inside your newly created .cpp file, override the constructor and set the type to be equal to this new category.
This is so functions inside of <span style="color:violet">**DA_CoreItem**</span> can perform the correct casts to retrieve data from its children.
You can find examples of this inside of the children of <span style="color:violet">**DA_CoreItem**</span> that come with the plugin.