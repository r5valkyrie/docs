# Entities, Entity Spawning, Entity Keyvalues and Flags

===================================================================
## Table of Contents
### 1. Introduction
### 2. Entity Types
### 3. Entity Methods & Functions
### 4. Keyvalues & Associated Methods and Functions
### 5. Flags & Associated Methods and Functions
### 6. Script example for spawning an entity (dome shield)
### 7. Entity Spawn Methods / Functions
===================================================================

# 1. Introduction

In the Source Engine, entities are objects meant to be interactable for players or interacted with by other entities via [the I/O system.](https://developer.valvesoftware.com/wiki/Inputs_and_Outputs)  

Entities are created with CreateEntity( string entityType), but can only be spawned in the world with DispatchSpawn( entity entityName ) or tailor-made spawn functions.

The entity class (CBaseEntity) is defined in the engine code and it has multiple subclasses (CPlayer, CWeaponX, etc.)  

All models NECESSARILY belong to an instance of an entity type. Models cannot be spawned "on their own", without being attached / assigned to an entity.

Many entities are inherited from Valve's Source Engine, as ReSource (Respawn Source Engine) is a fork of the Portal 2 Branch of the Source Engine.

Entity subclasses inherit the base entity class' attributes (associated key-value pairs) and methods (associated functions).

The entity class has many keyvalue attributes, which are accessed with entity.kv  

# 2. Entity types:

```
- player // has player-specific global structures accessible with player.p
- weapon // has weapon-specific global structures accessible with weapon.w and can also use a global table per-instance with weapon.s
- point_viewcontrol
```

## Prop Entities
```
- prop_dynamic
- prop_static // Environment objects on maps, once maps are compiled, they are no longer considered individual entities
- prop_script // Scripted props (they have a script attached)
```

## NPC Entities
```
- npc_pilot
- npc_marvin
```


## Func Entities
```
- func_breakable
- func_brush
```


## Info Entities
```
- info_player_start
- info_target
```


## Logic Entities
```
- logic_choerographed_scene
- logic_proximity
```

## Trigger Entities

### Shape Triggers and vortex sphere
```
- trigger_activate
- trigger_cylinder
- trigger_cylinder_heavy
- vortex_sphere // there is no trigger_sphere, so this is often used as a substitute for that (with a custom script setup!)
- trigger_proximity
```
### Gravity Triggers 
```
- trigger_point_gravity
- trigger_gravity
```
###  Mover Triggers
```
- trigger_push
- trigger_updraft
- trigger_slip
- trigger_auto_crouch
```
### Location / Geo / Entity triggers 
```
- trigger_proximity
- trigger_brush
- trigger_capture_point // capture location
- trigger_hardpoint // hardpoint location
- trigger_door
- trigger_indoor_area
- trigger_location // location-specific trigger
- trigger_location_sp // single-player location specific trigger
- trigger_mp_spawn_zone // defines a multiplayer spawn area
- trigger_startpoint
- trigger_spawn
- trigger_out_of_bounds // defines an area as out-of-bounds (OOB)
- trigger_warp_gate // phase-drivers / teleporters
- trigger_teleport
- trigger_pve_zone // PvE areas like the areas surrounding Prowler Dens, Spider Nests, etc.
- trigger_no_grapple // makes an area ungrapple-able
- trigger_no_zipline // makes an area unzipline-able
- trigger_wind
```

### Flashlight Triggers 
```
- trigger_player_flashlight_on
- trigger_player_flashlight_off
- trigger_player_flashlight_zone
```
### Action Triggers
```
- trigger_playermovement
- trigger_look
```
### Physics Triggers 
```
- trigger_impact
- trigger_ignoresolids
```
### Damage Triggers
```
- trigger_hurt
- trigger_death_fall
```
### Scripted Sequence Triggers
```
- trigger_fastball
```
### Flag Triggers
```
- trigger_flag_clear
- trigger_flag_set
```
### Sound Triggers
```
- trigger_soundscape
```

### Use(?) triggers
```
- trigger_once
- trigger_multiple
- trigger_multiple_clientside
```

### Player Triggers 
```
- trigger_skydive
- trigger_weaponless
```

### Environment Triggers
```
- env_fog_controller
- env_sprite
- env_beam
```

# 3. Entity Methods & Functions

## Visibility Methods

```
entity.Show()
entity.Hide()
```

## Position Methods

```
entity.GetOrigin()
entity.GetAngles()
```

# 4. Keyvalues & Associated Methods and Functions

## Keyvalue Methods and Functions
```
entity.HasKey( string keyName )
entity.GetValueForKey( string keyName )
```
## Size Keyvalues
```
entity.kv.Width
entity.kv.height
entity.kv.scale
entity.kv.halfheight
entity.kv.halfwidth

Size Methods and functions:

entityModel.SetModelScale( float scale )

```
## Material Keyvalues
```
entity.kv.rendercolor // emissive map color
entity.kv.rendermode
entity.kv.renderamt
entity.kv.TextureScale
entity.kv.RopeMaterial // ziplines only
entity.kv.modelskin
entity.kv.amplitude
entity.kv.duration
entity.kv.frequency
entity.kv.magnitude
entity.kv.intensity
entity.kv.GlowProxySize
entity.kv.HDRColorScale
entity.kv.impacteffectcolorid
entity.kv.electricEffect
```
## Model Keyvalues
```
entity.SetValueForModelKey()
```
## Vector / Position Keyvalues
```
entity.kv.origin
entity.kv.angles
```

## Zipline Keyvalues

```
entity.kv.Zipline
entity.kv.ZiplineAutoDetachDistance
entity.kv.ZiplineSagEnable
entity.kv.ZiplineSagHeight
entity.kv.ZiplineVertical
entity.kv._zipline_rest_point_0
entity.kv._zipline_rest_point_1
entity.kv.ZiplinePreserveVelocity
entity.kv.ZiplinePushOffInDirectionX
entity.kv.Slack
```
## Interactables Keyvalues
```
entity.kv.singleUse
entity.kv.usable
entity.kv.toggleSwitch
entity.kv.motionActivated
entity.kv.allowshoot
entity.kv.shootable
entity.kv.startOpen
entity.kv.multiUseDelay
```
## Team Keyvalues
```
entity.kv.squadname
entity.kv.teamnumber
entity.kv.TeamNum
entity.kv.team
```
## Inventory Keyvalues
```
entity.kv.disable_offhand_ordnance
entity.kv.disable_offhand_defense
entity.kv.disable_offhand_tactical
entity.kv.disable_offhand_core
entity.kv.grenadeWeaponName
entity.kv.secondaryWeaponName
entity.kv.additionalequipment
```
## Movement Keyvalues
```
entity.kv.airSpeed
entity.kv.airAcceleration
entity.kv.gravity
entity.kv.zero_g_movement
```
## UI Keyvalues
```
entity.kv.hintString_hold
entity.kv.hintString_press
entity.kv.disabledHintString
```
## Script Keyvalues
```
entity.kv.script_start_moving
entity.kv.script_player_ignore
entity.kv.script_pitch_limit
entity.kv.script_gravityscale
entity.kv.script_fight_radius
entity.kv.script_radius
entity.kv.script_height
entity.kv.script_hotdrop
entity.kv.script_sound
entity.kv.script_noteworthy
entity.kv.deathScriptFuncName
entity.kv.scriptDamageType
entity.SetScriptName()
```
## Text Keyvalues
```
entity.kv.message
entity.kv.radius
entity.kv.color
entity.kv.color2
entity.kv.fadein
entity.kv.fadeout
entity.kv.holdtime
entity.kv.fxtime
entity.kv.text
```
## Signal Keyvalues
```
entity.kv.WaitSignal
entity.kv.SendSignal
```
## Sound Keyvalues
```
entity.kv.fan_loop_sound
entity.kv.shutoff_sound
entity.kv.sound_start_move
```
## NPC / Creature Keyvalues
```
entity.kv.health
entity.kv.max_health
entity.kv.FieldOfView
entity.kv.FieldOfViewAlert
entity.kv.AccuracyMultiplier
entity.kv.WeaponProficiency
entity.kv.disengageEnemyDist
entity.kv.drop_battery
entity.kv.alwaysAlert
entity.kv.TitanType
entity.kv.carrying_battery
entity.kv.targetname
entity.kv.noSoul
entity.kv.job
entity.kv.LinkedJobsOnly
entity.kv.follow_mode
entity.kv.leveled_titan_settings
entity.kv.level_aisettings
```
## Player Keyvalues
```
entity.kv.startDisconnected
```
## Animation Keyvalues
```
entity.kv.DisableBoneFollowers
entity.kv.framerate
entity.kv.leveled_animation
```
## Lighting Keyvalues
```
entity.kv.disableshadows
```

## Physics Keyvalues
```
entity.kv.solid = solidType // 0 = no collision, 2 = bounding box, 6 = use vPhysics, 8 = hitboxes only
entity.kv.CollisionGroup
entity.kv.physics_pull_strength
entity.kv.physics_side_dampening
entity.kv.physics_max_mass
entity.kv.physics_max_size
entity.kv.physdamagescale
entity.kv.SpawnAsPhysicsMover
entity.kv.player_collides
```
## Trigger Keyvalues
```
entity.kv.triggerFilterUseNew
entity.kv.triggerFilterPhaseShift
entity.kv.triggerFilterNonCharacter
entity.kv.triggerFilterTeamMilitia
entity.kv.triggerFilterTeamIMC
entity.kv.triggerFilterPlayer
entity.kv.triggerFilterNpc
entity.kv.triggerFilterTeamBeast
entity.kv.triggerFilterTeamOther // = 1 , key for survival
entity.kv.trigger_once
```
## Mover Keyvalues
```
entity.kv.MoveSpeed
entity.kv.rotate_to_time
entity.kv.rotate_to_ease
entity.kv.rotation_axis
entity.kv.movedirection
entity.kv.path_speed
entity.kv.rotate_to_degrees
entity.kv.rotate_to_return_delay
entity.kv.rotate_to_loop_time
entity.kv.rotate_forever_speed
entity.kv.SpawnAsPhysicsMover
entity.kv.move_time
entity.kv.move_amount
entity.kv.scripted_rotator
entity.kv.perfect_circular_rotation
entity.kv.ease_to_node
entity.kv.use_local_rotation
entity.kv.PositionInterpolator
```

## Other Keyvalues
```
entity.kv.right
entity.kv.AccuracyMultiplier
entity.kv.fadedist
entity.kv.Subdiv
entity.kv.Type
entity.kv.NextKey
entity.kv.change_navmesh
entity.kv.start_delay
entity.kv.leveledplaced
entity.kv.parent_linked_ents
entity.kv[key]
entity.kv["breakLinksToAttachedSpawner"]
entity.kv.editorclass
entity.kv.inertiaScale
entity.kv.minangle
entity.kv.zone_name
entity.kv.instance_name
entity.kv.minhealthdmg
entity.kv.nodamageforces
entity.kv.bullet_fov
entity.kv.teleport
entity.kv.body
entity.kv.lifetime
entity.kv.enabled
entity.kv.defenseActive
entity.kv.disable_vdu
entity.kv.crashOnDeath
entity.kv.skip_runto
entity.kv.face_angles
entity.kv.unlink
entity.kv.clear_potential_threat_pos
entity.kv.disable_assault_on_goal
entity.kv.TurretRange
entity.kv.num_smooth_points
entity.kv.hotspot
entity.kv.in_skybox
entity.SetBlocksRadiusDamage()
```

# 5. Flags & Associated Methods and Functions

## Flag Keyvalues
```

The flags are BIT FLAGS (Bitwise Flags) and can either be used with integer values or their corresponding hexadecimal values (i.e.: 0x0)

entity.kv.damageSourceId
entity.kv.contents // see: Content flags
entity.kv.VisibilityFlags // see: Visibility flags
entity.kv.spawnflags // see: Spawn flags
entity.kv.script_flag // see: Script flags
entity.kv.WaitFlag
entity.kv.SetFlag
entity.kv.flag_clear_resets
entity.kv.scr_flag_set
entity.kv.scr_flag_hack_started
entity.kv.scr_flagToggle
entity.kv.scr_flagRequired
entity.kv.toggleFlagWhenHacked
entity.kv.damagePilots // determines whether the entity can damage Pilots or Legends
entity.kv.damageTitans // determines whether the entity can damage Titans
```

## Flag Methods and Functions

```
// flag = string flagName or macro 

FlagInit( flag )
Flag( flag ) // returns value is whether the flag exists osr not
FlagSet( flag )
FlagClear( flag )
FlagWaitClear( flag )
FlagWait( flag )
FlagWait( "EntitiesDidLoad" )
FlagEnd( flag )

bool function IsBitFlagSet()

DamageInfo_GetDamageFlags( damageInfo )
DamageInfo_GetCustomDamageType( damageInfo )
TEMP_GetDamageFlagsFromString( damageFlagsString )

weapon.GetWeaponDamageFlags()
weapon.GetWeaponType( weaponTypeFlag )

player.IsDisabledFor( weaponTypeFlag )

AddCinematicFlag( entity player, flag )
RemoveCinematicFlag( entity player, flag )

npc.SetNPCFlag( flags )
npc.EnableNPCFlag( flags )
npc.DisableNPCFlag( flags )

npc.SetNPCMoveFlag( flags )
npc.EnableNPCMoveFlag( flags )
npc.DisableNPCMoveFlag( flags )

```

## Visibility flags:

```
// These flags are BIT FLAGS (Bitwise Flags) set with entity.kv.VisibilityFlags =
// Bitwise operators are required to set multiple bitflags, i.e.: ent.kv.VisibilityFlags = (ENTITY_VISIBLE_TO_OWNER | ENTITY_VISIBLE_TO_FRIENDLY)

ENTITY_VISIBLE_TO_OWNER
ENTITY_VISIBLE_TO_FRIENDLY
ENTITY_VISIBLE_TO_ENEMY
ENTITY_VISIBLE_TO_EVERYONE
ENTITY_VISIBLE_TO_NOBODY
```

## Spawn flags:
```
// These flags are BIT FLAGS (Bitwise Flags) set with entity.kv.spawnflags =
// Bitwise operators are required to set multiple bitflags, i.e.: npc.kv.spawnflags = (SF_NPC_ALLOW_SPAWN_SOLID | SF_NPC_NO_PLAYER_PUSHAWAY)


SF_PHYSPROP_DEBRIS

SF_INFOTARGET_ALWAYS_TRANSMIT_TO_CLIENT

SF_WEAPON_START_CONSTRAINED
SF_BLOCK_OWNER_WEAPON

SF_ABSORB_CYLINDER
SF_ABSORB_BULLETS

SF_ENVEXPLOSION_NO_DAMAGEOWNER
SF_ENVEXPLOSIO_NO_NPC_SOUND_EVENT
SF_ENVEXPLOSION_NOSAOUND_FOR_ALLIES
SF_ENVEXPLOSION_MASK_BRUSHONLY
SF_ENVEXPLOSION_UPWARD_FORCE
SF_ENVEXPLOSION_INCLUDE_ENTITIES

SF_SHAKE_RUMBLE_ONLY // Rumble, no camera shake
SF_SHAKE_NO_RUMBLE // Camera shake, no rumble
SF_SHAKE_INAIR // Camera shake in air

SF_NPC_ALLOW_SPAWN_SOLID
SF_NPC_ALLOW_SPAWN_SOLID
SF_NPC_NO_PLAYER_PUSHAWAY
SF_BLOCK_NPC_WEAPON_LOF // LOF = Line of Fire = Line of Sight = LOS
```

## Physics flags:

```
SOLID_HITBOXES
SOLID_OBB
```

## Content flags:

```
CONTENTS_SOLID
CONTENTS_BLOCKLOS
CONTENTS_PLAYERCLIP
CONTENTS_MONSTERCLIP
CONTENTS_MONSTER
CONTENTS_BULLETCLIP
CONTENTS_HITBOX
```

## Damage flags:

```
// These flags are BIT FLAGS (Bitwise Flags) set with entity.kv.spawnflags =
// Bitwise operators are required to set multiple bitflags, i.e.: npc.kv.spawnflags = (SF_NPC_ALLOW_SPAWN_SOLID | SF_NPC_NO_PLAYER_PUSHAWAY)
// Damage flags can be obtained from the damageInfo dump using DamageInfo_GetDamageFlags( src ) or from the weapon's .txt config with weapon.GetWeaponDamageFlags()


DF_GIB											// 1
DF_DISSOLVE										// 2
DF_INSTANT										// 3
DF_NO_SELF_DAMAGE								// 4
DF_IMPACT										// 5
DF_BYPASS_SHIELD								// 6, bypasses shield damage, damages health directly 
DF_RAGDOLL										// 7
DF_SHIELD_BREAK 								// 8
DF_RADIUS_DAMAGE 		CODE_DF_RADIUS_DAMAGE	// 9
DF_ELECTRICAL 									// 10
DF_BULLET 										// 11
DF_EXPLOSION			CODE_DF_EXPLOSION		// 12
DF_MELEE				CODE_DF_MELEE			// 13
DF_NO_INDICATOR									// 14
DF_KNOCK_BACK			CODE_DF_KNOCK_BACK		// 15
DF_STOPS_TITAN_REGEN							// 16
DF_DISMEMBERMENT								// 17
DF_MAX_RANGE									// 18
DF_SHIELD_DAMAGE								// 19
DF_CRITICAL										// 20
DF_SNIPER										// 21
DF_HEADSHOT										// 22
DF_VORTEX_REFIRE								// 23
DF_OVERSHIELD									// 24
DF_KNOCKDOWN								    // 25
DF_KILLSHOT										// 26
DF_SHOTGUN										// 27
DF_SKIPS_DOOMED_STATE							// 28
DF_DOOMED_HEALTH_LOSS							// 29
DF_SHADOW_DAMAGE								// 30
DF_DOOM_FATALITY								// 31
DF_NO_HITBEEP			                        // 32 current max


global const int DAMAGEFLAG_NOPAIN = 16
global const int DAMAGEFLAG_VICTIM_ARMORED = 1
global const int DAMAGEFLAG_VICTIM_HAS_VORTEX = 4
global const int DAMAGEFLAG_VICTIM_INVINCIBLE = 2
```

## Collision Group Flags
```
TRACE_COLLISION_GROUP_DEBRIS
```

## Weapon Flags
```
// can be seen in codeconsts_client.txt and codeconsts_server.txt

global const int WPF_DEPLOYABLE = 4
global const int WPF_DYNAMIC_UPDATE = 8
global const int WPF_IN_RANGE = 32
global const int WPF_IS_VISIBLE = 16
global const int WPF_NONE = 0
global const int WPF_VISBILITY_LOS = 1
global const int WPF_VISIBILITY_SONAR = 2
```

## Weapon Type Flags
```
// can be seen in codeconsts_client.txt and codeconsts_server.txt

global const int WPT_CONSUMABLE = 16
global const int WPT_GRENADE = 64
global const int WPT_INCAP_SHIELD = 32
global const int WPT_MELEE = 2
global const int WPT_OTHER = 128
global const int WPT_PRIMARY = 1
global const int WPT_TACTICAL = 4
global const int WPT_ULTIMATE = 8
global const int WT_ANTITITAN = 2
global const int WT_DEFAULT = 0
global const int WT_DEFENSE = 6
global const int WT_INVENTORY = 8
global const int WT_MELEE = 3
global const int WT_SHOULDER = 4
global const int WT_SIDEARM = 1
global const int WT_TACTICAL = 7
global const int WT_TITANCORE = 5
```

## Cinematic Flags
```
CE_FLAG_HIDE_MAIN_HUD_INSTANT
CE_FLAG_HIDE_PERMANENT_HUD 
CE_FLAG_EXECUTION
```

## NPC Flags
```
// can be seen in codeconsts_server.txt

NPC_ALLOW_PATROL
NPC_ALLOW_INVESTIGATE
NPC_ALLOW_FLEE
NPC_ALLOW_HAND_SIGNALS
NPC_STAY_CLOSE_TO_SQUAD
NPC_DISABLE
NPC_ANIM_FOV
NPC_ANIM_ADVANCE_CYCLE_EVERY_FRAME
NPC_LOOK_FOR_CORPSE
NPC_DIRECTIONAL_MELEE
NPC_RAGDOLL_IMMEDIATE
NPC_DIE_ON_ANY_DAMAGE
NPC_AIM_DIRECT_AT_ENEMY	// instead of at LSP
NPC_MUTE_TEAMMATE
NPC_NO_LISTEN_TEAMMATE
NPC_IGNORE_FRIENDLY_SOUND
NPC_NEW_ENEMY_FROM_SOUND
NPC_TEAM_SPOTTED_ENEMY
NPC_PAIN_IN_SCRIPTED_ANIM
NPC_NO_PAIN
NPC_NO_GESTURE_PAIN
NPC_CROUCH_COMBAT
NPC_IGNORE_ALL
NPC_DISABLE_SENSING
NPC_USE_SHOOTING_COVER
NPC_NO_WEAPON_DROP
NPC_NO_MOVING_PLATFORM_DEATH
```
## NPC Move Flags
```
NPCMF_DISABLE_ARRIVALS
NPCMF_IGNORE_CLUSTER_DANGER_TIME
NPCMF_WALK_ALWAYS
NPCMF_WALK_NONCOMBAT
NPCMF_PREFER_SPRINT
NPC_FOLLOW_SAFE_PATHS
```
## Pass Through Flags
```
PTF_ADDS_MODS
PTF_NO_DMG_ON_PASS_tHROUGH
```


# 6. Script example for spawning an entity (dome shield)

From _bubble_shield.gnut

```
	entity bubbleShield = CreateEntity( "prop_dynamic" )
	bubbleShield.SetValueForModelKey( BUBBLE_BUNKER_SHIELD_COLLISION_MODEL )
	bubbleShield.kv.solid = SOLID_VPHYSICS
    bubbleShield.kv.rendercolor = "81 130 151"
    bubbleShield.kv.contents = (int(bubbleShield.kv.contents) | CONTENTS_NOGRAPPLE)
	bubbleShield.SetOrigin( origin )
	bubbleShield.SetAngles( angles )
     // Blocks bullets, projectiles but not players and not AI
	bubbleShield.SetScriptName( BUBBLE_SHIELD_SCRIPTNAME )
	bubbleShield.kv.CollisionGroup = TRACE_COLLISION_GROUP_BLOCK_WEAPONS
	bubbleShield.SetBlocksRadiusDamage( true )
	DispatchSpawn( bubbleShield )
	bubbleShield.SetOwner( owner )
	bubbleShield.Hide()
```

# 7. Entity Spawn Methods / Functions

TODO TODO TODO TODO

## Spawn NPCs
```
entity function SpawnNPC( int npcType, vector spawnOrigin, vector spawnAngles, int rank = 0 )
entity function SpawnNPCDetailed( NpcSpawnInfo infoRaw )
```
### Spawn Stalkers
```
DEV_SpawnStalkerAtCrosshair( int team = 99 )
SpawnFromStalkerRack()
```

## Spawn Loot
```
SpawnGenericLoot( lootRef, lootOrigin, lootAngles, 500 )
SpawnLoot( lootref, lootOrigin, lootAngles, 500)
```
## Spawn Environment Loot Givers
```
SpawnLootTick()
SpawnLootTickAtCrosshair()
SpawnLootRoller_Parented() // the loot rollers, not the drones!
SpawnLootRoller_DispatchSpawn() // the loot rollers, not the drones!
SpawnLootRoller_NoDispatchSpawn() // the loot rollers, not the drones!
SpawnLootDrones() // the loot drones (cargo bots), not rollers
```

