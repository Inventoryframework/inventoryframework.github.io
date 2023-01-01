# Creating custom widgets

---
## Containers, Tiles and Items
These three will be the widgets you'll most likely end up working with the most. To improve the hierarchy, I've tried to not add any components that aren't necessary. Those that are essential for the widgets to function, I've added Getter functions that request the essential widget. This way you're design hierarchy can be in whatever design you want and still use the parent widgets. But this does mean you need to override those functions and passs the widget through as a variable.

All three are abstract, so you must either use the widgets in the demo folder or make your own children.

### Containers
- Border is needed for scrolling support and sizing (<span style="color:brown">**GetBorder**</span>)
- ContainerOverlay holds the item widgets (<span style="color:brown">**GetOverlay**</span>)
- Grid panel holds the tiles (<span style="color:brown">**GetGridPanel**</span>)
- (Not mandatory, but is recommended) - Scroll box is needed for scrolling (<span style="color:brown">**GetScrollBox**</span>)

### Tiles
- Size Box to control the size (<span style="color:brown">**GetSizeBox**</span>)

### Inventory Item
- size box to control the size (<span style="color:brown">**GetSizeBox**</span>)
- Image for the item icon (<span style="color:brown">**GetImage**</span>)

The hierarchy inside your custom widget can be structured in whatever way you want, but they must contain the above mentioned widgets inside the hierarchy.