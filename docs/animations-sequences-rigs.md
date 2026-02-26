# Animations, Animation Sequences and Rigs




Proprietary animation sequence format: RSEQs, part of RTECH Respawn proprietary tech stack

Proprietary skeletal mesh / rig / armature format: RRIG, part of RTECH Respawn proprietary tech stack

Proprietary model format: RMDL, part of RTECH Respawn proprietary tech stack

All are binary files


Models have rigs and animation seqeuencdes associated as dependencies (rigs: [], seqs: [])

Rigs have animation sequences associated as dependencies (seqs: [])


animation activities = indirected names for a collection of related animations (group alias)

animation events = flags inside of the animations that convey that an event has happened to the engine


activity commands

activity modifiers

custom activity registration

custom activitymodifier registration

activity_dump

activitymodifier_dump


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