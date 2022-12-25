---
tags: [underrevision]
---

# BP_PreviewActor

==- Blueprint Preview Actor
File Location: Content\Core\Preview\BP_PreviewActor.uasset
==-

!!!danger
Under revision
!!!

---

This is an actor that “should” perfectly duplicate the appearance of any specific actor.
It does this by getting all components of the owning actor and then duplicating it and reconstructing the hierarchy structure to match the hierarchy structure of the owning actor. This includes any attached actors.

It currently supports <span style="color:violet">**SceneComponent**</span>, <span style="color:violet">**PrimitiveSceneComponent**</span>, <span style="color:violet">**ChildActorComponent**</span>, <span style="color:violet">**StaticMeshComponent**</span> and <span style="color:violet">**SkeletalMeshComponent**</span>. Though <span style="color:violet">**ChildActorComponent**</span>'s are known to be extremely buggy and I personally don't suggest you use them with this system.

==- Notes regarding <span style="color:violet">**ChildActorComponent**</span>'s

There is currently no way to disable <span style="color:brown">**BeginPlay**</span>, where as with Actor's you can with a ExposedOnSpawn boolean at the beginning of your BeginPlay logic. If you have any logic connected to your <span style="color:brown">**BeginPlay**</span>, you will need to find a way to disable it for the preview.

===

It should be easy to add compatibility for more components, you’ll have to add them yourself, but the currently supported components should support the vast majority of components that have any sort of visual importance.
You can optimize the construction or simply prevent this actor from duplicating a specific component by going into <span style="color:slateblue">**ComponentTags**</span> and adding “**DONOTPREVIEW**”.
Though remember, since the actor is trying to reconstruct the hierarchy setup inside your original actor, any components attached to components that you labeled as “**DONOTPREVIEW**” will break.