---
tags: 
order: 85
---

# Example Project

!!! Important
This documentation only covers the new example project. The old example project does not have any documentation
!!! 

The example project can be downloaded from the marketplace linked at the top of the site. This document will cover how the example project works and how best to utilize it.

- First, I never recommend people build ON TOP of any example projects from any asset, including `IFP`. You should always create a fresh project and use the example project as either inspiration, or get access to the Github repo and start forking the example projects.
- The example project does not contain anything by itself. It is rather a host for 1 plugin (`IFP_CommonAssets`) which hosts common assets and classes, and then all example projects are their own plugins utilizing this plugin. This way, it is extremely simple for people to get going with a basic example with simple integration into YOUR project, while having Github submodule support.

!!! Important
If you install any of the example plugins, remember to: 
    1. Update your asset manager to include their `Items` folder inside the `Directories` array. If you are using `IFP`'s crafting system, you will also want to include the `Crafting -> Recipes` folder.
    2. Go into your project settings -> GameplayTags and add the plugins tag table into the `Gameplay Tag Table List`.
!!!

---
## Scope
The scope of example projects are very small, for a few reasons:
1. I do not have infinite time on my hands. I'd rather improve the base plugin rather than spend too much time on the examples.
2. To keep them as simple as possible with as few classes as possible. You are already diving into a pretty large framework, I do not want to create example projects that may be too complicated for some.
3. With point 2 in mind, replication is NOT kept in mind for the example projects in some cases, for example playing a montage and ensuring its playing on all clients. `IFP` is already replicated and it already automatically handles the majority of replication for you and many projects built with `IFP` are being built with replication in mind, but not everyone is making a multiplayer game. So I would just be making more complicated code and taking up more of my time to test things that should be replaced with your systems or customized to fit your project anyways. This trade-off is necessary for allowing me to continue creating example projects, developing `IFP` and still work on my personal projects and game.
4. Extremely few assets, such as models, textures and so forth. These projects are meant to be as small as possible in not just assets, but also disk size.

---
## Project structures
Each example project is built with a dependency on `IFP_CommonAssets`, so no matter what project you pick, that plugin also has to be installed. This section covers how each project is structured and handled.
- Hierarchy between classes is kept to a minimum and the character blueprint only handles the bare minimum, such as adding the HUD and handling common inputs. Rather, much of the code is handled through actor components, which allows you to quickly get started and you do not have to worry about hierarchy-hell. Most components have a comment box at the top of the event graph explaining their purpose. If you wish to use any of these components in your project, I suggest making a child of each so any updates won't override your changes.
    - <span style="color:violet">**AC_QuickStartHelper_IFP**</span> - Handles common delegates and inputs based around inventories, such as adding the inventory to the screen and starting the inventory component. This is simply to quickly get you started to make sure `IFP` is working correctly in your project or act as a wrapper to contain all code based around `IFP` without modifying your character blueprint.
    - <span style="color:violet">**AC_BasicInteraction_IFP**</span> - Enables the basic interaction system that comes with IFP (This system is designed to be replaced, which this documentation covers) TODO: Add link
    - <span style="color:violet">**AC_QuickLootHelper_IFP**</span> - Handles the quick loot example system.
    - <span style="color:violet">**BP_AC_EquipmentHelper**</span> - Handles `IFP`'s default equipment system, which can be modified or completely replaced.
    - <span style="color:violet">**BP_AC_Interactable**</span> - Enables the actor to be treated as an interactable. Such as a chest or a vendor.
- Each plugin gets its own gameplay tags table.

---
## Examples
- **Showcase**: This example is simply trying to fit in all features of `IFP` into one showcase.
- **RPG**: Example of a traditional inventory, with a quick slot system and a upgrade station.
- **Survival**: Example of a survival game with grid inventories, with drag and drop hotbar.

---
## Github repository access
To access the plugins repository and the associated example plugins plugins, you will need to verify your purchase by forwarding your Epic Games receipt to my email (Varianprofessional@gmail.com) with your Github username

---
## Old example project differences
The primary way that the old example project and the new one are different is how the player character is setup. Both in my personal project and in `IFP`, I am trying to de-bloat the player character as much as possible. All the components listed above contain the vast majority of the logic that used to be in the player character.

---
## Color Sheet:
<span style="color:brown">**functions**</span> - <span style="color:slateblue">**variable**</span> - <span style="color:green">**category**</span> - <span style="color:violet">**class**</span>