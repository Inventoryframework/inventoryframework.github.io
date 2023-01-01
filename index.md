# Inventory Framework Plugin

#### Last update: 29th December, 2022
#### Latest version: Still under development

!!!danger
This plugin has not been released yet, everything is still under review and can be spontaneously removed or changed.
!!!

## What is the Inventory Framework Plugin?

Nearly every inventory system shares the same foundation, a container with tiles and items slot into those tiles. This Inventory Framework Plugin (IFP) attempts to be that foundation for your project, while allowing for a high level of inventory design, item customization, rapid prototyping and system integration.

From my research, inventory systems were taking 2-6 months or more (depending on how advanced it was) to be designed, bug tested, network tested and to be documented. With this framework, I try to provide you the framework needed to create your perfect inventory system and bring that initial creation time down.

What this provides is the C++ foundation for your programmers and a good Blueprint foundation for your designers, which includes:
1. Actor component and item parents that handle all the logic for you and are abstract enough so you can implement your mechanics and systems into it.
2. Widgets that are designer friendly (Easy to implement new logic, add animations to events, create children, etc). They also handle all the drag, drop, highlighting, splitting, combining logic and more.
3. Data asset which only shows relevant information to your desired rules with inbuilt functions and [**instanced objects**](https://inventoryframework.github.io/classes-and-settings/o_itemobjectandac_itemdriver/) that can have custom  variables per data asset.
4. A system that has no reliance on widgets to create or handle its data, meaning it is possible to use the data for any widget you create and not be dependent on the widgets that come with this system.
5. Container system that allows for near infinite (array limit) items inside of items. This means you can make a backpack, which has several containers, then a gun inside one of those containers and then that gun can have containers which have items inside of them. It can keep going for as long as you want.
6. Actor appearance duplication system that copies the appearance of your items allowing you to both inspect items and auto-generate icons. It works with all actors, not just item actors. No longer do you need to follow a hierarchy setup like many other inventory systems need. You can create your items however you want and the system will automatically duplicate the appearance for either generating item icons or letting you examine an item.

It is NOT an inventory to fully replicate the mechanics of any specific game. If you want your system to behave like a specific game, you will have to implement those specific mechanics yourself. This system does try to provide you with the functions and data to replicate most inventory systems.

If you have any issues or questions neither the videos or documentation answer then feel free to message me on Discord (Variann#8404) or join the discord channel.
You can look at the FAQ for more info.

## Color Sheet:
<span style="color:brown">**functions**</span> - <span style="color:slateblue">**Variable**</span> - <span style="color:green">**category**</span> - <span style="color:violet">**class**</span>