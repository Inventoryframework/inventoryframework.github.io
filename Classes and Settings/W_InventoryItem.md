# W_InventoryItem
==- Widget Inventory Item
File Location: Source\InventoryFrameworkPlugin\Public\Core\Widgets\W_InventoryItem.h
File Location: Source\InventoryFrameworkPlugin\Private\Core\Widgets\W_InventoryItem.cpp
==- Blueprint Child: WBP_InventoryItem
File Location: Content\Core\Widgets
==- Demo widget: WBP_DemoInventoryItem
File Location: Content\Demo\Widgets\WBP_DemoInventoryItem.uasset
==-

---

These are the widget representations of your item.

The <span style="color:brown">**GetItemData**</span> function automatically gets the direct struct version of the item it is representing from the component the item belongs to. This is so we don’t store the same item information in two places and whenever the item is updated you don’t need to update the widget as well.

This should allow for more designer friendly architecture, as you could make two children of this widget, one for example the normal inventory item widget the player has in their inventory and then another for vendors and you could have them store items in a list format and things would still work together.

<span style="color:violet">**WBP_InventoryItem**</span> -> <span style="color:brown">**GenerateItemIcon**</span> is where icon generation is handled. It’s pretty straight forward, copying the behavior to other children should be simple. It uses the [**BP_PreviewActor**](https://inventoryframework.github.io/classes-and-settings/bp_previewactor/) to achieve this.
The data asset editor window has a <span style="color:green">**UI**</span> category which has various settings to adjust or optimize the generated icons.