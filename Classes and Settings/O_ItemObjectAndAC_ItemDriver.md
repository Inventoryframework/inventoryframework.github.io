---
tags: [item]
---

# O_ItemObject and AC_ItemDriver
==- Object Item
File Location: Source\InventoryFrameworkPlugin\Public\Core\Objects\Parents\O_ItemObject.h
File Location: Source\InventoryFrameworkPlugin\Private\Core\Objects\Parents\O_ItemObject.cpp
==- Blueprint Child: BP_O_ItemObject
File Location: Content\Core\ActorParents\BP_O_ItemObject.uasset
==-

==- Actor Component Item Driver
File Location: Source\InventoryFrameworkPlugin\Public\Core\Components\AC_ItemDriver.h
File Location: Source\InventoryFrameworkPlugin\Private\Core\Components\AC_ItemDriver.cpp
==- Blueprint Child: BP_AC_ItemDriver
File Location: Content\Core\ActorParents\BP_AC_ItemDriver.uasset
==-

---
Ever wanted to tie gameplay logic to your items without cluterring up your character and AI blueprints? If so, then this is the place for you!

These are instanced <span style="color:violet">**UObjects**</span> which allow you to edit variables inside the item data asset and actor components that run gameplay logic, using the <span style="color:violet">**UObjects**</span> data.

Instanced <span style="color:violet">**UObjects**</span> are NOT intended to run much, if any logic, mainly it is meant to store data. But you can still have functions inside them, just like how you can with data assets, but it is important for you to know that these functions should not have delays, timers, async tasks or RPC’s. Do not write any data to these <span style="color:violet">**UObjects**</span>. The engine has had a notorious bug for a while where any data written to an instanced <span style="color:violet">**UObjects**</span> can sometimes modify the class itself on-disk.

For those types of logic, the <span style="color:violet">**ItemDriver**</span> is used. These are simple actor components that can use the data from the <span style="color:violet">**UObject**</span> to run its logic. For example, you can have an <span style="color:violet">**ItemObject**</span> and <span style="color:violet">**ItemDriver**</span> designed to consume an item. The <span style="color:violet">**ItemObject**</span> holds the data for how much health it restores and what animation to play, the <span style="color:violet">**ItemDriver**</span> is then responsible for driving that logic. It can then be re-used as often as you want in item data assets and change those variables without having to create a new <span style="color:violet">**ItemObject**</span> or <span style="color:violet">**ItemDriver**</span>. The data inside of the <span style="color:violet">**ItemDriver**</span> is meant to be written, and can be used just like any other actor component.

These components can also be viewed as a proxy between the inventory system and any other system. These <span style="color:violet">**ItemObject**</span> and <span style="color:violet">**ItemDrivers**</span> have been designed to be completely independent from the system and extremely abstract, so you can modify them however you want without it affecting how the inventory works. You could even completely remove this system and the inventory will still function.

<span style="color:violet">**ItemDrivers**</span> have three replication methods; <span style="color:slateblue">**Server**</span>, <span style="color:slateblue">**Client**</span> and <span style="color:slateblue">**Both**</span>. Server <span style="color:violet">**ItemDriver**</span>'s only exists on the server, client <span style="color:violet">**ItemDriver**</span>'s only exists on the client, and <span style="color:slateblue">**Both**</span> are created on the server and replicated to the client. You’ll most likely be using <span style="color:slateblue">**Both**</span> most of the time. 
Clients are able to request the creation of an <span style="color:violet">**ItemDriver**</span> and pass in a tag, which you can associate with a function. This prevents those pesky situations where you are working on client code, for example a widget, and are trying to activate a function inside an item driver. 
The function <span style="color:brown">**GetItemDriver**</span> will create the driver for you, but if you are on the client and trying to create an <span style="color:violet">**ItemDriver**</span> set to <span style="color:slateblue">**Both**</span>, the function won’t be able to return it, because it has to send an RPC to the server which is not instant. 

This function allows you to pass in a <span style="color:slateblue">**tag**</span> which will trigger any function you have associated with the tag once the <span style="color:violet">**ItemDriver**</span> is replicated. Look at the code comment for <span style="color:violet">**AC_ItemDriver.h**</span> -> <span style="color:brown">**ActivateEvent**</span>.
If you are unfamiliar with GameplayTags, I highly recommend you start using them as they are extremely useful.

There is an annoying issue though, all variables that can be modified in blueprint will show up as editable variables in the instanced <span style="color:violet">**ItemObject**</span> even if they aren’t set as Instance Editable (This might be a bug, if anyone knows a fix, please tell me). In the meantime, I’ve made it so any variables inside a <span style="color:green">**DoNotShow**</span> category or with no category at all will not show up here. You can change the behavior inside <span style="color:violet">**O_ItemObject.h**</span> in the UCLASS macro. Remember you can create sub categories like so; DoNotShow|MyNewCategory. This should help keep all variables that are meant to be hidden organized.
You can of course inverse this behavior in C++ by using the ShowCategory UCLASS specifier. 

==- Important notes
- Whenever possible, use soft references and try to avoid hard references. Item data bases are one of the fastest ways of creating a nasty web of hard references and before you know it, the majority of your game is loaded at times where you don't want it to be.
==-
