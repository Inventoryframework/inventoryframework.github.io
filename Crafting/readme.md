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

The plugin itself does NOT come with any widgets to use the system. Only the example project has widgets. These are not needed, they are simply an example of how I would use the component and data asset.


---
## Recipe handling
Many games have recipes bound to things such as a "crafting bench", which is completely natural. The pain points come when you are deciding where you want to store recipe's and finding out what recipe's the player can and cannot craft.

When a player unlocks a recipe, even if it is tied to a unique object such as a crafting bench or area, the player still holds onto the recipe.
This is so it is much easier to evaluate how far the player is into the game and evaluating whether or not you should grant the player a recipe as the other object might not be loaded. In general, it is much simpler and efficient to just have the player hold a reference to all recipe's they have unlocked, then either blocking the craft or preventing it from being displayed (or both) through other means, such as applying a tag on the player when they are near a bench and the recipe requires that tag to be displayed.

---
## Serialization
To save the players recipe's, the crafting component has an array of "Recipes" which you will want to save and then restore when loading a save.

---
## Removing the crafting system
To remove the crafting system from the plugin, follow these steps:
1. Remove all references in your project from any of the crafting system. (If you have just installed the plugin and aren't building on top of the example project, you can skip this step)
If you are building on top of the example project, you'll want to remove the crafting component from <span style="color:violet">**BP_PlayerCharacter**</span>.
Then go into <span style="color:violet">**WBP_Inventory**</span> and remove all the crafting widgets and remove the crafting sub graph.
2. Go into the plugin folder and into Content and delete the Crafting folder.
3. Go into the plugin folder and into Source and delete the IFP_Crafting and IFP_Crafting_Editor folders.

---
## Color Sheet:
<span style="color:brown">**functions**</span> - <span style="color:slateblue">**Variable**</span> - <span style="color:green">**category**</span> - <span style="color:violet">**class**</span>