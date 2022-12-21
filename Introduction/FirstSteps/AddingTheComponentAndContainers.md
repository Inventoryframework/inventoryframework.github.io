---
tags: [underrevision, inventorycomponent]
order: 50
---

# Adding the component and containers

!!!danger
Under revision
!!!

1. There are two components, one is the C++ parent and the other is the blueprint child of the C++ component. There is nothing stopping you from making your own BP component with the C++ component as its parent but I still recommend using the Blueprint component. You can also make a blueprint child of the blueprint component, which is the recommended path if you are going to override any functions.
You want to add it to your actor, if you're making an item, I suggest using BP_SM_PhysicalItem as its parent and the component will automatically get added.

2. Bind the containers to your widget.
Go into your widget and add WBP_Container or any children of that widget to your layout. Then go into your graph and implement the I_Inventory interface. Find the GetContainers interface function and make an array and add your containers. This order is important as the component often searches for things beginning at index 0, so your most used containers should go first to optimize speed.