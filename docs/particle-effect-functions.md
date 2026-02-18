## Particle Effect Commands

PrecacheParticleSystem($"particleassetname") <- MANDATORY in order to be able to use Particle Effects inside the engine!

Example:

PrecacheParticleSystem($"P_loba_staff_meny_dlight")


---
## Animation Events (inside QCs, in $sequence)

{ event AE_CL_CREATE_PARTICLE_EFFECT framenumber "particleeffectname follow_attachment attachmentname" }

{ event AE_CL_STOP_PARTICLE_EFFECT framenumber "particleeffectname" }

tied to an activity like 

activity ACT_VM_WEAPON_INSPECT 60

---

entity FX = StartParticleEffectOnEntityWithPos_ReturnEntity( entityname, PARTICLESYSTEMINDEX or GetParticleSystemIndex($"PARTICLEEFFECTASSETNAME"), FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID, <x, y, z>, ZERO_VECTOR )


entity cockpit = GetLocalViewPlayer().GetCockpit()

file.cockpitFxHandle = StartParticleEffectOnEntity( cockpit, GetParticleSystemIndex( GRAVITY_SCREEN_FX ), FX_PATTACH_POINT_FOLLOW, cockpit.LookupAttachment("CAMERA") )

EffectStop( file.cockpitFxHandle, false, true )