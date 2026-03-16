# Trigger Entities

This information is also included in the Entities article, but is presented here for ease of sharing and to be less overwhelming.

## How Triggers Work

Triggers are Entities that produce ("fire") an output when an entity touches them, is inside of them or leaves the area that they cover. They can affect entities when these events occur.  
Triggers can take on various 3D shapes, such as cylinders and spheres. The various types of triggers available in R5V are documented belowe.  
Triggers can be placed as Brush Entities in a map editor such as MRVN-Radiant or be created with squirrel_re scripts.   

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/triggerexample.png)

Just like other types of Entities, triggers are created in VScripts with:
```
CreateEntity( string entityType )

entity trigger = CreateEntity( "trigger_cylinder" ) // for creating a reference to the Entity
```

They are spawned in the 3D world with:

```
DispatchSpawn( trigger )
```


Triggers can have callback functions connected to various outputs of theirs, such as:  
```
trigger.ConnectOutput( string triggerOutput, funcref() callbackFunction )
trigger.ConnectOutput( "OnStartTouch", EntityEnterHoverTankVolume)

Possible Trigger Events:

OnStartTouch
OnTrigger
OnEndTouch
```

Various Trigger related functions can be found in _trigger_functions.gnut. Many Triggers have associated Thinker functions, which allow autonomous behavior.    
Read more on Thinker functions on the Valve Developer Wiki: [Thinking](https://developer.valvesoftware.com/wiki/Thinking), [ThinkerFunctions](https://developer.valvesoftware.com/wiki/Dota_2_Workshop_Tools/Scripting/ThinkerFunctions).    


Triggers can be used in combination with Threads, such as with:  
```
RegisterSignal( string signalName )
triggerEntity.WaitSignal( string signalName ) // trigger.WaitSignal("OnTrigger")
triggerEntity.Signal( string signalName ) // trigger.Signal("PlayerEnteredDoorTrigger")
triggerEntity.EndSignal( string signalName ) // i.e.: trigger.EndSignal("OnDestroy")
```

Triggers also have specific Methods and Keys:  
```
// setting a callback function on the Event that an Entity enters its volume
trigger.SetEnterCallback( funcref() callbackFunction ) // trigger.SetEnterCallback( DebrisTrap_OnTriggerEnter ) 
trigger.SetClientEnterCallback( funcref() callbackFunction )
trigger.SetLeaveCallback( funcref() callbackFunction )

// Touching / Contact with other Entities
trigger.SearchForNewTouchingEntity()
trigger.IsTouching( entity ent ) // while( trigger.IsTouching( player ) ) { code here }
trigger.GetTouchingEntities() // returns an array< entity >, can be used with foreach, in, etc.

// Radius
trigger.SetRadius( float triggerRadius )
trigger.SetCylinderRadius( float triggerRadius )

// Dimensions
trigger.SetAboveHeight( int aboveHeight )
trigger.SetBelowHeight( int belowHeight )

// Enable / Disable
trigger.Enable()
trigger.Disable()

// Bounding Box
trigger.GetBoundingMins() // returns a vector
trigger.GetBoundingMaxs() // returns a vector

// Set which Entities get damaged
trigger.kv.damagePilots = bool
trigger.kv.damageTitans = bool

// Script Trigger Data KVs
trigger.e.scriptTriggerData.enabled = bool
trigger.e.scriptTriggerData.flags = bitflags
trigger.e.scriptTriggerData.enterCallbacks // add with .append(), can also use other array methods
trigger.e.scriptTriggerData.leaveCallbacks // add with .append(), can also use other array methods


trigger.kv.trigger_once

trigger.SetPhaseShiftCanTouch( bool )

// Targets
trigger.GetTargetName() // returns a string

// Points
trigger.ContainsPoint( vector positionVector )
trigger.UsePointCollision()

// Trigger Type
trigger.SetTriggerType() // i.e.: TT_JUMP_PAD, etc.
trigger.GetTriggerType()

// KVs tailor-made for jump pads
trigger.SetLaunchScaleValues( JUMP_PAD_PUSH_VELOCITY, 1.7 )
trigger.SetViewPunchValues( JUMP_PAD_VIEW_PUNCH, JUMP_PAD_VIEW_PUNCH_HARD, JUMP_PAD_VIEW_PUNCH_RAND )
trigger.SetLaunchScaleValues( float value1, float value2 )
```

// Trigger filter keys
```
trigger.kv.triggerFilterUseNew = bool // this overrides spawnflags (in this case ignores them) if set to 1 / true, and uses the keys listed below instead!
trigger.kv.triggerFilterPlayer = "all" // possible values = all, none, pilot, titan
trigger.kv.triggerFilterLifeState = "any" // possible values = any, alive, dead
trigger.kv.triggerFilterPhaseShift = "any" // possible values = any, phaseshift, nonphaseshift
trigger.kv.triggerFilterNpc = "none" // possible values = all, none, soldier, titan, etc
trigger.kv.triggerFilterNpcOwnedByPlayer = "any" // possible values = any, owned, notowned
trigger.kv.triggerFilterNonCharacter = bool
trigger.kv.triggerFilterTeamMilitia = bool
trigger.kv.triggerFilterTeamIMC = bool
trigger.kv.triggerFilterTeamNeutral = bool
trigger.kv.triggerFilterTeamBeast = bool
trigger.kv.triggerFilterTeamOther = bool // for SURVIVAL

```

As sub-classes of the Entity class, they also inherit certain Entity-specific Methods and Keys:  

```
// Hierarchy
trigger.GetOwner()
trigger.SetOwner( entity ownerEntity )
trigger.GetParent()
trigger.SetParent( entity ownerEntity )

// Positioning
trigger.SetOrigin( vector origin )
trigger.GetOrigin()
trigger.SetAngles( vector angles )
trigger.GetAngles()
trigger.SetAbsOrigin( vector absOrigin )
trigger.SetAbsAngles( vector absAngles )

// Visibility
trigger.Hide()
trigger.Show()

// State
trigger.Solid()
trigger.NotSolid()

// Realms
trigger.RemoveFromAllRealms()
trigger.AddToRealm( int realm )
trigger.AddToOtherEntitysRealms( entity ent )

// Script Name = The name the entity is referenced by inside scripts
trigger.GetScriptName()
trigger.SetScriptName( string scriptName ) // trigger.SetScriptName("WallTrigger_oob_timer")

// Garbage collection
trigger.Destroy()

// Teams
trigger.GetTeam()
trigger.SetTeam()

// Accessing or Testing for Keyvalues
trigger.HasKey( string keyName )
trigger.kv[ string keyName ] // trigger.kv[ "breakLinksToAttachedSpawner" ]
trigger.GetValueForKey( string keyName ) // trigger.GetValueForKey( "maintainVelocity" )

// Linked Entities
trigger.GetLinkEntArray()
trigger.GetLinkEnt()
trigger.LinkToEnt( entity ent )
trigger.UnlinkFromEnt( entity ent )

```

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
- trigger_weaponless
```

### Environment Triggers
```
- env_fog_controller
- env_sprite
- env_beam
```