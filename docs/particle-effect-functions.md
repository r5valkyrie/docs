# Particle Effects

This article documents various Particle System / Particle Effect functions and commands.

Before any particle effect can be used in the engine, it is MANDATORY to precache it, using the function:
```
PrecacheParticleSystem( $"particleassetname" )  
Example: PrecacheParticleSystem( $"P_loba_staff_menu_dlight" )  
```
## Weapon entity methods

Weapon entity methods, where "weapon" is the weapon entity:

### Playing a weapon-related particle effect

```
weapon.PlayWeaponEffect( asset $"FirstPersonFXHandle", asset $"ThirdPersonFXHandle", string "WeaponModelAttachmentHandle" )  
Example: weapon.PlayWeaponEffect( $"wpn_muzzleflash_arc_cannon_fp", $"wpn_muzzleflash_arc_cannon", "muzzle_flash" )
```
```
weapon.PlayWeaponEffectNoCull( asset $"FirstPersonFXHandle", asset $"ThirdPersonFXHandle", string "WeaponModelAttachmentHandle" )  
Example: weapon.PlayWeaponEffectNoCull( $"wpn_arc_cannon_electricity_fp", $"wpn_arc_cannon_electricity", "muzzle_flash" )
```
### Stopping a weapon-related particle effect

```
weapon.StopWeaponEffect( asset $"FirstPersonFXHandle", asset $"ThirdPersonFXHandle" )  
Example: weapon.StopWeaponEffect( $"wpn_arc_cannon_electricity_fp", $"wpn_arc_cannon_electricity" )
```

## Non-method particle effect functions

### Checking if a particle effect exists
```
EffectDoesExist("EffectHandle")
```
### Stopping a particle effect 
```
EffectStop("EffectHandle")
```
### Setting a particle effect's control point vector
```
EffectSetControlPointVector()
```
### Memory management functions
```
EffectSleep("EffectHandle")
EffectWake("EffectHandle")
```


## Particle Effects inside of QC's

The following commands are used inside of model QC files, used by StudioMDL to compile models, to be used in-game. For a comprehensive list of QC commands, consult the Valve Developer Wiki: [QC Commands](https://developer.valvesoftware.com/wiki/Category:QC_commands), [QC Commands 2](https://developer.valvesoftware.com/wiki/QC_Commands)

```
{ event AE_CL_CREATE_PARTICLE_EFFECT framenumber "ParticleEffectHandle follow_attachment AttachmentHandle" }

{ event AE_CL_STOP_PARTICLE_EFFECT framenumber "ParticleEffectHandle" }
```

These QC commands are contained inside $sequence(s) and are usually tied to activities. For more information on Activities, consult the Valve Developer Wiki: [Activity](https://developer.valvesoftware.com/wiki/Activity)

```
activity ACT_VM_WEAPON_INSPECT 60
```
## Playing first person player screen particle FX
```
entity FX = StartParticleEffectOnEntityWithPos_ReturnEntity( entityname, ParticleEffectHandle or GetParticleSystemIndex($"ParticleEffectAssetName"), FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID, <x, y, z>, ZERO_VECTOR )
```

```
entity cockpit = GetLocalViewPlayer().GetCockpit()

file.cockpitFxHandle = StartParticleEffectOnEntity( cockpit, GetParticleSystemIndex( GRAVITY_SCREEN_FX ), FX_PATTACH_POINT_FOLLOW, cockpit.LookupAttachment("CAMERA") )

EffectStop( file.cockpitFxHandle, false, true )
```

## FX Particle / Point (?) Attachment Behavior Flags
```
3rd argument of StartParticleEffectOnEntity()

StartParticleEffectOnEntity( entity cockpit, fxID, FX_PATTACH_X <- SUBSTITUTE HERE, attachmentID)

The most commonly used attachment behavior flags are:

FX_PATTACH_ABSORIGIN_FOLLOW 
FX_PATTACH_EYES_FOLLOW
FX_PATTACH_POINT_FOLLOW

and their _NOROTATE variants

However, the complete list of flags is much larger:

global const int FX_PATTACH_ABSORIGIN = 0
global const int FX_PATTACH_ABSORIGIN_FOLLOW = 1 // absolute origin follow
global const int FX_PATTACH_ABSORIGIN_FOLLOW_NOROTATE = 2
global const int FX_PATTACH_BIG_MAP_ZOOM_SCALE = 55
global const int FX_PATTACH_BOOST_METER_FRACTION = 36
global const int FX_PATTACH_CAMANGLES_FOLLOW = 14
global const int FX_PATTACH_CUSTOMORIGIN = 3
global const int FX_PATTACH_CUSTOMORIGIN_FOLLOW = 4 // custom origin follow
global const int FX_PATTACH_CUSTOMORIGIN_FOLLOW_NOROTATE = 5
global const int FX_PATTACH_DEATHFIELD_DISTANCE = 53
global const int FX_PATTACH_ENEMY_TEAM_ROUND_SCORE = 46
global const int FX_PATTACH_ENEMY_TEAM_SCORE = 45
global const int FX_PATTACH_EYEANGLES_FOLLOW = 13
global const int FX_PATTACH_EYES_FOLLOW = 9 // follows the entity's eyes
global const int FX_PATTACH_FRIENDLINESS = 18
global const int FX_PATTACH_FRIENDLY_TEAM_ROUND_SCORE = 44
global const int FX_PATTACH_FRIENDLY_TEAM_SCORE = 43
global const int FX_PATTACH_GAME_FULLY_INSTALLED_PROGRESS = 49
global const int FX_PATTACH_GLIDE_METER_FRACTION = 37
global const int FX_PATTACH_HEALTH = 16
global const int FX_PATTACH_HEAL_TARGET = 17
global const int FX_PATTACH_MINIMAP_SCALE = 47
global const int FX_PATTACH_MINIMAP_ZOOM_SCALE = 52
global const int FX_PATTACH_OVERHEAD_FOLLOW = 10
global const int FX_PATTACH_PLAYER_GRAPPLE_POWER = 20
global const int FX_PATTACH_PLAYER_SHARED_ENERGY = 21
global const int FX_PATTACH_PLAYER_SHARED_ENERGY_FRACTION = 22
global const int FX_PATTACH_PLAYER_SUIT_POWER = 19
global const int FX_PATTACH_POINT = 6
global const int FX_PATTACH_POINT_FOLLOW = 7 // follows a point
global const int FX_PATTACH_POINT_FOLLOW_NOROTATE = 8 
global const int FX_PATTACH_ROOTBONE_FOLLOW = 12
global const int FX_PATTACH_SCRIPT_NETWORK_VAR = 40
global const int FX_PATTACH_SCRIPT_NETWORK_VAR_GLOBAL = 41
global const int FX_PATTACH_SCRIPT_NETWORK_VAR_LOCAL_VIEW_PLAYER = 42
global const int FX_PATTACH_SHIELD_FRACTION = 38
global const int FX_PATTACH_SOUND_METER = 48
global const int FX_PATTACH_STATUS_EFFECT_SEVERITY = 39
global const int FX_PATTACH_STATUS_EFFECT_TIME_REMAINING = 54
global const int FX_PATTACH_TASKLIST_ITEM_FLOAT = 51
global const int FX_PATTACH_WAYPOINT_FLOAT = 50
global const int FX_PATTACH_WAYPOINT_VECTOR = 15
global const int FX_PATTACH_WEAPON_AMMO_MAX = 32
global const int FX_PATTACH_WEAPON_AMMO_REGEN_RATE = 34
global const int FX_PATTACH_WEAPON_CHARGE_FRACTION = 23
global const int FX_PATTACH_WEAPON_CLIP_AMMO_FRACTION = 28
global const int FX_PATTACH_WEAPON_CLIP_AMMO_MAX = 31
global const int FX_PATTACH_WEAPON_CLIP_MIN_AMMO_FRACTION = 29
global const int FX_PATTACH_WEAPON_DRYFIRE_FRACTION = 27
global const int FX_PATTACH_WEAPON_LIFETIME_SHOTS = 33
global const int FX_PATTACH_WEAPON_READY_TO_FIRE_FRACTION = 25
global const int FX_PATTACH_WEAPON_RELOAD_FRACTION = 26
global const int FX_PATTACH_WEAPON_REMAINING_AMMO_FRACTION = 30
global const int FX_PATTACH_WEAPON_SMART_AMMO_LOCK_FRACTION = 24
global const int FX_PATTACH_WEAPON_STOCKPILE_REGEN_FRAC = 35
global const int FX_PATTACH_WORLDORIGIN = 11
```