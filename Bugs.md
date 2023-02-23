---
order: 8
icon: bug
tags: [underrevision]
---

## Known Bugs

!!!danger
Under revision
!!!

- There's a bug with the engine where if any item in your blueprint has <span style="color:slateblue">**Simulate Physics**</span> enabled, it'll remove the root component. There are some workarounds but none were perfect, especially with how flexible I wanted the system to be. UE5 preview fixed some issues but I immediately found new ones.
There's an interface event called <span style="color:brown">**UpdateCollision**</span> and some items might need to override this function. It's default is in <span style="color:violet">**BP_SM_ItemPhysical**</span> and it *should* work in most projects.

- The <span style="color:violet">**BP_PreviewActor**</span>'s animation is snapping to frame 0 of the animation whenever the preview is refreshed. This is an issue I’m trying to solve. You can of course replace my actor preview system with a normal preview system, but those require a specific workflow and actor component hierarchy setup, whereas mine does not.
This might take a while as I’m also trying to figure out a way to have particle effects also not get reset.

- In 5.1, RenderTargets can break if your quality settings are below Epic.

- When equipping a blueprint item, the attachment does not seem to be working if the components are set to replicate by default. You will want to go into your blueprint, go through all components and uncheck "Component Replicates" until the issue is gone. They will still replicate, even though they aren't set to replicate.

- The "Reset to Default Property Value" button for container settings is not resetting values to their proper defaults and it's always showing for all values, even if the value is set to its default. I am unsure what is causing this.


## Common Problems
These aren't bugs, but problems that can be misinterpreted as bugs.

- The inspector debugger changes are being set but not being saved after closing the editor. This is because the engine seemingly refuses to label assets as dirty while a gameplay session is active and doesn't label them as dirty after closing the gameplay session. An asset that is dirty will have an asterisk (*) indicating that you have changes that need to be saved, but since it refuses to label it as dirty, it means that you have to manually open the asset and pressing save, even though there is no star indicating that you have changes that need to be saved. This may be a engine bug or is intended, but there is nothing I can do to fix this right now.

- The <span style="color:violet">**WBP_Highlight**</span> widget and <span style="color:violet">**WBP_InventoryItem**</span> widget padding doesn't take into account any scaling their parent widgets have to do (like borders). So if you are getting issues with items being dropped in a different tile or an item not lining up properly, there is most likely an issue with your UMG hierarchy/scaling.
You might have this problem with scroll bars most of the time. An easy fix for that is to go into the ScrollBox, go into Scrollbar Padding, let's say the scroll bar is on the right side, go into the right side padding and minus the thickness of the bar.

- <span style="color:violet">**BP_PreviewActor**</span>'s automated camera distance is determined by all components which have collision enabled. This means if you have collision hitboxes or something similar on your actors, the camera will try to adjust to fit that in the camera view.
Remember to use the tag “**DONOTPREVIEW**” on those collision components if the <span style="color:violet">**BP_PreviewActor**</span> is copying them.

- Your item is not showing up in the asset manager or your getting a "Ignoring PrimaryAssetType Items - Conflicts with SOMECLASSNAME - Asset: ITEMNAMEHERE" in your message log. This happens when there are two identical data assets. This will most likely happen when you have two completely blank data assets and/or haven't filled in any of the data yet.

- The highlight widget is going outside the boundaries of the container or is getting clipped. This happens if the sizing of the widget does not match the container dimensions. This is most common if the container is slotted into a canvas panel. If that is the case, you can check "Size To Content". For other widgets, you will have to manage the sizing of the container and in some cases, it's handled automatically.

||| Example 1
![](/pictures/InaccurateHighlightProblem1.png)
||| Example 2
![](/pictures/InaccurateHighlightProblem2.png)
|||

- Unable to save <span style="color:violet">**DA_ItemVoid**</span> or getting crashes when closing the editor/changing levels while using it. This is because a object reference is being stored inside the data asset, but when your closing the editor or changing levels its not letting that object get garbage collected. In C++ you will want to add "SkipSerialization" to the UProperty (Look at FS_UniqueID for example) and reset all the data inside.

- After adding or changing default values inside <span style="color:slateblue">**FS_InventoryItem**</span>, the InventoryHelper will falsely report a value is no longer set to default and will reset it to the wrong value when pressing the reset button. This is happening because the struct inside the InventoryHelper did not get updated. You must go into <span style="color:violet">**EUW_InventoryHelper**</span> and find <span style="color:slateblue">**SelectedItem**</span> and update its default settings.
The same might happen to the context menu, so you must go to <span style="color:violet">**WBP_ContextMenu_EditorOnly**</span> and find <span style="color:slateblue">**Item**</span> and update its default settings.