# W_Tile
==- Widget Tile
File Location: Source\InventoryFrameworkPlugin\Public\Core\Widgets\W_Tile.h
File Location: Source\InventoryFrameworkPlugin\Private\Core\Widgets\W_Tile.cpp
==- Bluerint Child: WBP_Tile
File Location: Content\Core\Widgets\WBP_Tile.uasset
==- 

---

There is very little logic or variables handled in this widget, as you have to keep in mind that this widget can end up with hundred copies or more, so it can get bad for memory allocation if overdone and if you don’t manage the variables those tiles are initializing.

Creating these tiles is by far the most expensive process I’ve found in this component. It is essential they store as little data and perform as few functions as possible so we can optimize the construction cost.