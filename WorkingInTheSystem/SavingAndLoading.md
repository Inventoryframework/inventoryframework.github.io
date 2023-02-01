---
tags: [underrevision]
---

# Saving and loading

!!!danger
Under revision
!!!

Everything is saved in structs, so it should be compatible with your save system. It’s up to your save and load system to manage this, but if you need an example, the demo project has an example of a simple save and load system.

But let’s be honest, Unreal Engine’s save system is not very good and lacks too many fundamental features. I recommend a plugin from the marketplace called <a href="https://www.unrealengine.com/marketplace/en-US/product/savior" target="_blank">**Saviour**</a>.

## Things to keep in mind
UniqueID's must reset whenever you try to save containers and items. The parent component object reference can not be saved. The component has a function called <span style="color:brown">**GetContainersForSaveState**</span> which provides you with a copy of the containers, but some data has been modified to be compatible with save files. I recommend looking inside this function to better understand what can not be stored in the save file.
