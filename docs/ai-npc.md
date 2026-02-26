# AI / NPCs

This article contains various types of information on AI / NPC entities and systems.

## Check Functions (Bool Returns)
```
IsNPC( entity npc )
IsSpectre( entity npc )
IsSniperSpectre( entity npc )
IsSuicideSpectre( entity npc ) 
IsStalker( entity npc )
IsSuperSpectre( entity npc ) // Reapers
IsFragDrone( entity npc ) // Explosive ticks
IsTick( entity npc )
IsGrunt( entity npc )
IsFlyer( entity npc ) // The velociraptor-looking avians
IsProwler( entity npc )
IsSpider( entity npc )
IsTitan( entity npc )
```

## NPC Entity Methods

### General Keyvalue Check Methods
```
npc.HasKey( string keyName )
npc.GetValueForKey( string keyName )
npc.Dev_GetAISettingByKeyField( string keyName )
```
### Visual Methods
```
npc.EnableRenderAlways()
npc.SetSkin( int skinID ) // for models with multiple materials that can be switched between
npc.SetDecal( int decalIndex ) // int?
npc.SetCamo( int camoIndex )
npc.GetSettingModelName()
npc.GetModelName()
npc.SetModel( modelName )
npc.SetValueForModelKey( npc.GetSettingModelName( ) )
npc.MakeInvisible()
npc.MakeVisible()
npc.Show()
npc.Hide()
npc.GetBodygroup( string bodyGroupName ) // bodyGroupName = "gear", etc. - defined inside the model's .qc file, pre-compile. Post-compile (referring to the model's compilation using studiomdl.exe and a .qc file), bodygroups are read-only.
npc.SetBodyGroup( bodyGroupIndex, stateIndex ) // stateIndex = 1 or 0 for enabled or disabled
```
### Attachment Methods
```
npc.LookupAttachment( string attachmentName ) // attachmentName = "CHESTFOCUS" (default), "EYEGLOW", etc. // used for checks inside if statements, etc. to see if the attachment exists
npc.GetAttachmentOrigin( int attach_id )
npc.GetAttachmentAngles( int attach_id )
```

### Physics Methods
```
npc.Solid()
npc.NotSolid()
npc.SetCollisionAllowed( bool )
```
### Position Methods
```
npc.GetOrigin()
npc.SetOrigin()
npc.GetAngles()
npc.SetAngles()

npc.EyeAngles() // gets an NPC's line of sight angles
npc.EyeAngles().x
npc.EyeAngles().y
npc.EyeAngles(.z)

npc.SnapToAbsOrigin( vector positionVector )
```

### Parenting Methods
```
npc.SetParent()
npc.ClearParent()
```
### Health / Shield Methods
```
npc.SetValidHealthBarTarget( bool )

npc.SetInvulnerable()
npc.ClearInvulnerable()

npc.GetMaxHealth()
npc.SetMaxHealth( int maxHealth )
npc.GetHealth()
npc.SetHealth( int Health )

npc.GetShieldHealth()
npc.SetShieldHealth()
```

### Damage Methods
```
npc.SetTakeDamageType( typeflag ) // typeflag = DAMAGE_YES, etc.

npc.SetDamageNotifications( bool )
npc.SetDamageNotifications( bool )
```

### NPC State / Hibernation Methods
```
npc.GetNPCState() // returns a string with the NPC state, i.e.: "combat"
npc.EnableHibernation()
npc.DisableHibernation()
```

### Sound Methods 
```
npc.SetFootstepType( string footstepType ) // footstepType = "bloodhound", "pathfinder", etc.
```

### Realm Methods
```
npc.RemoveFromAllRealms()
npc.AddToRealm( int realmID )
```

### Weapon Methods
```
npc.GiveWeapon()
npc.GetActiveWeapon( eActiveInventorySlot.SLOTNAME )
npc.GetMainWeapons()
npc.SetActiveWeaponByName( eActiveInventorySlot.SLOTNAME, string weaponName )
npc.GiveOffhandWeapon( string weaponName, int slotNumber)
npc.TakeOffhandWeapon( SLOT ) // SLOT = OFFHAND_ORDNANCE, OFFHAND_SPECIAL, OFFHAND_ANTIRODEO, etc.
```

### AI Settings Methods
```
npc.HasAISettings()
npc.GetAISettingsName()
npc.AISetting_SummonDrone()
npc.AISetting_GetGrenadeWeapon()
```
### AI Class Methods
```
npc.GetAIClass()
npc.GetClassName()
```

### Team Methods
```
npc.GetTeam()
npc.SetAutoSquad()
```

### Flag Methods
```
npc.SetCapabilityFlag( flag, bool ) // bits_CAP_NO_HIT_SQUADMATES, bits_CAP_SYNCED_MELEE_ATTACK, etc.
npc.EnableNPCFlag( flag )
npc.DisableNPCFlag( flag )
npc.SetNPCMoveFlag( flag )
npc.EnableNPCMoveFlag( flag )
npc.DisableNPCMoveFlag( flag ) 
```




### Behavior Methods
```
npc.InitFollowBehavior( entity entityToBeFollowed, followBehavior )
npc.EnableBehavior( string behaviorName ) // behaviorName = "Follow", "Assault", etc.
npc.DisableBehavior( entity behaviorName ) // behaviorName = "Follow", "Assault", etc.
```
### Assault Behavior Methods
```
npc.AssaultSetGoalRadius( int radius )
npc.AssaultSetGoalHeight( int height )
npc.AssaultSetAngles( angles, bool )
npc.AssaultSetFightRadius( int fightRadius )
npc.AssaultPoint( vector assaultPoint )
npc.AssaultPointClamped( vector pointClamped )
```
### Potential Threat Methods
```
npc.SetPotentialThreatPos ( vector positionVector )
npc.ClearPotentialThreatPos()
```


### Grapple Methods
```
npc.DisableGrappleAttachment()
```

### Animation Methods
```
npc.SetDoFaceAnimations( bool )
npc.Anim_Play( anim )
npc.Anim_Stop()
npc.SetActivityModifier(ACTIVITYMODIFIER_NAME, bool) // ACTIVITYMODIFIER_NAME = ACT_MODIFIER_STAGGER, ACT_MODIFIER_ONEHANDED, etc.
npc.ClearMoveAnim()
npc.Anim_ScriptedPlayActivityByName( string activityName, bool, float value) // activityName = "ACT_STAND_TO_STAGGER", etc.
npc.UseSequenceBounds( bool ) // likely for (geometrically clamped) scripted sequences
npc.IsInterruptableForScriptedAnim() // returns a bool
```

### Titan Methods
```
npc.GetTitanSoul()
npc.GetTitanSoul().SetTitanSoulNetEnt( "dialogueEnt", dialogueEnt )
npc.SetNumRodeoSlots(SLOTS) // SLOTS = PROTOTYPE_DEFAULT_TITAN_RODEO_SLOTS, etc.
```

### NPC Title Methods
```
npc.GetTitle()
npc.SetTitle()
npc.GetSettingTitle()
```

### Enemy Related Methods
```
npc.SetEnemyChangeCallback( funcref() callback )
entity enemy = npc.GetEnemy()
vector ornull lkp = npc.LastKnownPosition( entity enemy )
npc.ClearAllEnemyMemory()
```

### NPC Interactibility Methods
```
npc.SetUsableByGroup( string groupName ) // "enemies pilot"
```
### Entity Linking Methods
```
npc.IsLinkedToEnt( entity entityToCheckIfLinkedTo )
npc.LinkToEnt( entity entityToLinkTo )
npc.UnlinkFromEnt( entity entityToUnlinkFrom )
```

### Execution Methods
```
npc.SetCanBeMeleeExecuted( bool )
npc.SetCanBeGroundExecuted( bool )
```

### NPC Death / Destruction Methods
```
npc.Die( damageAttacker, npc, { force = force, scriptType = DF_GIB, damageSourceId = eDamageSourceId.suicideSpectreAoE } )
npc.Die() // doesn't require all those arguments
npc.Dissolve( ENTITY_DISSOLVE_CORE, <0,0,0>, 500 )
npc.Destroy()
npc.Gib( vector gibVector ) // npc.Gib( <0, 0, 100> ) , etc.
npc.SetDeathNotifications( bool )
```

### NPC Boss Methods
```
npc.GetBossPlayer() // gets the player that "owns" an NPC, for example after hacking (internally named "leeching") an NPC with the Data Knife
```

## NPC Keyvalues
```
npc.kv.solid // 0, 2, 4, 6, 8
npc.kv.unlink // indicates whether the entity is ready / meant to be unlinked from another entity at a certain point, works like a script flag, essentially
npc.executedSpawnOptions
npc.kv.additionalequipment
npc.kv.grenadeWeaponName
npc.kv.start_alert
npc.kv.alwaysalert
npc.kv.physdamagescale
npc.kv.follow_mode
npc.kv.TitanType
npc.kv.disable_vdu
npc.kv.CollisionGroup
npc.kv.crashOnDeath
npc.ai.titanSpawnLoadout
npc.ai.titanSettings
npc.ai.titanSettings.titanSetFile
npc.ai.bossTitanType
npc.ai.bossTitanVDUEnabled
npc.ai.fragDroneArmed // bool
npc.ai.leechInProgress // bool
npc.ai.damageFlagsVulnerable = (DF_SNIPER) // for setting the damage flags for the damage types the NPC is vulnerable to, i.e.: DF_SNIPER, DF_SHOTGUN, etc. - if multiple, use the bitwise operator | in between them, between brackets, i.e.: (DF_SNIPER | DF_SHOTGUN)
npc.ai.preventFriendlyDamage // bool
npc.spawnTime // float timestamp
npc.ai.npcType // takes its values from the eNPC enum, i.e.: eNPC.PROWLER
soul = npc.GetTitanSoul()
soul.soul.titanLoadout = npc.ai.titanSpawnLoadout
npc.ai.droneSpawnAISettings
npc.ai.shouldDropBattery
npc.ai.preventOwnerDamage
npc.ai.crawling // bool
npc.ai.startCrawlingTime // float timestamp
npc.kv.drop_battery

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

### NPC Names
```
Militia (Titanfall faction)
Rocket Grunt = TEAM_MIL_GRUNT_MODEL_ROCKET
Shotgun Grunt = TEAM_MIL_GRUNT_MODEL_SHOTGUN
SMG Grunt = TEAM_MIL_GRUNT_MODEL_SMG
Rifle Grunt = TEAM_MIL_GRUNT_MODEL_RIFLE

IMC (Titanfall faction)
Rocket Grunt = TEAM_IMC_GRUNT_MODEL_ROCKET
Shotgun Grunt = TEAM_IMC_GRUNT_MODEL_SHOTGUN
SMG Grunt = TEAM_IMC_GRUNT_MODEL_SMG
Rifle Grunt = TEAM_IMC_GRUNT_MODEL_RIFLE
```
### AI Classes
```
AIC_TITAN_BUDDY // BT, Buddy Titan
```

## Squad Functions
```
SetNPCSquadMode( npc.kv.squadname, squadMode ) // SQUAD_MODE_MULTIPRONGED_ATTACK
npc.SetGrade( npcRank ) // rank inside the squad, i.e. squad leader?
```

## Animation Functions
```
PlayCrawlingAnim( npc, "ACT_RUN" )
```

## AI Schedules (Respawn version of Valve's ACTBUSY system)
```
npc.SetDefaultSchedule( string scheduleName ) // scheduleName = "SCHED_PATROL_PATH", etc.
```

## Minimap Functions

```
npc.Minimap_Hide( TEAM, null ) // TEAM = TEAM_IMC, TEAM_MIL, etc.
npc.Minimap_AlwaysShow( TEAM, null) // TEAM = TEAM_IMC, TEAM_MIL, etc.
```

## Team Functions
```
SetTeam( entity npcEntity, TEAM) // TEAM = TEAM_IMC, TEAM_MIL, etc. - an NPC can be on multiple teams, i.e. TEAM_IMC | TEAM_MILITIA
```


## Leech Mechanic


TODO