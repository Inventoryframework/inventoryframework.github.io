---
tags: [item]
---

# Traits and Components

## Item Trait

**Item Traits** are pure data-only UObjects associated with item data assets. They allow you to add variables and functions to an item data asset without modifying the parent data asset.

- **Retrieving Data**: Use `GetTraitsByTag` or `GetTraitsByClass` functions. The class version automatically casts to the correct blueprint, while the tag version requires manual casting.
  
- **Use Cases**: Traits are ideal for adding variables or functions that don't fit into the item category, such as quest data, without needing to add that data to the item asset

- **Function Limitations**: Traits can run functions but cannot use delays, timers, async tasks, or RPCs. Avoid writing data to trait variables as traits are shared among items with the same data asset. Unique trait instances are not created per item.

- **Important Note**: Prefer soft references over hard references to avoid creating a complex web of hard references, which can lead to excessive loading of game assets. Item data bases are one of the biggest culprits to create a large web of hard references in almost every project.

---

## Item Component

**Item Components** are actor components that attach to the owner of the inventory component that drive gameplay logic related to items, such as equipping, consuming, or creating widgets. They can be assigned through any trait derived from `IT_ItemComponentTrait.h`. An ItemComponent relies on a trait, but a trait does not rely on an item component. It's a one-way dependency.

- **Requirements**: Each Item Component needs an associated trait to function, but not all traits require an Item Component.

- **Role**: Item Components act as proxies between the inventory system and other systems, allowing flexibility without affecting core inventory functionality. They operate like any other actor component, supporting delays, timers, RPCs, etc.

- **Replication**: 
  - **Server**: Exists only on the server.
  - **Client**: Exists only on the client.
  - **Both**: Created on the server and replicated to the client (typically used).
   
- **Client Requests**: Clients can request the creation of an Item Component with a specific tag to associate with a function. Note that if creating a "Both" component, the function may not return immediately due to RPC delays.

- **Paylod**: When a component is created, the client can pass in a payload that gets sent to the server. This struct is not used by the plugin, so it is safe to modify for your projects needs.

## Why are they separated?
1. Due to the type of logic that components could handle, such as casting, handling abilities, VFX and so forth, you would want it to be soft-referenced. But you can't have it be instanced (as in, able to edit its properties in the item asset) and be soft-referenced. So we allow the trait to be hard-referenced, and the component to be soft-referenced
2. Due to the simplicity of traits, they take up very little memory. Like what was talked about in point 1, we can't soft-reference traits. Actor components have a lot of irrelevant data that doesn't need to be loaded when the item asset is loaded.
3. In some cases, you want to run a function without loading the component class or creating an instance of the component. For example, the item ability trait has a function to "loosely" check if the item ability would even activate successfully after being created. In this case, you can save performance by checking this type of logic inside of the trait, rather than the component.

---
## Tips and tricks

![](/pictures/BadObjectClassNames.png)
By default, your traits will display their default class name in the drop down menu.


![](/pictures/GoodObjectClassNames.png)
You can change the name they are given inside this drop down menu by following the next steps.

-![](/pictures/ClassNameExample.png)
By going into your Class Settings (1) you can change the class name (2). The drop down menu will now display whatever text you put in here.