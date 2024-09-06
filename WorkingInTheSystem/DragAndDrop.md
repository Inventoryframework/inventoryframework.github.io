# Drag and drop

---
A lot of the information <span style="color:brown">**PerformDrop**</span> function needs to do its calculations and logic should be stored in <span style="color:violet">**W_Drag**</span>.
This is because setting and resetting all that information for all the tiles to optimize memory is simply just so much hassle compared to just putting your stuff in the drag widget, which is always just 1 of so it’s always optimized for memory and no hassle in managing it.

---

`IFP` takes a similar approach to the default drag and drop operation, but slightly modified to get around 2 hard coded practices in the engine:
1. Drag and drop visual has interpolation that can not be overriden.
2. Drag and Drop operations will only work while holding the drag button, then attempt to drop it the moment that key is released. Where as in some cases, like with gamepad navigation, you want to fully press and release the drag button, move the cursor to its destination, then press and release the button again to perform the drop operation.

> It is not possible to completely abandon the engines drag and drop system, because the editor tools get a custom viewport that does not play well with the drag widget that comes with the plugin, so editor tools will not support the custom `IFP` solution.

Inside the inventory components class defaults, there is a setting to toggle point 2 from the default behavior and to `IFP`'s custom implementation. If you want to change this behavior during runtime, you'd want to call <span style="color:brown">**GetLocalInventoryComponent**</span> -> <span style="color:slateblue">**UseDefaultDragBehavior**</span> 

To achieve this, item widgets have a <span style="color:brown">**StartItemDrag**</span> and <span style="color:brown">**StopItemDrag**</span> functions, which will handle either the custom implementation or default. Then a custom set of delegates will be called over the default OnDrop and OnDropCancelled.

The widget that is now responsible for handling navigation and input handling is the container widget.