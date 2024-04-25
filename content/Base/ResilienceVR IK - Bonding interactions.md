202305091810
***
[[00 VRChat]]
***
ResilienceVR is a social VR app I’m developing to experiment and iterate outside of the boundaries of existing platforms.
In this article, I will explain how my first experiment works: Enabling the ability to make the avatar IK react to objects and players, despite the latency.

## Networked IK in existing applications

In social VR, users are connected together in a single virtual space, usually puppeteering a single virtual representation of themselves (an avatar).

There are currently two major ways that social VR applications transmit the virtual representation of the player to other players:
- Either transmit the hip position and bone rotations of the avatar, or
- Transmit the position and rotations of critical bones of the avatar (such as head and hands) or hardware trackers.

Transmit the position and rotations of critical bones of the avatar (such as head and hands) or hardware trackers.

Regardless, both of these approaches result in a large quantity of data that needs to be sent through the network, so additional simplifications and optimizations are needed for this to scale depending on the application’s needs.

Generally, in the worse case scenario when everyone’s avatars are in close virtual proximity in the virtual space, network usage in such applications can be estimated by the **square of the number of players**, as the data of any new player joining needs to be sent to all the other existing players in the instance.

Without optimizations:

- *Double the players* an instance with 10 players would be **4 times** more taxing on the network than an instance with 5 players.
- *Multiply the number of players by 10* an instance with 80 players would be 100 times more taxing on the network than an instance with 8 players.

This scaling issue is a main motivator for many optimization techniques, because networking non-static data can have a serious financial cost.

## The problem with latency
Users are still separated by latency, which in some cases can result in large delays between the players in the order of 200ms to even one full second.

These delays hamper the ability for the players to puppeteer their avatar in reaction to another player’s actions. Attempting to hold hands, dance together, or manipulate an object in the virtual space that is also being manipulated by another player, visually causes a mismatch due to the latency.

## Bonding interactions
In order to provide a partial solution to this issue, ResilienceVR experiments with an approach that enables some degree of reactive interactions between players, but not all. Social VR has various contexts, and the one I’m particularly interested in is bonding between users. Provide a way for users to hold hands and dance together by putting hands on each other.

For our purposes, we’ll focus on something that is known outside of VR as “victim control”, a term that is used by stage combat in the field of acting. When acting a fight scene, the actor who is the aggressor is not actually in control of the victim. The victim preserves their full freedom of movement. This means if an actor acts grappling the victim actor’s head, the victim actor’s head can freely move around at the victim’s will, not the aggressor’s will.

In our case, the person in control of someone’s limbs is not the person who initiated the interaction.

Context is important, so this experiment does not focus on other social VR interactions where players are put in confrontation to each other, such as a hands-to-hands fighting game or gun game.

A combat-focused networking could bring additional challenges such as requiring the ability for a player to take control of another players’ limbs, as the players engaged in combat would be confrontational in this context. What would happen to your hand if someone else were to grab your elbow away? A “networked Boneworks” would be a drastically different problem to solve. This could be a subject for a future experiment in ResilienceVR.

## Interactions with non-players

Even though this system was primarily designed for bonding between players, it just so happens that the system can easily be extended to enable interactions with items and objects in the virtual space, and even items held by other players.

Heavily inspired by single player games like Boneworks and Half-Life: Alyx, the feature was further extended to make the player’s hand attracted to objects, and make that attraction visible to other players without latency.

It should be noted that this is a rudimentary form of interaction, as there is no rigidbody simulation or collisions involved unlike those singleplayer games.

## How it’s done
In short, the local player hands tells the other players if they’re attracted to something. At the moment of grabbing, it tells other players where they’ve grabbed it. It is not continuous transmission of data.

Each player then computes how strongly each player’s hand is attracted to their respective objects and runs a partial IK solver to place their hand somewhere between their networked IK hand placement and the object’s hand placement; the stronger it’s attracted, the closer it is.

In detail:
### Step 1: Local user stores the player pose for networked IK
The normal IK solver runs, and the solved pose is stored so that it can be sent as part of networked IK. Networked IK poses are always sent with the local data in its configuration as if the entire system that is described below in the following steps didn’t exist.

All the users’ avatars are posed based on networked IK.

### Step 2: Local user finds the most attractive object
In local, the player’s hand searches for objects around it that it can be attracted to (if it’s not already grabbing an object).

For our purposes, the players’ limbs are attractive objects.

Once it finds available objects, it calculates where the hand would be placed on the object if it were to be fully attracted to it given the current hand placement.

With the potential hand placements computed, we calculate a magnetization amount for each potential hand placement, which is a scalar estimate of how close the current hand placement is to the potential hand placement. Also, some objects are more attractive than others.

The object that has the greatest magnetization amount is the winner.

- If the user is attracted to something, the user sends which object it is attracted to the other users. The object is represented by a number which is an identifier for attractive objects.
- If the user decides to grab that object, the user sends the hand position and rotation relative to that object. This is not updated after the initial grab action *(with one exception explained later)*.
- If the user is not attracted to anything, the user sends the information that it is attracted to nothing.

The advantages of doing this is that it’s cheap. It’s event driven. Attraction doesn’t always change and the data being sent is minimal. There is very little need for interpolation of networked data outside of phase changes.

The advantages of doing this is that it’s cheap. It’s event driven. Attraction doesn’t always change and the data being sent is minimal. There is very little need for interpolation of networked data outside of phase changes.

The magnetization amount can be reused to drive user input: If the magnetization amount is greater than a certain threshold, then the user can interact with that object, useful for climbing for instance.

Note that the magnetization amount is not transmitted to other players. Magnetization will be recalculated on each player to better account for delays.

## Step 3: All users run the partial IK for users that are attracted
The users solves the IK locally for all users that apply, including themselves.

The IK solver will not run locally for a given player:

- When the given player is not visible, or
- When the given player is not attracted to anything, or
- When the player is attracted to an object that is disabled, or
- When the player is attracted to an object that is located on someone’s avatar that is hidden (i.e. the animator not being updated because it is culled)

Doing this saves iterating on users that don’t need it.

ResilienceVR uses a custom written IK solver, which provides more control on what part of the IK can run, and how it can run, so it can choose to only resolve the arm based on the currently posed avatar.

The user computes the hand placement to that specific object, and then computes the magnetization amount.

This magnetization amount is used to interpolate between the networked hand placement and the final hand placement. This leads to the hand being attracted to the object as it approaches it locally, rather than remotely.

There are two ways to iterate:

- Run all the iterations of the solver on each user separately, in the same order for all players, or
- Run one iteration solver on each user, then apply the pose on every user, then iterate again.

They both come with drawbacks. In a scenario where each user decides to grab the others’ hand, or users decide to grab each others’ elbows, this can lead to stability issues that still need to be experimented upon.

## Relative movement (in step 2)
I want to allow that player to move their grabbed hand while still benefiting from networked IK.

When a player decides to initiate that action, it sends another networked event which contains:

- the position and rotation of the hand at the moment this action was initiated, and
- the position and rotation of the object being held at the moment this action was initiated.

This is never updated afterwards, so there’s no interpolation. In the solver phase, we’ll treat the player’s hand as being a difference relative to the object. This allows it to benefit from the smoothness of any networked IK without having to deal with interpolation ourselves in this step.

When the players decides to stop this action, it sends a new grab action with freshly baked positions and rotations.

## Implementation in VRChat
The “ResilienceVR IK Demo” Udon VRChat world is a port of the above system, brought back under the allowed limitations of the VRChat platform.

The current state of Udon is limiting due to execution slowness, but good enough to showcase this feat. It should be noted that this is a naïve port with very few Udon-specific optimizations, as I want to keep the changes in the code minimal from ResilienceVR.

Despite this, I still had to disable some parts of the IK solver for simplification. If this were a native Udon project, I would have written the solver more efficiently.

Since VRChat does not provide any access to the avatar’s meshes, nor write access to its bones, the world itself contains the models of the avatars as SkinnedMeshRenderers, and moves their armature, effectively making it seem like the user is wearing an avatar.

The avatar pedestal in the world ensures that the bone rotations and lengths are correct, and that the animator is running when visible. If the animator is not running because no mesh were to be visible, Udon’s VRCPlayerApi would not return updated bone rotations.

The VRChat world works in the following way:

## Step 1: Local user stores the player pose for networked IK
All players’ bone positions and rotations (using VRCPlayerApi) are collected and copied into memory. These positions and rotations are read and written multiple times in the IK solving process.

Unlike ResilienceVR, since VRChat is the one responsible for Networked IK, we don’t have to worry about any of this, since there is nothing the world can do to influence the transmitted bones in the future.

## Step 2: Local user finds the most attractive object
This part is almost identical to ResilienceVR, but uses Udon’s manual sync to transmit the events with the help of 
[CyanPlayerObjectPool](https://cyanlaser.github.io/CyanPlayerObjectPool/). The event driven nature of the original makes it very easy to carry over in Udon.

## Step 3: All users run the partial IK for users that are attracted
Udon doesn’t give sufficient access to players’ data to accomplish most of the optimizations.

In this case, I’ve limited to the following:

The users solves the IK locally for all users that apply, including themselves.

The IK solver will not run locally for a given player:

- When the given player is not wearing the correct avatar
- When the given player is not attracted to anything

We can detect if the player is wearing the correct avatar by measuring the individual distances between the hips, spine, chest, and neck. This can lead to false positives if the player is wearing an original avatar of the same model.

Players are iterated individually, with the poses applied immediately. This can lead to some discrepancies as the execution order may affect the result.

The solved poses are then applied to the bones of the SkinnedMeshRenderers’ armatures located in the world.

## The result
This experiment resulted in the creation of a VRChat world where you can experience it for yourselves. I want my work to inspire existing social VR users to desire and demand more from their favorite social VR platforms.

ResilienceVR will continue to be a social VR app that I can use to iterate upon new ideas to share.

There is still a lot to research, as player-to-player interactions creates a bunch of interesting challenges in terms of user experience. I’ve received a lot of feedback, especially regarding the grabbing interaction and the strength of magnetization that I’ll be able to work upon.

## Patreon
Thank you to all of my supporters on Patreon, Pixiv Fanbox, and tippers of my free tools on Booth!

If you like my work and research, consider supporting me on [Patreon](https://www.patreon.com/vr_hai)!