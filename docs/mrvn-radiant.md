# MRVN-Radiant Guide
============================================
# Table of Contents
## Introduction
## Chapter 1: Entities
## Chapter 2: Brushes
## Chapter 3: Materials
## Chapter 4: Decals
## Chapter 5: The Brush Tool
## Chapter 6: The Terrain Tool
============================================

# Introduction

Your toolkit:

- Transform Gizmos: standard rotate, scale, resize, translate, transform

- Brush Tool: the go-to tool for adding map geometry and brushes with special roles (i.e.: clipbrushes, etc.)

- Terrain Tool: a tool that uses Source Engine displacements to generate realistic terrain surfaces

- Model Browser: shows the set of models registered and parsed by MRVN-Radiant

- Surface Inspector: displays surface properties for level geometry

- Entity Inspector: displays the properties of a selected entity

- Entity List: lists all entities in the map


Currently, the asset resource paths for MRVN-Radiant are the following:

Engine path = the path to the directory which contains r5apex.exe

Models: [engine path]/platform/models

Textures: [engine path]/platform/textures

Shaders: [engine path]/platform/shaders


# Chapter 1: Entities

## What Is An Entity? 

From the R5V Documentation article about Entities:

In the Source Engine, entities are objects meant to be interactable for players or interacted with by other entities via [the I/O system.](https://developer.valvesoftware.com/wiki/Inputs_and_Outputs)  

The entity class (CBaseEntity) is defined in the engine code and it has multiple subclasses (CPlayer, CWeaponX, etc.)  

All models NECESSARILY belong to an instance of an entity type. Models cannot be spawned "on their own", without being attached / assigned to an entity.

Many entities are inherited from Valve's Source Engine, as ReSource (Respawn Source Engine) is a fork of the Portal 2 Branch of the Source Engine. An example would be "info_player_start".

Entities are separated into INTERNAL Entities and NON-INTERNAL Entities. INTERNAL Entities are entities which are processed by VBSP, then deleted or merged into another entity. These entities do NOT exist at map run-time, therefore they do NOT count towards the Entity limit. Non-internal entities exist at map run-time and are standalone.

Some entitities are known as "Point Entities" - these entities are distinguished by the fact that they usually do not have an associated 3D model, their position in the world affects / is relevant to their functionality and they are NOT Brush Entities.

## What Types of Entities Are There?

Since an entity is an abstraction representing an interactable object, there can be many types of entities.

## Model Entities

These entities have a 3D model attached to them. There are multiple sub-categories of Model Entities:

### Prop Entities

These entities represent objects that exist on the map. They usually have a model attached to them.
Among props, there are, in large (non-exhaustive list), four main categories:
- prop_static: objects used to decorate the level's environment; they're deemed "static" because the player is not meant to perform any specific actions on them, unlike prop_dynamic and prop_script; once the .map file is compiled into a .BSP (binary space partition) file, these props are no longer considered standalone entities and are merged into the BSP inside an entity lump, hence they cannot be controlled from squirrel_re scripts! Mapmakers coming over from other Source games may be wondering if prop_detail is used in Apex Legends and the answer is no.
- prop_dynamic: objects that the players / NPCs are meant to interact with; there is an immense variety to what these can represent, ranging from respawn beacons to supply bins to dome shields; these can be spawned from squirrel_re scripts to populate the world
- prop_script: props that have scripts attached to them; these can be spawned from squirrel_re scripts to populate the world - these props are in the map's .ent file
- prop_physics: props with interactive simulated physics, such as bouncing balls, loot rollers, dropped loot items (in certain conditions); these can be spawned from squirrel_re scripts to populate the world - these props are in the map's .ent file

The complete list of Prop Entities used by Apex Legends is this:
```
- prop_combine_ball 
- prop_control_panel 
- prop_data 
- prop_death_box 
- prop_debug 
- prop_door 
- prop_droppod 
- prop_dynamic 
- prop_dynamic_clientside 
- prop_dynamic_lightweight 
- prop_dynamic_override 
- prop_exfil_panel 
- prop_lightweightPropsSkipAnimData 
- prop_physics 
- prop_physics_override 
- prop_refuel_pump 
- prop_script 
- prop_static 
- prop_shield 
- prop_survival 
- prop_survivalSkipsAnimData 
- prop_weapon_rack
```
### The Weapon Entity

This entity subclass (CWeaponX in the engine) manages weapons and weapon mechanics.

### The Player Entity

The player entity (CPlayer) controls players.

### NPC Entities

NPC / AI Entities

```
- npc_saito
- npc_sarah
- npc_stalker
- npc_super_spectre // Reaper
- npc_support
- npc_soldier
- npc_spectre
- npc_spider
- npc_prowler
- npc_pilot
- npc_pilot_elite
- npc_marvin
- npc_operator
- npc_flyer
- npc_frag_drone // Explosive Tick
- npc_goliath
- npc_gunship
- npc_gunship_scripted
- npc_drone
- npc_drone_cloak
- npc_drone_worker
- npc_dropship
- npc_dummie
- npc_bish
- npc_turret_mega
- npc_turret_sentry
- npc_titan
```

## Point Entities

These entities are distinguished by the fact that they usually do not have an associated 3D model, their position in the world influences their functionality and they are NOT Brush Entities.

### Info Entities

These entities provide additional information to the engine, such as where the player should spawn in a level, i.e.: info_player_start. All of these entities are Point Entities.

A list of Info Entities:
```
- info_null
- info_player_start // Point entity
- info_player_teamspawn
- info_player_deathmatch
- info_target // Point entity
- info_target_clientside
- info_particle_system
- info_hardpoint
- info_hardpoint_frontier
- info_landmark
- info_gravity
- info_spawnpoint_marvin_barrel
- info_spawnpoint_control_panel
- info_spawnpoint_start
- info_spawnpoint
- info_spawnpoint_droppod_start
- info_spawnpoint_droppod
- info_spawnpoint_human_classname
- info_spawnpoint_human_start
- info_spawnpoint_human
- info_spawnpoint_turret
- info_spawnpoint_mega_turret
- info_spawnpoint_titan_classname
- info_spawnpoint_titan_start
- info_spawnpoint_titan
- info_spawnpoint_dropship_start
- info_spawnpoint_dropship
- info_spawnpoint_spectre
- info_spawnpoint_marvin
- info_spawnpoint_prowler
- info_vehicle_groundspawn
- info_replacement_titan_spawn
- info_spawnpoint_flag
- info_teleport_destination

// Likely additional data for AI behavior
- info_node 
- info_node_hint 
- info_node_air
- info_node_air_hint 
- info_node_link 
- info_node_link_controller 
- info_radial_link_controller 
- info_node_sniper 
- info_node_climb 
- info_node_safe_hint 
- info_frontline
- info_node_cover
- info_node_cover_left
- info_node_cover_right
- info_node_cover_stand
- info_node_cover_crouch
- info_node_cover_vantage
///////////////////////////////////////////

- info_intermission
- info_camera_link
- info_lightprobe
- info_hint
- info_placement_helper
```

## Other Types of Entities

### Logical Entities

Logical entities influence game logic, as their name implies. They are not physical objects and they are not considered Point Entities because their position in the map does not affect their functionality.

A list of Logic Entities:

```
- logic_choerographed_scene
- logic_proximity
```

### Environment Entities

These are entities related to the world environment. Some of these, like env_fog_controller are considered Logical Entities but have been grouped here for convenience.

A list of Environment Entities:
```
- env_cubemap
- env_fog_controller
- env_cascade_light
- env_exposure
- env_sprite_clientside // Client-side only sprite
- env_sprite
- env_sprite_oriented // Likely for billboards that orient themselves in the direction the player is facing
- env_beam
- env_laser
- env_wind
- env_entity_dissolver // dissolves entities on contact 
- env_shake
- env_explosion // Server only entity: Environment explosion. Deals radial linear falloff damage.
- env_physexplosion
- env_physimpact
- env_dropzone
- env_soundscape // Soundscape for ambient sounds
- env_soundscape_proxy
- env_soundscape_triggerable
```



# Chapter 2: Brushes

## What Are Brushes?

From the [Valve Developer Wiki](https://developer.valvesoftware.com/wiki/Brush):   


Brushes are convex 3D shapes, used by map makers / level designers to define the shape of the world and to create brush entities.   

When a map is compiled VBSP converts brush faces that touch a visleaf to groups of polygons. The resulting 'brush models' are stored within the BSP file and can be claimed by entities (e.g. the world, or your brush entity). The original brushes are retained in the BSP, though the benefits of this are not clear.  

In comparison to models, brushes are:  

- Unique every time  
- Low-detail and cheap  
- Lit with pre-computed lightmaps  
- Rigid (cannot deform)  


## Brush Entity Types

# Chapter 3: Materials

## What Are Materials?

Materials represent packages of data that affect the look and often the sounds and various other properties of an object. They reference various texture maps, shaders and material properties such as surface properties.

## How To Apply Materials To World Geometry

# Chapter 4:  Decals

## What Are Decals?

## How To Apply Decals To World Geometry


# Chapter 5: The Brush Tool

# Chapter 6: The Terrain Tool


The Terrain Tool uses Source Engine displacements to create terrain meshes.

Displacements are a complex topic. It is recommended to read [this article from the Valve Developer Wiki](https://developer.valvesoftware.com/wiki/Displacement) for more information on this subject.

