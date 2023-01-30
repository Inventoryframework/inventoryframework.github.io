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

You are able to either change the default for children of <span style="color:violet">**DA_CoreItem**</span> to be something unique, or change it per-item. If you have massive item data bases, it is recommended to organize your data base using custom asset types.

Whatever you fill in inside of <span style="color:slateblue">**Asset Registry Category**</span>, you will need to follow the above steps, but replacing the Primary Asset Type with whatever you put into <span style="color:slateblue">**Asset Registry Category**</span>. You also must save the data asset manually after the change.

---
## Validating
To validate your items are being discovered correctly, I suggest going into the [InventoryHelper](http://inventoryframework.github.io/tools/productivity/#inventory-helper) and hitting Refresh near the bottom right. You can also search for your item.

If an item is not appearing there, either the above steps were not followed correctly or the asset manager is ignoring it because no data has been changed inside of it or it is identical to another item.

If an item is still not appearing there, go into "Windows" and open the output log and look for "Ignoring PrimaryAssetType Items - Conflicts with DA_CoreItem - Asset: ITEMNAMEHERE" in your message log.