# Item Sockets

`ItemSockets` is a system that allows you to assign a socket to an item, similar to a static or skeletal mesh. This socket contains a name, a local position, a tag, and a rotation. Then using helper functions to get the tile index of that socket (<span style="color:brown">**GetTileForSocket**</span>), with another function allowing you to provide an offset to the sockets tile (<span style="color:brown">**GetTileForSocketWithOffset**</span>).

The `ItemSocket` system can be enabled for items by opening the item asset and adding the `Socket Manager` trait.

It is recommended to open the `ItemEditor` tool as that will provide you with a editor utility widget to visually assign your sockets.