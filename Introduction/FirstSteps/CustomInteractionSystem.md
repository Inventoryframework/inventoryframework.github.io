---
order: 10
---

# Custom Interaction system

---
The plugin does come with an extremely basic interaction system, which is designed to be replaced. This page covers how to replace it.

 - There are two functions, <span style="color:brown">**StartInteract**</span> and <span style="color:brown">**FinishInteract**</span>. The first will send an RPC to the server, then the server will send the client any data around the component the client needs, then trigger <span style="color:violet">**AC_Inventory**</span> -> <span style="color:brown">**OnDataReceivedFromOtherComponent**</span>, which will call FinishInteract.
 This is handled in this way because allowing players to interact with the actors component before replication is finished can lead to a lot of bugs. Not because there are issues in the plugin, but because of the nature of clients modifying an actor that isn't in sync. Virtually every multiplayer game handles it in this way unless they have client prediction and network rollback.
 This is the only "gotcha" that needs to be kept in mind. In general, you should already not be allowing players to interact with actors that are waiting on an RPC.


With the above point in mind, you can start replacing all the functions with your own. The only ones that other systems rely on are <span style="color:brown">**GetAnimationData**</span> and <span style="color:brown">**UpdatePreview**</span>.

---
## Color Sheet:
<span style="color:brown">**functions**</span> - <span style="color:slateblue">**Variable**</span> - <span style="color:green">**category**</span> - <span style="color:violet">**class**</span>
