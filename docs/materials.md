# Materials


## Introductory note:

The acronym "GUID" refers to "Globally Unique Identifier", which, in this case, represents a unique hexadecimal value that points to assets inside of RPAKs. Example: 0x1EBD063EA03180C7


# Main section

Materials in Apex Legends function similarly to how they function in most 3D Graphics pipelines: they represent containers for files and data that dictate how 3D objects should be displayed on the screen, by communicating with 3D Graphics APIs ( in Apex Legends' case, DirectX 11 (R5 Reloaded and R5 Valkyrie) or DirectX 12, in the retail version of the game). The 3D Graphics APIs interface with the engine (ReSource / Respawn Source) and the GPU.

At their core, material files are binary files which store data such as:
## The material's name 
- This is used so that the material can be pointed to inside the engine and manipulated 
## Width and height parameters 
- These establish the size parameters of the texture maps used by material to control display parameters such as reflectivity, glossiness, etc.
- Examples: 2048 x 2048, 1024 x 1024, etc.
## Flags
-  Very small code flags (Read from the Mozilla Foundation: [bitwise flags](https://developer.mozilla.org/en-US/docs/Glossary/Bitwise_flags)) used to indicate which of the various rendering features are used
- Examples: 
	- Glue flags - material behavior flag for material compositing
	- Blend state mask flag - defines blend behavior for textures & materials 
	- Blend flags - defines blend behavior for textures & materials (There is an abundance of educational resources on material blending. As an example, think of a dirt material brush blending with a mud material brush)
	- Depth stencil flags - used for Direct3D depth / stencil buffers, which control which pixels are drawn / hidden for special effects, etc.
		- For more information, consult [Microsoft's documentation](https://learn.microsoft.com/en-us/windows/uwp/graphics-concepts/depth-and-stencil-buffers) directly
	- Rasterizer flags (see: Rasterization as a technique of drawing 3D models. [Article from NVIDIA](https://blogs.nvidia.com/blog/whats-difference-between-ray-tracing-rasterization/))
	- Uber buffer flags (UberShaders are large, complex shaders)
- If a flag's value is set to "0x0" (0x is a prefix denoting hexadecimal notation and 0 is a null value), this represents the "disabled" state.
## Features
- Sets other features which are to be used in rendering
## Samplers
- Very likely refers to the notion of "Texture Samplers", which can be read about in depth [in this section of "A Trip through the Graphics Pipeline"](https://alaingalvan.gitbook.io/a-trip-through-the-graphics-pipeline/chapter4-texture-samplers), courtesy of Alain Galvan
## Surface properties
- Similarly to [Valve's material files (VMT's)](https://animcoding.com/post/animation-tech-intro-part-1-skinning/), which were the precursors to Respawn's own material files (and were used in their 2014 title, Titanfall), Apex Legends materials include key information about surface properties, which influence behavior in the virtual world
- Examples: "default" (generic), "plastic", "flesh", etc.
- Apex Legends material files include two keys for surface properties (surfaceProp and surfaceProp2)
## Shader type
- Different types of models use different types of materials. 
- Examples: 
	- Character model, weapon viewmodels, other models which use more than one bone in their armature / rig, for animations, use the material types "SKNP" (SKiNned model, Packed vertex positions), SKNC and SKNU (Regular vertices)
		- Important Note: Skinning, in this case, refers to skeletal meshes and animations, NOT textures! For more information on what skinning represents in this use case, refer to these articles:
		- Pluralsight: [Understanding Skinning: A Vital Step for Any Rigging Project](https://www.pluralsight.com/resources/blog/software-development/understanding-skinning-vital-step-rigging-project)
		- Anim Coding: [Animation Tech Intro Part 1: Skinning](https://animcoding.com/post/animation-tech-intro-part-1-skinning/)
	- Static props (such as set dressing / map environmental detail) and models that are only rigged with one bone in their armature / rig use RGDC / RGDP materials
	- Ziplines, certain water bodies, etc. use RGDU materials
	- World geometry brushes use WLDC (packed vertex positions) / WLDU (regular vertex positions) materials, WLD = World
	- Particle systems / particle effects use PTCU (regular vertex positions) / PTCS materials, PTC = Particle
## Shader set
 - (stores the individual shaders used for the material, which are programs written, in this case (because of Direct3D), in Microsoft's HLSL (High-Level Shader Language), that control how light is supposed to interact with objects in the 3D world, how objects should be displayed, etc.)

## Textures
 - These represent a series of keys and values in which the keys are slots and the values point to texture map assets inside of RPAKs. 
 - The values of the keys MUST be:
	 - file paths pointing to the texture maps inside of the RPAKs, such as "texture/art/mshop/weapons/class/sniper/kraber/base/kraber_base_main_albedoTexture.rpak"
	 - OR
	 - the GUIDs which point to the texture maps
- The keys themselves point to the slot of the texture, which indicates the type of texture map each image represents. The keys MUST be identical to the keys in $textureTypes and those keys MUST correspond EXACTLY with the meanings set inside of the Apex Legends engine. For example, slot 0 can ONLY ever be assigned to albedo / diffuse maps, slot 1 can ONLY ever be assigned to normal maps, etc.
## Texture types
- As stated in the previous subsection, these key-value pairs indicate the meaning and order of each texture map assigned to the material. These MUST match the EXACT slot numbers set in the engine.
- Many slots for Titanfall 2 SKN materials are DIFFERENT from Apex Legends slots! ALWAYS check to see if they are set properly.
- The meaning of each slot in the engine is represented in this list below:

slot0: _col  (albedo / diffuse map - [Note: these maps contain the base surface colors](https://help.autodesk.com/view/3DSMAX/2017/ENU/?guid=GUID-69603DC2-F58C-4053-A955-EA19FDB8D084))  
slot1: _nml (normal map - [Note: these mimic light interaction with real surface geometry, to simulate depth, surface details, etc.](https://docs.unity3d.com/6000.3/Documentation/Manual/StandardShaderMaterialParameterNormalMap.html))  
slot2: _gls/_exp (glossiness map - [Note: controls glossiness levels](https://help.autodesk.com/view/3DSMAX/2017/ENU/?guid=GUID-76F225CD-982A-4F17-9935-A656E6878BCD))    
slot3: _spc (specular map - [Note: greyscale texture map for shininess / matteness](https://help.autodesk.com/view/3DSMAX/2017/ENU/?guid=GUID-76F225CD-982A-4F17-9935-A656E6878BCD))  
slot4: _ilm (emissive / self-illumination map - [Note: texture map for making the object appear to glow](https://help.autodesk.com/view/3DSMAX/2017/ENU/?guid=GUID-0584ED4B-FE91-4B0B-A09C-22557D5D51DD))    
slot5: _ao (ambient occlusion map - [Note: article on ambient occlusion maps, by Paul H. Paulino](https://www.paulhpaulino.com/the-vfx-map-baking-guide-ambient-occlusion))    
slot6: _cav/cvt (cavity map - [Note: article on cavity maps, by Paul H. Paulino](https://www.paulhpaulino.com/improve-your-organic-textures-with-a-cavity-map))    
slot7: _opa (opacity map - Note: controls opacity / transparency)  
slot8: detail/camo   
slot9: _dm_nml/_nml (detail normal map)  
slot10: _msk (detail texture mask - [Note: article on texture masks by Epic Games](https://dev.epicgames.com/documentation/en-us/unreal-engine/using-texture-masks-in-unreal-engine))    

Blend Materials (used for maps):  
slot16: _bm (blendmap)  
slot22: _col (albedo / diffuse map - Note: base colors)  
slot23: _nml (normal map - Note: light interaction with the surface, to simulate depth, surface details, etc.)  
slot24: _gls/_exp (glossiness map)  
slot25: _spc (specular map - Note: greyscale texture map for shininess / matteness)

IMPORTANT NOTE: Different material types have different texture slots (i.e.: SKNP, PTCU, etc.)

References courtesy of Autodesk, Unity Technologies, Epic Games and Paul H. Paulino

## Additional materials
 - In this last section of the material files, it is possible to point the material to other materials it should inherit, such as Depth Shadow materials, Depth Prepass materials (game dev is an iterative process with multiple passes), VSM (virtual shadow map) materials, Colpass (color pass) materials, Texture Animation materials
 - The values are GUIDs that reference other material assets. A value of "0x0" indicates that there is no map assigned to the key.


# Examples

An example of a .JSON manifest (used for packing assets into RPAKs using RePak) for a SKNP material, used by the Kraber:

```
{
"name": "art/mshop/weapons/class/sniper/kraber/base/kraber_base_main",
"width": 2048,
"height": 2048,
"depth": 0,
"glueFlags": "0x56000020",
"glueFlags2": "0x100000",
"blendStates": [
   "0xF0000000",
   "0xF0000000",
   "0xF0000000",
   "0xF0000000",
   "0xF0000000",
   "0xF0000000",
   "0xF0000000",
   "0xF0000000"
],
"blendStateMask": "0x4",
"depthStencilFlags": "0x17",
"rasterizerFlags": "0x6",
"uberBufferFlags": "0x0",
"features": "0x1F5A92BD",
"samplers": "0x1D0300",
"surfaceProp": "default", // surface properties
"surfaceProp2": "",
"shaderType": "sknp", // proprietary Respawn material
"shaderSet": "0x846292DC5424105A",
"$textures": { 
   "0": "texture/art/mshop/weapons/class/sniper/kraber/base/kraber_base_main_albedoTexture.rpak",
   "1": "texture/art/mshop/weapons/class/sniper/kraber/base/kraber_base_main_normalTexture.rpak",
   "2": "texture/art/mshop/weapons/class/sniper/kraber/base/kraber_base_main_glossTexture.rpak",
   "3": "texture/art/mshop/weapons/class/sniper/kraber/base/kraber_base_main_specTexture.rpak",
   "4": "texture/art/mshop/weapons/class/sniper/kraber/base/kraber_base_main_aoTexture.rpak",
   "5": "texture/art/mshop/weapons/class/sniper/kraber/base/kraber_base_main_cavityTexture.rpak"
},
"$textureTypes": {
   "0": "albedoTexture",
   "1": "normalTexture",
   "2": "glossTexture",
   "3": "specTexture",
   "4": "aoTexture",
   "5": "cavityTexture"
},
"$depthShadowMaterial": "0x2B93C99C67CC8B51", // depth materials
"$depthPrepassMaterial": "0x1EBD063EA03180C7",
"$depthVSMMaterial": "0xF95A7FA9E8DE1A0E",
"$depthShadowTightMaterial": "0x227C27B608B3646B",
"$colpassMaterial": "0x11A4A3CBA679E447", // colpass material
"$textureAnimation": "0x0"
}

```

## Animated Materials

Lots of animated materials use what are known as "scrolling textures" with special shaders that scroll through the texture.

## VMTs

VMT (Valve Material File) is Valve's proprietary material file format for the Source Engine. In order for VMTs to be usable by Source, they must be packed into VPKs (Valve Packs). The Branch of Source that Respawn forked for their own use was the Portal 2 Branch, which used VMTs. Titanfall 1 (Dev Codename R1 for "Respawn 1", as in Respawn Entertainment's first title) exclusively used VMTs, alongside material proxies. Titanfall 2 (Dev Codename R2) used VMTs alongside proprietary Respawn materials (SKN, WLD, etc.) and Apex Legends (Dev Codename R5) also used VMTs alongside proprietary materials (SKNP, WLDC, etc.) until season 18, when VPKs and VMTs were fully phased out.  

Since R5V is based on the Season 3 build of Apex Legends, it still uses VMTs for some particles, world materials, decals and billboards, alongside VPKs, RPAKs and proprietary Respawn materials.    

VMTs are also fallback materials if the engine cannot locate material dependencies in R5V.  

An example of using the Valve asset fallback system is presented below:

- Using a hex editor (such as 010 Editor) and the Respawn Binary Templates written by r-exx and rika, it is possible to open Respawn assets such as RMDL's and set their material / texture GUIDs to 0 / null them, prior to packing them into RPAKs, using RePak

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/vmtoverride.png)

- This will cause the Valve asset fallback system to kick in

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/vpkfallback1.png)

- The checkerboard texture indicates a missing material and is the default VPK texture for missing textures. This means that the englishclient_common VPK can be patched to include new VMT materials or that a separate VPK can be loaded with the VMT materials

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/vpkfallback2.png)

While Titanfall 2 mainly used proprietary Respawn materials, such as SKN, PTC, etc., the game also simultaneously used VMTs for everything from particle effects / systems (muzzle flashes, dirt, smoke, etc.) to sprites, to cables (ziplines, tethers, grapple cables), to VGUI (Valve Graphical User Interface) materials for Titan & MRVN UI and various other UI elements.

Example of a VMT from Titanfall 2:

```
// From materials/particle/electric_arc/electric_arc_trail_ob3.vmt, from Titanfall 2's englishclient_mp_common.bsp.pak00_dir.vpk

"SpriteCard"
{
	$basetexture "particle\electric_arc\electric_arc_trail"
//	$RAMPTEXTURE "particle/ramps/elec_ramp.vtf"
//	$POWERFUNCTION "[0.8 2.5 5]"
//	$texAlphafromColor 4
	$texColorFromAlpha 1
	$additive 	1
	$translucent 	1
	$nocull	1
	$splinetype	1
	$overbrightfactor 3
	$vertexFogAmount 1
    $disableTSAA 0
    $tsaaFullyResponsive 1
    $tsaaMotionAlphaThreshold 0.01
    $tsaaMotionAlphaRamp 10
}
```

## How VMTs Are Applied To Models

Pre-compile, models point to the directory containing the VMTs corresponding to the model, using $cdmaterials, starting from the *game* ROOT (not system root). The engine is aware of specifically which VMT files it's supposed to load from the listed directory / directories from the model's .SMD (Studio Model Data) file(s), which contain(s) the model's / models' UV map(s). Specific VMTs are mapped to wrap around specific regions of the 3D meshes.  

A concrete example is outlined below, concerning the Archer Rocket Launcher's viewmodel  

From ptpov_law.qc (the QC file corresponding to the Archer Rocket Launcher's viewmodel):  

```
// obtained by decompiling with Crowbar 

$modelname "weapons/shoulder_rocket_SRAM/ptpov_law.mdl"

$bodygroup "ptpov_law"
{
	studio "v_shoulder_rocket_SRAM.smd"
}


// skipping

$cdmaterials "models\weapons\attachments"
$cdmaterials "models\weapons\shoulder_rocket_sram"
$cdmaterials ""

$attachment "CAMERA_BASE" "jx_c_pov" 0 0 0 rotate -90 -90 0

// skipping
$attachment "SCR_BL" "def_c_sightRear_screen" 1.72 -0.68 -0.03 rotate 0 180 -180
$attachment "SCR_TR" "def_c_sightRear_screen" 0.29 0.34 -0.03 rotate 0 180 -180
$attachment "muzzle_flash" "muzzle_flash" 0 0 0 rotate 0 0 0
// , etc.
```

Looking inside Titanfall 1's "englishclient_mp_common.bsp.pak000_dir" VPK, we can see the following:  

materials/models/weapons/shoulder_rocket_sram contains:  

**glass.vmt**  
**rocket_launcher.vmt**  
**rocket_launcher_ironsights.vmt**    

And the VMTs' corresponding VTF texture maps. Keep these names in mind.  

materials/models/weapons/attachments contains the VMTs and VTFs for all attachments in the game.  


From v_shoulder_rocket_SRAM.smd, used as an input for studiomdl.exe alongside ptpov_law.qc:  

```
// line 174
triangles // defines the vertices and triangle faces for the mesh while also defining the UV map
ammo_counter_pistol
  73 3.277271 49.654732 6.843596 0.000000 -0.923878 -0.382687 0.531934 0.283372 1 73 1.000000
  73 3.733402 49.654732 6.843596 0.000000 -0.923878 -0.382687 0.717284 0.283372 1 73 1.000000
  73 3.733402 49.654732 7.379279 0.000000 -1.000000 0.000000 0.717284 0.014278 1 73 1.000000
ammo_counter_pistol
  73 3.277271 49.654732 6.843596 0.000000 -0.923878 -0.382687 0.531934 0.283372 1 73 1.000000
  73 3.733402 49.654732 7.379279 0.000000 -1.000000 0.000000 0.717284 0.014278 1 73 1.000000
  73 3.277271 49.654732 7.379279 0.000000 -1.000000 0.000000 0.531934 0.014278 1 73 1.000000
ammo_counter_pistol
  73 3.302308 49.731125 6.821100 0.901516 0.175039 -0.395766 0.607662 0.423167 1 73 1.000000
  73 3.302308 50.122726 6.994298 0.901516 0.175039 -0.395766 0.607662 0.923588 1 73 1.000000
  73 3.311612 50.105915 7.008056 0.901516 0.175039 -0.395766 0.589090 0.907973 1 73 1.000000
ammo_counter_pistol
  73 3.302308 49.731125 6.821100 0.901516 0.175039 -0.395766 0.607662 0.423167 1 73 1.000000
  73 3.311612 50.105915 7.008056 0.901516 0.175039 -0.395766 0.589090 0.907973 1 73 1.000000
  73 3.311612 49.732258 6.842794 0.901516 0.175039 -0.395766 0.589090 0.436269 1 73 1.000000

// skipping ahead

rocket_launcher_ironsights
  78 3.282058 51.710392 8.568565 -0.688342 -0.161681 -0.707139 0.332505 0.251007 1 78 1.000000
  78 3.313523 51.690903 8.542393 -0.688342 -0.161681 -0.707139 0.324821 0.243319 1 78 1.000000
  78 3.294027 51.659439 8.568565 -0.688342 -0.161681 -0.707139 0.319771 0.251007 1 78 1.000000
rocket_launcher_ironsights
  78 3.516387 50.712811 8.568565 -0.688342 -0.161681 -0.707139 0.083265 0.251005 1 78 1.000000
  78 3.547853 50.693321 8.542393 -0.688342 -0.161681 -0.707139 0.078212 0.243316 1 78 1.000000
  78 3.528357 50.661854 8.568565 -0.688342 -0.161681 -0.707139 0.070526 0.251005 1 78 1.000000
rocket_launcher_ironsights
  79 4.749236 50.905853 8.217897 0.428194 0.903687 0.000000 0.802424 0.690988 1 79 1.000000
  79 4.713510 50.922783 8.190126 0.428194 0.903687 0.000000 0.795620 0.700731 1 79 1.000000
  79 4.713510 50.922783 8.217897 0.428194 0.903687 0.000000 0.802457 0.700717 1 79 1.000000

// skipping ahead

rocket_launcher
  73 -2.955390 49.729454 4.125346 -0.711272 0.702917 -0.000014 0.508274 0.142563 1 73 1.000000
  73 -3.161590 49.525780 3.964416 -0.711272 0.702917 -0.000014 0.516058 0.135988 1 73 1.000000
  73 -2.936515 49.586090 14.598495 -0.711272 0.702917 -0.000014 0.511896 0.436211 1 73 1.000000
rocket_launcher
  73 -0.191396 44.562500 10.850089 -0.707107 0.687809 0.164069 0.074165 0.318153 1 73 1.000000
  73 -0.191396 43.962021 11.624443 -0.721859 -0.099298 0.684879 0.084771 0.309931 1 73 1.000000
  73 -0.191396 44.392731 11.561791 -0.517139 0.577466 0.631744 0.084329 0.316598 1 73 1.000000
rocket_launcher
  73 -0.191396 44.562500 10.850089 -1.000000 0.000000 0.000000 0.074165 0.318153 1 73 1.000000
  73 -0.191396 43.397552 11.363914 -0.715385 -0.149896 0.682462 0.081205 0.302199 1 73 1.000000
  73 -0.191396 43.962021 11.624443 -0.721859 -0.099298 0.684879 0.084771 0.309931 1 73 1.000000

  // skipping ahead

ammo_counter_screen_bg
  73 3.311612 49.732258 6.842794 0.000000 0.404485 -0.914545 0.987854 0.005548 1 73 1.000000
  73 3.311612 50.105915 7.008056 0.000000 0.404485 -0.914545 0.987854 0.990395 1 73 1.000000
  73 3.699062 50.105915 7.008056 0.000000 0.404485 -0.914545 0.008062 0.990395 1 73 1.000000
ammo_counter_screen_bg
  73 3.311612 49.732258 6.842794 0.000000 0.404485 -0.914545 0.987854 0.005548 1 73 1.000000
  73 3.699062 50.105915 7.008056 0.000000 0.404485 -0.914545 0.008062 0.990395 1 73 1.000000
  73 3.699062 49.732258 6.842794 0.000000 0.404485 -0.914545 0.008062 0.005548 1 73 1.000000
ammo_counter_clip_digit0
  73 3.500935 49.826782 6.855247 0.000000 0.404485 -0.914545 0.000000 0.000000 1 73 1.000000
  73 3.343543 49.826782 6.855247 0.000000 0.404485 -0.914545 1.000000 0.000000 1 73 1.000000
  73 3.343543 50.093567 6.973066 0.000000 0.404485 -0.914545 1.000000 1.000000 1 73 1.000000
ammo_counter_clip_digit0
  73 3.500935 49.826782 6.855247 0.000000 0.404485 -0.914545 0.000000 0.000000 1 73 1.000000
  73 3.343543 50.093567 6.973066 0.000000 0.404485 -0.914545 1.000000 1.000000 1 73 1.000000
  73 3.500935 50.093567 6.973066 0.000000 0.404485 -0.914545 0.000000 1.000000 1 73 1.000000
ammo_counter_clip_digit1
  73 3.664040 49.826782 6.855247 0.000000 0.404485 -0.914545 0.000000 0.000000 1 73 1.000000
  73 3.500935 49.826782 6.855247 0.000000 0.404485 -0.914545 1.000000 0.000000 1 73 1.000000
  73 3.500935 50.093567 6.973066 0.000000 0.404485 -0.914545 1.000000 1.000000 1 73 1.000000
ammo_counter_clip_digit1
  73 3.664040 49.826782 6.855247 0.000000 0.404485 -0.914545 0.000000 0.000000 1 73 1.000000
  73 3.500935 50.093567 6.973066 0.000000 0.404485 -0.914545 1.000000 1.000000 1 73 1.000000
  73 3.664040 50.093567 6.973066 0.000000 0.404485 -0.914545 0.000000 1.000000 1 73 1.000000
ammo_counter_meter
  73 3.664040 49.760117 6.825617 0.000000 0.404485 -0.914545 0.000000 0.000000 1 73 1.000000
  73 3.343543 49.760117 6.825617 0.000000 0.404485 -0.914545 1.000000 0.000000 1 73 1.000000
  73 3.343543 49.826782 6.855247 0.000000 0.404485 -0.914545 1.000000 1.000000 1 73 1.000000
ammo_counter_meter
  73 3.664040 49.760117 6.825617 0.000000 0.404485 -0.914545 0.000000 0.000000 1 73 1.000000
  73 3.343543 49.826782 6.855247 0.000000 0.404485 -0.914545 1.000000 1.000000 1 73 1.000000
  73 3.664040 49.826782 6.855247 0.000000 0.404485 -0.914545 0.000000 1.000000 1 73 1.000000
glass
  81 5.532819 50.576344 7.877347 0.000000 -1.000000 0.000000 0.865270 0.101287 1 81 1.000000
  81 6.957448 50.576344 7.877347 0.000000 -1.000000 0.000000 0.031864 0.100585 1 81 1.000000
  81 6.957448 50.576344 7.894001 0.000000 -1.000000 0.000000 0.031836 0.088749 1 81 1.000000
glass
  81 5.532819 50.576344 7.877347 0.000000 -1.000000 0.000000 0.865270 0.101287 1 81 1.000000
  81 6.957448 50.576344 7.894001 0.000000 -1.000000 0.000000 0.031836 0.088749 1 81 1.000000
  81 5.532819 50.576344 7.894001 0.000000 -1.000000 0.000000 0.867270 0.088749 1 81 1.000000


// etc.  

```

Hence, the model will have the following VMTs as dependencies that it will search for:  

- ammo_counter_pistol.vmt  
- rocket_launcher_ironsights.vmt  
- rocket_launcher.vmt  
- ammo_counter_screen_bg.vmt  
- ammo_counter_clip_digit0.vmt  
- ammo_counter_clip_digit1.vmt  
- ammo_counter_meter.vmt  
- glass.vmt  

These VMTs were mapped onto the 3D model using UV Unwrapping to create a matrix for mapping 2D textures onto a 3D model. This UV Map / Matrix is stored inside the model and is not a texture map, but a 2D matrix of coordinates corresponding to points in the 3D model. UV comes from the axes oU (horizontal) and oV (vertical), because X, Y, Z and W were taken already by Quaternions which store coordinates for a position in 3D space and the rotation of an object. Both the oU and oV axes range from 0 to 1, where 1 is the max height / width, meaning that 0.5 , 0.5 is always the center of the UV Map. For more information, consult the [Valve Developer Wiki Article on UV Maps](https://developer.valvesoftware.com/wiki/UV_map).


## Material Proxies

Material Proxies represent code that allows the properties of VMTs to be manipulated, hence they are used to create dynamic or animated textures.  
For Titanfall 1, which exclusively used Valve's Source Engine tech stack, Respawn mainly used Material Proxies to create the in-world (diegetic) weapon displays (i.e.: Ammo counters, the Archer Rocket Launcher's screen, the L-STAR's heat mechanic, etc.).  
Titanfall 2 used VMT Material Proxies alongside RUI while they were developing their own proprietary tech stack, known as RTech (Respawn Tech).  
In Apex Legends, Material Proxies were all but phased out in favor of RUI, with the exception of Titanfall 2 remnants.  

More information can be found on the Valve Developer Wiki: [Material Proxies](https://developer.valvesoftware.com/wiki/Material_proxies), [List of Material Proxies](https://developer.valvesoftware.com/wiki/List_of_material_proxies).

Material Proxies from Titanfall 1 and Titanfall 2 are still fully functional in Season 3 of Apex Legends, with some small exceptions.

The Titanfall 1's RE-45 Auto model is illustrated below, imported into Blender, using the Blender Source Tools Blender plugin, after having decompiled the .MDL into a .QC and .SMDS, with Crowbar / Studiomdl  

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/blenderre45r1.png)

The model's UV Map / Matrix is present in the model's .SMD  

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/re45uvunwrap.png)

VMTs containing Material Proxies are applied in the marked locations  

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/vmts3.png)

Here are some examples of Material Proxies, beginning from the "Proxies" section:

```
// From materials/models/weapons/lstar/lstar_heat_charge.vmt, from Titanfall 2's englishclient_mp_common.bsp.pak00_dir.vpk

"UnlitTwoTexture"
{
	"$surfaceprop"	"metal"
	"$basetexture" "models\weapons\lstar\lstar_heat_charge_col"
	"$texture2" "effects\laserplane_atmosphere"
	$allowoverbright 1
	$decal 1
	"$translucent" 1
	"$additive" 1
	$layercolor1 "[2 1.38 1]"
	$layeralpha1 "1"
	$layercolor2 "[3 1 0.5]"
	$layeralpha2 "1"

	"Proxies" // Proxies start here
	{

		"TextureScroll"
		{
			"texturescrollvar" "$texture2transform"
			"texturescrollrate" "-0.05"
			"texturescrollangle" "180"
			"texturescale"	.25
		}

		"LSTARHeatFraction"
		{
			"resultVar" "$alpha"
		}
	}

}
```

```
// From materials/models/humans/mri/mri_male_v1/mri_male_v1.vmt, from Titanfall 2's englishclient_mp_common.bsp.pak00_dir.vpk
// This is used for Sonar scans

"UnlitTwoTexture"
{
	"$surfaceprop" "metal"
	//"$basetexture" "models\titans\titan_ar_marker\titan_ar_marker_col"
	"$basetexture" "effects\white"
	$texture2 		effects\dev_scanline_small

	$alpha 1
	$nodecal 1
	$model 1
	$ignorez 1
	$translucent 1
	$nofog 1
	$no_fullbright 1
	$allowoverbright 1
	$translucent 1
	$additive 1
	$screenspacecoordssquare2 1
	$layercolor1 "[0.1 0.0 0.0]"
	$layeralpha1 "0.1"
	$layercolor2 "[0.4 0.05 0.0]"
	$layeralpha2 "1.0"
	$addlayers 1
	
	$fresnel 0

	$fresnelSharpness1 1.0
	$fresnelOuterStrength1 0.0
	$fresnelInnerStrength1 1.0

	$fresnelSharpness2 0.0
	$fresnelOuterStrength2 1.0
	$fresnelInnerStrength2 0.0

	$t2scroll		"[0 1]"
	$t2scale		"[3 3]"

	//$matchColor		"[0.0 0.25 0.4]"
	//$mismatchColor	"[0.4 0.05 0.0]"
	$matchColor		"[0.0 1.5 2.4]"
	$mismatchColor	"[20. 0.60 0.0]"

	Proxies // Proxies start here
	{
		EntitySonarColor
		{
			sameTeamColorVar	$matchColor
			otherTeamColorVar	$mismatchColor

			resultvar	$layercolor2
		}

		EntitySonarColor
		{
			sameTeamColorVar	$matchColor
			otherTeamColorVar	$mismatchColor

			resultvar	$layercolor1
		}

		EntityScript
		{
			scriptFuncName	"VMTCallback_MPEntitySonarFrac" // Material Proxies can be used to add logic to trigger callbacks directly from a material
			resultVar $alpha
		}

		TextureScroll
		{
			"textureScrollVar" "$t2scroll"
			"textureScrollRate" 0.8
			"textureScrollAngle" -90
		}

		TextureTransform
		{
			translateVar	$t2scroll
			scaleVar		$t2scale
			resultVar 		$texture2transform
		}
	}
}
```

```
// From materials/models/titans/titan_destruction/titan_destruction_c.vmt, from Titanfall 2's englishclient_mp_common.bsp.pak00_dir.vpk

"UnlitTwoTexture"
{
	"$nodecal" "1"
	$allowoverbright 1

	"$no_fullbright" 1
	"$model" "1"

	"$basetexture" "effects\electric_anim"
	"$Texture2" "models\titans\titan_destruction\titan_destruction_c_col"
//	"$Texture2" "effects\white"


//	"$color2" "[3 2 1]" //fire
//	"$color2" "[1 2 3]" //Electricity

	"$sinemin" 0
	"$sinemax" 0
	"$tex2scroll" 0

    $matchColor		"[0.1 0.25 1.0]"
    $mismatchColor	"[1.0 0.25 0.1]"
	$allowDiffuseModulation "0"

	"Proxies" // Proxies start here
	{
		EntitySonarColor
		{
			sameTeamColorVar	$matchColor
			otherTeamColorVar	$mismatchColor

			resultvar	$color2
		}

		//Play baseTexture frames
		"AnimatedTexture" // Animated texture proxy examples
		{
			"animatedtexturevar" "$basetexture"
			"animatedtextureframenumvar" "$frame"
			"animatedtextureframerate" "24"
		}
		//Scroll Base Texture
		"TextureScroll"
		{
			"texturescrollvar" "$BASETEXTURETRANSFORM"
			"texturescrollrate" 0.2
			"texturescrollangle" -90
		}
		//Tex2 Minimum Scroll Speed
		"GaussianNoise"
		{
			"mean" "-2"
			"halfWidth" -4
			"minval" -1
			"maxval" -2
			"resultVar" "$sinemin"
		}
		//Tex2 Maximum Scroll Speed
		"GaussianNoise"
		{
			"mean" "2"
			"halfWidth" 4
			"minval" 1
			"maxval" 2
			"resultVar" "$sinemax"
		}
		"Sine"
		{
			"sineperiod" ".15"
			"sinemin" "$sinemin"
			"sinemax" "$sinemax"
			"resultVar" "$tex2scroll"
		}
		//Scroll Texture2
		"TextureScroll"
		{
			"texturescrollvar" "$texture2transform"
			"texturescrollrate" "$tex2scroll"
			"texturescrollangle" "-90"
		}

	}
}
```