---
tags: [item]
---

# Objects and Drivers

1. Item drivers should be destroyed after their logic is complete, especially if it’s from a stackable item. The reason being that it’s really hard or almost impossible to keep track of the stack it came from as that stack can be put into other stacks, split or fully used.

2. You must always keep in mind if the item driver is running logic that’s on a timer,  you need to be aware that the item the driver belongs to might become invalid at any point. There is always the chance the item that owns the driver might get destroyed during the timer logic. I also recommend “locking” the player from interacting with the item during this timer or having some sort of failsafe if the player manipulates the item in some way while the timer is still running (For example; a gun playing its pickup animation and the player drops it mid-animation)
