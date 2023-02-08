# Creating custom functions

Once you’re comfortable enough in the system to make your own functions and/or change the current functions, here are some things to keep in mind.

You should always assume an item does not have a widget version of itself. If you need access to the widget of an item, you can use <span style="color:brown">**GetWidgetForItem**</span>. Remember to check if the widget is valid.
Widgets also can’t be transported through a network, so if multiplayer is important to you, your server functions can’t rely on data from widgets.

## Finding Items

Finding an item is a very common scenario when you start making your own functions.

The container's <span style="color:slateblue">**TileMap**</span> combined with an item's <span style="color:slateblue">**UniqueID**</span> is the most reliable way of finding items.

The component includes a function to get an item at item at a specific index of a container called <span style="color:brown">**GetItemAtSpecificIndex**</span>, then there is a bit more expensive way of finding an item but it’ll go through all containers, <span style="color:brown">**GetItemByUniqueID**</span>.

1. The <span style="color:slateblue">**UniqueID**</span> is the most reliable method of finding a very specific item, container or item driver, other than a direct reference to the item, container or item driver, but even then a direct reference might get outdated if the player moves the item or modifies it in some way. The <span style="color:slateblue">**UniqueID**</span> is only updated for containers and items if they are moved to a new component.
2. <span style="color:violet">**W_Tile**</span> also have a reference to the item on top of them, but to reduce memory, tiles have a <span style="color:slateblue">**OwningItem**</span> integer which references the top left tile of the item, which holds the <span style="color:slateblue">**ParentItem**</span> reference variable.
So if you are inside the <span style="color:violet">**W_Tile**</span> and need to get the item it contains, you would use <span style="color:slateblue">**ParentContainer**</span> -> <span style="color:slateblue">**Tiles**</span>[ParentTile] -> <span style="color:slateblue">**OwningItem**</span> -> <span style="color:brown">**GetItemData**</span>. Remember to check if <span style="color:slateblue">**ParentTile**</span> is -1 first, as that means this tile widget doesn’t have any item inside of it.

---

## Network optimizations

It is recommended, where ever possible, to use the item or container <span style="color:slateblue">**UniqueID**</span> instead of the struct in your RPC calls, and then have your server use the <span style="color:slateblue">**UniqueID**</span> to figure out which struct to use.



||| Image
![](/pictures/networkoptimization.png)
||| Info
Here you can see 2 sets of RPC calls. One is <span style="color:brown">**ReduceItemCount**</span> (indicated in red), which is passing the item struct to the server and then to the client.
The other is <span style="color:brown">**IncreaseItemCount**</span> (indicated in blue), which is passing the <span style="color:slateblue">**UniqueID**</span> to the server and the server is then using that <span style="color:slateblue">**UniqueID**</span> to resolve what item we are trying to modify. 

Switching to using the <span style="color:slateblue">**UniqueID**</span> method reduced the RPC size from around 75 bytes down to 13.
|||

The only downside is that it is not possible to verify if the client has illegally modified it's data, and it's very difficult to verify that the data inside the item/container are synced. It is up to you whether that is a big enough problem to do this. Some anti-cheat methods already do a very good job at preventing clients from modifying the games data with external tools.
If you need to validate the data, you can have the server function still accept the full item/container struct, validate the data, then only pass the <span style="color:slateblue">**UniqueID**</span> to the client.

We covered that [UniqueID's scale](https://inventoryframework.github.io/introduction/howdoesthenetworkingwork/#unique-id) extremely well on another page, but to give you an idea of how well it scales, here's an example

||| Image
![](/pictures/InventoryItemVSuniqueID.png)
||| Info
These are two functions using an array with 5 entries of <span style="color:slateblue">**S_UniqueID**</span>'s (Blue) and an array with 5 entries of <span style="color:slateblue">**S_InventoryItem**</span> (Red)
|||