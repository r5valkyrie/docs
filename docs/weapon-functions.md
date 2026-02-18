## Weapon Commands / Functions

PrecacheWeapon($"weapontxtconfigname") <- MANDATORY in order to register a new weapon and be able to use it in-game!

Example: PrecacheWeapon($"mp_weapon_wingman")

weapon.GetWeaponOwner() - returns an entity (usually the player entity)

weapon.GetWeaponViewmodel() - returns the viewmodel entity

viewmodel.FindBodygroup( "bodygroupnamestring" ) <- finds the index for a bodygroup according to its name

viewmodel.SetBodygroupModelByIndex( bodygroupid, 0 OR 1 ) - the second parameter of the function is a boolean - FALSE or TRUE, which determines whether the bodygroup is HIDDEN or VISIBLE

---

weapon.HasMod("modnamestring") - mods are defined inside a key-value pair table contained in the weapon / ability's .txt config file

Example (from mp_weapon_alternator.txt):

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

.
.
.


		optic_cq_threat // Note: Optics are enabled or disabled through mods - a hacky implementation because this key-value pair needs to be attached to something
		{
			"bodygroup1_set"			"1"
		}

		bullets_mag_l1 // Note: Attachments are enabled or disabled through mods - a hacky implementation because this key-value pair needs to be attached to something
 
 // NOTE: "bullets" is the internal name for the "Light" ammo type! Heavy = highcal, "Energy" = special, "Shotgun" = shotgun, Sniper = sniper, Arrows = arrow

 // Ammo types are declared in ammo_pool_types.txt, then the entries are read by the engine, which generates client and server enums named eAmmoPoolType (in codeconsts_client.txt and codeconsts_server.txt, respectively)

		{
			"ammo_clip_size"   						"24"
		}

.
.
.


        bullets_mag_l4 // Note: Attachments are enabled or disabled through mods - a hacky implementation because this key-value pair needs to be attached to something
        {
            "ammo_clip_size"   						"29"
        }

		// // NOTE: override barrel FX for both Alternator barrels
		// barrel_stabilizer_l1
		// {
			// "viewkick_yaw_base" 				"*0.85"
			// "viewkick_yaw_random"   			"*0.8"

			// "fx_muzzle_flash_view"							""
			// "fx_muzzle_flash_world"							""
			// "fx_muzzle_flash_attach"						""
		// }

.
.
.


		// barrel_stabilizer_l4_flash_hider
		// {
			// "viewkick_pitch_base" 				"*0.8"
			// "viewkick_pitch_random"   			"*0.65"
			// "viewkick_yaw_base" 				"*0.75"
			// "viewkick_yaw_random"   			"*0.7"

			// "fx_muzzle_flash_view"							""
			// "fx_muzzle_flash_world"							""
			// "fx_muzzle_flash_attach"						""
		// }
		"laser_sight_l1"
		{
			//"targeting_laser_enabled" "1"
			//"custom_laser_sight_color_enabled" "1"
			"spread_stand_hip" "*0.90"
			"spread_stand_hip_run" "*0.90"
			"spread_stand_hip_sprint" "*0.90"
			"spread_crouch_hip" "*0.90"
			"spread_air_hip" "*0.90"
			"spread_moving_increase_rate" "*0.90"
			"spread_moving_decay_rate" "*1.10"
			"spread_decay_rate" "*1.10"
			"spread_decay_delay" "*0.90"
			"spread_max_kick_stand_hip" "*0.90"
			"spread_max_kick_crouch_hip" "*0.90"
			"spread_max_kick_air_hip" "*0.90"
			"spread_kick_on_fire_stand_hip" "*0.90"
			"spread_kick_on_fire_crouch_hip" "*0.90"
			"spread_kick_on_fire_air_hip" "*0.90"
			"bodygroup32_set" "1"
		}

.
.
.

		
		infinite_ammo // Infinite ammo mod
		{
			"ammo_min_to_fire"                      "0"
			"ammo_no_remove_from_clip"              "1"
			"ammo_no_remove_from_stockpile"         "1"
			"low_ammo_fraction"                     "0.0"
		}
	}



weapon.AddMod("modnamestring") <- adds one of the previously defined mods to the weapon INSTANCE (only the individual weapon entity!)

IMPORTANT MENTION! A mod cannot be added to a weapon if has not been defined in the weapon's config file!

weapon.RemoveMod("modnamestring") <- removes one of the currently equipped mods from the weapon INSTANCE (only the individual weapon entity!)

weapon.SetWeaponMods() <- takes an array of mods as an argument

weapon.GetMods() <- returns an array of mods applied to the weapon

GetWeaponMods( weapon ) <- returns an array of mods applied to the weapon, this function is not a method of the CWeaponX entity class, but effectively accomplishes the same function as the above-mentioned method

---

weapon.GetWeaponSettingInt() <- returns the value for a key which is assigned integer values, from the weapon's .txt config file

Example:

weapon.GetWeaponSettingInt( eWeaponVar.ammo_per_shot )


weapon.GetWeaponSettingFloat() <- returns the value for a key which is assigned floating point (decimal) values, from the weapon's .txt config file

Example:

weapon.GetWeaponSettingFloat( eWeaponVar.rechamber_time )


weapon.GetWeaponSettingAsset() <- returns the value for a key which is assigned ASSET values (i.e.: "viewmodel" (key) "mdl/weapons/p2011/ptpov_p2011.rmdl" (value)), from the weapon's .txt config file


weapon.GetWeaponSettingBool() <- returns the value for a key which is assigned boolean values, from the weapon's .txt config file


weapon.GetWeaponSettingString() <- returns the value for a key which is assigned a string value, from the weapon's .txt config file


weapon.GetWeaponSettingVector() <- returns the value for a key which is assigned a vector value, from the weapon's .txt config file


weapon.GetWeaponSettingEnum() <- returns the value for a weapon setting inside an Enumeration (Enum)

Examples: 

weapon.GetWeaponSettingEnum( eWeaponVar.cooldown_type, eWeaponCooldownType )

weapon.GetWeaponSettingEnum( eWeaponVar.fire_mode, eWeaponFireMode )

---


ALTERNATE FUNCTIONS FOR GETTING SETTINGS (NOT A METHOD OF THE CWeaponX ENTITY CLASS )

GetWeaponInfoFileKeyField_Global[OPTIONAL, RETURN TYPE HERE: String, Int, etc.]()

Examples:

GetWeaponInfoFileKeyField_Global( ref, variable )

GetWeaponInfoFileKeyField_GlobalString( data.baseWeapon, "printName" )

GetWeaponInfoFileKeyField_GlobalInt( weapon, "ammo_clip_size")

GetWeaponInfoFileKeyField_GlobalFloat( weaponRef, "burst_fire_delay" )

GetWeaponInfoFileKeyFieldAsset_Global( weaponName, "playermodel")

---


player.GetActiveWeapon(eActiveInventorySlot.mainHand) <- gets the weapon currently held by the player, using a constant value from the eActiveInventorySlot enumeration

global enum eActiveInventorySlot
{
	mainHand = 0
	altHand = 1
	utility = 2
}


player.GetNormalWeapon( weaponSlot ) 

SURVIVAL_GetWeaponBySlot( player, weaponSlot)

Weapon Inventory Slots

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



weapon.GetWeaponClassName() <- returns the weapon's engine class name, such as CWeaponX


weapon.SetWeaponName()

weapon.GetWeaponName() <- returns the weapon's name, such as "mp_weapon_nemesis"


weapon.GetWeaponAmmoPoolType() <- returns the weapon's ammo pool type 

global enum eAmmoPoolType
{
	bullet = 0
	special = 1
	highcal = 2
	shotgun = 3
	sniper = 4
	explosive = 5
}

weapon.SetClipCount() - this sets current MAGAZINE ammo, incorrectly labelled as "Clip" by Respawn

GetWeaponPrimaryAmmoCount( AMMOSOURCE_STOCKPILE )

weapon.SetWeaponSkin( skinIndex ) <- "Skin" here refers to the $skinfamilies (see the Valve Developer Wiki for QC Commands). It's not used in the same sense as "Legendary Skins", etc - it is a material swap 

weapon.SetWeaponPrimaryClipCount() <- this sets MAGAZINE ammo capacity, incorrectly labelled as "Clip" by Respawn

weapon.GetWeaponPrimaryClipCount() <- this gets MAGAZINE ammo capacity, incorrectly labelled as "Clip" by Respawn

weapon.GetWeaponPrimaryClipCountMax() <- this gets max MAGAZINE ammo capacity, incorrectly labelled as "Clip" by Respawn

weapon.SetWeaponPrimaryClipCountMax() <- this sets max MAGAZINE ammo capacity, incorrectly labelled as "Clip" by Respawn

weapon.SetWeaponChargeFraction() - used by weapons and abilities, highly dependent on the weapon / ability and its implementation

weapon.SetWeaponChargeFractionForced()

weapon.GetWeaponType()




























