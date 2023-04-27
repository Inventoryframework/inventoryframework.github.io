---
tags: [inventorycomponent]
order: 50
---

# Adding the component and containers

---
1. There are two components, one is the C++ parent and the other is the blueprint child of the C++ component. There is nothing stopping you from making your own BP component with the C++ component as its parent but I still recommend using the Blueprint component. You can also make a blueprint child of the blueprint component, which is the recommended path if you are going to override any functions.
You want to add your component to your actor, if you're making an item, I suggest using <span style="color:violet">**BP_SM_PhysicalItem**</span>  as its parent and the component will automatically get added. Do note though that the item Data Asset only accepts children of <span style="color:violet">**BP_SM_PhysicalItem**</span>. **[Go here](https://inventoryframework.github.io/classes-and-settings/da_coreitem/#changing-the-physical-actor-class)** if you want to change this behavior

2. Inside the component you'll find an array called <span style="color:slateblue">**ContainerSettings**</span>. These are your settings and where you'll add any items to the container. It is most common to give them a unique name, which you'll later use in the next step to assign them to a container widget.
You will also need to set what kind of type this container is inside <span style="color:slateblue">**InventoryType**</span> and give the component a widget inside <span style="color:slateblue">**WidgetClass**</span> which it'll use to display its containers. Typically for player characters, this is the inventory screen.
The order of these containers are important. There are many functions that try and find the first available container, which scans the containers from index 0 to the last index.

3. Now you'll need to bind your containers to a widget to represent the container. This is optional, there are cases where you might not want a container to have a widget representation. Though the player will not have a widget to interact with the container through.
Go into your widget and add a child of <span style="color:violet">**WBP_Container**</span> to your layout. The plugin comes with a widget called <span style="color:violet">**WBP_DemoContainer**</span> which has all the visuals set up for you. Then go into your graph and implement the <span style="color:violet">**I_InventoryWidgets**</span> interface. Find the <span style="color:brown">**GetContainers**</span> interface function and make an array and add your containers.
Somewhere throughout your logic you will need to call <span style="color:brown">**BindContainerWithWidget**</span>, you can see an example on how I call this function inside <span style="color:violet">**BP_Interactable**</span> or <span style="color:violet">**BP_PlayerCharacter**</span>.

4. The actor must implement the <span style="color:violet">**I_Inventory**</span> interface and override the <span style="color:brown">**GetInventoryComponent**</span> function and pass in a reference to the inventory component.