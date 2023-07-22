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

The chain of events begin at <span style="color:violet">**WBP_InventoryItem**</span> ->  <span style="color:brown">**OnDragDetected**</span>. An important note to consider is that Epic, in their ever-ending glorious top tier coding, have a built in interpolation for the drag and drop drag visual that you CAN NOT interact with without modifying the engine.
But you have to go through a lot of trouble to replace the built-in drag and drop system for Unreal. So I leave the drag visual for the drag/drop operation as empty and allow <span style="color:violet">**WBP_Drag**</span> to drive the visuals and most of the logic. The Drag/Drop operation is primarily just used to trigger the "On Drop" events.