# W_Attachment Parent
Widget Attachment Parent
Blueprint Child: WBP_Attachment

---

The parent widget for widgets that are used to hold the containers an item owns.
For example, a gun has a sight and a magazine slot and you want to represent those slots with a container. That container needs to be hosted by *some* widget.

The base widget does not have any kind of layout so you are free to make any layout your project needs.

Both the C++ parent and the blueprint child are abstract, so it won't appear in any drop-down menus. These are designed for the intention to have children made from them.