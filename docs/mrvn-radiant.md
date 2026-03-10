# MRVN-Radiant Guide
========================================================================
# Table of Contents
## How To Compile MRVN-Radiant (Windows)
## Introduction
## Chapter 1: The Viewports
## Chapter 2: Entities
## Chapter 3: Brushes
## Chapter 4: Materials
## Chapter 5: Decals
## Chapter 6: Gizmos
## Chapter 7: The Brush Tool
## Chapter 8: The Terrain Tool
## Chapter 9: Getting Started On Your First Map
========================================================================

# How To Compile MRVN-Radiant (Windows)

Prerequisites:
- Download the necessary dependencies at https://github.com/r5valkyrie/MRVN-WinDeps (latest release)
- Make sure you have Visual Studio 2026 installed with the C++ development packages and MSVC

1) Clone the R5V MRVN-Radiant Repo at https://github.com/r5valkyrie/MRVN-Radiant
2) Extract windeps.7z in the root directory of MRVN-Radiant. Make sure the path to the dependencies is ROOT/windeps/ and not ROOT/windeps/windeps
3) Extract install.7z (or install-debug.7z if you're running a Debug build) in ROOT/install. Make sure the path to those dependencies is ROOT/install and not ROOT/install/install.
4) Open Visual Studio 2026 and open the MRVN-Radiant Repo folder. Make sure you set the correct Build version (Release or Debug, for general use, select Release). VS 2026 will automatically generate the necessary CMake configuration (has to be done manually in older version of Visual Studio). 
5) Go to Build -> Build All
6) Wait for all the files to be compiled.
7) Open your MRVN-Radiant Repo and go to the "install" folder. In there, you will find radiant.exe.
8) Open radiant.exe and optionally pin it to your taskbar for easier access.
9) You are now ready to use MRVN-Radiant!

# Introduction

The R5V Fork of MRVN-Radiant is a fork of NetRadiant-custom, which is a fork of NetRadiant, which is a fork of GtkRadiant 1.5. NetRadiant was conceived as free, open-source software for map creation for Quake / idTech-based games. MRVN-Radiant built on that, with map creation capabilities for Titanfall Online, Titanfall 2 and Apex Legends. The R5V Fork of MRVN-Radiant adds a lot of crucial functionality to MRVN-Radiant, such as collision, lighting, static props, a terrain tool, a significantly improved interface and more.

Since the roots of MRVN-Radiant tie back to Quake and Apex Legends uses a heavily modified version of the Portal 2 Source Engine (whose roots can also be traced to the idTech Engine) engine branch, which stems from IdTech, there are many similarities between MRVN-Radiant and Valve's Source Engine map editor, Hammer (also known as WorldCraft / The Forge).

Your toolkit:

- Transform Gizmos: standard rotate, scale, resize, translate, transform

- Brush Tool: the go-to tool for adding map geometry and brushes with special roles (i.e.: clipbrushes, etc.)

- Terrain Tool: a tool that uses Source Engine displacements to generate realistic terrain surfaces

- Model Browser: shows the set of models registered and parsed by MRVN-Radiant

- Surface Inspector: displays surface properties for level geometry

- Entity Inspector: displays the properties of a selected entity

- Entity List: lists all entities in the map


Currently, the asset resource paths for MRVN-Radiant are the following:  
```
Engine path = the path to the directory which contains r5apex.exe

Models: [engine path]/platform/models

Textures: [engine path]/platform/textures

Shaders: [engine path]/platform/shaders
```

In order to familiarize yourself with the keybinds available at your disposal, go to View -> Shortcuts. Many, but not all, action keybinds can be remapped.  

For additional useful information, it is recommended to take a look at the View -> Show dropdown.  

CTRL + Z serves as Undo while CTRL + Y serves as Do.  

The toolbar can be dragged from the dot cluster symbol to be expanded and / or docked vertically, to the side. This will reveal many additional functionalities.  

For more settings, go to Edit -> Preferences.    

R5V can be launched directly from MRVN-Radiant; the launch settings are in Edit -> Preferences -> Settings category -> Launch.  

# Chapter 1: Viewports

For a good overview of the tools available in MRVN-Radiant, it is recommended to select the second layout from Edit -> Preferences -> Interface category -> Layout.  

This will show the following:  
```
- The 2D Viewport
- The 3D Viewport
- The Texture Inspector (bottom right)
- Logs (bottom left)
```

The two axes displayed on the 2D Viewport are shown in the top left corner of the 2D Viewport. These axes can be quickly changed by pressing the Change Views button (Default: CTRL+ TAB).    

Basic navigation in the 2D Viewport work in the following ways:  
- Right click drag will pan the view around  
- Scroll down will Zoom Out  
- Scroll up will Zoom In AT THE CURSOR POSITION  
- Left click dragging will insert a brush with the dimensions of the selection  
- Left clicking outside the newly inserted brush will de-select the brush  
- Left clicking the brush will select it  
- While the brush is selected, the area it covers can be dragged to reposition the brush in the world  
- Dragging the edges of the brush will NOT resize the brush  
- Resizing the brush can be done by activating the Resize tool (Default: Q) and dragging the red lines  


Basic navigation in the 3D Viewport work in the following ways:  
- Right click unlocks the camera for free navigation in 3 dimensions (WASD + mouse movement for camera angle)  
- Right clicking again will lock the camera  
- Right click drag pans the camera around  
- Scroll Up and Scroll Down function as Zoom In / Zoom Out and can be combined with free camera navigation  
- Left click drag inserts a brush, by default  
- Holding left click drag will allow resizing the brush in one plane  
- Holding SHIFT and right click drag create a marquee selection (rectangular selection)  


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

### Camera Entities

```
point_viewcontrol // camera entity which controls the Player's view
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

When a map is compiled VBSP converts brush faces that touch a [visleaf](https://developer.valvesoftware.com/wiki/Visleaf) to groups of polygons. The resulting 'brush models' are stored within the BSP file and can be claimed by entities (e.g. the world, or your brush entity). The original brushes are retained in the BSP.

In comparison to models, brushes are:  

- Unique every time  
- Low-detail and cheap  
- Lit with pre-computed lightmaps  
- Rigid (cannot deform)  


## Brush Entity Types

### Trigger Brush Entities

These brushes are invisible in normal gameplay and fire signals / outputs or have a certain on other entities that touch them.

A list of Trigger brushes:

```
- trigger_activate
- trigger_cylinder
- trigger_cylinder_heavy
- vortex_sphere // there is no trigger_sphere, so this is often used as a substitute for that (with a custom script setup!)
- trigger_proximity

- trigger_point_gravity
- trigger_gravity

- trigger_push
- trigger_updraft
- trigger_slip
- trigger_auto_crouch

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

- trigger_player_flashlight_on
- trigger_player_flashlight_off
- trigger_player_flashlight_zone

- trigger_playermovement
- trigger_look

- trigger_impact
- trigger_ignoresolids

- trigger_hurt
- trigger_death_fall

- trigger_fastball

- trigger_flag_clear
- trigger_flag_set

- trigger_soundscape

- trigger_once
- trigger_multiple
- trigger_multiple_clientside

- trigger_skydive
- trigger_weaponless
```

### Clip Brush Entities (Clipbrushes)

These brushes are invisible in normal gameplay and influence which entities can pass or cannot pass through them. 

In common use, Player Clip Brushes, which define areas that the player cannot access / physically pass through, got shortened to "Clipbrushes", hence the name.

Clipbrushes are also used to smoothen map navigation, such as overlapping smooth ramps over stair steps.

Creature clipbrushes are also known as "Monsterclips", which is where former high-profile Respawn developer Jason McCord's name comes from.

# Chapter 3: Materials

## What Are Materials?

Materials represent packages of data that affect the look and often the sounds and various other properties of an object. They reference various texture maps, shaders and material properties such as surface properties.

## How To Apply Materials To World Geometry

In order to apply materials to world geometry, select the geo brush, browse inside your Texture Inspector for the Material / Texture you want to apply (open folders and sub-folders with double left click), select the geometry brush and then double-click the Material in the Texture Inspector window. Note: the previews only show albedo / diffuse maps but the materials include all other texture maps used.

# Chapter 4: Decals

## What Are Decals?

## How To Apply Decals To World Geometry


# Chapter 5: Gizmos

Gizmos are used to control the position, orientation, rotation and size of entities inside MRVN-Radiant.  

The Gizmos currently available are:  
```
- Translate (Default: W)
- Rotate (Default: R)
- Scale
- Transform (Default: Q)
- Resize (Default: ...also Q; TODO: FIX THIS)
- Clipper (Default: X)
- UV Tool (Default: G)
```

Tip: Try holding the SHIFT key with everything to see what it does! In many cases it acts as a modifier for different behaviors  

The "Translate" tool (default keybind: W) will be your best friend. It can double as a resize tool, when used in tandem with Vertices (V) mode, Edges (E) mode or Faces (F) mode.  

The Rotate tool (default keybind: R) snaps to certain increments when holding SHIFT AFTER pressing a rotation circle (it must be after; TODO: fix this). This is very useful for fast 45 - 90 - 180 degree rotations.  

Holding SHIFT and right click dragging in the 3D viewport creates a marquee selection (rectangular selection).  

Holding SHIFT and clicking multiple Entities creates a multi-selection.  

# Chapter 6: The Brush Tool

# Chapter 7: The Terrain Tool

The Terrain Tool uses Source Engine displacements to create terrain meshes.

Displacements are a complex topic. It is recommended to read [this article from the Valve Developer Wiki](https://developer.valvesoftware.com/wiki/Displacement) for more information on this subject.

# Chapter 8: Getting Started On Your First Map

In order to be able to playtest your map and iterate on its design, you must first name and save your map.   

In order to be able to actually load the map in R5V, there are three mandatory aspects that need to be addressed:  

- The map's name MUST start with "mp_"; it is recommended that you name your map with the prefix "mp_rr_"  
- The map MUST have an associated VPK archive, even if it's empty! If you only plan to host the map on a listen server, you only need an englishclient_mp_rr_MAPNAME.bsp.pak000_dir.vpk, with a client_mp_rr_MAPNAME.bsp.pak000_000.vpk; if the map is to be hosted on DEDICATED servers, it MUST have an associated Server VPK as well.  
- The map MUST, for all intents and purposes, have an associated RPAK (.rpak, .starpak, opt.starpak) with the SAME NAME AS THE MAP, either in the main R5V Win64 paks directory (it must have the same name as the map so that the engine loads it automatically on map load) or inside the Win64 paks directory of the mod it comes with; if the map is to be hosted on DEDICATED servers, it MUST have an associated RPAK in Win64_Server as well.  

To create a basic map:
- Insert a brush that will act as a ground surface, so that you do not fall down into the Void.         
- Insert an info_player_start Entity. This Point Entity dictates the starting spawn location of the Player on the map.        
- It is now possible to Build [the map] and Launch the Game by pressing the green "Play" button in the Toolbar (Build comes from building / compiling, because the .map must be compiled into a BSP / Binary Space Partitioning file to be used by the game).
- After you are done testing, it is possible to exit the game by typing "quit" in the Developer Console (~).       