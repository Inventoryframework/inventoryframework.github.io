# Loot Tables

---

There are tons of ways to handle loot tables, but ultimately all they do is add and/or remove items from an array. The abstract design of this system does try to provide you with an interface that allows you to create any type of loot table system, but that does mean that the loot table is slightly more complicated than some other loot table systems. Here are the design parameters that the loot table system follows:
- Handled through a actor component and instanced objects. This means you can create custom loot table systems and even mix multiple types of loot tables.
- Usable on any actor with a inventory component, including item actors, interactables, player character, etc. This includes allowing item data assets to declare default loot tables.
- Items added through loot tables should also have their loot tables imported, meaning recursive loot tables.
- `Modular Game Feature` (`MGF`) support. This means that loot tables should be extendable through content updates/DLC/expansions.

The loot table system comprises of 2 parts; A component (loot table) and an array of instanced objects (loot pools) that live inside the component.

!!!
Game features are still labeled as "Beta", so do expect some bugs around `MGF`'s. But they have been battle tested extensively through Fortnite, last time I heard they had over 200+ `MGF`'s.
This system does not rely on `MGF`'s, it's simply meant to be utilized by them
!!!

---
## Loot table component
The loot table component is the heart of the loot table system.
When the inventory component calls <span style="color:brown">**StartComponent**</span>, it will first try and find any loot table components and run <span style="color:brown">**PreInventoryInitialized**</span> and forward that to the `Loot Pool Object`'s. Most functions will not work as expected, because this is before anything is initialized. You are just expected to manually modify the <span style="color:slateblue">**ContainerSetting**</span>'s array as if you were modifying it yourself by hand. Most changes will be done here.
Once <span style="color:brown">**StartComponent**</span> is finished, it'll call <span style="color:brown">**PostInventoryInitialized**</span>. All functions will now work just as normal, but you are expected to add most items during the pre-initialization phase.

- You can have as many loot table components on an actor as you want. This is primarily used for better `MGF` support.
    - Loot table components are sorted by their <span style="color:slateblue">**Priority**</span>. Higher priority components are ran first. This is to allow future components added through `MGF`'s to increase the chances of new content being prioritized. But keep in mind, this gives some loot tables a slight bias over others.
- Loot table components are automatically destroyed on clients. They are only utilized by the server.
- The <span style="color:brown">**PostInventoryInitialized**</span> function does open up the possibility of doing async loot tables or spacing out loot additions. This should really only be considered if you are doing hundreds of items. It is of course up to you to handle when you start this async operation and implementing it.

---
## Loot Pool object
These objects are what actually try to modify the inventory components <span style="color:slateblue">**ContainerSetting**</span>'s, store any data and run any meaningful functions. These are stored in the loot table components <span style="color:slateblue">**LootPools**</span> array. By default, this has nearly nothing inside of it.

The plugin comes with 2 examples, a simple loot pool and a advanced loot pool.
- Simple example: Contains an array of items, each has a percentage to spawn and the order is then randomized.
- Advanced example: This loot pool object has the ability to be globally modified during runtime through `MGF`'s. This system utilizes a world subsystem that only exists on the server and `MGF`'s can dynamically add and remove items from the subsystem. The subsystem has a map of tags and this object fetches the items to use through this tag.
This means that these advanced loot pools can be expanded on in future updates very easily, without referencing any assets and allowing the loot to have the same probability as other items in the pool, where as the simple example will always have some bias as one loot pool has to be executed before the new ones added through `MGF`'s are executed. Remember, `MGF`'s are one-way referencable only, in your base game, you shouldn't reference content inside of `MGF`'s.
    - This system also has the ability to dynamically add/remove items from loot pools during runtime, so you could for example have a chest that has more items inside of it, depending on if the player has defeated a specific boss.

---
## Modular Game Features
Some games using IFP might get content updates, DLC, expansions, something that adds content to the game, such as items. Loot tables is the primary way that most games handle additional content post-release.
- It's not recommended to put item assets into MGF's because it highly complicates how the inventory of all actors is managed. What if the player is wielding a weapon added through a MGF and the MGF gets disabled? Do you delete the item? When it's enabled again, how would you restore it? If you do decide to add item assets through MGF's, then you are responsible for managing inventories if it's ever disabled. The only exception to this is if you are permanently enabling this MGF, like an expansion. Then this scenario won't ever happen.
- By default, there are 2 ways to add new content to loot tables. One is to add another loot table component to actors. This follows the same procedure as the standard `Add Components` action MGF's come with. The other is used by the advanced example loot pool object and that is registering more items to the subsystem. If you don't like either option, then you can always implement more methods by creating new loot table objects.

---
## Overriding the loot table system
If you want to completely override, customize or even implement your own entire custom loot table system, that is also quite easy to do. Just simply override the <span style="color:brown">**ProcessLootTablesPreInitialized**</span> and <span style="color:brown">**ProcessLootTablesPostInitialized**</span> functions in your inventory component.

---
## Pre-loading assets
Loot pools have the ability to start loading any assets before the loot table is processed by calling <span style="color:brown">**PreLoadAssets**</span>.
It is recommended to roll your spawn chances here, then only load the assets that succeeded their spawn chance and populate a second array with these items. This way you don't uneccessary load any assets that would have failed their spawn chance.

- By default, there is nothing calling this function. It is up to you to decide when and how this is called.

---
## Item Asset Loot Tables
To have item assets have loot tables, you must override the <span style="color:brown">**GetLootTables**</span> function and pass in an array of loot tables. An example can be found in the `DA_Equippable` and `DA_Weapon` item assets that come with the plugin.

When <span style="color:brown">**StartComponent**</span> is called, it'll check any item that has the `IFP.Initialization.IncludeLootTables` tag and then create loot table components with the same settings as the ones declared in the item asset and attach them to the same actor that the inventory component belongs to. This tag is then removed afterwards.

Loot tables can get the item they were created from and its containers through the <span style="color:brown">**GetParentItem**</span> and <span style="color:brown">**GetParentItemsContainers**</span> functions. Loot pools have a <span style="color:brown">**GetLootTable**</span>  function, so loot pools also have access to this data through this function.

---
## Data Registry
`MGF`'s reference a "data registry" which seem to have been introduced around 4.26, but at the time of writing, there is very little documentation and examples of how it works or how best to utilize it, but the lead engine programmer at Epic did discuss it in this video and he even refences a loot table system.
https://www.youtube.com/live/7F28p564kuY?si=xX839PLPJHtXyZjN&t=5027

I won't be using data registries in the example project for now, because some of the functions around it are still labelled as experimental and due to the lack of documentation or any sources of how to best utilize it, but it might be worthwhile to some to investigate data registries for their loot table systems.