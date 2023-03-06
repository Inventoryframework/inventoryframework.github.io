---
order: 20
---

# Setting up vendors

---
To have a inventory component be set to vendor, you will want to go into the component settings and setting <span style="color:slateblue">**InventoryType**</span> to <span style="color:slateblue">**Vendor**</span>.

Right now, the system only considers items that are children of </span> - <span style="color:violet">**IDA_Currency.h**</span> as viable currencies that can be exchanged for a vendor item.
Prices are calculated inside <span style="color:violet">**FL_InventoryFramework.h**</span> -> <span style="color:brown">**GetItemPrice**</span>, and the function that evaluates if a player can afford an item is <span style="color:violet">**FL_InventoryFramework.h**</span> -> <span style="color:brown">**CanAffordItem**</span>

The system by default has an array of currencies which players can use to buy items, but it does not by default provide a way of allowing the player to select which currency to use to buy or sell an item. It simply uses the first available currency it can find. It's up to you to implement that sort of logic.

Most games tend to have vendors keep track of how much money the vendor has, then when you sell an item to the vendor they spend their own currency and grant it to the player. But this currency should stay hidden and a simple number is displayed. The demo has a showcase for how to handle this, but generally what is done in most games is that the vendor either has a simple replicated value that gets updated whenever a vendor sells or buys an item, or in some cases it's an attribute.

Exchanging currencies can get complicated, but worst of all if your currencies have a max stack limit and the client is in the process of exchanging multiple stacks of currencies and a packet fails, drops, or the player disconnects, there's gonna be problems. It's much simpler and safer to have simple values that get decreased and increased, and if that succeeds, then move the item the seller is trying to give to the buyer.

There are many ways of handling vendor behaviour. Nearly every game handles it slightly differently, which is why the framework does not try to handle any of this logic for you at a C++ level.

Instead, there are three points in the system where you can inject your own custom logic and rules for handling vendor interactions.
1. The function <span style="color:violet">**AC_Inventory.h**</span> -> <span style="color:brown">**CanAffordItem**</span> will call a interface function <span style="color:violet">**I_Inventory.h**</span> -> <span style="color:brown">**MeetsCurrencyCheck**</span>, this simply asks the owner  of the component what they value the item as.
2. You can make a child of <span style="color:violet">**BP_AC_Inventory**</span> and override the <span style="color:brown">**CanAffordItem**</span> function to implement your own custom logic.
3. The buyer and seller will receive a delegate on both the server and client either after or before the transaction, the notify is handled inside of <span style="color:violet">**WBP_Tile**</span> -> <span style="color:brown">**PerformDrop**</span> -> MoveItem. This is where you would perform any currency exchange, and since the vendor belongs to the server, it's allowed to call <span style="color:brown">**TryAddNewItem**</span> if you want to give either the vendor or player currency after the transaction.