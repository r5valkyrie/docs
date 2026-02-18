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
	**{ event AE_CL_PLAYSOUND 21 "wpn_nemesis_reload_raise" }**
	**{ event AE_CL_STOPSOUND 59 "wpn_nemesis_reload_raise" }**
	**{ event AE_CL_PLAYSOUND 27 "wpn_nemesis_reload_magInsert" }**
	**{ event AE_CL_STOPSOUND 59 "wpn_nemesis_reload_magInsert" }**
	**{ event AE_CL_PLAYSOUND 29 "wpn_nemesis_reload_magSmack" }**
	**{ event AE_CL_STOPSOUND 59 "wpn_nemesis_reload_magSmack" }**
	**{ event AE_CL_PLAYSOUND 34 "wpn_nemesis_reload_magSpin" }**
	**{ event AE_CL_STOPSOUND 59 "wpn_nemesis_reload_magSpin" }**
	{ event AE_WPN_CLIPBODYGROUP_SHOW 5 "" }
	{ event AE_WPN_RELOAD_MILESTONE_2 21 "" }
	{ event AE_WPN_RUMBLE 21 "reload_pilot_large" }
	{ event AE_WPN_RUMBLE 36 "reload_pilot_small" }
	{ event AE_WPN_FILLAMMO 36 "" }
	{ event AE_WPN_READYTOFIRE 46 "" }
	node NODE_2
}

From: ptpov_nemesis.qc


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

From: flyer_kingscanyon_animated.qc


---

As shown above, audio events can be associated with / assigned to an activity such as

activity ACT_VM_WEAPON_INSPECT 60

by using Animation Events (prefixed with AE inside the .qc's). Animation Events can either be clientsided or trigger serversided callbacks, for global audio! (heard by all players in range)

---

## Script Functions / Commands


StopSoundOnEntity( ENTITY, AUDIOEVENTNAME )

StopSoundAtPosition ( COORDS, "audioeventname" )

weapon.EmitWeaponSound_1p3p( 1PSOUND, 3PSOUND ) - weapon-specific audio method (belongs to CWeaponX entity class, used per-weapon-instance)

EmitSoundOnEntityOnlyToPlayer( entityname, playerentity, "audioeventname" ) - Sound is emitted only to the player (first-person)

EmitSoundOnEntityExceptToPlayer( entityname, playerentity, "audioeventname") - Sound is emitted to all EXCEPT for the player

EmitSoundOnEntity( entityname, "audioeventname" )  - general purpose

EmitSoundAtPosition( TEAM, POSITION_VECTOR_COORDS, "audioeventname", entityname ) - general purpose, emitted at a specified position, team arguments are TEAM_ANY, TEAM_UNASSIGNED, ent.GetTeam(), etc.

EmitSoundAtPositionExceptToPlayer( WHICHTEAM, POSITIONVECTORCOORDS playerentity, "audioeventname" ) - emitted at a specified position to ALL except the player 

EmitSoundOnEntityToTeam( entityname, "audioeventname", PLAYERTEAM ) - team-based, team arguments are TEAM_ANY, TEAM_UNASSIGNED, ent.GetTeam(), etc.

EmitSoundOnEntityToEnemies( entityname, "audioeventname", PLAYERTEAM) - team-based, team arguments are TEAM_ANY, TEAM_UNASSIGNED, ent.GetTeam(), etc.

EmitSoundOnEntityAfterDelay( entity, "audioeventname", delayinseconds ) 

EmitSoundOnEntityDelayed( entityname, "audioeventname", delayinseconds)

EmitSoundOnEntityOnlyToPlayerWithSeek( entity, playerentity, "audioeventname", seekTime) - used for seeking to a specific time point inside an audio file assigned to an event; used a lot for Winter Express audio implementation

GetSoundDuration("AUDIO_EVENT_NAME")


Tip: functions can be threaded with "thread FUNCTIONNAME(arg1, arg2, ...)"! This can be very useful for sustained / constant sound emissions!