# Item Query

The `ItemQuery` system is a separate module (Just like how the crafting system is a separate module) that includes a system that allows for quickly filtering and querying a specific set of items while supporting multithreading.

These queries are very simple; They listen to specific events and if they pass the filtering function, they are added to an array, which can be reused extremely efficiently.

To enable item querying, add the <span style="color:violet">**AC_ItemQueryManager**</span> component to your actor.

To add a `ItemQuery` to the manager, either add one to the <span style="color:slateblue">**ItemQueries**</span> array inside the component, or call <span style="color:brown">**ConstructObjectFromClass**</span> (We use this function so any `ExposeOnSpawn` parameters can be set) or use <span style="color:brown">**NewObject<>()**</span> if you're in C++, and then call <span style="color:violet">**AC_ItemQueryManager**</span> -> <span style="color:brown">**RegisterItemQuery**</span>

To then get the items from the query, you can either call <span style="color:violet">**AC_ItemQueryManager**</span> -> <span style="color:brown">**GetItemsFromQueryByClass**</span>/<span style="color:brown">**GetItemsFromQueryByIdentifier**</span> or call <span style="color:brown">**GetItemsFromQuery**</span> on the item query instance you just created.

The `ItemQuery` is now responsible for listening to any events relevant to it. For example, if you wanted to filter out all items with a specific tag, you would listen to the ItemTagAdded/ItemTagRemoved delegate. If an item had the tag added, you'd register the item. If the item had the tag removed, you're unregister the item.
By default, all queries will automatically listen to 4 delegates:
1. ComponentStarted - It will try to register all items.
2. ComponentStopped - All items will now be removed from the query.
3. ItemAdded - It will try to register a newly added item
4. ItemRemoved - It will remove the item from the query.

---
## Creating a query
To create a `ItemQuery`: 
1. Right click the content browser and select InventoryFramework -> Other -> ItemQuery
2. Override the <span style="color:brown">**QueryRegistered**</span> function and bind the relevent delegates your query will use. Then call <span style="color:brown">**RegisterItem**</span> and <span style="color:brown">**UnregisterItem**</span> when needed.
3. Override the <span style="color:brown">**DoesItemPassFilter**</span> to dictate what items will ultimately get registered with the query.

The example project includes 2 examples.

---
## Performance
In a test of over 300 items, registering a query that checked for a tag on the item took on average 500 microseconds, which is only 1/32 of a 60 FPS frame budget. Getting those items from the query then took on average 100 microseconds.
- For maximum performance, it's recommended to remove the `BlueprintNativeEvent` from the <span style="color:brown">**DoesItemPassFilter**</span> function and only use C++ item queries. As talked about in the `Optimizations` page, removing Blueprint support can improve performance by about 16 times.
- The main area that C++ dominates Blueprints are heavy for-loops. In this test, running a similar hard-coded query through blueprint code took a whopping **34.7 milliseconds**. These queries provide a simple interface for blueprint programmers to leverage the speed of C++ while staying in Blueprints by just overriding one function.
- The only downside for queries in regards to performance is that they take up memory, but it shouldn't be that much that you'd notice.
- Queries can be multithreaded. This can be extremely powerful in the right scenarios. For example, spawning the equipment can be expensive. This is why the manager component will try to multithread its default queries while that is happening, greatly improving CPU utilization.
    - Do keep in mind that sending a task to a different thread does have a small cost. I advise you to use Unreal Insights to evaluate whether it is worth for your inventory to bother with multithreading or not
> Keep in mind, Blueprint debugger does not work when blueprint code is NOT being executed on the game thread. So blueprint breakpoints won't work in the multithreading scenario

---
## Storing extra information
Queries do not just have to store a list of items. Since queries are living objects, you can store whatever information you want.
The example project includes a query that keeps track of specific item asset and how many of that item asset there are.