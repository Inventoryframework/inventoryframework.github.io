---
tags: [Tools]
---
# Item Inspector

---
This is not a editor utility widget, but rather a in-game tool. The files can be found inside Core -> Widgets -> ExamineAndInspect
<span style="color:violet">**WBP_Inspect**</span> has a ToggleDebug button on the top bar. Pressing this will expose this tool to the user.

There is a drop down menu allowing you to select whether you want to modify the generated icon settings or the inspection settings.

You can use this for figuring out what rotation and location you want to feed into the generated icon settings.
Pressing any of the XYZ rotation or location buttons will rotate or move the mesh by the amount set in the slider. You can also manually put in a value in any of the text boxes. Zoom refers to the camera distance from the item.

You can save your values to the data asset by pressing the **Save to Asset** button.
It is always more efficient for the game thread to NOT generate the item icons via the preview system. It is recommended whenever possible to save the icons and assigning that to the data asset, then disabling <span style="color:slateblue">**Use Generated Item Icon**</span> inside the data asset.

---
!!!Important
1. The engine does not load assets added to the content browser while a game session is active. For saved item icons to work properly, you must first right click and save the texture, then right click it and press **Asset Actions -> Reload**
2. The engine will not properly label the data asset as dirty (an asterisk * indicating that the asset has changes that need to be saved.) after pressing the **Save to Asset** Button or the **Set Icon** buttons. You must open the data asset yourself and press save, even though there is no star indicating that there are changes waiting to be saved. If you don't do this, your changes will be lost after closing the editor and it won't prompt you if you want to save the data asset.
!!!

---
-![](/pictures/ItemInspection.png)

-![](/pictures/ItemIconGeneration.png)
