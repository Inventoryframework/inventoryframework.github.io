# Tag system

<a href="https://docs.unrealengine.com/4.26/en-US/ProgrammingAndScripting/Tags/" target="_blank">**Gameplay tags**</a> are extremely powerful and this system attempts to allow itself to expand to various needs using not just gameplay tags, but also a value associated with tags.

There are two systems, a regular gameplay tag and the second being a tag value system.
The component, containers and items can have both. These tags can be updated, removed and added during runtime and are fully replicated.

For example; The system does not by default have a durability system. But with the tag value system, you can add a tag Item.Stat.Durability and give it a value, and voila! You now have a durability system! (Of course you have to implement the background logic that would reduce or increase this value).

The tag system is fairly simple, containers could use them as labels, for example Container.Type.Fridge and now any food items could have a tag value such as Item.Food.Quality which would slowly go down over time, but while in a container with a Container.Type.Fridge tag, it would decay much slower.

There are helper functions for resolving the value of specific tags, finding items or containers with specific tags and more.

Since these tags and values live on the container and item struct, it is very simple to serialize for your save file.

---

The add, remove and set functions have the option to "IgnoreNetworkQueue", if this is check to true, the item will not be added or removed from the network queue. This can be useful for moments where you are fine with server and client data not being in sync for a short moment. If you check this to true, you should NOT rely on that tag for any data validation.

---
## Tag Value calculation
The Setter functions have a class parameter, which allows you to inject your own calculations before the final value is set. This is a prime location for clamping certain values, such a durability value. You might have a CurrentDurability tag value and a MaxDurability tag value, then whenever you modify CurrentDurability, you can pass in a tag value calculation class that will handle the clamping for you.