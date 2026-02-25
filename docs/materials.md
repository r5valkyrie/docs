# Materials


## Introductory note:

The acronym "GUID" refers to "Globally Unique Identifier", which, in this case, represents a unique hexadecimal value that points to assets inside of RPAKs. Example: 0x1EBD063EA03180C7

---



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
		- For more information, consult [Microsoft's documention](https://learn.microsoft.com/en-us/windows/uwp/graphics-concepts/depth-and-stencil-buffers) directly
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
- The meaning of each slot in the engine is represented in this list below:

slot0: _col  (albedo / diffuse map - [Note: base colors](https://help.autodesk.com/view/3DSMAX/2017/ENU/?guid=GUID-69603DC2-F58C-4053-A955-EA19FDB8D084))

slot1: _nml (normal map - [Note: light interaction with the surface, to simulate depth, surface details, etc.](https://docs.unity3d.com/6000.3/Documentation/Manual/StandardShaderMaterialParameterNormalMap.html))

slot2: _gls/_exp (glossiness map - [Note: shockingly, controls glossiness levels](https://help.autodesk.com/view/3DSMAX/2017/ENU/?guid=GUID-76F225CD-982A-4F17-9935-A656E6878BCD))  

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

References courtesy of Autodesk, Unity Technologies, Epic Games and Paul H. Paulino

## Additional materials
 - In this last section of the material files, it is possible to point the material to other materials it should inherit, such as Depth Shadow materials, Depth Prepass materials (game dev is an iterative process with multiple passes), VSM (virtual shadow map) materials, Colpass (color pass) materials, Texture Animation materials
 - The values are GUIDs that reference other material assets. A value of "0x0" indicates that there is no map assigned to the key.



An example of a .json manifest (used for packing assets into RPAKs using RePak) for a SKNP material, used by the Kraber:

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

"surfaceProp": "default",

"surfaceProp2": "",

"shaderType": "sknp",

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

"$depthShadowMaterial": "0x2B93C99C67CC8B51",

"$depthPrepassMaterial": "0x1EBD063EA03180C7",

"$depthVSMMaterial": "0xF95A7FA9E8DE1A0E",

"$depthShadowTightMaterial": "0x227C27B608B3646B",

"$colpassMaterial": "0x11A4A3CBA679E447",

"$textureAnimation": "0x0"

}