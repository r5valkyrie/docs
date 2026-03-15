# Audio Systems and Audio Implementation



For Titanfall 2 and Apex Legends, Respawn chose to use Miles Sound System ("Miles") as their audio middleware, because they had contacts at RAD Game Tools (which was acquired by Epic Games in 2021), which meant they could request features tailor-made to suit their audio pipeline and could influence the direction of the development of Miles and also closely cooperate with the developers of Miles.

Miles Sound System is a relatively niche, closed-source audio middleware currently being developed by Epic Games' Epic Games Tools division.

As is the paradigm with all major audio middlewares, Miles has an event-based system, with PLAY, STOP, PAUSE, RESUME, SEEK events.

Respawn does not use a single naming convention for audio samples and audio events, hence you will see different naming schemes used by different sound designers who worked at Respawn over the years.

You will notice all sorts of naming schemes:

```
weapon_devotion_firstshot_1p
Weapon_Car_FirstShot_1P

Weapon_Car_SecondShot_1P

Weapon_Devotion_Loop_1P
Weapon_Car_Loop_1P

weapon_devotion_loopend_3p
Weapon_Car_LoopEnd_3P

weapon_devotion_lastshot_1p

wpn_pickup_MG_1P

hemlok_dryfire

Weapon_bulletCasings.Bounce

Devotion_LowAmmo_Shot1
car_LowAmmo_Shot1

weapon_devotion_ads_in
Weapon_Car_ADS_In

Weapon_LSTAR_ADS_Out
```

## Footstep Audio Implementation

Different materials with different Surface Properties different different Switches to play different footstep sound depending on the surface material.

In Titanfall 2, footstep audio configurations for different character classes are defined in corresponding .set (settings) files

Examples:

```
From Titanfall 2's englishclient_mp_common.bsp.pak000_dir.vpk, in scripts/players/mp, in pilot_base.set

Some settings have been excluded so as to not bloat the example.

"pilot_base"
{
	raceOrSex					"race_human_male" //TODO: Get code work for this

	"global"
	{
		///////////////////////////////////////////////////
		// other pilot set files overwrite these defaults
		///////////////////////////////////////////////////
		bodymodel			"models/humans/pilots/pilot_medium_reaper_m.mdl"
		armsmodel			"models/weapons/arms/pov_pilot_medium_reaper_m.mdl"
		cockpitmodel		"models/weapons/arms/human_pov_cockpit.mdl"
		///////////////////////////////////////////////////
		//
		///////////////////////////////////////////////////

		ui_targetinfo				"ui/targetinfo_pilot"

		weaponClass					"human"
		automantle					1
		healthpacks					1
		jump						1
		dodge						0
		slide						1
		fov							70
		viewmodelfov				70
		health						100
		healthShield				200
		mechanical					0
		ArmorType					normal
		leech_range					64
		impactSpeed					380
		pitchOffsetScale			9.2
		doFaceAnim					true
		aiEnemy_priority			10
		footstepImpactTable			"pilot_foostep"
		landingImpactTable			"pilot_landing"
		slideEffectTable			"pilot_slide"
		sound_slide_prefix			"pilotslide"
		deathcamRotateSpeed			8
		viewmodelDuckOffset			-0.75
		viewPunchSpring				punch_pilot

		headshotFX 					"P_headshot_pilot"

		[VIEWKICK SETTINGS IRRELEVANT TO SOUND]

		[SLIDE SETTINGS IRRELEVANT TO SOUND]

		[SPRINT SETTINGS IRRELEVANT TO SOUND]

		passThroughThickness		32

		context_action_can_melee	1
		context_action_can_use		1

		[AIM ANGLE SETTINGS IRRELEVANT TO SOUND]

		[AIM ASSIST SETTINGS IRRELEVANT TO SOUND]

		[CHASE CAM SETTINGS IRRELEVANT TO SOUND]

		melee_cone_trace_range		400
		melee_cone_trace_angle		40

		[RUMBLE SETTINGS]

		[COCKPIT SWAY SETTINGS IRRELEVANT TO SOUND]

		[GRAPPLE SETTINGS IRRELEVANT TO SOUND]

		[DEATHCAM SETTINGS IRRELEVANT TO SOUND]

		[PHYSICS SETTINGS IRRELEVANT TO SOUND]
		
		[DAMAGE IMPULSE SETTINGS IRRELEVANT TO SOUND]

		gibModel0						"models/gibs/human_gibs.mdl"

		sound_superJump					"Player.SuperJump"
		sound_superJumpFail				"Player.SuperJumpFail"
		sound_dodge_1p					""
		sound_dodge_3p					""
		sound_dodgeFail					""
		sound_groundImpact				"Pilot.GroundImpact"
		sound_wallrunImpact				"wallrun_impact"
		sound_wallrunSlip				""
		sound_wallrunFall				""
		sound_wallHangStart				"Default.WallCling_Attach"
		sound_wallHangComplete			"Pilot_CrouchStand_1P"
		sound_wallHangFall				"Default.WallCling_Detach"

		sound_freefall_start_1p			"Jumpjet_Freefall_Start_1P"
		sound_freefall_start_3p			"Jumpjet_Freefall_Start_3P"
		sound_freefall_body_1p			"Jumpjet_Freefall_Body_1P"
		sound_freefall_body_3p			"Jumpjet_Freefall_Body_3P"
		sound_freefall_finish_1p		"Jumpjet_Freefall_End_1P"
		sound_freefall_finish_3p		"Jumpjet_Freefall_End_3P"

		sound_jumpjet_jump_start_1p		"Jumpjet_Jump_Start_1P"
		sound_jumpjet_jump_start_3p		"Jumpjet_Jump_Start_3P"
		sound_jumpjet_jump_body_1p		"Jumpjet_Jump_Body_1P"
		sound_jumpjet_jump_body_3p		"Jumpjet_Jump_Body_3P"
		sound_jumpjet_jump_finish_1p	"Jumpjet_Jump_End_1P"
		sound_jumpjet_jump_finish_3p	"Jumpjet_Jump_End_3P"

		sound_jumpjet_jet_start_1p		"Jumpjet_Jet_Start_1P"
		sound_jumpjet_jet_start_3p		"Jumpjet_Jet_Start_3P"
		sound_jumpjet_jet_body_1p		"Jumpjet_Jet_Body_1P"
		sound_jumpjet_jet_body_3p		"Jumpjet_Jet_Body_3P"
		sound_jumpjet_jet_finish_1p		"Jumpjet_Jet_End_1P"
		sound_jumpjet_jet_finish_3p		"Jumpjet_Jet_End_3P"

		sound_jumpjet_wallrun_start_1p	"Jumpjet_Wallrun_Start_1P"
		sound_jumpjet_wallrun_start_3p	"Jumpjet_Wallrun_Start_3P"
		sound_jumpjet_wallrun_body_1p	"Jumpjet_Wallrun_Body_1P"
		sound_jumpjet_wallrun_body_3p	"Jumpjet_Wallrun_Body_3P"
		sound_jumpjet_wallrun_finish_1p	"Jumpjet_Wallrun_End_1P"
		sound_jumpjet_wallrun_finish_3p	"Jumpjet_Wallrun_End_3P"

		sound_pain_layer1_loop 			"Pilot_Wounded_Loop_1P"
		sound_pain_layer1_end  			"Pilot_WoundedLoop_End_1P"
		sound_pain_layer2_start 		"Pilot_Critical_Breath_Start_1P"
		sound_pain_layer2_loop 			"Pilot_Critical_Drone_Loop_1P"
		sound_pain_layer3_loop			"Pilot_Critical_Breath_Loop_1P"
		sound_pain_layer3_end 			"Pilot_Wounded_BreathLoop_End_1P"

		wallrun_hangTimeLimit		4
		wallrun_timeLimit			4.0

		ClassMods
		{
			pas_stealth_movement
			{
			}

			pas_wall_runner
			{
				wallrun_timeLimit		"2.5++"
				wallrun_hangTimeLimit	"21++"
			}

			pas_pilot_hardcore_settings
			{
				health 50
			}

			triple_jump
			{
			}
		}
	}
	"crouch"
	{
		footstepWalkSoundRadius	0
		footstepRunSoundRadius	0
		footstepMinSpeed		80 // DETERMINES THE MINIMUM SPEED REQUIRED TO PLAY FOOTSTEP SOUNDS!
		footstepInterval 		450 // DETERMINES THE INTERVAL AT WHICH TO PLAY FOOTSTEP SOUNDS!

		viewheight 				"0 0 38"
		firstpersonproxyoffset 	"0 0 -38"
		hull_min 				"-16 -16 0"
		hull_max 				"16 16 47"
	}

	"dead"
	{
		viewheight 				"0 0 14"
	}

	"observe"
	{
		hull_min 				"-10 -10 -10"
		hull_max 				"10 10 10"
	}

	"stand"
	{
		footstepWalkSoundRadius	100 // THE RADIUS AT WHICH WALK SOUNDS ARE AUDIBLE
		footstepRunSoundRadius	300 // THE RADIUS AT WHICH RUN SOUNDS ARE AUDIBLE
		footstepMinSpeed		30 // DETERMINES THE MINIMUM SPEED REQUIRED TO PLAY FOOTSTEP SOUNDS!
		footstepInterval		400 // DETERMINES THE INTERVAL AT WHICH TO PLAY FOOTSTEP SOUNDS!
		footstepIntervalSprint	275 // DETERMINES THE INTERVAL AT WHICH TO PLAY FOOTSTEP SOUNDS WHEN SPRINTING!

		viewheight 				"0 0 60"
		firstpersonproxyoffset 	"0 0 -60"
		hull_min 				"-16 -16 0"
		hull_max 				"16 16 72"
	}
}
```

INHERITED BY ALL PILOT CLASSES  

VVVVVVVVVVVVVVVVVVVVVVVVVVVVVV  

```
#base "pilot_mp.set"
"pilot_grapple"
{
	"global"
	{
		sound_standToCrouch		"Pilot_CrouchDown_1P"
		sound_crouchToStand		"Pilot_CrouchStand_1P"
		sound_wallHangSlip		"Pilot_CrouchDown_1P"

		footstep_type          	"human"
	}
}
```

INERITED BY EACH PILOT GENDER  

VVVVVVVVVVVVVVVVVVVVVVVVVVVVV  

```
#base "pilot_grapple.set"
"pilot_grapple_male"
{
	"global"
	{
		bodymodel			    "models/humans/pilots/pilot_medium_geist_m.mdl"
		armsmodel			    "models/weapons/arms/pov_pilot_medium_geist_m.mdl"
		cockpitmodel		    "models/weapons/arms/human_pov_cockpit.mdl"

		quadMoveAnims		    0
	}
}

```

Titans have similar settings files and a similar hierarchy, going from titan_base.set to the titan chassis type (i.e.: titan_stryder.set) to the Titanfall 2 Titan type (i.e.: titan_stryder_leadwall.set, for the Ronin Titan).

BT-7274 has a dedicated settings file: titan_buddy.set, which also inherits titan_base.set.

In Apex Legends, footstep audio configurations for different character classes are defined in corresponding .stgs (settings) files

Example:

```
From common.rpak, in settings/player/mp/pilot_survival_closer.stgs
These are the settings for Wraith

{
	"layoutAsset": "settings_layout/settings_player_layout.rpak",
	"settings": {
		"gibModels": [


              [GIB SETTINGS IRRELEVANT TO AUDIO]


			],
		"gibModelsSoftened": [

			
			
			[GIB SETTINGS IRRELEVANT TO AUDIO]


			],
		"gibAttachments": [
 
			[GIB SETTINGS IRRELEVANT TO AUDIO]

			],
		"poseSettings": [
				{
					"hull_min": "<-16.000000,-16.000000,0.000000>",
					"hull_max": "<16.000000,16.000000,66.000000>",
					"viewheight": "<0.000000,0.000000,56.000000>",
					"firstpersonproxyoffset": "<0.000000,0.000000,-56.000000>",
					"footstepInterval": 400,
					"footstepIntervalSprint": 275,
					"footstepWalkSoundRadius": 100,
					"footstepRunSoundRadius": 300,
					"footstepMinSpeed": 30.000000,

					...

				},
				{
					"hull_min": "<-16.000000,-16.000000,0.000000>",
					"hull_max": "<16.000000,16.000000,47.000000>",
					"viewheight": "<0.000000,0.000000,32.000000>",
					"firstpersonproxyoffset": "<0.000000,0.000000,-32.000000>",
					"footstepInterval": 450,
					"footstepIntervalSprint": 475,
					"footstepWalkSoundRadius": 0,
					"footstepRunSoundRadius": 0,
					"footstepMinSpeed": 20.000000,
					"lowSpeed": -1.000000,
					"speed": 80.000000,
					"sprintspeed": 0.000000,
					"acceleration": 2500.000000,
					"deceleration": -1.000000,
					"lowAcceleration": -1.000000,
					"sprintAcceleration": -1.000000,
					"sprintDeceleration": -1.000000,
					"operatorHoverHeight": 0.000000
				},

					...

			],
		"assetName": "settings/player/mp/pilot_survival_closer.rpak",
		"printName": "",
		"shortPrintName": "",
		"description": "",
		"subclass": "wallrun",
		"voice": "wraith",
		"painDeathSoundType": "CLASS_BANSHEE", // in-dev name for Wraith
		"bodyModel": "mdl/humans/class/light/pilot_light_wraith.rmdl",
		"bodyModelRigWeight": "light",
		"armsModel": "mdl/weapons/arms/pov_pilot_light_wraith.rmdl",
		"cockpitModel": "mdl/weapons/arms/human_pov_cockpit.rmdl",
		"weaponClass": "human",

		...

		"raceOrSex": "race_human_female",
		"unitframe_icon": "rui/hud/unitframes/pilot_banshee",
		"hud_follow_icon": "",
		"hud_guard_icon": "",
		"core_building_icon": "",
		"core_ready_icon": "",
		"autoprecache_script": "",
		"execution_anim": "",
		"startup_sound": "",
		"fx_jetwash_impactTable": "pilot_boost_jetwash",
		"fx_hover_friendly": "P_team_jet_hover_HLD",
		"fx_hover_enemy": "P_enemy_jet_hover_HLD",
		"sharedEnergyNotUsableSound": "",
		"sharedEnergyRegenSound": "",
		"classActivityModifier": "pilot_survival_closer",
		"footstep_type": "wraith",

		...

		"landingImpactTable": "pilot_landing",
		"footstepImpactTable": "pilot_foostep",
		"dodgeImpactTable": "",
		"slideEffectTable": "pilot_slide",
		"aimassist_adspull_centerAttachmentName": "CHESTFOCUS",
		"aimassist_adspull_headshotAttachmentName": "HEADSHOT",
		"gibFX": "",
		"gibSound": "",
		"sound_superJump": "Player.SuperJump",
		"sound_superJumpFail": "Player.SuperJumpFail",
		"sound_dodge_1p": "jumpjet_slide_start_1p",
		"sound_dodge_3p": "jumpjet_slide_start_3p",
		"sound_dodgeFail": "",
		"sound_groundImpact": "Pilot.GroundImpact",
		"sound_wallrunImpact_1p": "wallrun_impact_1P",
		"sound_wallrunImpact_3p": "wallrun_impact_3p",
		"sound_wallHangStart": "Default.WallCling_Attach",
		"sound_wallHangComplete_1p": "",
		"sound_wallHangComplete_3p": "",
		"sound_wallrunSlip": "jumpjet_wallrun_slip_1p",
		"sound_wallHangSlip": "",
		"sound_wallrunFall_1p": "",
		"sound_wallrunFall_3p": "",
		"sound_wallHangFall": "Default.WallCling_Detach",
		"sound_standToCrouch_1p": "wraith_crouchdown_1p",
		"sound_standToCrouch_3p": "wraith_crouchdown_onlygear_3p",
		"sound_crouchToStand_1p": "wraith_crouchstand_1p",
		"sound_crouchToStand_3p": "wraith_crouchstand_onlygear_3p",
		"sound_freefall_start_1p": "",
		"sound_freefall_start_3p": "",
		"sound_freefall_body_1p": "",
		"sound_freefall_body_3p": "",
		"sound_freefall_finish_1p": "",
		"sound_freefall_finish_3p": "",
		"sound_jumpjet_jump_start_1p": "",
		"sound_jumpjet_jump_start_3p": "",
		"sound_jumpjet_jump_body_1p": "",
		"sound_jumpjet_jump_body_3p": "",
		"sound_jumpjet_jump_finish_1p": "",
		"sound_jumpjet_jump_finish_3p": "",
		"sound_jumpjet_jet_start_1p": "",
		"sound_jumpjet_jet_start_3p": "",
		"sound_jumpjet_jet_body_1p": "",
		"sound_jumpjet_jet_body_3p": "",
		"sound_jumpjet_jet_finish_1p": "",
		"sound_jumpjet_jet_finish_3p": "",
		"sound_jumpjet_wallrun_start_1p": "",
		"sound_jumpjet_wallrun_start_3p": "",
		"sound_jumpjet_wallrun_body_1p": "",
		"sound_jumpjet_wallrun_body_3p": "",
		"sound_jumpjet_wallrun_finish_1p": "",
		"sound_jumpjet_wallrun_finish_3p": "",
		"sound_jumpjet_slide_start_1p": "",
		"sound_jumpjet_slide_start_3p": "",
		"sound_boost_short_1p": "Boost_Jump_Start_1P",
		"sound_boost_short_3p": "Boost_Jump_Start_3P",
		"sound_boost_start_1p": "Boost_Jump_Start_1P",
		"sound_boost_start_3p": "Boost_Jump_Start_3P",
		"sound_boost_body_1p": "Boost_Jump_Body_1P",
		"sound_boost_body_3p": "Boost_Jump_Body_3P",
		"sound_boost_finish_1p": "Boost_Jump_End_1P",
		"sound_boost_finish_3p": "Boost_Jump_End_3P",
		"sound_glide_start_1p": "Boost_Glide_Start_1P",
		"sound_glide_start_3p": "Boost_Glide_Start_3P",
		"sound_glide_body_1p": "Boost_Glide_Body_1P",
		"sound_glide_body_3p": "Boost_Glide_Body_3P",
		"sound_glide_finish_1p": "Boost_Glide_End_1P",
		"sound_glide_finish_3p": "Boost_Glide_End_3P",
		"sound_jetpack_start_1p": "Boost_Glide_Start_1P",
		"sound_jetpack_start_3p": "Boost_Glide_Start_3P",
		"sound_jetpack_body_1p": "Boost_Glide_Body_1P",
		"sound_jetpack_body_3p": "Boost_Glide_Body_3P",
		"sound_jetpack_finish_1p": "Boost_Glide_End_1P",
		"sound_jetpack_finish_3p": "Boost_Glide_End_3P",
		"sound_jetpack_afterburner_start_1p": "",
		"sound_jetpack_afterburner_start_3p": "",
		"sound_jetpack_afterburner_body_1p": "",
		"sound_jetpack_afterburner_body_3p": "",
		"sound_jetpack_afterburner_finish_1p": "",
		"sound_jetpack_afterburner_finish_3p": "",
		"sound_hover_start_1p": "Boost_Hover_Start_1P",
		"sound_hover_start_3p": "Boost_Hover_Start_3P",
		"sound_hover_body_1p": "Boost_Hover_Body_1P",
		"sound_hover_body_3p": "Boost_Hover_Body_3P",
		"sound_hover_finish_1p": "Boost_Hover_End_1P",
		"sound_hover_finish_3p": "Boost_Hover_End_3P",
		"sound_climb_start_1p": "",
		"sound_climb_start_3p": "",
		"sound_climb_body_1p": "",
		"sound_climb_body_3p": "",
		"sound_climb_finish_1p": "",
		"sound_climb_finish_3p": "",
		"sound_jetwash_start_1p": "Boost_Hover_Start_1P",
		"sound_jetwash_start_3p": "Boost_Hover_Start_3P",
		"sound_jetwash_body_1p": "Boost_Hover_Body_1P",
		"sound_jetwash_body_3p": "Boost_Hover_Body_3P",
		"sound_jetwash_finish_1p": "Boost_Hover_End_1P",
		"sound_jetwash_finish_3p": "Boost_Hover_End_3P",
		"sound_boost_meter_depleted_1p": "",
		"sound_boost_meter_depleted_3p": "",
		"sound_boost_meter_recharged_1p": "",
		"sound_boost_meter_recharged_3p": "",
		"sound_boost_meter_fail_1p": "",
		"sound_boost_meter_fail_3p": "",
		"sound_pain_layer1_start": "",
		"sound_pain_layer1_loop": "Pilot_Wounded_Loop_1P",
		"sound_pain_layer1_end": "Pilot_WoundedLoop_End_1P",
		"sound_pain_layer2_start": "Pilot_Critical_Breath_Start_1P",
		"sound_pain_layer2_loop": "Pilot_Critical_Breath_Loop_1P",
		"sound_pain_layer2_end": "Pilot_Wounded_BreathLoop_End_1P",
		"sound_pain_layer3_start": "",
		"sound_pain_layer3_loop": "",
		"sound_pain_layer3_end": "",
		"sound_grapple_fire_1p": "Pilot_Grapple_Fire",
		"sound_grapple_fire_3p": "Pilot_Grapple_Fire_3P",
		"sound_grapple_retract_1p": "Pilot_Grapple_Retract_1P",
		"sound_grapple_retract_3p": "Pilot_Grapple_Retract_3P",
		"sound_grapple_traverse_1p": "PIlot_Grapple_Traverse_1P",
		"sound_grapple_traverse_3p": "Pilot_Grapple_Traverse_3P",
		"sound_grapple_swing_1p": "PIlot_Grapple_Traverse_1P",
		"sound_grapple_swing_3p": "Pilot_Grapple_Traverse_3P",
		"sound_grapple_reel_in_target_1p": "Pilot_Grapple_ReelInTarget_1P",
		"sound_grapple_reel_in_target_3p": "Pilot_Grapple_ReelInTarget_3P",
		"sound_grapple_reset_1p": "Pilot_Grapple_Reset",
		"sound_grapple_reset_3p": "Pilot_Grapple_Reset_3P",
		"player_sound_fallingEffects_1p": "player_fallingdescent_windrush",
		"sound_slide_prefix": "wraith_slide",
		"ui_targetinfo": "ui/targetinfo_survival",
		"camera_lcd": "",
		"camera_lcdStartup": "",
		"headshotFX": "P_headshot_pilot",
		"viewPunchSpring": "punch_pilot",
		"generalClass": 2,
		"fov": 70.000000,
		"viewmodelfov": 70.000000,
		"viewmodelDuckOffset": -0.751000,
	    
		...

		"stealthSounds": false
	},
	"$modNames": [
		"pas_stealth_movement",
		"pas_wall_runner",
		"pas_pilot_hardcore_settings",
		"zero_g",
		"zero_g_follow",
		"shadowfall",
		"pas_ads_hover",
		"pas_power_cell",
		"pas_wallhang",
		"pas_fast_health_regen",
		"enable_doublejump",
		"enable_wallrun",
		"disable_slide",
		"disable_targetinfo",
		"jumpkit_lv1",
		"jumpkit_lv2",
		"jumpkit_lv3",
		"empty_handed_run",
		"hub_settings",
		"bleedout",
		"shadow_squad"
	],
	"$modValues": [
		
			...

		{ // originally mapped to offset 1176
			"index": 11,
			"type": "string",
			"value": "",
			"field": "sound_jumpjet_jump_start_1p"
		},
		{ // originally mapped to offset 1184
			"index": 11,
			"type": "string",
			"value": "ot_survival_closer.rpak",
			"field": "sound_jumpjet_jump_start_3p"
		},
		{ // originally mapped to offset 1192
			"index": 11,
			"type": "string",
			"value": "oser.rpak",
			"field": "sound_jumpjet_jump_body_1p"
		},
		{ // originally mapped to offset 1200
			"index": 11,
			"type": "string",
			"value": "r.rpak",
			"field": "sound_jumpjet_jump_body_3p"
		},
		{ // originally mapped to offset 1208
			"index": 11,
			"type": "string",
			"value": "pak",
			"field": "sound_jumpjet_jump_finish_1p"
		},
		{ // originally mapped to offset 1216
			"index": 11,
			"type": "string",
			"value": "ANSHEE",
			"field": "sound_jumpjet_jump_finish_3p"
		},
		{ // originally mapped to offset 1224
			"index": 11,
			"type": "string",
			"value": "l/humans/class/light/pilot_light_wraith.rmdl",
			"field": "sound_jumpjet_jet_start_1p"
		},
		{ // originally mapped to offset 1232
			"index": 11,
			"type": "string",
			"value": "rmdl",
			"field": "sound_jumpjet_jet_start_3p"
		},
		{ // originally mapped to offset 1240
			"index": 11,
			"type": "string",
			"value": "ons/arms/pov_pilot_light_wraith.rmdl",
			"field": "sound_jumpjet_jet_body_1p"
		},
		{ // originally mapped to offset 1248
			"index": 11,
			"type": "string",
			"value": "dl/weapons/arms/human_pov_cockpit.rmdl",
			"field": "sound_jumpjet_jet_body_3p"
		},
		{ // originally mapped to offset 1256
			"index": 11,
			"type": "string",
			"value": "man_pov_cockpit.rmdl",
			"field": "sound_jumpjet_jet_finish_1p"
		},
		{ // originally mapped to offset 1264
			"index": 11,
			"type": "string",
			"value": "pov_cockpit.rmdl",
			"field": "sound_jumpjet_jet_finish_3p"
		},
		{ // originally mapped to offset 1272
			"index": 11,
			"type": "string",
			"value": "t.rmdl",
			"field": "sound_jumpjet_wallrun_start_1p"
		},
		{ // originally mapped to offset 1280
			"index": 11,
			"type": "string",
			"value": "_human_female",
			"field": "sound_jumpjet_wallrun_start_3p"
		},
		{ // originally mapped to offset 1288
			"index": 11,
			"type": "string",
			"value": "human_female",
			"field": "sound_jumpjet_wallrun_body_1p"
		},
		{ // originally mapped to offset 1296
			"index": 11,
			"type": "string",
			"value": "uman_female",
			"field": "sound_jumpjet_wallrun_body_3p"
		},
		{ // originally mapped to offset 1304
			"index": 11,
			"type": "string",
			"value": "unitframes/pilot_banshee",
			"field": "sound_jumpjet_wallrun_finish_1p"
		},
		{ // originally mapped to offset 1312
			"index": 11,
			"type": "string",
			"value": "shee",
			"field": "sound_jumpjet_wallrun_finish_3p"
		},
		{ // originally mapped to offset 1072
			"index": 11,
			"type": "string",
			"value": "",
			"field": "sound_wallrunFall_1p"
		},
		{ // originally mapped to offset 1080
			"index": 11,
			"type": "string",
			"value": "sh",
			"field": "sound_wallrunFall_3p"
		},
		{ // originally mapped to offset 1128
			"index": 11,
			"type": "string",
			"value": "h",
			"field": "sound_freefall_start_1p"
		},
		{ // originally mapped to offset 1136
			"index": 11,
			"type": "string",
			"value": "team_jet_hover_HLD",
			"field": "sound_freefall_start_3p"
		},
		{ // originally mapped to offset 1144
			"index": 11,
			"type": "string",
			"value": "t_hover_HLD",
			"field": "sound_freefall_body_1p"
		},
		{ // originally mapped to offset 1152
			"index": 11,
			"type": "string",
			"value": "LD",
			"field": "sound_freefall_body_3p"
		},
		{ // originally mapped to offset 1160
			"index": 11,
			"type": "string",
			"value": "_jet_hover_HLD",
			"field": "sound_freefall_finish_1p"
		},
		{ // originally mapped to offset 1168
			"index": 11,
			"type": "string",
			"value": "HLD",
			"field": "sound_freefall_finish_3p"
		},
		{ // originally mapped to offset 1176
			"index": 11,
			"type": "string",
			"value": "survival_closer",
			"field": "sound_jumpjet_jump_start_1p"
		},
		{ // originally mapped to offset 1184
			"index": 11,
			"type": "string",
			"value": "wraith",
			"field": "sound_jumpjet_jump_start_3p"
		},
		{ // originally mapped to offset 1192
			"index": 11,
			"type": "string",
			"value": "ump",
			"field": "sound_jumpjet_jump_body_1p"
		},
		{ // originally mapped to offset 1200
			"index": 11,
			"type": "string",
			"value": "mp",
			"field": "sound_jumpjet_jump_body_3p"
		},
		{ // originally mapped to offset 1208
			"index": 11,
			"type": "string",
			"value": "e_pilotOnDoubleJump",
			"field": "sound_jumpjet_jump_finish_1p"
		},
		{ // originally mapped to offset 1216
			"index": 11,
			"type": "string",
			"value": "ubleJump",
			"field": "sound_jumpjet_jump_finish_3p"
		}
			...
		},
		{ // originally mapped to offset 808
			"index": 20,
			"type": "string",
			"value": "le_pilotOnWallrunTimeout",
			"field": "footstep_type"
		}
	]	
}		
```

## Weapon Audio Implementation

All weapons have both first person (1P) and third person (3P) sound versions.

All weapons have a weapon pick-up sound, a weapon drop sound, weapon impact sounds (defined in an impact table), an ADS-In (Aim Down Sights) and an ADS-Out sound.

Some weapons have Raise (Pull Out) and Lower (Holster) sounds.

All weapons have a dry fire (empty magazine) sound, which is usually a generic weapon class based sound.

All Apex Legends weapons have inspect sounds (many of them generic class based weapons or repurposed from another weapon) because the game was designed with microtransactions in mind.

IMPORTANT: Weapon Mods (such as the Turbocharger, Disruptor Rounds, Hammerpoint Rounds, Anvil Receiver, Skullpiercer Rifling, etc.) can override the base sounds of weapons and have their own sounds:

```
        hopup_turbocharger
        {
			// faster ROF spin up
			"fire_rate"   									"6.8"  // start at higher rof
			"fire_rate_max_time_speedup"					"0.85"  // takes less time to spin up

			// Audio
			"looping_sounds"								"1"

			"burst_or_looping_fire_sound_start_1p"			"weapon_devotion_firstshot_1p"
			"burst_or_looping_fire_sound_middle_1p"			"Weapon_Devotion_Turbo_Loop_1P"
			"burst_or_looping_fire_sound_end_1p"			"weapon_devotion_lastshot_1p"

			"burst_or_looping_fire_sound_start_3p"			""
			"burst_or_looping_fire_sound_middle_3p"			"Weapon_Devotion_Turbo_Loop_3P"
			"burst_or_looping_fire_sound_end_3p"			""

			"burst_or_looping_fire_sound_start_npc"			""
			"burst_or_looping_fire_sound_middle_npc"		""
			"burst_or_looping_fire_sound_end_npc"			""

            ...

        }    

```

### Semi-automatic weapons

The simplest audio implementation, only uses pitch and sample randomization for each shot, coupled with a randomized selection of the weapon-specific gunshot tail and a randomized selection of an environment-specific gunshot tail.

Some semi-automatic weapons have a special charge mechanic (such as the Charge Rifle, mp_weapon_defender), in which case they also have:

- A Charge-Up / Wind-Up sound
- A Charge-Down / Wind-Down sound

### Automatic weapons

Whenever the weapon is fired, an initial First Shot sound is played. Some weapons also have a Second Shot sound.

Whenever a new trigger pull is registered, a trigger pull sound is played.

If +fire released after one shot, play single shot layers, a randomized selection of the weapon-specific gunshot tail and a randomized selection of an environment-specific gunshot tail.

If +fire continues to be held, transition to auto fire middle loop (in Apex, for auto weapons, this is usually one audio clip)

Respawn seems to have consolidated and integrated low-ammo mech loop directly into the auto fire loop, together with volume automation for a fade-in.

On top of the low-ammo mech integrated into the loop itself, there are also low-ammo one-shots layers that can be defined from inside the weapon's .txt config. There can be anywhere from a whopping 15 different low-ammo sounds (The L-STAR) to a single low-ammo sound.

Automatic weapons almost always have weapon firing divided into 3 sections:

- Fire Sound Start: usually the First Shot sound
- Fire Sound Middle: the middle section loop
- Fire Sound End: the tail of the loop

Some weapons have a special charge mechanic, in which case they also have:

- A Charge-Up / Wind-Up sound (sometimes it's integrated into the firing loop, like with the devotion)
- A Charge-Down / Wind-Down sound (usually handled by VScripts, through callback functions; check the weapon's .txt file for the assigned callbacks for each weapon)

```

```

Examples of weapon sound definitions from weapon .txt files: 

- The Devotion (mp_weapon_esaw also refered to as hemlocklmg, since it was developed from the Hemlok platform, like the Volt, which is also known as the hemloksmg. The same is the case with the C.A.R. (CAR101) and the Longbow DMR (DMR101))  

```
	"burst_or_looping_fire_sound_start_1p"			"weapon_devotion_firstshot_1p"
	"burst_or_looping_fire_sound_middle_1p"			"Weapon_Devotion_Loop_1P"
	"burst_or_looping_fire_sound_end_1p"			"weapon_devotion_lastshot_1p"

	"burst_or_looping_fire_sound_start_3p"			""
	"burst_or_looping_fire_sound_middle_3p"			"Weapon_Devotion_Loop_3P"
	"burst_or_looping_fire_sound_end_3p"			"weapon_devotion_loopend_3p"

	"burst_or_looping_fire_sound_start_npc"			""
	"burst_or_looping_fire_sound_middle_npc"		""
	"burst_or_looping_fire_sound_end_npc"			""

	"sound_dryfire"									"hemlok_dryfire"
	"sound_pickup"									"wpn_pickup_MG_1P"
	"sound_trigger_pull"							"Weapon_LMG_Trigger"

	"looping_sounds"								"1"

	//Sounds
	"sound_zoom_in"									"weapon_devotion_ads_in"
	"sound_zoom_out"								"weapon_devotion_ads_out"

	"fire_sound_1_player_1p"						"Weapon_Devotion_SecondShot_1P"
	"fire_sound_1_player_3p"						""
	"fire_sound_1_npc"								"Weapon_ColdWar_Fire_3P"
	//"fire_sound_2_player_1p"						"Weapon_bulletCasings.Bounce"
	"fire_sound_2_player_3p"						"Weapon_devotion_SecondShot_3P"

	"low_ammo_sound_name_1"							"Devotion_LowAmmo_Shot1"
	"low_ammo_sound_name_2"							"Devotion_LowAmmo_Shot2"
	"low_ammo_sound_name_3"							"Devotion_LowAmmo_Shot3"
	"low_ammo_sound_name_4"							"Devotion_LowAmmo_Shot4"
	"low_ammo_sound_name_5"							"Devotion_LowAmmo_Shot5"
	"low_ammo_sound_name_6"							"Devotion_LowAmmo_Shot6"
```

- The C.A.R. (mp_weapon_car)

```
	"sound_dryfire"									"assault_rifle_dryfire"
	"sound_pickup"									"wpn_pickup_SMG_1P"
	"sound_trigger_pull"							"Weapon_Hemlok_Trigger"
	"sound_zoom_in"									"Weapon_Car_ADS_In"
	"sound_zoom_out"								"Weapon_Car_ADS_Out"

	"looping_sounds"								"1"

    "fire_sound_1_player_1p"						""
    "fire_sound_1_player_3p"						""
    "fire_sound_1_npc"								"Weapon_Car_SecondShot_NPC"

	"fire_sound_2_player_1p" 						"Weapon_Car_SecondShot_1P"
	"fire_sound_2_player_3p" 						""

    "burst_or_looping_fire_sound_start_1p"			"Weapon_Car_FirstShot_1P"
    "burst_or_looping_fire_sound_middle_1p"			"Weapon_Car_Loop_1P"
    "burst_or_looping_fire_sound_end_1p"			""

    "burst_or_looping_fire_sound_start_3p"			""
    "burst_or_looping_fire_sound_middle_3p"			"Weapon_Car_Loop_3P"
    "burst_or_looping_fire_sound_end_3p"			"Weapon_Car_LoopEnd_3P"

    "burst_or_looping_fire_sound_start_npc"			""
    "burst_or_looping_fire_sound_middle_npc"		"Weapon_Car_Loop_3P_NPC_A"
    "burst_or_looping_fire_sound_end_npc"			"Weapon_Car_LoopEnd_NPC"

	"low_ammo_sound_name_1"							"car_LowAmmo_Shot1"
```

- The L-STAR

```
	"looping_sounds"								"1"

	"sound_zoom_in"									"Weapon_LSTAR_ADS_In"
	"sound_zoom_out"								"Weapon_LSTAR_ADS_Out"

	"burst_or_looping_fire_sound_start_1p"			"Weapon_LSTAR_FirstShot_1P"
	"burst_or_looping_fire_sound_middle_1p"			"Weapon_LSTAR_Loop_1P"
	"burst_or_looping_fire_sound_end_1p"			"Weapon_LSTAR_LoopEnd_1P"

	"burst_or_looping_fire_sound_start_3p"			"Weapon_LSTAR_FirstShot_3P"
	"burst_or_looping_fire_sound_middle_3p"			"Weapon_LSTAR_Loop_3P"
	"burst_or_looping_fire_sound_end_3p"			"Weapon_LSTAR_LoopEnd_3P"

	"burst_or_looping_fire_sound_start_npc"			"Weapon_LSTAR_FirstShot_3P_npc_a"
	"burst_or_looping_fire_sound_middle_npc"		"Weapon_LSTAR_Loop_3P_npc_a"
	"burst_or_looping_fire_sound_end_npc"			"Weapon_LSTAR_LoopEnd_3P_npc_a"

	"sound_dryfire"									"lstar_dryfire"
	"sound_pickup"									"wpn_pickup_MG_1P"
	"low_ammo_sound_name_1"							"LSTAR_LowAmmo_Shot1"
	"low_ammo_sound_name_2"							"LSTAR_LowAmmo_Shot2"
	"low_ammo_sound_name_3"							"LSTAR_LowAmmo_Shot3"
	"low_ammo_sound_name_4"							"LSTAR_LowAmmo_Shot4"
	"low_ammo_sound_name_5"							"LSTAR_LowAmmo_Shot5"
	"low_ammo_sound_name_6"							"LSTAR_LowAmmo_Shot6"
	"low_ammo_sound_name_7"							"LSTAR_LowAmmo_Shot7"
	"low_ammo_sound_name_8"							"LSTAR_LowAmmo_Shot8"
	"low_ammo_sound_name_9"							"LSTAR_LowAmmo_Shot9"
	"low_ammo_sound_name_10"						"LSTAR_LowAmmo_Shot10"
	"low_ammo_sound_name_11"						"LSTAR_LowAmmo_Shot11"
	"low_ammo_sound_name_12"						"LSTAR_LowAmmo_Shot12"
	"low_ammo_sound_name_13"						"LSTAR_LowAmmo_Shot13"
	"low_ammo_sound_name_14"						"LSTAR_LowAmmo_Shot14"
	"low_ammo_sound_name_15"						"LSTAR_LowAmmo_Shot15"    
```


### Burst fire weapons

Burst fire weapons: 

```
	"sound_dryfire"									"hemlok_dryfire"
	"sound_pickup"									"wpn_pickup_Rifle_1P"
	"sound_trigger_pull"							"Weapon_Hemlok_Trigger"
	"sound_zoom_in"									"Weapon_Hemlok_ADS_In"
	"sound_zoom_out"								"Weapon_Hemlok_ADS_Out"

	// Sound

	"fire_sound_1_player_1p"						"Weapon_bulletCasings.Bounce"
	"fire_sound_1_player_3p"						"Weapon_bulletCasings.Bounce"

	"fire_sound_partial_burst_player_1p"			"Weapon_Hemlok_SingleShot_1P"
	"fire_sound_partial_burst_player_3p"			"Weapon_Hemlok_SingleShot_3P"

	"burst_or_looping_fire_sound_start_1p"			"Weapon_Hemlok_FirstShot_1P"
	"burst_or_looping_fire_sound_middle_1p"			"weapon_hemlok_firstshot_1p_Env"
	"burst_or_looping_fire_sound_end_1p"			"weapon_hemlok_firstshot_1p_Env"

	"burst_or_looping_fire_sound_start_3p"			"Weapon_Hemlok_FirstShot_3P"
	"burst_or_looping_fire_sound_middle_3p"			""
	"burst_or_looping_fire_sound_end_3p"			""

	"burst_or_looping_fire_sound_start_npc"			"Weapon_Hemlok_FirstShot_npc"
	"burst_or_looping_fire_sound_middle_npc"		""
	"burst_or_looping_fire_sound_end_npc"			""

	"low_ammo_sound_name_1"							"Hemlok_LowAmmo_Shot1"
	"low_ammo_sound_name_2"							"Hemlok_LowAmmo_Shot1"
	"low_ammo_sound_name_3"							"Hemlok_LowAmmo_Shot1"
	"low_ammo_sound_name_4"							"Hemlok_LowAmmo_Shot2"
	"low_ammo_sound_name_5"							"Hemlok_LowAmmo_Shot2"
```

## Map Audio Entities

Soundscape Triggers

trigger_soundscape

Ambiences

ambience_generic

Sound floors

sound_floor


## Other Audio Emitter Entities

Entities such as props can be programmed from VScripts to emit certain sounds