# Sorting items

There are a few ways to sort items, the primary way people will want to sort items is to also move items. Sorting an items array by itself is very simple, the item library editor tool shows an example of how to sort items alphabetically, but things become slighlty more complicated when you also want to move items inside of a container.

# Adding sorting options
---
I've designed the function SortAndMoveItem to only require one change for any custom sorting you want to perform.
1. In Async_InventoryFunctions.h there is an enum called ESortingType. You will want to add an option in there (I suggest adding your options to the end, the editor does not like it when you re-order or insert enum entries).
2. I suggest making your own IFP function library or making a IFP section in any of your existing function libraries. This is so if you ever update the plugin, any changes you make to the IFP function library won't be wiped.
Then make a function that sorts an array of items. An example can be found in FL_InventoryFramework.h -> SortItemStructsAlphabetically