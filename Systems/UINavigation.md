# UI Navigation

UI Navigation refers to an input such as a controller joystick, gamepad or arrow keys to attempt to navigate tiles and items inside of a container. This is handled through `EnhancedInput` and the `Container` widget.

> This documentation covers just container and item navigation. Things like context menu's and so forth fall under common navigation, which has a mariad of ways to handle and there are plenty of resources on how to handle common UI nagivation.

To handle navigation between multiple containers, either fill out the `Navigation` settings in the container widget instance or handle it through the `NavigatedOutOfBounds` delegate which will let you know which direction the user last tried to navigate. For example, if they tried to go down, but no valid tile was found, this delegate will go off and say they tried to navigate downwards. You could now grant focus to the container below the container that currently has focus.

![](/pictures/NavigationDetailsPanel.png)

> Input action and mapping context references are automatically assigned to the ones that come with `IFP`. The navigation system fetches the class defaults to know what action and context to use. To customize them, go into `BP_AC_Inventory` and change the settings inside of the `Settings -> Input Settings` category.

!!! Important
As of 5.4, there is a bug with all the properties in the `Navigation` category. Whenever you assign new values, you must recompile and resave the asset yourself. The bug is that the asset is not being marked as dirty and in need of recompilation.
!!!

---
## "Virtual" cursor
To simplify a lot of the logic, the container widget will take control of the cursor to act as a "virtual" cursor (Handled in <span style="color:brown">**NativePaint**</span>). `CommonUI` does the same, although they seem to have a "synthetic" cursor. As of writing, I am not able to find any sources, other than reverse engineering `CommonUI`, on how to make your own virtual cursor. The container will return control to the cursor if any movement is detected (<span style="color:brown">**OnMouseMove**</span>)

---
## Dynamic navigation
In some cases, containers are added to the screen during runtime, like opening a chest. In this case, the navigation panel above won't work.
To override the above settings, you can assign override containers for each direction (for example, NavigateRightContainerOverride variable will override the right input navigation).
Here is an example of how that would work:
![](/pictures/NavigationOverrideExample.png)

---
## Customizing Navigation
If you wish to completely override the navigation system for `IFP` or customize it, there's a small list of functions you can override.

Container widget functions:
- <span style="color:brown">**OnKeyDown**</span> (All logic starts here)
- <span style="color:brown">**GetNextItemToNavigateTo**</span>
- <span style="color:brown">**GetNextContainerToNavigateTo**</span>
- <span style="color:brown">**NavigationFocusUpdated**</span>
- <span style="color:brown">**ItemNavigationFocusUpdated**</span>
- <span style="color:brown">**SetCurrentNavigatedItem**</span>

Item widget:
- <span style="color:brown">**ItemHighlightUpdated**</span>
- <span style="color:brown">**StartDragItem**</span>
- <span style="color:brown">**StopDragItem**</span>
- <span style="color:brown">**CancelDragItem**</span>

---
## Scroll Boxes
By default, containers will keep the currently navigated tile in view within the scrollbox inside that container. But if you navigate to a new container inside your inventory UI that is not in view, you need to handle that, since that is outside of the containers code. 
Here is a basic example from the example projects inventory UI widget:
![](/pictures/ScrollBoxNavigationExample.png)

---
## CommonUI
`CommonUI` is not utilized by the plugin due a few reasons, but the primary reason is the lack of easy customizing the navigation around containers. For example, snapping to nearby items rather than navigating tile by tile.
The true questions around `CommonUI` that have to be asked is: What does it provide to `IFP` that justifies `IFP` having a dependency on it? Truly, `CommonUI` just provides 2 things: Reworked navigation and new widgets that handle common things. The reworked navigation is great, but lacks customization. The widgets provided by `CommonUI` are not beneficial to `IFP`. Maybe the example project, but not the base plugin.


`IFP` can be utilized by `CommonUI`. Many people have designed their inventory screen made out of `CommonUI` panel widgets.

But as of writing, `CommonUI` is too complex for beginners, lacks debugging tools and doesn't provide enough to `IFP` to justify being dependend on it. Many people do not want or like `CommonUI` and I dont want to force people to use it.

### CommonUI integration

> This section is a work-in-progress. I personally do not use `CommonUI` and very few people seem to be using `CommonUI` so some details and steps might be missing.

- As pointed above, the only thing `CommonUI` provides to `IFP` is the reworked navigation. But their navigation only searches for `CommonActivatableWidget`'s. Depending on how you want to control your navigation, you might want to reparent <span style="color:violet">**W_Container**</span>, <span style="color:violet">**W_Tile**</span>, or <span style="color:violet">**W_InventoryItem**</span> to <span style="color:violet">**UCommonActivatableWidget**</span> so `CommonUI`'s navigation can detect those widgets.