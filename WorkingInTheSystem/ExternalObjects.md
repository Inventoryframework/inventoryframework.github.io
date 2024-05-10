# External Objects

Both the item struct and container struct has an array of <span style="color:slateblue">**ExternalObjects**</span>. There are helper functions for both adding and removing widgets from this array.

What this array allows you to do is have other widgets receive the same update events from the <span style="color:violet">**I_ExternalObjects**</span> interface that the item widget and container widget receive without using delegates, which listen to ALL events, where as these events are item and container specific. This can be used for hotbars, ammo counters, quest trackers and more where a specific item needs to be monitored. The example project uses the tooltip widget as an external object, where it is listening to the override settings getting updated so it can update in real time alongside the item widget.

For your external widgets to receive these updates, you'll want to go to your class settings (1) and implement the <span style="color:violet">**I_ExternalObjects**</span> interface (2). You'll then see a <span style="color:green">**IFP|External Objects**</span> category with all the update events. Some of these are limited to just items or containers and some are sent to both.

![](/pictures/ExternalWidgetInterfaceExample.png)

The <span style="color:violet">**I_ExternalObjects**</span> interface is designed to be extended for your needs.

Utilizing external widgets can replace delegate events from the component, but there are still moments where delegates are better, for example a quest tracker keeping track of how much of a specific item data asset you have in your inventory.

## Adding custom events
The good news is, you don't even need to modify the <span style="color:violet">**I_ExternalObjects**</span> interface class. Just simply make some interface, create your function and use the <span style="color:violet">**GetObjectsForItemBroadcast**</span> to get all the objects that want to listen to the event

![](/pictures/ExternalObjectsInterfaceExample1.png)