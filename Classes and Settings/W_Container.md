# W_Container

==-Widget Container
File Location: Source\InventoryFrameworkPlugin\Public\Core\Widgets\W_Container.h
File Location: Source\InventoryFrameworkPlugin\private\Core\Widgets\W_Container.cpp
==- Blueprint Child: WBP_Container
File Location: Content\Core\Widgets\WBP_Container.uasset
==-

---

This widget is responsible for holding both the tile widgets and item widgets and also acts as a proxy for the component to access any tile widgets or item widgets.
The component (<span style="color:violet">AC_Inventory</span>) should not rely on these widgets at all for any data for its functions, like I mentioned at the start of this documentation.

Even though there are <span style="color:slateblue">ContainerSettings</span> inside of these, they are immediately overridden by the component when created. They are mainly used as a fallback default, used for previewing how the container will look or used to bind the widgets to a specific container in a component. I recommend looking at the comment on <span style="color:brown">GetContainerSettings</span>.

To reduce the work for everyone when modifying containers, <span style="color:violet">W_Container</span> has a function (<span style="color:brown">GetContainerSettings</span>) to get the <span style="color:slateblue">ContainerSettings</span> straight from the component it belongs to. This way you do not have to update the <span style="color:slateblue">ContainerSettings</span> for the widget  every time the container is modified in any way and also allows designers to make children of <span style="color:violet">W_Container</span> and still have access to the same data as other children.

<span style="color:slateblue">TileSize</span> refers to how large you want your tile widgets to be. Ideally both dimensions should be the same to minimize stretching. If you decide to not have the dimensions 1:1, your item icons will have to accommodate for your new icon ratio.
Yclamp refers to when you want the scroll bar to appear. 
This should be (Your Tile Y Size) * (How many Y tiles you want until the scroll bar appears)
So if I want mine to appear after 8 tiles while my container is 10 tiles tall and my tile Y size is 60, I would do 8*60.

<span style="color:slateblue">TileClass</span> and <span style="color:slateblue">ItemClass</span> are the default classes used to create the widgets for each instance of a container. It is recommended to try and keep these the same where ever your containers are to try and maintain consistent visuals through out your game.

<span style="color:violet">DA_CoreItem</span> has a override map, where if the current containers class if found in that map, it'll then use the item widget in that map instead of the assigned <span style="color:slateblue">ItemClass</span>. This could be useful for when a special item needs it own item widget.