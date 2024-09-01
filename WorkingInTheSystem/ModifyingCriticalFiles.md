# Modifying critical files

!!!danger
Modifying critical files has its pro's and con's. Removing data is not recommended unless you are ready to fix any problems it might cause. Adding data should not be a problem, but there are some important things you must keep in mind.
If your project is single-player only, adding data is a lot safer as there's no network bandwidth you need to manage and no replication you need to manage.
!!!

This page covers what is safe, dangerous or optimizations you can perform to critical files this system relies on.

---
# DA_CoreItem
This file is very safe to modify and is intended to be modified. Though it is recommended to make children for each type of item you will have (Weapons, consumables, armor, etc). Though there are several functions you might have to update if you remove any variables or functions.
Remember to use soft references as often as possible.

In the base plugin there are a lot of children of this class as examples. All of which are safe to remove if some don't fit your project, though I recommend giving them a quick look-over to explore what differences were made to them.

---
# FS_InventoryItem
This struct will eat up most of your network bandwidth, so adding anything to it can be dangerous.
Anything being removed from this struct might require a decent amount of reworking some functions.

---
# FS_ContainerSettings
It is not recommended to remove anything unless you are certain you won't need it. Most of the data, other than the tile map, is extremely small.
This struct does contain some small features, like TileTag's, not all projects need this feature, so it can be safely removed.