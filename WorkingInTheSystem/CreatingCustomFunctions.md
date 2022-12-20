# Creating custom functions

Once you’re comfortable enough in the system to make your own functions and/or change the current functions, here are some things to keep in mind.

You should always assume an item does not have a widget version of itself. If you need access to the widget of an item, you can use <span style="color:brown">GetWidgetForItem</span>. Remember to check if the widget is valid.
Widgets also can’t be transported through a network, so if multiplayer is important to you, your server functions can’t rely on data from widgets.

## Finding Items

Finding an item is a very common scenario when you start making your own functions.

The container's <span style="color:slateblue">TileMap</span> combined with an item's <span style="color:slateblue">UniqueID</span> is the most reliable way of finding items.

The component includes a function to get an item at item at a specific index of a container GetItemAtSpecificIndex, then there is a bit more expensive way of finding an item but it’ll go through all containers, GetItemByUniqueID.

1. The <span style="color:slateblue">UniqueID</span> is the most reliable method of finding a very specific item, container or item driver, other than a direct reference to the item, container or item driver, but even then a direct reference might get outdated if the player moves the item or modifies it in some way. The <span style="color:slateblue">UniqueID</span> is only updated for containers and items if they are moved to a new component.
2. <span style="color:violet">W_Tile</span> also have a reference to the item on top of them, but to reduce memory, tiles have a <span style="color:slateblue">OwningItem</span> integer which references the top left tile of the item, which holds the <span style="color:slateblue">ParentItem</span> reference variable.
So if you are inside the <span style="color:violet">W_Tile</span> and need to get the item it contains, you would use <span style="color:slateblue">ParentContainer</span> -> <span style="color:slateblue">Tiles</span>[ParentTile] -> <span style="color:slateblue">OwningItem</span> -> <span style="color:brown">GetItemData</span>. Remember to check if <span style="color:slateblue">ParentTile</span> is -1 first, as that means this tile widget doesn’t have any item inside of it.