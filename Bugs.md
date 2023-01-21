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
There's an interface event called <span style="color:brown">**UpdateCollision**</span> and some items might need to override this function. It's default is in <span style="color:violet">**BP_SM_ItemPhysical**</span>and it *should* work in most projects.

- <span style="color:violet">**WBP_Highlight**</span> is slightly not in the position it's supposed to be in. I’ve tried to solve this but it seems to be an issue with some floats getting truncated during some of the math. From my testing it was happening completely randomly so it’s been very challenging to diagnose. The Blueprint version is working just fine though. The performance difference between the two is almost none from my testing.

- The <span style="color:violet">**BP_PreviewActor**</span>'s animation is snapping to frame 0 of the animation it is supposed to play whenever the preview is refreshed. This is an issue I’m trying to solve. You can of course replace my actor preview system with a normal preview system, but those require a specific workflow and actor component hierarchy setup, whereas mine does not.
This might take a while as I’m also trying to figure out a way to have particle effects also not get reset.

- In 5.1, RenderTargets can break if your quality settings are below Epic.

- When equipping a blueprint item, the attachment does not seem to be working if the components are set to replicate by default. You will want to go into your blueprint, go through all components and uncheck "Component Replicates" until the issue is gone.

## Common Problems
These aren't bugs, but problems that can be misinterpreted as bugs.

- The <span style="color:violet">**WBP_Highlight**</span> widget and <span style="color:violet">**WBP_InventoryItem**</span> widget padding doesn’t take into account any scaling their parent widgets have to do (like borders). So if you are getting issues with items being dropped in a different tile or an item not lining up properly, there is most likely an issue with your UMG hierarchy/scaling.
You might have this problem with scroll bars most of the time. An easy fix for that is to go into the ScrollBox, go into Scrollbar Padding, let's say the scroll bar is on the right side, go into the right side padding and minus the thickness of the bar.

- <span style="color:violet">**BP_PreviewActor**</span>'s automated camera distance is determined by all components which have collision enabled. This means if you have collision hitboxes or something similar on your actors, the camera will try to adjust to fit that in the camera view.
Remember to use the tag “**DONOTPREVIEW**” on those collision components if the <span style="color:violet">**BP_PreviewActor**</span> is copying them.

- Your item is not showing up in the asset manager or your getting a "Ignoring PrimaryAssetType Items - Conflicts with DA_CoreItem - Asset: ITEMNAMEHERE" in your message log. This happens when there are two identical data assets. This will most likely happen when you have two completely blank data assets and haven't filled in any of the data yet.