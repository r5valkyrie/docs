# Entities and Entity Spawning


Entities can only be spawned in by using 

DispatchSpawn(entity entityToBeSpawned)

The entity class is defined in the engine code and it has multiple subclasses (CPlayer, CWeaponX, etc.)

All models belong to an entity type. Triggers are entities as well

Entity types:
```
- prop_dynamic
- prop_static
- prop_script
- trigger_cylinder
- vortex_sphere
- player
- weapon
- npc
```
The entity class has many keyvalue attributes, which are accessed with entity.kv

## Size Keyvalues
```
entity.kv.Width
entity.kv.height
entity.kv.scale
entity.kv.halfheight
entity.kv.halfwidth
```
## Material Keyvalues
```
entity.kv.rendercolor // emissive map color
entity.kv.rendermode
entity.kv.renderamt
entity.kv.TextureScale
entity.kv.RopeMaterial
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
## Flag Keyvalues
```
entity.kv.damageSourceId
entity.kv.contents
entity.kv.VisibilityFlags
entity.kv.spawnflags
entity.kv.script_flag
entity.kv.WaitFlag
entity.kv.SetFlag
entity.kv.flag_clear_resets
entity.kv.scr_flag_set
entity.kv.scr_flag_hack_started
entity.kv.scr_flagToggle
entity.kv.scr_flagRequired
entity.kv.toggleFlagWhenHacked
entity.kv.damagePilots
entity.kv.damageTitans
```

```
Visibility flags:
ENTITY_VISIBLE_TO_FRIENDLY
ENTITY_VISIBLE_TO_ENEMY
ENTITY_VISIBLE_TO_OWNER
ENTITY_VISIBLE_TO_NOBODY
ENTITY_VISIBLE_TO_EVERYONE

Script flags:
SF_NPC_ALLOW_SPAWN_SOLID
SF_NPC_NO_PLAYER_PUSHAWAY
SF_ABSORB_BUILLETS
SF_PHYSPROP_DEBRIS
SF_INFOTARGET_ALWAYS_TRANSMIT_TO_CLIENT
SF_BLOCK_OWNER_WEAPON
SF_BLOCK_NPC_WEAPON_LOF
SF_ABSORB_CYLINDER

Physics flags:
SOLID_HITBOXES
SOLID_OBB

Content flags:
CONTENTS_SOLID
CONTENTS_BLOCKLOS
CONTENTS_PLAYERCLIP
CONTENTS_MONSTERCLIP
CONTENTS_MONSTER
CONTENTS_BULLETCLIP
CONTENTS_HITBOX
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

## Script Example For Spawning a Dome Shield

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




