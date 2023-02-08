# Style Guide

I have tried to follow the popular <a href="https://github.com/Allar/ue5-style-guide" target="_blank">**UE5 Style Guide**</a>. Though I may sometimes accidentally or purposefully sway from it. I am very open to criticism, if something is confusing or inconsistent, don't hesitate to let me know.

---
## Functions
Networking functions are prefixed with **C_** for client, **S_** for server and **MC**_ for multicast.
For functions that attempt to automate replication, the bulk of the function's code is stored in a function with a **Internal_** prefix. These are typically called both by the server and client. If you want access to the internal functions, you will need to add **BlueprintCallable**  to the UFUNCTION above them.

In blueprint functions, local variables are prefixed with an **L**.

---
## Tools
All editor utility widgets are stored inside Plugin folder -> Content -> Core -> Widgets -> EditorUtilityWidgets -> Tools.
Any widgets or special components for each editor utility widget is stored in its own folder named after the editor utility widget inside the EditorUtilityWidgets folder.
This is done so you can bookmark/favourite the Tools folder and have access to all the tools quickly. You can also find all your editor utility widgets at the top of the editor -> Tools -> Tools, though there is no way to separate the inventory tools from your own editor utility widgets.