---
order: 9
icon: comment-discussion
tags: 
---

# F.A.Q

---
### Q: Can the system be used for a more traditional inventory system where all items are 1x1?
A: Of course! Containers have styles, and the `Traditional` style converts all items into 1x1.

---
### Q: Will stats/attributes or ability systems of any kind be added?
A: No. I believe most people should be using the Gameplay Ability System framework as it is becoming the industry standard or something similar (I personally suggest <a href="https://www.unrealengine.com/marketplace/en-US/product/gas-companion" target="_blank">**GAS Companion**</a> from the marketplace). A lot of people also already have a stat/attribute or ability system in place and I do not want to step on their toes.
The [tag system](https://inventoryframework.github.io/workinginthesystem/tagsystem/) does allow you to create basic stats and attributes for items, but I generally recommend people use GAS or a more fleshed out system designed around that.

---
### Q: Will I be adding any art, meshes or animations to the asset?
A: No, with the exception of some basic things like the Tile Icon and some other things, just enough so you don’t get any compile errors and can interact with the system before you implement your art.

---
### Q: Does the plugin feature an interaction system?
A: This plugin does not provide a fleshed-out interaction system for you, but an interface system is integrated in the demo's as that's the foundation of every interaction system I've seen. This is done so your project does not build a dependancy on my interaction system if you ever decide this plugin does not fit your project. The interaction system is designed to be replaced, as most people already have an interaction system already in place.

---
### Q: Does the plugin work with CommonUI and Enhanced Input?
A: Yes, the example project does not feature either one for a few reasons:
1. Try and keep the example project as simple as possible. Lots of beginners or people not familiar with either, especially CommonUI don't want a tremendous amount of knowledge dumped on them to start.
2. When IFP started, Enhanced Input was experimental and CommonUI had not been released. It is only recently (5.4) where proper documentation has been publicized for both and have had their experimental label removed. CommonUI's native Enhanced Input support has also had its experimental label removed only recently.

2.10 is currently the planned release to have native CommonUI and Enhanced Input support, both in the plugin and in the example project.

---
### Q: Will I be adding X feature/system?
A: Only if it’s a highly requested feature, only then will I either implement it into the base asset itself or advise you how to make it yourself. You can track what is being worked on and what will be worked on in the future by going to the Trello link at the top of the page.
