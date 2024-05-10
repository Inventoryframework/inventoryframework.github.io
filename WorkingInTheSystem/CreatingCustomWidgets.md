# Creating custom widgets

---
## Containers, Tiles and Items
These three widgets are the ones you'll most likely end up working with the most. To improve the hierarchy, I've tried to not add any components that aren't necessary. Those that are essential for the widgets to function, I've added Getter functions that request the essential widget. This way you're design hierarchy can be in whatever design you want and still use the parent widgets. But this does mean you need to override those functions and passs the widget through as a variable.

All three are abstract, so you must either use the widgets in the demo folder or make your own children.

!!!Important
Technically, these requirements can be ignored, depending on what features you need and don't need, but you might get some errors. For example; size boxes aren't really required as you can set the sizing on the image, but it requires a bit more work to setup correctly.
!!!

### Containers
- Border is needed for scrolling support and sizing (<span style="color:brown">**GetBorder**</span>)
- ContainerOverlay holds the item widgets (<span style="color:brown">**GetOverlay**</span>)
- Grid panel holds the tiles (<span style="color:brown">**GetGridPanel**</span>)
- (Not mandatory, but is recommended) - Scroll box is needed for scrolling (<span style="color:brown">**GetScrollBox**</span>)

### Tiles
- (2.6 and below) Size Box to control the size (<span style="color:brown">**GetSizeBox**</span>)
- (2.7 and above) There are no requirements in the tiles hierarchy. You don't even need an image! To control the sizing of the widget, a function called <span style="color:brown">**SetWidgetSize**</span> should be overriden and handle the sizing logic from there.

### Inventory Item
- size box to control the size (<span style="color:brown">**GetSizeBox**</span>)
- Image for the item icon (<span style="color:brown">**GetImage**</span>)
- (Not mandatory, but is recommended) - Loading Throbber for indicating the item is loading (<span style="color:brown">**GetLoadingThrobber**</span>)
This widget is updated through the <span style="color:violet">**I_WidgetUpdates**</span> interface. You can see an example of how these updates are used inside <span style="color:violet">**WBP_DemoInventoryItem**</span>.

The hierarchy inside your custom widget can be structured in whatever way you want, but they must contain the above mentioned widgets inside the hierarchy.
Nearly every function inside these widgets are designed to be overriden for maximum customization.

It is advised to read the [External widgets](https://inventoryframework.github.io/workinginthesystem/externalwidgets/) page to understand the <span style="color:violet">**I_WidgetUpdates**</span> interface.