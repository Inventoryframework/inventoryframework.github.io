---
order: 20
---

# Setting up vendors

---
To have a inventory component be set to vendor, you will want to go into the component settings and setting <span style="color:slateblue">**InventoryType**</span> to <span style="color:slateblue">**Vendor**</span>.

Right now, the system only considers items that are children of </span> - <span style="color:violet">**IDA_Currency.h**</span> as viable currencies that can be exchanged for a vendor item.
Prices are calculated inside <span style="color:violet">**FL_InventoryFramework.h**</span> -> <span style="color:brown">**GetItemPrice**</span>, and the function that evaluates if a player can afford an item is <span style="color:violet">**FL_InventoryFramework.h**</span> -> <span style="color:brown">**CanAffordItem**</span>