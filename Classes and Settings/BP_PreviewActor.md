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
It does this by getting all components of the **owning actor** and then duplicating it and reconstructing the hierarchy structure to match the hierarchy structure of the owning actor. This includes any attached actors.

It currently supports <span style="color:violet">**SceneComponent**</span>, <span style="color:violet">**PrimitiveSceneComponent**</span>, <span style="color:violet">**ChildActorComponent**</span>, <span style="color:violet">**StaticMeshComponent**</span> and <span style="color:violet">**SkeletalMeshComponent**</span>.

==- Notes regarding <span style="color:violet">**ChildActorComponent**</span>'s

1. There is currently no way to disable <span style="color:brown">**BeginPlay**</span>, where as with Actor's you can with a ExposedOnSpawn boolean at the beginning of your BeginPlay logic. If you have any logic connected to your <span style="color:brown">**BeginPlay**</span>, you will need to find a way to disable it for the preview if there's any logic you don't want the preview to run.
2. If possible, avoid <span style="color:violet">**ChildActorComponent**</span>'s. They are extremely prone to bugs and odd behaviour, and they don't seem to play well with networking.

===

It should be easy to add compatibility for more components, you’ll have to add them yourself, but the currently supported components should support the vast majority of components that have any sort of visual importance.
You can optimize the construction or simply prevent this actor from duplicating a specific component by going into <span style="color:slateblue">**ComponentTags**</span> and adding “**DONOTPREVIEW**”.
Though remember, since the actor is trying to reconstruct the hierarchy setup inside your original actor, any components attached to components that you labeled as “**DONOTPREVIEW**” will break.

This actor will render out a render target for you to use however you wish and this is how the item icon generation is handled.

Even though the render scene component only see’s the preview actor itself and it only receives lighting from lighting channel 2 (Channel can be changed, but it’s recommended not to have it on 1 if your game has day/night cycles), <span style="color:violet">**BP_PreviewActor**</span>’s lighting can still affect another <span style="color:violet">**BP_PreviewActor**</span>.
Even while spawning them at a ridiculously long range of random locations, during my playtesting I still had this scenario happen.
To fix this, there’s a <span style="color:violet">**BP_PreviewActorCollider**</span> which you will spawn first, which will test for collision and adjust its location. Then you spawn <span style="color:violet">**BP_PreviewActor**</span> itself and set its location inside the collider.

This does mean though that you need to handle the collision responses for this. I recommend creating a new collision channel specifically for both the <span style="color:violet">**BP_PreviewActor**</span> and <span style="color:violet">**BP_PreviewActorCollider**</span>.

---
## Camera management
The <span style="color:violet">**BP_PreviewActor**</span> will attempt to find the first component which has a socket called “PreviewCameraSocket” and attach the camera to that socket. If none is found, you’ll get an error message and the camera will stay attached to the PreviewActor’s root.
Ideally, all your meshes should have this socket that you want to use this actor on. But to improve prototyping, I’ve added a few options to the item data asset:
(ADD PICTURE HERE)

Once you're far enough into production or willing to keep all meshes up to date with the socket, you should remove these three settings.

The actor supports auto-generated estimated camera distances, so you don’t have to debug every item to create a camera distance while in a prototyping phase. You can disable this by checking <span style="color:slateblue">**Use Custom Arm Length**</span>.

---
## Skeletal Meshes
The default animations for skeletal meshes is retrieved via an interface function <span style="color:brown">**GetEquipmentData**</span>. This allows you to assign what animations the previewed skeletal mesh will play. For now it is only looping one animation, but an animation blueprint can be added to this struct if you wish.

---
## Optimizations
It is also up to you to manage the render targets and when they should be released from the GPU. You can see an example in <span style="color:violet">**WBP_InventoryItem**</span> -> <span style="color:brown">**TryReleaseGeneratedIcon**</span>. I’ve tried to automate the release and creation for the widgets that come with this plugin, but if you are making a new widget and are new to render scene components, you must call ReleaseRenderTarget2D for the texture.

It is hard to create a perfectly automated system, so if you ever encounter an icon vanishing for an item, that means the render target was released improperly. During your prototyping phase, I recommend you just release and wipe the references when the player closes their inventory. Then when you’re in your optimization phase, you can try to optimize this.
Do keep in mind that live captures can get very expensive, very quickly, since they are capturing a new image every frame.

You can limit it by enabling <span style="color:slateblue">**CaptureEveryFrame**</span> and limiting the <span style="color:slateblue">**TickInterval**</span> inside of <span style="color:violet">**SceneCaptureComponent2D**</span>.

I have these disabled by default, so if your actor isn’t animating, you need to enable CaptureEveryFrame.
