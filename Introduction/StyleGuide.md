# Style Guide

I have tried to follow the popular <a href="https://github.com/Allar/ue5-style-guide" target="_blank">**UE5 Style Guide**</a>. Though I may sometimes accidentally or purposefully sway from it. I am very open to criticism, if something is confusing or inconsistent, don't hesitate to let me know.

---
## Functions
Networking functions are prefixed with **C_** for client, **S_** for server and **MC**_ for multicast.
For functions that attempt to automate replication, the bulk of the function's code is stored in a function with a **Internal_** prefix. These are typically called both by the server and client. If you want access to the internal functions, you will need to add UFUNCTION(BlueprintCallable) above them.

In blueprint functions, local variables are prefixed with an **L**.