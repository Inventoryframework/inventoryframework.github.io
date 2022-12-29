# How does the networking work?

---
## Listeners

Certain inventory data can become quite large, especially with how many containers and items you allow in an inventory. To optimize, we try to simplify things. Players only receive data from components that they request data from and data from their own components, but also any update from components they are currently interacting with.

The component keeps track of the players that are currently interacting with a component with the <span style="color:slateblue">**Listeners**</span> array, then whenever PlayerA moves, adds, removes or interacts with an item in some way, the component sends RPC calls to the other "listeners" so their widgets and data get updated.

For functions that modifies a components data, such as moving an item or modifying an items stack count or modifying any other data, you must remember that clients only have authority over their own component, they do not have authority over other components data. This is simply how networking in Unreal Engine works.
Because of this, most functions require you to call the function on the component the client has authority over. Even though the item you are trying to modify is not on that component.
This is why, when you look at some of the source code, the functions are getting the component the item belongs to through its <span style="color:slateblue">**UniqueID**</span>.<span style="color:slateblue">**ParentComponent**</span> and modifying that component's data through the component the client has authority over.

To simplify the code, any function that attempts to automate the replication for you stores its logic in a function with the same name but with a **Internal_** prefix.

For most functions that modify data in some way, only a single version of that function is available for you to call, and that function handles the replication for you. This is to simplify the amount of functions exposed as it can get very daunting to try and handle every replication scenario and making sure your calling the right function. If you wish to modify how the replication is handled, you can always go into the functions, modify them, or enable the server/client/internal functions to be BlueprintCallable.

```mermaid
%%{init: {'theme':'forest'}}%%
graph TD
    A[Function called] --> B(Evaluate if function should be replicated)
    B -->|Is Single player| C[Call internal function]
    B -->|Is Multiplayer| D[Send server RPC]
    D --> E[Call internal function on server, gather list of listeners]
    E --> F[Send Client RPC's to listeners]
    F --> G[Call internal Function on client]
```

==- Code example


```C++
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
```
===

---
