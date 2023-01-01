# Introduction

- This plugin was created for an open-world game that has very strict performance requirements. Because of this, some logic is handled in C++ as an inventory system like this does a lot of math. The design architecture is also designed to give programmers more control of when widgets and data is initialized. The architecture has also been made in mind for large-scale projects.
This does mean though that there is a specific and strict workflow for setting up the component for items, especially sub containers. I've made an Editor Utility Widget which should prevent any human errors and allow people to work in the system without those strict requirements slowing down your creation process.

- The architecture was made with designers in mind, so certain C++ functions can be overridden in Blueprints and some functions are only needed on the Blueprint level. This system is not meant to be purely C++ or Blueprints, rather a healthy mix of the two.
If you can't write in C++ or don't have access to anyone who can write C++ code for you, the current functions should allow for you to create nearly any Blueprint logic you should need.

- Data Assets are being used instead of Data Tables for a few reasons:
    1. You can create children of the item data asset to create a brand new type of item with its own unique settings and not have those settings clutter the items that aren't relevant to those settings.
	2. You can control their loading and unloading in the Asset Manager.
	3. Easier to create Utility Widgets and automating tests.
	4. It's easier to find out what assets are referencing which data asset, while with Data Tables, it can get messy to find out what asset is referencing a specific row.
    5. Data tables are not ideal for managing massive databases. Anyone who has had to deal with source-control while working on an item will know the pains data tables can bring for a system like this.
    6. For more advanced users, it’s possible to add slate widgets into the data asset editor window. For example, a recoil pattern editor for gun items.

The only con regarding data assets is you don’t get a nice table view like you get with data tables. But you can use the matrix editor which gives you a similar editor to the data tables and is in some ways even more powerful.


- Native Controller support is not yet added but is planned.
- Split screen support is not something I will be adding.
- The plugin was designed to be [**installed locally**](https://inventoryframework.github.io/introduction/firststeps/installingtheplugin/) to your project, not the engine. With how the marketplace works and how the engine works, it was much simpler to make this asset a plugin. This has also had the nice side effect of updating the system to be much easier.
- Many things have been designed to be as abstract as possible and assume as little as possible about YOUR project and creation process. I do not want to dictate any creative vision and I do not want to step on any toes. I’ve tried to keep the system as painless as possible for you to integrate YOUR systems and assets with this inventory system, such as an attribute, ability or crafting system.
