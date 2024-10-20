# Ability System Integration

Video tour: https://youtu.be/rvd6SbspA9E

An important note around `IFP`'s design is that it was NOT built with any specific ability system in mind, but rather have good extension points to allow for it to interact with any ability system and for any ability system to interact with `IFP`. This design philosophy has led to `IFP` being extremely easy to integrate with any ability system.

This page will mostly cover `GAS`, since that is the most popular ability used with `IFP`. But concepts can still be adapted to any type of ability system.

> In 2.10 a separate module including several classes to help with `GAS` integration is included in the plugin. This does mean that `IFP` has a dependency on `GAS`. To remove that dependency, remove the `IFP_GAS` module folder, then open `InventoryFrameworkPlugin.uplugin` and remove the `IFP_GAS` module from the modules list and the `GameplayAbilities` plugin from the plugins list.

---
## Connecting IFP with your ability system
The primary way `IFP` is meant to connect to your ability system is through the `Traits and Components` system that can be found in item assets, or by either modifying the item asset or creating specialized children who then contain references to classes, such as an ability class. Then through code, you'd fetch these references and either grant, remove or activate them. The other method would be through the various delegates that come with the inventory component, such as the ItemEquipped delegate is a good candidate to grant the player abilities.

Then the primary way of your ability system interacting with `IFP` is through some type of reference to a specific item or a group of items. For example, a gun firing ability could fetch what is currently equipped in a specific container and use data stored on that item to run its logic, such as fire rate. In this case, the ability could remain active for the entire lifetime of the actor and simply continue using data from equipped items to run its logic. This is one of the most performant and perhaps most scalable solution. This also means you are only creating one object to handle this logic rather than creating a new one for each item.
- This topic is covered more in <a href="https://inventoryframework.github.io/classes-and-settings/da_coreitem/#gas-abilities" target="_blank">**here**</a>

In some cases, it might be worthwhile adding direct references to either system into the other system, for example adding ability class references to the item asset or modifying abilities to reference and depend on an inventory item.

---
## Native GAS support
As mentioned above, 2.10 includes `GAS` support out of the box. This section covers how it works and how to customize it.

> Remember, this is just one way of implementing `GAS`. Many projects might have different requirements or different needs. Hence why I made the native `GAS` implementation simple to remove. This is just one way for `GAS` to be implemented with `IFP`

- <span style="color:violet">**IT_GameplayAbilityReferences**</span>, <span style="color:violet">**IT_GameplayEffectReferences**</span> and <span style="color:violet">**IT_AttributeSetReferences**</span> are item traits which stores class references and include events for granting and removing their respective system.
- The actor component `AC_GASHelper` will handle all items containing these data-only objects and handling the events that grant and remove them. You will want to add this component to the same actor that the inventory component is on.
    - This component has a simple "Tag Event" system. This is meant to help filter out when to grant and remove abilities and attributes for specific events. These events can be called from anywhere by simply broadcasting the `TagEventActivated` delegate with a tag event. The component will now automatically get all items for that event and attempt to either grant or remove abilities, attributes and effects.
    - To process an item, you need to call the <span style="color:brown">**ProcessItem**</span> function and pass in an event. This function will call 6 functions, trying to grant and remove any abilities, attributes and effects. If you want to, you can call each function individually or separately.
    - This component can have a blueprint child and most functions can be overriden to customize its behavior.
    - By default, this component handles 3 events (You can override the <span style="color:brown">**BindDefaultEvents**</span> function to increase the number of events the component automatically handles): 
        1. When <span style="color:violet">**AC_GASHelper**</span> has its <span style="color:brown">**StartHelperComponent**</span> function called. 
        2. When an item is equipped. 
        3. When an item is unequipped.

### Notes
- This system does not keep track of duplicate items. For example, the player can equip 2 rings sharing the same item asset. When they unequip Ring1, then it will try and remove anything the asset wants removed, even though Ring2 should still grant them the asset wants them to have. To get around this, I'd recommend overriding the <span style="color:brown">**BindDefaultDelegates**</span> and do not call the parent, so it stops handling the `ItemEquipped` and `ItemUnequipped` event and you would now either make your own delegate that tries to accomodate this, or improve the code behind these two delegates, or handle this through some other code somewhere else.
- For effects, it is recommended to use `Infinite` effects. If you use `Instant` or `HasDuration`, the effect might not be reversable. For example, modifying an attribute with `Instant` will just modify the attribute and when we try to remove the effect, nothing will happen. If it has `Infinite` duration, then the modification made to the attribute will be reversed when you remove the effect.

---
## Item Component Ability
`IFP` includes a "ItemAbility" `ItemComponent`, which is not related to `GAS` or any other ability system, but is designed to look and behave very similar to `GAS` abilities (This is the closest `IFP` has to an "ability system"). While you should continue to use `GAS` abilities whenever possible, the levels of networking optimizations that Epic have provided for `GAS` has created a few limitations to `GAS`, which includes:
1. `Instanced Per Execution` does not seem to have replication support anymore.
2. Changing ownership of an instanced ability does not seem to be supported.
3. Having a `GAS` ability treat an item as its owner requires a lot of work (as in, an ability that would use `Instanced Per Actor` would only ever have 1 instance created for the owner of the inventory component, but you might want it to be something like `Instanced Per Item`, so you could have an instance made for an item, but only 1 per item).

This ability component is designed to get around these limitations. So again, use `GAS` abilities whenever you can, and only use this component if the above limitations are preventing you from making your ability.

To activate an item ability, you can call <span style="color:brown">**TryActivateItemAbility**</span> or <span style="color:brown">**TryActivateEquippedItemAbility**</span>

The scope for this ability component is very small to try and reduce complexity and just provide the basic interface of an ability system. Item Components already have networking support, but you will have to implement any RPC's to communicate to clients. By default, item abilities are completely server authoritative.

### Creating a new item ability

!!! Important
First, I advice you make your own item ability parent and ability trait parent, then have all your item abilities be children of your parent and override some functions to fit your project, for example <span style="color:brown">**GetOwnersTags**</span>
!!!

To create a new ability, create a new child of <span style="color:violet">**ID_ItemAbility**</span>, or of your item ability parent.
- Optional: You can also create a item ability object if you want to expose more variables to the item asset or override some functions. For example, you can override <span style="color:brown">**AllowNewInstance**</span> to customize the "instancing policy" for the ability or the <span style="color:brown">**LooselyCheckCanActivateAbility**</span> function to minimize failed activations.

You will now want to open your item asset and add an item ability object to your <span style="color:slateblue">**Traits and Components**</span> array and assign the <span style="color:brown">**Item Component**</span> to your new item ability