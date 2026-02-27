# Animations, Animation Sequences and Rigs


## What are animations?
```
Animations are sequences of frames (snapshots) in which the model is posed in various ways. Titanfall and Apex Legends animations were created at 30 FPS (frames per second). Hence, animations are collections of key frames and frame transitions spread across time. 
```
## What is a rig?
```
A rig / skeleton / armature is a collection of "bones" mapped onto vertex groups contained within the model's mesh, in order to be able to move it and control its movements

Animation has two prerequisite steps: skinning and rigging

Skinning, in this context, does not refer to texturing or retexturing a model, but to mapping bones to a model's vertices / vertex groups in order to create a skeleton.

Rigging encompasses weight painting and the manipulation of bones with bone controllers, inverse kinematics, etc.

The armature of a model can be seen by exporting the RMDL with RSX as SMDs (Studiomodel Data Files) and then importing them into [Blender](https://www.blender.org/), using [Blender Source Tools] (https://developer.valvesoftware.com/wiki/Blender_Source_Tools) on .qc / .smd import mode.

R1 (Titanfall 2014) and R2 (Titanfall 2, 2016) require extracting the .MDL files from the VPK archives (before RPAK archives existed) and then decompiling them with Studiomdl.exe, to obtain a .qc and .SMD files. They can be imported into Blender, afterwards, using the Blender Source Tools.

It is important to note that animation-only .MDL's exist. These .MDL's are usually used for shared animations that can be played by multiple different models. Animation-only .qc's also exist, alongside .qci files (.qc inherited / included).

Bones are exposed by going into "Edit Mode". From that view it is possible to observe bones, joints, bone modifiers, the bone hierarchy, etc.

Changes in position of bones between frames are known as "deltas" (differences / changes) and animations obtained from subtracting frames or animations from other animations are known as "delta animations". These "delta anims" are meant to be layered on top of other animations, as they do not have meaning on their own.

Delta layering does not completely override bone positions to the delta, but, instead, compounds the core animations with the delta animations.
```

## A brief historical recap of Respawn's animation implementation
```
Respawn's first title, Titanfall (2014), essentially used Valve's entire Source Engine tech stack, with modifications, meaning:

- VPK (Valve Pack archive, replacing GCF's, Gazelle Cache Files, which were named after Steam's old internal name, Gazelle)
- SMD (Studiomodel Data)
- VTX (Valve's proprietary mesh strip format)
- VVD (Valve Studiomodel Vertex Data File, storing positio n independent data for bone weights, normals, vertices, tangents and texture coordinates for UV Mapping, used by the MDL)
- MDL (Valve Model)
- PHY (Valve Physics)
- VGUI (Valve Graphical User Interface)
- VGUI Material Proxies
- VTF (Valve Texture Files)
- VMT (Valve Materials)
- VBSP (Valve Binary Space Partitioning Map Files)
- .txt configs with key-value pairs
- Valve's entity system

Even by Titanfall 2's release in 2016, Respawn had not yet finished developing their own proprietary tech stack (known as RTECH, Respawn Tech) and fully implementing it, resorting to using their proprietary tech alongside modified Valve tech, such as RUI (Respawn UI) alongside VGUI (i.e.: for the Archer Rocket Launcher). Titanfall 2 still used .MDL's.

It took until Apex Legend's release in 2019 for Respawn to mostly implement RTECH and later on in Apex Legend's life until RTECH had been FULLY implemented.

- At launch, models were separated into MDL, VTX, VVD and PHY. Later on, they were packed into RMDL's and separate .VG physics data.
- For the first years of Apex Legends' life, the engine still relied on VPK's, which were eventually phased out completely for RPAK's.
- The build that R5V uses, Season 3 Apex Legends, supports both VPKs and VMT materials.
- Animation sequences were separated from the MDL files into RSEQ's (Respawn Animation Sequence files). Rigs were also separated into RRIG's (Respawn Rig Files)
```
## Animations in R5 Valkyrie and retail Apex Legends
```
This leaves us at the present moment, where the current formats are used by Season 3 Apex Legends, which are part of the Respawn's Respawn proprietary tech stack RTECH, whose engineering was spearheaded by former Respawn employee Earl Hammon, after which the in-universe Hammond Corporation was named:

Proprietary animation sequence format: RSEQ, 

Proprietary skeletal mesh / rig / armature format: RRIG,

Proprietary model format: RMDL,

Proprietary UI format: RUI,

Proprietary Particle System format: EFCT,

Proprietary asset archive format: RPAK,

Proprietary DirectX Shader format (unnamed, but known as "MSW" / Multi-Shader Wrapper, created by R5 Reloaded creator and software engineer Kawe Mazidjatari / Amos)

All are binary files, meaning they cannot be opened as plaintext; they require hexadecimal editing or reverse engineering of their headers and structures for proper inspection, modification and repacking.

However, in Season 3, these are used in tandem with VPKs (maps, certain models, etc.), VMTs (some world materials, particle effect materials, etc.) and, still, VGUI and BSP.

Models have rigs and animation seqeuences associated as dependencies (defined inside RePak / RSX exported assets inside rigs: [], seqs: [], contained within RSON plaintext files)

Rigs have animation sequences associated as dependencies (defined inside RePak / RSX exported assets inside seqs: [], contained within RSON plaintext files)
```

## Animation Activities and Animation Activity Modifiers

Animation Activities represent indirected names for a collection of related animations (group aliases). They can be thought of as sets of related animations which are grouped under a common alias, such as first person reload animations being grouped under the "ACT_VM_RELOAD" viewmodel activity (VM, in this context, refers to viewmodel, not virtual machine).

Activities in the Source engine are defined inside activitylist.h and which activities are used for which animations is defined models' .qc files, pre-compile.

Animation Activity Modifiers act similarly to code flags, signalling that specific animations are supposed to play for an activity, depending on in-game contexts (such as Legend-specific animations, like Rampart's LMG animations). Essentially, they "modify activities" (offering additional context to the engine), as the name implies.

## Animation Events

```
Animation Events represent flags inside of the animations that convey that an event has happened to the engine. They can be included inside $sequence commands inside model or animation-only .qc files, in which case they are named in ALL-CAPS, use underscores, use the "AE_" prefix and are contained between two curly brackets { }

Examples:


There is an alternative to .qc Animation Events, which is the use of script-based Animation Events.

Examples:



R5V is developing a solution for registering custom activities and activity modifiers in the engine.

Commands exist for dumping all activities and activity modifiers to the console:

activity_dump

activitymodifier_dump



```



activity commands

activity modifiers

custom activity registration

custom activitymodifier registration



script animation events

open RRIGs with 010 hex editor to see poseparams, bones

bones same name as model vertex group names

same rig applied to the weapon viewmodel and the first person arms model and the same animation is played simultaneously on both models

if the aseq finds no vertex groups associated with a bone from the rrig, nothing happens - no error, by design

bone names MUST be the same for rseq and rrig, cannot use an alternator rrig with nemesis rseqs



It is crucial to know what QC Commands there are in order to work with animations


subtract -> creates delta animation by subtracting certain frames from an animation 

delta animations -> animations that are meant to be played as layers on top of other animations. they make no sense on their own

they are used to combine / blend between different animations / animation states such as firing with aiming down sights, etc.

layered animations

$sequence

Apex Legends animations are made at 30 FPS (and then interpolated to 60 FPS?)

default fade in, fade out at 0.2s (200 ms)

using nodes for transitions





$poseparam 

used to create smooth interpolation between different frames for bones on a model

example: nemesis magnet latches opening / closing

can be controlled from inside scripts

SetScriptPoseParam0()
GetScriptPoseParam0()




animation only .mdls 

you can recompile animations without recompiling the model as well



1) export as smd -> compile to mdl v49 with studiomdl -> convert to mdl v54 with rmdlconv

2) import from a different format into blender -> export as smd -> method 1

3) export rmdl, rrig and rseqs -> use R5-AnimInspector to get .qc -> modify .qc -> recompile animations (and optionally the model) 

.qci (qc inherited) -> look up qci for the discussion on the R5V discord server

4) convert from a higher version of mdl to mdl v54 with r5-animconv 





open rmdl with 010 hex editor to see material dependencies if you dont have .qc



hooks



entity methods


entity.Anim_Play("PathToRSEQ") or entity.AnimPlay("animation_name)

entity.AnimStop() (stops all animation sequences)

entity.Anim_SetInitialTime

entity.Anim_EnableUseAnimatedRefAttachmentInsteadOfRootMotion()

entity.GetSequenceDuration( victoryAnim )

string victoryAnim = "ACT_MP_MENU_LOBBY_SELECT_IDLE"

entity.Anim_SetPlaybackRate( float playbackRate )

entity.Anim_PlayOnly("animation_name")

entity.Anim_IsActive()

entity.Anim_ScriptedPlay()

entity.Anim_EnablePlanting()

entity.Anim_SetPaused( bool )

PlayCustomActivity
StopCustomActivity

entity.Anim_ScriptedPlayActivityByName("ACT_STUNNED", true, 0.1)

entity.Anim_PlayAttackGesture() // does this work??

entity.Anim_HasActivity("ACT_FLINCH_GRAPPLE")

activity commands

entity.Anim_ScriptedPlayActivityByName("ACT_FALL", false, 0.2)

entity.Anim_PlayGesture("ACT_SCRIPT_CUSTOM_ATTACK2", 0.2, 0.2, -1.0) // does this work??

entity_Anim_PlayWithRefPoint( animation, origin, angles, blendTime ) // requires that the entity be parented to the ref point ??


anim teleport


PlayAnimTeleport( entity, animation_name, reference = null, optionalTag = null, initialTime = -1.0, smooth = false )

PlayFPSAnimTeleport

PlayFPSAnimTeleportShowProxy


GetAnim()

HasAnim()

SetAnim()

PlayAnim( entity, animation_name, optionalparms)

PlayAnimWithTimeout()



proxy model, first person proxy


script animation events


int activity weapon.GetWeaponActivity()