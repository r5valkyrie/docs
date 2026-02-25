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
### Getting / Setting a weapon's bodygroup
```
viewmodel.FindBodygroup( "bodygroupnamestring" ) // finds the index for a bodygroup according to its name  
viewmodel.SetBodygroupModelByIndex( bodygroupid, 0 OR 1 ) // the second parameter of the function is a boolean - FALSE or TRUE, which determines whether the bodygroup is HIDDEN or VISIBLE  
```
### Getting a player's active weapon entity

A very commonly used function to get the weapon currently held by the player, using a constant value from the eActiveInventorySlot enumeration:  
player.GetActiveWeapon(eActiveInventorySlot.mainHand)   

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

player.GetNormalWeapon( weaponSlot )   
SURVIVAL_GetWeaponBySlot( player, weaponSlot) <- this function is not a method of the player entity, but takes the player entity as an argument upon call 

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

### Getting a weapon's engine class name

weapon.GetWeaponClassName() // returns the weapon's engine class name, such as CWeaponX  

### Getting / setting a weapon's name

weapon.GetWeaponName() // returns the weapon's name, such as "mp_weapon_nemesis"  
weapon.SetWeaponName()  

### Getting a weapon's ammo pool type

weapon.GetWeaponAmmoPoolType() <- returns the weapon's ammo pool type   

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

weapon.HasMod("modnamestring") - mods are defined inside a key-value pair table contained in the weapon / ability's .txt config file  

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
```
weapon.AddMod("ModName") <- adds one of the previously defined mods to the weapon INSTANCE (only the individual weapon entity!)  
weapon.RemoveMod("modnamestring") <- removes one of the currently equipped mods from the weapon INSTANCE (only the individual weapon entity!)  
weapon.SetWeaponMods() <- takes an array of mods as an argument  
weapon.GetMods() <- returns an array registered mods for this weapon  
GetWeaponMods( weapon )   
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




























