---
tags:
---

# Saving and loading

The demo includes an extremely basic save and load system. I will not be implementing anything advanced, because if you ever decide to remove the inventory system from your project, you will have to extract the save and load system from this project. You should have some system which you can bring to all of your projects.

Everything is saved in structs, so it should be compatible with your save system. It’s up to your save and load system to manage this, but if you need an example, the demo project has an example of a simple save and load system.

But let’s be honest, Unreal Engine’s blueprint save system is not very good and lacks too many fundamental features.

If you are going to make your own save and load system, I suggest using C++ as the current Blueprint functionality of Unreal's save and load system is really bad.
There are a few C++ saving and loading system tutorials on Youtube which cover how to serialize nearly any data you want, all of which should be compatible with this system.

## Things to keep in mind
UniqueID's must reset whenever you try to save containers and items. The parent component object reference can not be saved. The component has a function called <span style="color:brown">**GetContainersForSaveState**</span> which provides you with a copy of the containers, but some data has been modified to be compatible with save files. I recommend looking inside this function to better understand what can not be stored in the save file.

## Multiplayer
When a client connects, it's up to your save system to assign them the appropriate container settings.

## Equipment
When you load a save, you might want some equipment the player has to become visible.
You have two options:
1. The component has a delegate for when it is started, from which you can assign a Trigger Filter inside of the Equipment Item Component. These should be instantaneous attachments and no animations should occur at the same time as they can cancel each other before any critical notifies occur.
2. Your save and load system has the ability to restore actors. From here you must ensure the restored actor gets the owning items UniqueID as a tag, unless you've also implemented a way to save the owning items UniqueID.