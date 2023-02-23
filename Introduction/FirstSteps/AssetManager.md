---
order: 90
---

# Asset Manager

For the asset manager to pick up your item data base and for several tools to function correctly, you must set up the asset manager per project.
1. Go into Projects Settings.
2. Under the "Game" category, select "Asset Manager".
3. Add a new index and set "Primary Asset Type" as "Items", the asset base class to <span style="color:violet">**DA_CoreItem**</span>, then go into Directories and add all locations where you might store your items (This also searches sub folders).

![](/pictures/AssetManagerAddAssetType.png)

---
## Adding custom asset types
Inside your item data assets, you can see in the <span style="color:green">**Developer Settings**</span> there's a "<span style="color:slateblue">**Asset Registry Category**</span>" variable. The default is "Items", just like the asset type we just set up in the asset manager.

You are able to either change the default for children of <span style="color:violet">**DA_CoreItem**</span> to be something unique, or change it per-item. If you have massive item data bases (500+ items), it is recommended to organize your data base using custom asset types.

Whatever you fill in inside of <span style="color:slateblue">**Asset Registry Category**</span>, you will need to follow the above steps, but replacing the Primary Asset Type with whatever you put into <span style="color:slateblue">**Asset Registry Category**</span>. You also must save the data asset manually after the change.

---
## Validating
To validate your items are being discovered correctly, I suggest going into the [InventoryHelper](http://inventoryframework.github.io/tools/productivity/#inventory-helper) and hitting Refresh near the bottom right. You can also search for your item.

If an item is not appearing there, either the above steps were not followed correctly or the asset manager is ignoring it for some reason.
If you go into "Windows" and open the output log and find any "Ignoring PrimaryAssetType Items - Conflicts with SOMECLASSNAME - Asset: ITEMNAMEHERE" in your message log, it means no data has been changed inside of your data asset or it is identical to another data asset.

---
## Limitations
As of 5.1, there is still no way of mixing C++ and blueprint data assets, for now you can only have C++ data assets, and you can't do a lot of hierarchy setups. All of your parents must live at a C++ level.
Because the item data asset variable lives in C++, there is no way to reference blueprint data assets. If you check "Has Blueprint Classes" the system will not work.

--- 
## Bulk Editing
You'll sometimes find yourself with tons of items and need to modify a large amount of items. This is where a good folder hierarchy setup is essential. In the demo project, every item is in its own folder, but all of those folders are inside **DemoShowcase -> Items**, if you go into the Items folder, find the filter on the left of the search bar and go into **Miscellaneous -> Data Asset** you will see every item inside the demo. If you select multiple, then right click and go into **Asset Actions -> Bulk Edit Via Property Matrix**, you'll find the property matrix editor.

<a href="https://docs.unrealengine.com/4.27/en-US/Basics/UI/PropertyMatrix/" target="_blank">**Unreal's docs for the Property Matrix**</a>