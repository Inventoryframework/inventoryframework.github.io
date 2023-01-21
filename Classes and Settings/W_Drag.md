# W_Drag

==- Widget Drag
File Location: Source\InventoryFrameworkPlugin\Public\Core\Widgets\W_Drag.h
File Location: Source\InventoryFrameworkPlugin\Private\Core\Widgets\W_Drag.cpp
==- Blueprint Child: WBP_Drag
File Location: Content\Core\Widgets\DragAndSplit\WBP_Drag.uasset
==-

---

Visual representation of the item you are dragging, but also provides updates to <span style="color:violet">**W_Highlight**</span> whenever you hover over a tile so the highlight widget knows where to move.

All data you need when dragging and dropping an item should be stored in this widget.

Both <span style="color:violet">**W_Drag**</span> and <span style="color:violet">**W_Highlight**</span> are updated inside of <span style="color:violet">**WBP_Tile**</span> ->  <span style="color:brown">**PerformDrop**</span> and <span style="color:violet">**BP_AC_Inventory**</span> -> <span style="color:brown">**UpdateHighlight**</span>