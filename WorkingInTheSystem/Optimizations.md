# Optimizations

While IFP is already extremely optimized, there's a few things you can do and keep in mind to make sure you are getting the absolute most out of IFP.

To start, lets first cover what are the most expensive parts of `IFP` (Listed in semi-random pattern. It is not possible to 100% be certain what is the MOST expensive in all projects as there are multiple factors as when some things might become more expensive than others):
- The biggest performance culprit are the item widgets. About 33% of the cost of making a widget for an item just goes into creating the UMG hierarchy inside the widget.
- Finding an available tile during <span style="color:brown">**StartComponent**</span>. The more tiles an item occupies, the more calculations are required to find a spot. This is also where the InventoryHelper can improve performance. If a specific tile is provided to an item, it doesn't have to scan the entire container to find a free tile.
- Disabling rotation for items through <span style="color:brown">**AC_Inventory**</span> -> <span style="color:brown">**CanItemBeRotated**</span> will reduce the amount of calculations required during <span style="color:brown">**StartComponent**</span>.
- Unfortunately, removing some Blueprint features greatly improves performance. Let's take <span style="color:brown">**CheckCompatibility**</span> as an example. In a test with 100, this function in total took 1.5ms, but removing the `BlueprintNativeEvent` keyword from the UFUNCTION, which is the keyword that allows the function to be overriden at a Blueprint level, reduced the total time down to 0.090ms. Removing Blueprint support improved the performance by about 16 times. The same can be done to <span style="color:brown">**GetFirstAvailableTile**</span>, which will gain you the most performance gains.

## Separating lag spikes
One of the great benefits of IFP's structure is that it doesn't require widgets to function. Which means we can separate the lag spike of starting the component, creating the inventory, the widgets, and adding the inventory screen to the viewport into four chunks.

1. Calling <span style="color:brown">**StartComponent**</span>.
2. Creating the inventory screen widget.
3. Binding the containers with the widgets inside of the inventory screen.
4. Adding the inventory screen widget to the viewport.

Doing all of these at the same time will in some scenarios create a spike that is noticable to players. But separating them will make it (hopefully) invisible to players.

## Widgets
The tile widget is already highly optimized. The hierarchy can be reduced to just an image. (The demo widget has an overlay inside of it for debugging assistance. Removing it will almost double the construction speed.
- The only feature in the tile widget that is expensive is the `TileTags` feature. It's not that the functionality behind it is expensive, it's simply the fact that this feature is getting executed per tile and containers can end up being 100+ tiles and the price starts to pile up, which is something you need to keep in mind in general when working with tiles. All your code must be highly optimized when it comes to the tile widget as your code might be spammed by hundreds of tiles within one frame.

The item widget will be going under a few optimizations throughout the next updates, but there are some core optimizations you can do:
- Generated item icons are and always will be much more expensive than just disabling it and using a texture. The `ItemEditor` tool can take the generated item icons and save them to disk as a texture to assign in the data asset. Getting the best of both worlds, though this will remove any features where the icon updates depending on what is attached to an item, for example attaching a silencer on a gun and having the icon update accordingly.
- Staggering out the generated item icons. This is actually a default feature that can easily be enabled in <span style="color:violet">**WBP_InventoryItem**</span>.
- Depending on the simplicity of your equipment, you might want to do what some of the Resident Evil games do. They have a texture for each gun and every combination of it with attachments. Of course, they had very few weapons and very few combinations of equipments.
- Create some of the widget components during runtime. For example, the ItemCount text. If the majority of your items can't be stacked, you are then creating a text widget that will never be shown for a lot of items. By default, `IFP` does not do this as this is generally a more annoying workflow. This really comes down to per projects needs.

## Networking
The only real meaningful thing you can do about network performance is trimming down the `FS_ContainerSettings` and `FS_InventoryItem struct`. For example; if you don't need the overwrite settings for items, you can remove it to reduce the RPC size of all items.

## Equipment
- Do not use Blueprint Actors for your equipment. They are in some cases 11 times more expensive than skeletal/static meshes. Many people have proven that it is possible to do complex logic, like guns with animations and so forth without using Actor equipments. I personally suggest using GAS and whenever an item is equipped/unequipped, you fetch the mesh and let the GAS ability animate it and so forth.
- Equipment easily take up 70%+ of the lag spike caused by <span style="color:brown">**StartComponent**</span>. If you want to keep using lots of equipment, you need to start calling <span style="color:brown">**StartComponent**</span> during your loading screens or managing equipment better. In reality, only the player character should be spawning equipment meshes.

---
## Color Sheet:
<span style="color:brown">**functions**</span> - <span style="color:slateblue">**variable**</span> - <span style="color:green">**category**</span> - <span style="color:violet">**class**</span>