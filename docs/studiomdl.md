# StudioMDL

StudioMDL is the model compiler used in Source 1 games. It is a command line tool which takes a .QC (from QuakeC) file (essentially a sequence of keyvalues) as input and, using it as a template / instruction set, compiles .SMD (Studiomodel Data File) into a .MDL (Valve Model File) and usually, depending on its optional parameters, external files containing additional data, such as:

- VVD (Valve Studio Model Vertex Data) files

- VTX (Valve proprietary mesh strip format) files 

- ANI (additional animation data) files 

- PHY (Valve proprietary collision model data format) files


Seemingly paradoxically, animation-only .MDL files can exist; they are usually used for shared animations, so that they can easily be accessed by other models.


## Respawn's history with MDL

Respawn Entertainment acquired and used the Portal 2 (2011) branch of Source 1, from Valve, as its foundation for ReSource (Respawn Source Engine).

The developers at Respawn started with .MDL version 49 (v49) as the base for their models. For Titanfall (2014), they upgraded Valve's format to their own version, v52. Versions 50 and 51 are in-dev versions that were not shipped.

Titanfall 2 used .MDL version 53 (v53). With Apex Legends, Respawn finally transitioned to their own proprietary model format (based on Valve's), called "RMDL" (Respawn Model File). RMDL is known as .NDL subversion 54, a.k.a. MDL V54.

Respawn Entertainment's proprietary tech stack is known as "RTech" (Respawn Tech). It includes:

- RUI (Respawn UI, after Respawn ditched Valve's VGUI)
- RMDL (based on Valve's MDL, modelling done by Respawn in Maya & sculpting in ZBrush)
- RRIG (Respawn RIG for skeletal meshes / bones, done in Maya)
- RSEQ (Respawn animation SEQuence, animation done by Respawn in Maya and includes motion capture as well)
- RTK (currently unused in modded Apex, new format for some animated UI, introduced in Season 16, used concommitently with RUI)


TODO TODO TODO TODO

THIS ARTICLE IS VERY UNFINISHED

