# Audio Commands / Functions


## Animation Events (inside QCs, in $sequence):

{ event AE_CL_PLAYSOUND framenumber "EVENT NAME" }

{ event AE_CL_STOPSOUND framenumber "EVENT NAME" }

Examples: 

$sequence "animseq/weapons/nemesis/ptpov_nemesis/reloadempty_late1_onehanded" {
	"animseq_ptpov_nemesis_reload_empty_onehanded04_dmx__41_100_DD9685A2"

	fadein 0.2
	fadeout 0.2
	activity ACT_VM_ONEHANDED_RELOADEMPTY_LATE1 1
	//ikrule 0
	{ event AE_CL_PLAYSOUND 21 "wpn_nemesis_reload_raise" }
	{ event AE_CL_STOPSOUND 59 "wpn_nemesis_reload_raise" }
	{ event AE_CL_PLAYSOUND 27 "wpn_nemesis_reload_magInsert" }
	{ event AE_CL_STOPSOUND 59 "wpn_nemesis_reload_magInsert" }
	{ event AE_CL_PLAYSOUND 29 "wpn_nemesis_reload_magSmack" }
	{ event AE_CL_STOPSOUND 59 "wpn_nemesis_reload_magSmack" }
	{ event AE_CL_PLAYSOUND 34 "wpn_nemesis_reload_magSpin" }
	{ event AE_CL_STOPSOUND 59 "wpn_nemesis_reload_magSpin" }
	{ event AE_WPN_CLIPBODYGROUP_SHOW 5 "" }
	{ event AE_WPN_RELOAD_MILESTONE_2 21 "" }
	{ event AE_WPN_RUMBLE 21 "reload_pilot_large" }
	{ event AE_WPN_RUMBLE 36 "reload_pilot_small" }
	{ event AE_WPN_FILLAMMO 36 "" }
	{ event AE_WPN_READYTOFIRE 46 "" }
	node NODE_2
}

From: ptpov_nemesis.qc <- derived from the Nemesis viewmodel from its rmdl, rrig and rseqs, using Someoneatemylastsliceofpizza (SAMLSOP)'s R5-AnimInspector


$sequence "animseq/creatures/flyer/r2_flyer/flyer_caged_aggro_01" {
	"animseq_flyer_caged_aggro_01_dmx__rm_FE2D498"
	"animseq_flyer_caged_pain_01_dmx__rm_2BCBEED9"

	fadein 0.2
	fadeout 0.2
	activity ACT_IDLE_COMBAT 1
	blendwidth 2
	blend POSEPARAM_5 0 1
	//movement framemovement67
	{ event AE_CL_PLAYSOUND 12 "Flyer_Cage_Slam" } <- CLIENTSIDE AUDIO EVENT!
	{ event AE_CL_PLAYSOUND 22 "Flyer_Cage_JumpLand" } <- CLIENTSIDE AUDIO EVENT!
	{ event AE_CL_PLAYSOUND 20 "Flyer_Cage_Vocal_HeadShake" } <- CLIENTSIDE AUDIO EVENT!
	{ event AE_SV_VSCRIPT_CALLBACK 49 "worldsound:Flyer_Cage_Vocal_SnapScream AUDIO_HEAD" } <- GLOBAL AUDIO EVENT!
	{ event AE_SV_VSCRIPT_CALLBACK 58 "worldsound:Flyer_Cage_Vocal_JawSnap_Hard AUDIO_HEAD" } <- GLOBAL AUDIO EVENT!
	{ event AE_SV_VSCRIPT_CALLBACK 76 "worldsound:Flyer_Cage_Vocal_QuickScream AUDIO_HEAD" } <- GLOBAL AUDIO EVENT!
	{ event AE_CL_PLAYSOUND 83 "Flyer_Cage_JumpLand" } <- CLIENTSIDE AUDIO EVENT!
	{ event AE_SV_VSCRIPT_CALLBACK 97 "worldsound:Flyer_Cage_Vocal_LongScream AUDIO_HEAD" } <- GLOBAL AUDIO EVENT!
	{ event AE_SV_VSCRIPT_CALLBACK 143 "worldsound:Flyer_Cage_Vocal_QuickScream AUDIO_HEAD" } <- GLOBAL AUDIO EVENT!
	{ event AE_SV_VSCRIPT_CALLBACK 164 "worldsound:Flyer_Cage_Vocal_LongScream AUDIO_HEAD" } <- GLOBAL AUDIO EVENT!
	{ event AE_SV_VSCRIPT_CALLBACK 203 "worldsound:Flyer_Cage_Vocal_JawSnap_Light AUDIO_HEAD" } <- GLOBAL AUDIO EVENT!
	{ event AE_SV_VSCRIPT_CALLBACK 223 "worldsound:Flyer_Cage_Vocal_QuickScream AUDIO_HEAD" } <- GLOBAL AUDIO EVENT!
	{ event AE_SV_VSCRIPT_CALLBACK 240 "worldsound:Flyer_Cage_Vocal_MediumScream AUDIO_HEAD" } <- GLOBAL AUDIO EVENT!
	{ event AE_SV_VSCRIPT_CALLBACK 258 "worldsound:Flyer_Cage_Vocal_JawSnap_Light AUDIO_HEAD" } <- GLOBAL AUDIO EVENT!
}

From: flyer_kingscanyon_animated.qc <- derived from the Flyer model from its rmdl, rrig and rseqs, using Someoneatemylastsliceofpizza (SAMLSOP)'s R5-AnimInspector


-

As shown above, audio events can be associated with / assigned to an activity such as

activity ACT_VM_WEAPON_INSPECT 60

by using Animation Events (prefixed with AE inside the .qc's). Animation Events can either be clientsided or trigger serversided callbacks, for global audio! (heard by all players in range)

-

## Script Functions / Commands



## EMIT SOUND - BASIC


### EmitSound

- Signature: EmitSound()
- Purpose: Emit a generic sound
- Note: Base emit function



### EmitSoundOnEntity

- Signature: EmitSoundOnEntity(entity, soundAlias)

 -Purpose: Play a sound on a specified entity

 -Example: EmitSoundOnEntity( player, "Shadow_Legend_Shadow_Loop_1P" )

 -Example: EmitSoundOnEntity( door, "Door_Sliding_Metal_Close" )

 -Example: EmitSoundOnEntity( emitter, quipAlias )

 -Parameters:

 -entity: Target entity to attach sound to

 -soundAlias: String identifier of sound to play



### EmitSoundAtPosition

 -Signature: EmitSoundAtPosition(team, origin, soundAlias, entity)

 -Purpose: Play a sound at a world position (not attached to entity)

 -Example: EmitSoundAtPosition( TEAM_ANY, lootOrigin, "ShadowLegend_Shadow_DeathVanish", player )

 -Example: EmitSoundAtPosition( TEAM_UNASSIGNED, soundPosition, "Door_Single_Metal_Close_Start", door )

 -Parameters:

 -team: TEAM_ANY, TEAM_UNASSIGNED, or specific team

 -origin: Vector position in world

 -soundAlias: String sound identifier

 -entity: Source entity (optional reference)


## EMIT SOUND - VARIATIONS


### EmitSoundOnEntityExceptToPlayer

 -Signature: EmitSoundOnEntity(entity, exceptionPlayer, soundAlias)

 -Purpose: Play sound on entity except to specific player (heard by others)

 -Example: EmitSoundOnEntityExceptToPlayer( emitter, exceptionPlayer, quipAlias )

 -Parameters:

 -entity: Target entity

 -exceptionPlayer: Player who won't hear the sound

 -soundAlias: Sound string


### EmitSoundOnEntityOnlyToPlayer

 -Signature: EmitSoundOnEntityOnlyToPlayer(player, entity, soundAlias)

 -Purpose: Play sound on entity only to specific player (first-person only)

 -Example: EmitSoundOnEntityOnlyToPlayer( player, player, "ShadowLegend_Shadow_Spawn" )

 -Parameters:

 -player: Target player who hears sound

 -entity: Entity where sound originates

 -soundAlias: Sound string


### EmitSoundAtPositionExceptToPlayer

 -Signature: EmitSoundAtPositionExceptToPlayer(team, position, soundAlias, exceptionPlayer)

 -Purpose: Play sound at position except to specific player

 -Note: Heard by all except one player


### EmitSoundAtPositionOnlyToPlaye

 -Signature: EmitSoundAtPositionOnlyToPlayer(player, position, soundAlias)

 -Purpose: Play sound at position only to specific player

 -Note: First-person only audio


### EmitSoundOnEntityToEnemies

 -Signature: EmitSoundOnEntityToEnemies(entity, soundAlias)

 -Purpose: Play sound on entity only to enemies


### EmitSoundOnEntityToTeam

 -Signature: EmitSoundOnEntityToTeam(entity, soundAlias)

 -Purpose: Play sound on entity only to teammates


### EmitSoundOnEntityForLocalPlayer

 -Signature: EmitSoundOnEntityForLocalPlayer(entity, soundAlias)

 -Purpose: Play sound only for local player


## STOP SOUNDS


### StopSoundOnEntity

 -Signature: StopSoundOnEntity(entity, soundAlias)

 -Purpose: Stop a sound on specific entity

 -Example: StopSoundOnEntity( player, "Shadow_Legend_Shadow_Loop_1P" )

 -Example: StopSoundOnEntity( vaultPanel, VAULT_ALARM_SOUND )

 -Parameters:

 -entity: Target entity

 -soundAlias: Sound string to stop


### StopSound

 -Signature: StopSound()

 -Purpose: Stop a generic sound


### StopSoundAtPosition

 -Signature: StopSoundAtPosition(position, soundAlias)

 -Purpose: Stop sound at a world position


## FADE OUT SOUNDS


### FadeOutSoundOnEntity

 -Signature: FadeOutSoundOnEntity(entity, soundAlias, fadeTime)

 -Purpose: Fade out sound on entity over time

 -Example: FadeOutSoundOnEntity( ent, soundAlias, fadeTime )

 -Parameters:

 -entity: Target entity

 -soundAlias: Sound string

 -fadeTime: Time in seconds to fade


### FadeOutSoundOnEntityByName

 -Signature: FadeOutSoundOnEntityByName(entity, soundAlias)

 -Purpose: Fade out sound by name


## SOUND UTILITIES


### SetSoundName

 -Signature: SetSoundName(entity, soundAlias)

 -Purpose: Set the sound name for an entity

 -Example: ambientGeneric.SetSoundName( file.ambientGeneric )

 -Parameters:

 -entity: Target entity

 -soundAlias: Sound identifier


### GetSoundDuration

 -Signature: GetSoundDuration(soundAlias)

 -Purpose: Get duration of a sound in seconds

 -Returns: Float (duration)


### IsSoundStillPlaying

 -Signature: IsSoundStillPlaying(soundAlias)

 -Purpose: Check if a sound is currently playing

 -Returns: Boolean



### SetSoundVolume

 -Signature: SetSoundVolume(soundAlias, volume)

 -Purpose: Set volume for a sound


### GetSoundVolume

 -Signature: GetSoundVolume()

 -Purpose: Get current sound volume


### SetSoundCodeControllerEntity

 -Signature: SetSoundCodeControllerEntity(entity)

 -Purpose: Set entity as sound code controller


### SetSoundCodeControllerValue

 -Signature: SetSoundCodeControllerValue(value)

 -Purpose: Set sound code controller value


### nsetSoundCodeControllerValue

 -Signature: UnsetSoundCodeControllerValue()

 -Purpose: Unset sound code controller value


## WEAPON SOUNDS


### EmitWeaponSound

 -Signature: EmitWeaponSound()

 -Purpose: Emit weapon firing sound


### EmitWeaponSound_1p3p

 -Signature: EmitWeaponSound_1p3p()

 -Purpose: Emit weapon sound with 1st and 3rd person variants


### EmitWeaponSound_Script

 -Signature: EmitWeaponSound_Script()

 -Purpose: Emit weapon sound from script


### EmitWeaponNpcSound

 -Signature: EmitWeaponNpcSound()

 -Purpose: Emit NPC weapon sound


### StopWeaponSound

 -Signature: StopWeaponSound()

 -Purpose: Stop weapon sound
 -

### StopWeaponSound_Script

 -Signature: StopWeaponSound_Script()

 -Purpose: Stop weapon sound from script


## WHIZBY/BULLET SOUNDS


### EmitBulletWhizbyForLocalPlayer

 -Signature: EmitBulletWhizbyForLocalPlayer()

 -Purpose: Play bullet whizby sound only to local player

 -Note: Plays when bullet passes near player


### EmitProjectileWhizbyForLocalPlayer

 -Signature: EmitProjectileWhizbyForLocalPlayer()

 -Purpose: Play projectile whizby sound only to local player


## AI SOUNDS


### EmitAISound

 -Signature: EmitAISound()

 -Purpose: Emit NPC/AI sound


### EmitAISoundToTarget

 -Signature: EmitAISoundToTarget()

 -Purpose: Emit AI sound directed at target


### EmitAISoundWithOwner

 -Signature: EmitAISoundWithOwner()

 -Purpose: Emit AI sound with owner reference


### EmitAISoundWithOwnerToTarget

 -Signature: EmitAISoundWithOwnerToTarget()

 -Purpose: Emit AI sound from owner to target


## VEHICLE SOUNDS


### VehiclePlaySoundOnEntityForOccupants

 -Signature: VehiclePlaySoundOnEntityForOccupants(vehicle, entity, soundAlias)

 -Purpose: Play sound on vehicle for all occupants

### VehiclePlayReliableSoundOnEntityForOccupants

 -Signature: VehiclePlayReliableSoundOnEntityForOccupants(vehicle, entity, soundAlias)

 -Purpose: Play reliable sound (guaranteed delivery) for vehicle occupants


### VehiclePlaySoundOnEntityForNonOccupants

 -Signature: VehiclePlaySoundOnEntityForNonOccupants()

 -Purpose: Play sound for players not in vehicle


## VOICE FUNCTIONS


### GetVoicePackIndex

 -Signature: GetVoicePackIndex()

 -Purpose: Get current voice pack index

 -Returns: Integer


### SetVoicePackIndex

 -Signature: SetVoicePackIndex(index)

 -Purpose: Set voice pack for player


### IsVoiceMuted
 -Signature: IsVoiceMuted()

 -Purpose: Check if voice is muted

 -Returns: Boolean


### IsPlayerVoiceMutedForUID

 -Signature: IsPlayerVoiceMutedForUID(playerUID)

 -Purpose: Check if specific player's voice is muted

 -Returns: Boolean


### TogglePlayerVoiceMute

 -Signature: TogglePlayerVoiceMute()

 -Purpose: Toggle player voice mute state


### TogglePlayerVoiceMutedForUID

 -Signature: TogglePlayerVoiceMutedForUID(playerUID)

 -Purpose: Toggle voice mute for specific player UID


### Script_IsVoiceMuted

 -Signature: Script_IsVoiceMuted()

 -Purpose: Script version of IsVoiceMuted


### Script_PlayTextToSpeech

 -Signature: Script_PlayTextToSpeech(text)

 -Purpose: Play text-to-speech audio
 -

## PAUSE/RESUME


### PauseSoundOnEntity

 -Signature: PauseSoundOnEntity(entity, soundAlias)

 -Purpose: Pause sound on entity



### ResumeSoundOnEntity

 -Signature: ResumeSoundOnEntity(entity, soundAlias)

 -Purpose: Resume paused sound on entity



### PauseUISoundByName

 -Signature: PauseUISoundByName(soundAlias)

 -Purpose: Pause UI sound



### ResumeUISoundByName

 -Signature: ResumeUISoundByName(soundAlias)

 -Purpose: Resume UI sound



## SOUNDSCAPE


### trigger_soundscape

 -Signature: trigger_soundscape()

 -Purpose: Trigger soundscape entity



### playsoundscape

 -Signature: playsoundscape(soundscape)

 -Purpose: Play soundscape



### stopsoundscape

 -Signature: stopsoundscape()

 -Purpose: Stop current soundscape



## COMMON PATTERNS


Pattern 1: Playing a sound on entity

 -EmitSoundOnEntity( entity, soundAlias )


Pattern 2: Stopping a sound

 -StopSoundOnEntity( entity, soundAlias )


Pattern 3: Position-based sound

 -EmitSoundAtPosition( team, origin, soundAlias, sourceEntity )


Pattern 4: First-person only sound

 -EmitSoundOnEntityOnlyToPlayer( player, entity, soundAlias )
 -

Pattern 5: Fading out sound

 -FadeOutSoundOnEntity( entity, soundAlias, fadeTime )



