# Tile Tags

The tile tag system is a simple way of adding tags to specific tiles inside of a container.
In the base plugin, this system doesn't do much of anything, only being used to hide specific tiles to create custom shaped containers.

Tags can be added and removed with the <span style="color:brown">**AddTagsToTile**</span> and <span style="color:brown">**RemoveTagsFromTile**</span> during runtime or edited in the details panel while in the editor.

Adding and removing tags will call two delegates:
1. <span style="color:violet">**AC_Inventory**</span> -> TileTagsAdded
2. <span style="color:violet">**AC_Inventory**</span> -> TileTagsRemoved

Binding to these two delegates will let your code respond to any tag updates.
An example can be found in <span style="color:violet">**WBP_DemoTile**</span> -> <span style="color:brown">**Construct**</span>

## Networking
Even though the `TileTag` system is automatically replicated and can be modified during runtime, it is not recommended to overuse it for multiplayer games. This system can start eating up a lot of bandwidth very quickly, but this of course depends on the scope of your game.

## Use case
Like mentioned above, this is primarily used to create custom shaped containers by hiding specific tiles. But this has more use cases that some people have used it for:
1. Locking tiles and unlocking them based on some in game reward or event.
2. Empowering items that are in "empowered" tiles.
3. Items durability slowly improving while in a specific tile.

Some have even used the size of an item as a trade-off. Larger items would take more space, leaving the player with less space to carry other items, but the larger items could then occupy more of these beneficial tiles and gain these positive perks.