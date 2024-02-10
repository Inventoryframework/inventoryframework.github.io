# Adding new items during runtime

---
## Single player
Adding new items during runtime for single player is no problem and has no restrictions. Just call the function <span style="color:violet">**AC_Inventory**</span> -> <span style="color:brown">**TryAddNewItem**</span>.

---
## Multiplayer
For multiplayer though, I've had to wrap this function with a very strict safe guard. Clients are under no circumstance allowed to call this function. If they do find a way, they will get kicked. There is simply no circumstance where the client should even have the slightest chance of gaining access to this function as they could potentially feed it bad data and grant themselves any item within your data base.

Only the listen server or dedicated server is allowed to use this function.

This means if you have a quest system or anything of the sort where the player is granted an item, you will have two options:
1. Pre-store the item in a component and call the <span style="color:brown">**MoveItem**</span> function, but this will permanently remove the item from the old component. Though you could always reset the data after the function is called.
2. The system which is calling <span style="color:brown">**TryAddNewItem**</span> is already owned by the server, and thus is allowed to call the function.

---
## Example
Here is an example of adding a backpack to another component with another backpack inside of it, and that backpack having items inside of it.
If your item has no containers, just make an empty array for the <span style="color:slateblue">**ItemsContainers**</span>
<iframe src="https://blueprintue.com/render/vey0u3ar/"scrolling="no" allowfullscreen width="100%" height="700"></iframe>