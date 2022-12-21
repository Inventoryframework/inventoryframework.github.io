---
tags: [inventorycomponent]
---

# How does the component work?

With how strict the performance requirements were for this plugin, I’ve had to design this inventory system slightly differently from other systems you might find either on the marketplace or Youtube tutorials.
The biggest thing to keep in mind is that this system does NOT rely on widgets being valid or created to keep track of its data.

This is to achieve four things:
1. This greatly speeds up widget construction since all the data they need (for example where an item is inside the container and so forth) is already prepped.
2. Allows you to remove widgets from RAM and still have the necessary data to find out important things like looting an item and finding a free space for that item.
3. Since widgets have way fewer references, there is less work to be done to manage widgets getting garbage collected properly.
4. Designers are able to create new widgets and prototype much faster since all the data they need about containers and items is always available from the component.

<span style="color:slateblue">ContainerSettings</span> is an array of all the settings your designers will use and some settings only the programmers will use, and with all the settings combined you can figure out everything about that container.
When creating a widget for a container, the widget uses a <span style="color:slateblue">UniqueID</span> to find out what container it is representing.
Items differentiate themselves from each other by using a <span style="color:slateblue">UniqueID</span> as items can have the same name, same data asset and so forth. A component can not give items or containers the same <span style="color:slateblue">UniqueID</span> twice. This is essential for functions to work properly.
The component is designed to live on individual actors, not the player controller.
