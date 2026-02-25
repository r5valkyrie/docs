# Particle Effects

This article documents various Particle System / Particle Effect functions and commands.

Before any particle effect can be used in the engine, it is MANDATORY to precache it, using the function:
```
PrecacheParticleSystem( $"particleassetname" )  
Example: PrecacheParticleSystem( $"P_loba_staff_menu_dlight" )  
```
## Weapon entity methods

Weapon entity methods, where "weapon" is the weapon entity:
```
weapon.PlayWeaponEffect( asset $"FirstPersonFXHandle", asset $"ThirdPersonFXHandle", string "WeaponModelAttachmentHandle" )  
Example: weapon.PlayWeaponEffect( $"wpn_muzzleflash_arc_cannon_fp", $"wpn_muzzleflash_arc_cannon", "muzzle_flash" )
```
```
weapon.PlayWeaponEffectNoCull( asset $"FirstPersonFXHandle", asset $"ThirdPersonFXHandle", string "WeaponModelAttachmentHandle" )  
Example: weapon.PlayWeaponEffectNoCull( $"wpn_arc_cannon_electricity_fp", $"wpn_arc_cannon_electricity", "muzzle_flash" )
```
```
weapon.StopWeaponEffect( asset $"FirstPersonFXHandle", asset $"ThirdPersonFXHandle" )  
Example: weapon.StopWeaponEffect( $"wpn_arc_cannon_electricity_fp", $"wpn_arc_cannon_electricity" )
```
## Particle Effects inside of QC's

The following commands are used inside of model QC files, used by StudioMDL to compile models, to be used in-game. For a comprehensive list of QC commands, consult the Valve Developer Wiki: [QC Commands](https://developer.valvesoftware.com/wiki/Category:QC_commands), [QC Commands 2](https://developer.valvesoftware.com/wiki/QC_Commands)

{ event AE_CL_CREATE_PARTICLE_EFFECT framenumber "ParticleEffectHandle follow_attachment AttachmentHandle" }

{ event AE_CL_STOP_PARTICLE_EFFECT framenumber "ParticleEffectHandle" }

These QC commands are contained inside $sequence(s) and are usually tied to activities. For more information on Activities, consult the Valve Developer Wiki: [Activity](https://developer.valvesoftware.com/wiki/Activity)

activity ACT_VM_WEAPON_INSPECT 60

## Playing first person player screen particle FX
```
entity FX = StartParticleEffectOnEntityWithPos_ReturnEntity( entityname, ParticleEffectHandle or GetParticleSystemIndex($"ParticleEffectAssetName"), FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID, <x, y, z>, ZERO_VECTOR )
```

```
entity cockpit = GetLocalViewPlayer().GetCockpit()

file.cockpitFxHandle = StartParticleEffectOnEntity( cockpit, GetParticleSystemIndex( GRAVITY_SCREEN_FX ), FX_PATTACH_POINT_FOLLOW, cockpit.LookupAttachment("CAMERA") )

EffectStop( file.cockpitFxHandle, false, true )
```