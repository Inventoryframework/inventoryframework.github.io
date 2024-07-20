---
order: 8
icon: bug
tags:
---

This list is only relevant to the latest version of the plugin.

## Known Bugs

- Some tools are causing heavy lag spikes. This is an engine bug, the `DetailsView` widget is invalidating itself hundreds of times when it's updated. This issue seems to have started with Unreal 5.4

- The "Reset to Default Property Value" button for container settings is not resetting values to their proper defaults and it's always showing for all values, even if the value is set to its default. I am unsure what is causing this.

- The drag widgets sizing is much larger than it is supposed to be for a couple of frames. There is currently a 0.05 delay inside <span style="color:violet">**WBP_Drag**</span> to resolve this. This is a band-aid fix, I'll try to resolve this fully in the future.

- While inside a editor utility widget, the gameplay tags slate widget is not updating properly. The DetailsView widget is riddled with a few bugs as of at least 5.2 when it comes to custom slate struct widgets. The best you can do right now is open the gameplay tag to see what values are inside of it and adding/removing something to update it, then it'll show the values inside the tag correctly.

- In 5.2, saving an icon has a chance of crashing the editor. This is happening because Epic modified how this is handled into an async task, but left no delegate for me to bind. For now, a delay is being used instead of a delegate, but on slower computers, the async task might not be finished by the time the delay finishes, causing a crash. If you are crashing, go into <span style="color:violet">**WBP_InspectDebug**</span> and click on B_SaveIcon and find its OnPressed event. You'll find a delay and you'll want to increase it.

- The inventory component might be falsely reporting it might be "corrupted" or a custom property list is not initialized correctly. This is happening because when a component is declared in C++ and then the ComponentClass is set to a blueprint, it starts loading everything connected to that blueprint. Some assets seem to be breaking something inside of UE's loading process and causing issues. I haven't noticed any patterns, but most of the issues seem to occur when a player character is referenced.
Because of this, you really have to mind your hard references, or go through the process of removing the C++ version, which I do not recommend. You should be minding your hard references, especially when it comes to the inventory component.


## Common Problems
These aren't bugs, but problems that can be misinterpreted as bugs.

- The inspector debugger changes are being set but not being saved after closing the editor. This is because the engine seemingly refuses to label assets as dirty while a gameplay session is active and doesn't label them as dirty after closing the gameplay session. An asset that is dirty will have an asterisk (*) indicating that you have changes that need to be saved, but since it refuses to label it as dirty, it means that you have to manually open the asset and pressing save, even though there is no star indicating that you have changes that need to be saved. This may be a engine bug or is intended, but it seems that there's nothing I can do about this.

- The <span style="color:violet">**WBP_Highlight**</span> widget and <span style="color:violet">**WBP_InventoryItem**</span> widget padding doesn't take into account any scaling their parent widgets have to do (like borders). So if you are getting issues with items being dropped in a different tile or an item not lining up properly, there is most likely an issue with your UMG hierarchy/scaling.
You might have this problem with scroll bars most of the time. An easy fix for that is to go into the ScrollBox, go into Scrollbar Padding, let's say the scroll bar is on the right side, go into the right side padding and minus the thickness of the bar.

- <span style="color:violet">**BP_PreviewActor**</span>'s automated camera distance is determined by all components which have collision enabled. This means if you have collision hitboxes or something similar on your actors, the camera will try to adjust to fit that in the camera view.
Remember to use the tag “**DONOTPREVIEW**” on those collision components if the <span style="color:violet">**BP_PreviewActor**</span> is copying them.

- Your item is not showing up in the asset manager or your getting a "Ignoring PrimaryAssetType Items - Conflicts with SOMECLASSNAME - Asset: ITEMNAMEHERE" in your message log. This happens when there are two identical data assets. This will most likely happen when you have two completely blank data assets.

- The highlight widget is going outside the boundaries of the container or is getting clipped. This happens if the sizing of the widget does not match the container dimensions. This is most common if the container is slotted into a canvas panel. If that is the case, you can check "Size To Content". For other widgets, you will have to manage the sizing of the container and in some cases, it's handled automatically.

||| Example 1
![](/pictures/InaccurateHighlightProblem1.png)
||| Example 2
![](/pictures/InaccurateHighlightProblem2.png)
|||

- After adding or changing default values inside <span style="color:slateblue">**FS_InventoryItem**</span>, the InventoryHelper will falsely report a value is no longer set to default and will reset it to the wrong value when pressing the reset button. This is happening because the struct inside the InventoryHelper did not get updated. You must go into <span style="color:violet">**EUW_InventoryHelper**</span> and find <span style="color:slateblue">**SelectedItem**</span> and update its default settings.
The same might happen to the context menu, so you must go to <span style="color:violet">**WBP_ContextMenu_EditorOnly**</span> and find <span style="color:slateblue">**Item**</span> and update its default settings.