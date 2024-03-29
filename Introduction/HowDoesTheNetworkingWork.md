# How does the networking work?

---
## Listeners

Certain inventory data can become quite large, especially with how many containers and items you allow in an inventory. To optimize, we try to simplify things. Players only receive data from components that they request data from and data from their own components, but also any update from components they are currently interacting with.

The component keeps track of the players that are currently interacting with a component with the <span style="color:slateblue">**Listeners**</span> array, then whenever PlayerA moves, adds, removes or interacts with an item in some way, the component sends RPC calls to the other "listeners" so their widgets and data get updated.

For functions that modifies a components data, such as moving an item or modifying an items stack count or modifying any other data, you must remember that clients only have authority over their own component, they do not have authority over other components and thus can't call functions on other components. This is simply how networking in Unreal Engine works.
Because of this, most functions require you to call the function on the component the client has authority over. Even though the item you are trying to modify is not on that component.
This is why, when you look at some of the source code, the functions are getting the component the item belongs to through its <span style="color:slateblue">**UniqueID**</span>.<span style="color:slateblue">**ParentComponent**</span> and modifying that component's data through the component the client has authority over.

To simplify the code, any function that attempts to automate the replication for you stores its logic in a function with the same name but with a **Internal_** prefix.

For most functions that modify data in some way, only a single version of that function is available for you to call, and that function handles the replication for you. This is to simplify the amount of functions exposed as it can get very daunting to try and handle every replication scenario and making sure your calling the right function. If you wish to modify how the replication is handled, you can always go into the functions, modify them, or enable the server/client/internal functions to be BlueprintCallable (Example would be <span style="color:violet">**AC_Inventory**</span> -> <span style="color:brown">**Internal_AdjustContainerSize**</span>).

```mermaid
graph TD
    A[Function called] --> B(Evaluate if function should be replicated)
    B -->|Is Single player| C[Call internal function]
    B -->|Is Multiplayer| D[Send server RPC]
    B -->|Is Multiplayer| H[Add item to network queue]
    D --> E[Call internal function on server, gather list of listeners]
    E --> F[Send Client RPC's to listeners]
    F --> G[Call internal Function on client]
    G --> R[Remove item from network queue]
```

==- Code example


```C++
//Increase the count of an item by @Count
void IncreaseItemCount(FS_InventoryItem Item, int32 Count, int32& NewCount)
{
    if(UKismetSystemLibrary::IsStandalone(this))
	{
        //Single player, don't bother with any replication stuff
		InternalIncreaseItemCount(Item.UniqueID, Count, NewCount);
		return;
	}

    //Update server data
	S_IncreaseItemCount(Item.UniqueID, Count);

    //Predict the result if item is stackable.
	if(Item.Item->CanItemStack() == true)
	{
		NewCount = FMath::Clamp(Item.Count + Count, 1, Item.Item->MaxStack);
	}
	else
	{
		NewCount = Item.Count;
	}
    
    //Notify the player the item is currently busy
	C_AddItemToNetworkQueue(Item.UniqueID);
}

//Increase the item count on the server
void S_IncreaseItemCount(FS_UniqueID ItemID, int32 Count)
{
    int32 NewStackCount
    if(GetOwner()->GetRemoteRole() == ROLE_SimulatedProxy) //This is dedicated or listen server. Don't bother with client RPC
	{
		InternalIncreaseItemCount(ItemID, Count, NewStackCount);
	}
	else //Client is calling, update server, then call client RPC
	{
		InternalIncreaseItemCount(ItemID, Count, NewStackCount);
		C_IncreaseItemCount(ItemID, Count);
	}
	
	for(const auto& CurrentListener : ItemID.ParentComponent->Listeners)
	{
		//Update all clients that are currently listening to this component's replication calls.
		if(CurrentListener->GetOwner()->GetRemoteRole() == ROLE_AutonomousProxy && CurrentListener != this)
		{
			CurrentListener->C_IncreaseItemCount(ItemID, Count);
		}
	}
}

//Increase the item count for listening clients
void C_IncreaseItemCount_Implementation(FS_UniqueID ItemID, int32 Count)
{
    //Find the item we are processing
	bool ItemFound;
	FS_InventoryItem Item;
	ItemID.ParentComponent->GetItemByUniqueID(ItemID, ItemFound, Item);
	if(ItemFound)
	{
		int32 NewStackCount;
		InternalIncreaseItemCount(ItemID, Count, NewStackCount);
        //Item has been processed, remove from network queue
		C_RemoveItemFromNetworkQueue(Item.UniqueID);
	}
}

void UAC_Inventory::InternalIncreaseItemCount(FS_UniqueID ItemID, int32 Count, int32& NewCount)
{
    //Start moodifying the item in here.
    //This was called on both client and server, so the same logic is ran for everyone.
}
```
===

### Why not just use a multicast or RepNotify?

Because of how large some RPC's can get, I did not find multicasts to be acceptable for most of the functions. A lot of the RPC's are just simply not relevant for most clients anyways, and managing relevancy is a lot more complicated than a simple array of actors.

RepNotify's also can not be used because there is no way to ensure their order. When you label a RPC as "Reliable", Unreal will make sure all reliable RPC's get replicated in the order you called them. Variables will always get replicated, even with packet loss, so they are in a way "reliable", but not truly reliable. Variables will replicate whenever they get an opportunity to, and they have a lower priority than RPC's.

---
## Unique ID

The UniqueID is a simple method to quickly find containers or items, or ensuring containers or items are valid, and for linking some object to an item (Such as the [item driver's](https://inventoryframework.github.io/classes-and-settings/o_itemobjectandac_itemdriver/)). This is covered in multiple sections in the documentation, but here are some things to consider for networking.

- Clients and servers must stay in sync when it comes to UniqueID's. This leaves you with two options;

1. Pre-generate the <span style="color:slateblue">**UniqueID**</span>'s on the server, then pass them to the client and have your logic assign that <span style="color:slateblue">**UniqueID**</span> to the item. This is not advised as it can get bad for networking and it is generally cumbersome to program and manage it.
2. Use <span style="color:brown">**GenerateUniqueIDWithSeed**</span>. Now the only thing you need to ensure is that the client and server are using the same seed and they'll both generate the same <span style="color:slateblue">**UniqueID**</span>.

- <span style="color:slateblue">**UniqueID**</span>'s [scale extremely](https://inventoryframework.github.io/workinginthesystem/creatingcustomfunctions/#network-optimizations) well with RPC's. It is advised to use <span style="color:slateblue">**UniqueID**</span>'s as often as possible.
- There's no way to evaluate from a <span style="color:slateblue">**UniqueID**</span> if it was assigned to an item or container without going through all items and containers until you find a match. It is not recommended to add more information to this struct as it's meant to be small and simple, so that it gets replicated extremely quickly and cheaply.
- <span style="color:slateblue">**UniqueID**</span>'s from components the client does not have information on will not work. For example, two players are interacting with a chest and Player-1 moves an item from their inventory to the chest, Player-2 will not have the information necessary to move the item on their side, since they do not have the information needed from Player-1's inventory component. Some functions simply need more information than the <span style="color:slateblue">**UniqueID**</span> can provide.