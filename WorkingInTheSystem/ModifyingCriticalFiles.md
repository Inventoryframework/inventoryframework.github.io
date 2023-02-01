# Modifying critical files

!!!danger
Modifying critical files has its pro's and con's. Removing data is not recommended unless you are ready to fix any problems it might cause. Adding data should not be a problem, but there are some important things you must keep in mind.
!!!

This page covers what is safe, dangerous or optimizations you can perform to critical files this system relies on.

---
# DA_CoreItem
This file is very safe to modify and is intended to be modified. Though it is recommended to make children for each type of item you will have (Weapons, consumables, armor, etc). Though there are several functions you might have to update if you remove any variables or functions.
Remember to use soft references as often as possible.

In the base plugin there are a lot of children of this class. All of which are safe to remove if some don't fit your project, though I recommend giving them a quick look-over to explore what differences were made to them.

---
# FS_InventoryItem
This struct will eat up most of your network bandwidth, so adding anything to it can be dangerous.
If you are looking to optimize this struct, you "can" remove the ContainerIndex and ItemIndex, as those are used to optimize look-ups, though this should be a last resort as a lot of functions will have to be updated.

---
# FS_ContainerSettings
This struct might get a small revamp in the future. The tile map, while it is useful for single player games and drastically speeds up collision tests and look-ups, it's becoming a bit too expensive for replication.
It is not recommended to remove anything unless you are certain you won't need it. Most of the data, outside of the tile map, is extremely small outside of the tile map.