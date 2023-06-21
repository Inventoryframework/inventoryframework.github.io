# External Widgets

!!!Important
This is a 1.2 feature, which will be sent for Epic's review process immediately after 1.1.1 is accepted.
!!!

Both the item struct and container struct has an array of <span style="color:slateblue">**ExternalWidgets**</span>. There are helper functions for both adding and removing widgets from this array.

What this array allows you to do is have other widgets receive the same update events from the <span style="color:violet">**I_WidgetUpdates**</span> interface that the item widget and container widget receive. For example, the name getting updated. This can be used for hotbars, quest trackers and more. The example project uses the tooltip widget as an external widget, where it is listening to the override settings getting updated so it can update in real time alongside the item widget.

For your external widgets to receive these updates, you'll want to go to your class settings (1) and implement the <span style="color:violet">**I_WidgetUpdates**</span> interface (2). You'll then see a <span style="color:green">**IFP**</span> category with all the update events. Some of these are limited to just items or containers and some are sent to both.

![](/pictures/ExternalWidgetInterfaceExample.png)

The <span style="color:violet">**I_WidgetUpdates**</span> interface is designed to be extended for your needs.

Utilizing external widgets can replace delegate events from the component, but there are still moments where delegates are better, for example a quest tracker keeping track of how much of a specific item data asset you have in your inventory.