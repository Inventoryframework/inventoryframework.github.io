# Crafting

---
The inventory system does come with an **optional** module for crafting.

The crafting is a completely independent system and the inventory system has no reliance on it. Removing it will have no impact on the inventory system as this is a one-way relationship. The crafting system relies on the inventory, the inventory does not rely on the crafting system.

---
## Classes

Most classes can be created by right clicking the content browser and going into InventoryFrameworkPlugin -> Crafting, the entire crafting system revolves around two classes:

- <span style="color:violet">**AC_Crafting**</span>: The actor component that handles the crafting and replication. It also stores all recipe's a player owns.

- <span style="color:violet">**DA_CoreCraftingRecipe**</span>: The core crafting recipe data asset, these are your definitions for a recipe. This asset uses four different classes to create the recipe:
    - <span style="color:violet">**Recipe Requirements**</span>: All must return true for the recipe to be craftable. This is only asked on clients to enable the interaction of some widgets, but then asked again on the server when starting the craft.
    - <span style="color:violet">**Display Requirement**</span>: All must return true for the recipe to be visible when opening the crafting screen.
    - <span style="color:violet">**Craft Events**</span>: Code that is ran when a craft is successful, such as playing a sound effect or granting a crafting skill experience.
    - <span style="color:violet">**Recipe Data**</span>: This is ambiguous data meant mostly for blueprint programmers who don't want to modify or create custom children of the data asset. This allows you to add any data to a recipe, such as "Time to Craft" or a description to display in the UI.


---
## Recipe handling
Many games have recipes bound to things such as a "crafting bench", which is completely natural. The pain points come when you are deciding where you want to store recipe's and finding out what recipe's the player can and cannot craft.

When a player unlocks a recipe, even if it is tied to a unique object such as a crafting bench or area, the player still holds onto the recipe.
This is so it is much easier to evaluate how far the player is into the game and evaluating whether or not you should grant the player a recipe as the other object might not be loaded. In general, it is much simpler and efficient to just have the player hold a reference to all recipe's they have unlocked, then either blocking the craft or preventing it from being displayed (or both) through other means, such as applying a tag on the player when they are near a bench and the recipe requires that tag to be displayed.

---
## Widgets
The plugin itself does NOT come with any widgets to use the system. Only the example project has widgets and that is the primary learning source for creating custom widgets and UI. These examples are not needed in your project, they are simply an example of how I would use the component and data asset.

My setup begins inside of <span style="color:violet">**WBP_Inventory**</span> -> <span style="color:violet">**Receive External Component**</span>, which will lead to a subgraph called CraftingExample. Most of the system runs on delegates (For blueprint users, these are most often called Event Dispatchers, in C++ and C#, they are referred to as Delegates, so I will continue calling Event Dispatchers as Delegates), which can be simply handled since the player is responsible for holding all recipe's as mentioned above. I always name any functions related to a delegate with a **D** prefix and most often sort them into their own **Delegates** category.

Since the system uses soft references for most recipe's, you can decide whether or not to instantly or async load your recipe's. I've chosen to async load them as that is better for performance.

There are no rules on how to make your own widgets when using the crafting system, my system primarily uses the recipe requirements, display requirements, craft events and craft data to design how and when recipe's should be visualized.

---
## Serialization
To save the players recipe's, the crafting component has an array of "Recipes" which you will want to save and then restore when loading a save.

---
## Timed Crafts
When doing a timed craft, there's a few things to consider. First, you'll want to add the <span style="color:violet">**RD_TimeToCraft**</span> recipe data to your recipe asset. Then when you are about to perform a craft, you will want to call <span style="color:brown">**GetUniqueCraftHandle**</span> and pass that into <span style="color:brown">**S_CraftRecipe**</span> and save the value on the client. If you now wish to cancel the craft, for example if the player dies, then you can call <span style="color:brown">**S_CancelCraft**</span> and pass in the handle.

By default, clients do not get any kind of callback for when a craft is starting to handle something like a craft bar. Most games simply do not handle it this way. One way to handle this is to simply start the visual timer - for example a progress bar - instantly. Then retrieve the <span style="color:violet">**RD_TimeToCraft**</span> data to get the timer length, but then add the players ping on top of that. Other methods would include simply allowing the progress bar to reach the end, but not actually vanish until the server says the craft is finished.
The majority of players pings are low enough to not notice most methods. It's up to your code to handle how the visuals are displayed, what events cancel these crafts and for you to store and retrieve these handles.

---
## Removing the crafting system
To remove the crafting system from the plugin, follow these steps:
1. Remove all references in your project from any of the crafting system. (If you have just installed the plugin and aren't building on top of the example project, you can skip this step)
If you are building on top of the example project, you'll want to remove the crafting component from <span style="color:violet">**BP_PlayerCharacter**</span>.
Then go into <span style="color:violet">**WBP_Inventory**</span> and remove all the crafting widgets and remove the crafting sub graph and the crafting delegate functions.
2. Go into the plugin folder and into Content and delete the Crafting folder.
3. Go into the plugin folder and into Source and delete the IFP_Crafting and IFP_Crafting_Editor folders.