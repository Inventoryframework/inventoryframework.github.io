---
tags: [Tools]
---
# Item Editor
The item editor is a simple, but powerful tool that allows you to select any item the asset manager can detect and directly edit the asset without opening a new window.

This tool is also meant to provide a simple interface for designers to create tools to help with their workflow. There are two tabs, the library and the editor. The library simply is where you can find all your assets and filter them. The second tab is your asset editor. At the bottom you'll find a toolbox. By default there aren't many tools in here, but it is meant for you to expand for your games needs.

---
## Tool box
With the asset comes a icon editor, a shape editor and a socket manager.
- The icon editor allows you to edit the inspect and generated item icon settings, and allowing you to see how the item will look like in game with whatever item widget you have selected. You might want to go into <span style="color:violet">**WBP_ItemWidgetPreview**</span> and setting the default widget class to a class you commonly use.
- The shape editor allows you to easily modify the <span style="color:slateblue">**DisabledTiles**</span> inside the <span style="color:violet">**IT_Shape**</span> object, if it has been added to the item.
    - Tip: You can click and drag to change the state of multiple tiles without having to click over and over again.
- The socket manager lets you manage the sockets on the item if it has the <span style="color:violet">**IT_SocketManager**</span> object.

---
## Tips and tricks
- You can resize the segments with the white bars.
- If you click and drag the item asset icon into the viewport, you can place the item actor, just as if you were dragging the item actor from the content browser.
- Right clicking the asset icon will open the item asset just as if you were to double click it in the content browser.
- The top section of the library allows you to refresh the library, in case you create new assets while the editor is open. You can also scale the icons up with the slider. The final button will save the currently selected item asset.