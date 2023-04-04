---
tags: 
order: 100
---

# Installing the plugin

---
Because of how integral an inventory system is to a game, I highly recommend you DO NOT install the plugin to the engine and then use it that way, but instead install the plugin locally to your project. That way you can use the plugin in multiple projects without having a change that works in one project mess up the other project. This means you can also have much better control how the parents behave and work and not have to worry about changes made for another project. But it does mean updating the system is a bit more work, but it is very straight forward if you have source-control set up, as it'll tell you all the changes and you can reimplement any custom changes you made to suit your project.

This does NOT cover how to install an IDE such as Rider or Visual Studio. You should already have an IDE installed.
<a href="https://docs.unrealengine.com/4.27/en-US/ProductionPipelines/DevelopmentSetup/VisualStudioSetup/" target="_blank">**Unreal's docs for setting up Visual Studio**</a>
There are also a few demo files that you’ll want to replace and enum entries you need to rename to fit your project.

1. If your project doesn’t have any C++ code, you will want to go into the engine and at the top go to **Tools -> New C++ Class**. It doesn’t matter what you create here or where you save it. This is just to get the engine to generate all the files you need to compile the plugin.
2. Go to your **Unreal Engine installation -> Engine -> Plugins -> Marketplace** and copy the InventoryFrameworkPlugin folder.
3. Go into your project's root folder (Where your .uproject is), create a folder called “Plugins” if you don’t have one.
4. Paste the InventoryFrameworkPlugin folder into the plugin folder.
5. Go back to your .uproject, right click and press "Generate Visual Studio Project Files”.
6. Open the solution file with whatever IDE you use (For example Visual Studio or Rider) and build the project.
7. (Optional) You can now uninstall the plugin from the engine through the Epic Games Launcher, though you won't get notifications from the Epic Games client whenever an update is available (The discord announces every update as well).

Everything in the demo folder is designed to be replaced by your assets/Blueprints. It is only there so you don’t get any errors on startup.

---
# Finding the plugin content in your content browser

To find the blueprints, you will want to start the engine, go into your content browser and make sure that in your settings (1) and make sure "Show Plugin Content" (2) is checked. Once that is checked, you will find a "Plugins" folder and in there you will find "InventoryFrameworkPlugin content" (3)

![](/pictures/ShowPluginContent.png)

If you decide to NOT install the plugin locally, you will also have to check "Show Engine Content" and then go into Engine -> Plugins -> InventoryFrameworkPlugin Content

---
Once you are done installing the plugin, I **HIGHLY** advise you make children of any class in this system once you want to modify. This should make updating in the future a lot easier and gives you a lot more customizability.

It is highly recommended to head over to the [How does the system work?](https://inventoryframework.github.io/introduction/howdoesthesystemwork/) page and giving a read before proceeding.
