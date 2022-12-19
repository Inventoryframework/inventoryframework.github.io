# Notes about the design decisions and its architecture

- This plugin was created for an open-world game that has very strict performance requirements. Because of this, some logic is handled in C++ as an inventory system like this does a lot of math. The design architecture is also designed to give programmers more control of when widgets and data is initialized. The architecture has also been made in mind for large-scale projects.
This does mean though that there is a specific and strict workflow for setting up the component for items, especially sub containers. I've made an Editor Utility Widget which should prevent any human errors and allow people to work in the system without those strict requirements slowing down your creation process.

- The architecture was made with designers in mind, so certain C++ functions can be overridden in Blueprints and some functions are only needed on the Blueprint level and in some cases there are both C++ and Blueprint versions of the same function. This system is not meant to be purely C++ or Blueprints, but a healthy mix of the two.
If you can't write in C++ or don't have access to anyone who can write C++ code for you, the current functions should allow for you to create nearly any Blueprint logic you should need.

- Data Assets are being used instead of Data Tables for a few reasons:
	 1. You can create children of the item data asset to create a brand new type of item with its own unique settings and not have those settings clutter the items that aren't relevant to those settings.
	 2. You can control their loading and unloading in the Asset Manager.
	 3. Easier to create Utility Widgets and automating tests.
	 4. It's easier to find out what assets are referencing which data asset, while with Data Tables, it can get messy to find out what asset is referencing a specific row.
     5. Data tables are not ideal for managing massive databases. With every row, any variable that is a hard reference will be loaded by any item that is referencing the data table. If itemA references the data table but uses RowA, it’s going to load every single row in the data table and all the hard references in there, not just RowA. This also means that every time you’d add an item, the size map and hard references would grow for every item in the game.
This quickly spirals out of control and quickly your assets can become 100+ MB’s, whereas with data assets, they are self-contained and only load the hard references made inside of them.