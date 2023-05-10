---
tags: [Tools]
---
# Item Editor
The item editor is a simple, but powerful tool that allows you to select any item the asset manager can detect and directly edit the asset without opening a new window.

This tool is also meant to provide a simple interface for designers to create tools to help with their workflow. There are two tabs, the library and the editor. The library simply is where you can find all your assets and filter them. The second tab is your asset editor. At the bottom you'll find a toolbox. By default there aren't many tools in here, but it is meant for you to expand for your games needs.

## Tool box
With the asset come two tools in this box, the icon editor and shape editor.

1. The icon editor allows you to edit the inspect and generated item icon settings, and allowing you to see how the item will look like in game with whatever item widget you have selected. You might want to go into WBP_ItemWidgetPreview and setting the default widget class to a class you commonly use.
2. The shape editor allows you to more simply and efficiently edit the shape of the item. All items are grids of squares, and to shape the item you have to add to an array what squares are collapsed. Editing a 2D grid in a 1D array list isn't the best experience, so the shape editor simplifies it and gives you a more pleasant UI to work in.