# Weapon Functions and Commands

This article documents various Weapon functions and commands.

Before any weapon can be used in the engine, it is MANDATORY to precache it, using the function:
```
PrecacheWeapon($"weapontxtconfigname")   
Example: PrecacheWeapon($"mp_weapon_wingman")  
```
## Weapon Get / Set Functions and Methods

### Getting a weapon's owner entity (Player or NPC entity)
```
weapon.GetWeaponOwner() // returns an entity (usually the player entity)
```
### Getting a weapon's viewmodel entity
```
weapon.GetWeaponViewmodel() // returns the viewmodel entity  
```

### Getting a player's active weapon entity

A very commonly used function to get the weapon currently held by the player, using a constant value from the eActiveInventorySlot enumeration:  
```
player.GetActiveWeapon(eActiveInventorySlot.mainHand)   
```

The active inventory slot is obtained from this global enum:
```
global enum eActiveInventorySlot  
{  
	mainHand = 0  
	altHand = 1  
	utility = 2  
}  
```
### Getting one of the player's weapons
```
player.GetNormalWeapon( weaponSlot )   
SURVIVAL_GetWeaponBySlot( player, weaponSlot) <- this function is not a method of the player entity, but takes the player entity as an argument upon call 
```
The two above-mentioned functions get weapon inventory slots from these constants:
```
global const int WEAPON_INVENTORY_SLOT_ANTI_TITAN = 4 // Anti-Titan weapon slot, still exists in Apex Legends  
global const int WEAPON_INVENTORY_SLOT_ANY = -2 // Any  

global const int WEAPON_INVENTORY_SLOT_DUALPRIMARY_0 = 5 // Used for leftover dev akimbo function  
global const int WEAPON_INVENTORY_SLOT_DUALPRIMARY_1 = 6 // Used for leftover dev akimbo function  
global const int WEAPON_INVENTORY_SLOT_DUALPRIMARY_2 = 7 // Used for leftover dev akimbo function  
global const int WEAPON_INVENTORY_SLOT_DUALPRIMARY_3 = 8 // Used for leftover dev akimbo function  

global const int WEAPON_INVENTORY_SLOT_INVALID = -1 // Invalid  

global const int WEAPON_INVENTORY_SLOT_PRIMARY_0 = 0 // Primary  
global const int WEAPON_INVENTORY_SLOT_PRIMARY_1 = 1 // Secondary  

global const int WEAPON_INVENTORY_SLOT_PRIMARY_2 = 2 // Melee (selected from melee select key, 3 by default)  
global const int WEAPON_INVENTORY_SLOT_PRIMARY_3 = 3 // Melee (from direct use melee key, V by default)  
```

### Getting a weapon's class name
```
weapon.GetWeaponClassName() // returns the weapon's name, such as "mp_weapon_nemesis"
```
### Getting / setting a weapon's name
```
weapon.GetWeaponName()
weapon.SetWeaponName()  
```
### Getting a weapon's ammo pool type
```
weapon.GetWeaponAmmoPoolType() <- returns the weapon's ammo pool type   
```
The ammo pool type is obtained from the following global enumeration:
```
global enum eAmmoPoolType  
{  
	bullet = 0  
	special = 1  
	highcal = 2  
	shotgun = 3  
	sniper = 4  
	explosive = 5  
}  
```
### Getting / setting weapon parameters

```
weapon.SetClipCount() - this sets current MAGAZINE ammo, incorrectly labelled as "Clip" by Respawn  
GetWeaponPrimaryAmmoCount( AMMOSOURCE_STOCKPILE )  
weapon.SetWeaponSkin( skinIndex ) <- "Skin" here refers to the $skinfamilies (see the Valve Developer Wiki for QC Commands). It's not used in the same sense as "Legendary Skins", etc - it is a material swap   
```

```
weapon.SetWeaponPrimaryClipCount() <- Magazine ammo capacity, incorrectly labelled as "clip" by Respawn  
weapon.GetWeaponPrimaryClipCount() <- Magazine ammo capacity, incorrectly labelled as "clip" by Respawn  
```

```
weapon.GetWeaponPrimaryClipCountMax() 
weapon.SetWeaponPrimaryClipCountMax()  
```

```
weapon.SetWeaponChargeFraction() <- used by weapons and abilities, highly dependent on the weapon / ability and its implementation  
weapon.SetWeaponChargeFractionForced()  
```

```
weapon.GetWeaponType()  
```

## Weapon Mod functions (Hop-ups, etc.)

Weapon mods are defined inside a key-value pair table contained in the weapon / ability's .txt config file  

Hop-ups are defined as mods inside the .txt config of every weapon that is meant to support them, with their own stats inside their own keyvalue table.

Optics, barrel stabilizers and laser sights are also mods.

For weapons that use a charge mechanic, Respawn also used mods to define certain parameters and add them or remove them selectively from the weapon (i.e.: Nemesis, Bocek Bow, etc.)

IMPORTANT MENTION: A mod cannot be added to a weapon if has not been defined in the weapon's config file!  

Example (excerpt from mp_weapon_alternator.txt):  

```
Mods
	{
		barrel_synchronizer
		{
			"ammo_clip_size"   								"8"
			
			"damage_near_value"   							"32"
			"damage_far_value"								"32"
			"damage_very_far_value"							"32"
			"damage_near_value_titanarmor"					"32"
			"damage_far_value_titanarmor" 					"32"
			"damage_very_far_value_titanarmor" 				"32"
			
			"fire_rate"   									"4"

			"burst_or_looping_fire_sound_start_1p"					""
			"burst_or_looping_fire_sound_middle_1p"					""
			"burst_or_looping_fire_sound_end_1p"					""
			"fire_sound_1_player_1p"						"weapon_devotion_singleshot_1p"
			"fire_sound_1_player_3p"						"weapon_devotion_secondshot_3p"

			"tracer_effect"   								"P_wpn_tracer_pulse"
			"impact_effect_table" 							"pulse_bullet"
		}

		blood_harvest
		{
		}
		
        gold // Note: determines what mods are applied to weapons in Gold rotation
        {
        }
		
		survival_finite_ammo // Note: Regular Battle Royale (known as "survival" mode in the game's code) loot
		{
			"ammo_default_total"							"0"
			"ammo_stockpile_max"							"24"
			"ammo_no_remove_from_stockpile"					"0"

			"low_ammo_fraction" 							"0.3"

			"uses_ammo_pool"								"1"
		}

		hopup_shield_breaker // Disruptor Rounds
		{
			"damage_shield_scale"                       "1.20"
			"projectile_trail_effect_0"                 "P_tracer_proj_smg_shield_breaker"
			"impact_effect_table" 				        "shield_breaker_bullet"

			"burst_or_looping_fire_sound_start_1p"		"Weapon_Alternator_FirstShot_1P"
			"burst_or_looping_fire_sound_middle_1p"		"Weapon_Alternator_Loop_ShieldBreaker_1P"
			"burst_or_looping_fire_sound_end_1p"		"Weapon_Alternator_LoopEnd_ShieldBreaker_1P"

			"burst_or_looping_fire_sound_start_3p"		""
			"burst_or_looping_fire_sound_middle_3p"		"Weapon_Alternator_Loop_ShieldBreaker_3P"
			"burst_or_looping_fire_sound_end_3p"		"Weapon_Alternator_LoopEnd_ShieldBreaker_3P"

			"burst_or_looping_fire_sound_start_npc"		""
			"burst_or_looping_fire_sound_middle_npc"	"Weapon_Alternator_Loop_3p_NPC_a"
			"burst_or_looping_fire_sound_end_npc"		"Weapon_Alternator_LoopEnd_NPC"
		}

		optic_cq_hcog_classic // Note: Optics are enabled or disabled through mods - a hacky implementation because this key-value pair needs to be attached to something
		{
			"bodygroup1_set"			"1"
		}
```
These are the script methods / functions that control weapon mods:

```
weapon.AddMod("ModNameString") // adds one of the previously defined mods to the weapon INSTANCE (only the individual weapon entity!)  
weapon.RemoveMod("ModNameString") // removes one of the currently equipped mods from the weapon INSTANCE (only the individual weapon entity!)  

weapon.HasMod("ModNameString") // returns true or false

weapon.SetWeaponMods(array<string> modsArray) // takes an array of mods as an argument 

weapon.GetMods() // returns an array of registered mods for this weapon (the mods that the weapon supports, from the weapon's .txt file)
GetWeaponMods( weapon ) // where weapon = the weapon entity  
```

### Getting / Setting a weapon's bodygroup

Bodygroups are a long-existing feature in Source games and StudioMDL-compiled models. They represent sections of a model / additional models which can be selectively drawn / hidden depending on in-game circumstances.

Bodygroups are how weapon attachments are displayed on weapons; weapons are compiled with all of their attachments already installed (attachments that have models and affect the appearance of the weapon, so not items like Stocks), and, depending on the attachments picked up and attached by the player, they are displayed / hidden. Optics and suppressors fall within this category. In Titanfall 2, Pro Screens (which tracked eliminations with a specific weapon) were also included as a separate bodygroup.

Bodygroups are also used for different game mechanics such as energizing (the Rampage has a "thermite" bodygroup) or for hiding / displaying ability-related models on legend models (such as Crypto's drone when the drone hasn't been deployed yet, or hiding the drone after it has been deployed)

Bodygroups MUST be defined in the weapon model's .qc file, PRE-compile!

Excerpt from ptpov_peacekeeper.qc:

```
$bodygroup "sight_holo_mag"
{
	blank
	studio "ptpov_peacekeeper_sight_holo_mag_1_lod0.smd"
}
$bodygroup "suppressor_round_large"
{
	blank
	studio "ptpov_peacekeeper_suppressor_round_large_1_lod0.smd"
}

```
Functions for getting / setting bodygroups:

```
viewmodel = the weapon's viewmodel entity, acquired with weapon.GetWeaponViewmodel()
viewmodel.FindBodygroup( "bodygroupnamestring" ) // finds the index for a bodygroup according to its name  
viewmodel.SetBodygroupModelByIndex( bodygroupid, 0 OR 1 ) // the second parameter of the function is a boolean - FALSE or TRUE, which determines whether the bodygroup is HIDDEN or VISIBLE  
```

Optics and suppressors have corresponding mods defined in the weapon's .txt config file. Optics explicitly set bodygroups inside their mod keyvalue table.

Excerpt from mp_weapon_alternator_smg.txt:

```
		optic_cq_hcog_classic
		{
			"bodygroup1_set"			"1"
		}

		optic_cq_hcog_bruiser
		{
		  "bodygroup1_set"			"1"
		}

		optic_cq_holosight
		{
			"bodygroup1_set"			"1"
		}

		optic_cq_holosight_variable
		{
			"bodygroup1_set"			"1"
		}

		optic_cq_threat
		{
			"bodygroup1_set"			"1"
		}
```
Weapon magazines (incorrectly named "clips" by Respawn, even though Bangalore has a voiceline specifically about this) also have .txt / code controlled bodygroups.

Excerpt from mp_weapon_dmr.txt:

```
// Bodygroup - Clip
"clip_bodygroup"                           "clip"
"clip_bodygroup_index_shown"               "0"
"clip_bodygroup_index_hidden"              "1"
"clip_bodygroup_show_for_milestone_0"      "1"
"clip_bodygroup_show_for_milestone_1"      "0"
"clip_bodygroup_show_for_milestone_2"      "1"
"clip_bodygroup_show_for_milestone_3"      "1"
```

## Weapon Parameter Get / Set Functions


```
weapon.GetWeaponSettingInt() <- returns the value for a key which is assigned integer values, from the weapon's .txt config file  
Example: weapon.GetWeaponSettingInt( eWeaponVar.ammo_per_shot )  
```


```
weapon.GetWeaponSettingFloat() <- returns the value for a key which is assigned floating point (decimal) values, from the weapon's .txt config file
Example: weapon.GetWeaponSettingFloat( eWeaponVar.rechamber_time )
```


```
weapon.GetWeaponSettingAsset() <- returns the value for a key which is assigned ASSET values (i.e.: "viewmodel" (key) "mdl/weapons/p2011/ptpov_p2011.rmdl" (value)), from the weapon's .txt config file  
weapon.GetWeaponSettingBool() <- returns the value for a key which is assigned boolean values, from the weapon's .txt config file  
weapon.GetWeaponSettingString() <- returns the value for a key which is assigned a string value, from the weapon's .txt config file  
weapon.GetWeaponSettingVector() <- returns the value for a key which is assigned a vector value, from the weapon's .txt config file  
```


```
weapon.GetWeaponSettingEnum() <- returns the value for a weapon setting inside an Enumeration (Enum)  
Examples:     
weapon.GetWeaponSettingEnum( eWeaponVar.cooldown_type, eWeaponCooldownType )  
weapon.GetWeaponSettingEnum( eWeaponVar.fire_mode, eWeaponFireMode )  
```

## Alternate functions for getting weapon settings

Note: these functions are not methods of the CWeaponX entity class (the generic weapon class)

```
GetWeaponInfoFileKeyField_Global() and its variations, depending on the return type of the function  

Examples:  
GetWeaponInfoFileKeyField_Global( ref, variable )  
GetWeaponInfoFileKeyField_GlobalString( data.baseWeapon, "printName" )  
GetWeaponInfoFileKeyField_GlobalInt( weapon, "ammo_clip_size")  
GetWeaponInfoFileKeyField_GlobalFloat( weaponRef, "burst_fire_delay" )  
GetWeaponInfoFileKeyFieldAsset_Global( weaponName, "playermodel")  
```

## Weapon Callback Functions

Respawn set up a framework for defining callback functions for when certain events happen, inside the native code (inside the binary of the game, r5apex.exe). These callback events are located inside the weapons' .txt config files. They can be found in this list:

```
"OnWeaponActivate"
"OnWeaponDeactivate"

"OnWeaponAttemptOffhandSwitch" // i.e.: mp_ability_mirage_ultimate

"OnWeaponReadyToFire" // i.e.: mp_ability_grapple

"OnWeaponTossPrep" // Used by grenades / ordnance and throwable abilities, i.e.: mp_weapon_deployable_medic
"OnWeaponTossReleaseAnimEvent" // Used by grenades / ordnance and throwable abilities, i.e.: mp_weapon_crypto_drone
"OnWeaponTossCancel // Used by grenades / ordnance and throwable abilities, i.e.: mp_weapon_thermite_grenade
"OnWeaponNpcTossGrenade" // Used by grenades / ordnance and throwable abilities

"OnWeaponChargeBegin" // NOTE: Abilities CAN and DO use these! i.e.: mp_weapon_phase_tunnel (Wraith's Portal!)
"OnWeaponChargeLevelIncreased" // i.e.: mp_weapon_doubletake
"OnWeaponChargeEnd" // i.e.: mp_weapon_energy_shotgun

"OnWeaponPrimaryAttack"
"OnWeaponNpcPrimaryAttack"

"OnClientAnimEvent"
"OnWeaponPrimaryAttackAnimEvent"// i.e.: mp_ability_mobile_respawn_beacon

"OnWeaponCustomActivityStart" // NOTE: Inspecting is also considered a Custom Activity!
"OnWeaponCustomActivityEnd"

"OnProjectileCollision" // i.e.: mp_weapon_grenade_bangalore
"OnWeaponBulletHit" // i.e.: mp_weapon_smart_pistol
"OnProjectileIgnite" // i.e.: mp_weapon_grenade_bangalore	

"OnWeaponRegenEnd" // i.e.: mp_weapon_bubble_bunker

"OnWeaponOwnerChanged" // i.e.: mp_weapon_tesla_trap

"OnWeaponStartZoomIn" // i.e.: mp_weapon_sentinel
"OnWeaponStartZoomOut" // i.e.: mp_weapon_sentinel

```


These are keys in the weapons' associated .txt files which tell the engine which functions to call when these certain events happen, for example:

```
"OnWeaponActivate" -> "OnWeaponActivate_weapon_3030" // this function is declared and defined in mp_weapon_3030.nut, in vscripts / weapons

"OnProjectileCollision" -> "OnProjectileCollision_weapon_doubletake" // this function is declared and defined in mp_weapon_doubletake.nut, in vscripts / weapons

"OnProjectileCollision" -> "OnProjectileCollision_Generic" // used by the Kraber, in mp_weapon_sniper.txt

"OnProjectileCollision" -> "OnProjectileCollision_weapon_basic_bolt" // used by the R-301, in mp_weapon_rspn101.txt

NOTE: It does not matter where the scripts are placed, they are linked at compile-time (depending on which VM they are intended for: SERVER, CLIENT, UI, shared). Scripts are placed in vscripts for organization purposes. It is also important to keep in mind the scope of the functions and structures.
```

NOTE: Some weapons do not have any callbacks explicitly assigned to them specifically in their .txt configs. They use generic scripts and native functions (i.e: mp_weapon_basic_bolt). Examples are the RE-45 AUTO (mp_weapon_autopistol) and the Devotion (mp_weapon_esaw).

There are only a limited amount of callbacks pre-defined by Respawn. They are named in the aforementioned list; for other purposes they have to be custom-made and called.

Abilities (passive, tactical, ultimate) are also created as weapons (they use the weapon template and are technically weapons), which have a different activation / summoning method (offhand) and different inventory slots (offhand).

Examples:

```
mp_ability_gibraltar_shield // (Gibraltar passive)
mp_ability_heal // (Octane passive)
mp_ability_valk_cluster_missile // (Valkyrie tactical)
mp_weapon_black_hole // (Horizon ultimate)
```

## Weapon Firing Functions


```
weapon.FireWeaponBolt ( weaponFireBoltParams )
Takes an instance of the WeaponFireBoltParams global structure as its argument
```

```
global struct WeaponFireBoltParams
{
	vector pos
	vector dir
	float speed
	int scriptTouchDamageType
	int scriptExplosionDamageType
	bool clientPredicted
	int additionalRandomSeed
	bool dontApplySpread
	int projectileIndex
	bool deferred
}
```

```
weapon.FireWeaponGrenade ( weaponFireGrenadeParams )
Takes an instance of the WeaponFireGrenadeParams global structure as its argument
```

```
global struct WeaponFireGrenadeParams
{
	vector pos
	vector vel
	vector angVel
	float fuseTime
	int scriptTouchDamageType
	int scriptExplosionDamageType
	bool clientPredicted
	bool lagCompensated
	bool useScriptOnDamage
	bool isZiplineGrenade = false
	int projectileIndex
}
```

```
weapon.FireWeaponMissile( weaponFireMissileParams )
Takes an instance of the WeaponFireMissileParams global structure as its argument
```

```
global struct WeaponFireMissileParams
{
	vector pos
	vector dir
	float speed
	int scriptTouchDamageType
	int scriptExplosionDamageType
	bool doRandomVelocAndThinkVars
	bool clientPredicted
	int projectileIndex
}
```

```
weapon.FireWeaponDefault( attackParams.pos, attackParams.dir, speedScale, patternScale, ignoreSpread )
Takes attributes belonging to an instance of the WeaponPrimaryAttackParams structure as arguments
```

```
weapon.FireWeaponBullet( attackParams.pos, attackParams.dir, bulletCount, damageType )
Takes attributes belonging to an instance of the WeaponPrimaryAttackParams structure as arguments
```

```
global struct WeaponPrimaryAttackParams
{
	vector pos
	vector dir
	bool firstTimePredicted
	int burstIndex
	int barrelIndex
}
```

```
weapon.FireWeaponBulllet_Special( weaponFireBulletSpecialParams )
Takes an instance of the WeaponFireBulletSpecialParams global structure as its argument
```

```
global struct WeaponFireBulletSpecialParams
{
	vector pos
	vector dir
	int bulletCount
	int scriptDamageType
	bool skipAntiLag
	bool dontApplySpread
	bool doDryFire
	bool noImpact
	bool noTracer
	bool activeShot
	bool doTraceBrushOnly
}
```






















