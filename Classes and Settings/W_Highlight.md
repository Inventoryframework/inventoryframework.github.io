# W_Highlight
Widget Highlight
Blueprint Child: WBP_Highlight

---

The majority of the logic for this widget is done in C++ as it’s doing a lot of math on tick to achieve its animation style. There is a pure BP version of the tick logic in <span style="color:violet">WBP_Highlight</span> in case you aren’t a programmer and want to try out your own animation system or if you just want a blueprint version to read to better understand the C++ code if you aren’t familiar with C++.

There will be a second version without an animation style and will be designed more around performance (currently it can be a bit expensive if you have hundreds of tiles in one container) but that will be in a later version.
