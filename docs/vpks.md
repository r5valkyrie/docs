# VPKs (Valve Packs)

VPK is a proprietary Valve file format used for archives that store game assets in the Source Engine. VPKs superseded their predecessors, .GCF's (Gazelle Cache Files). Gazelle was an in-dev code name for Steam.

In order for Valve asset types (.MDL, .VTF, .VMT, etc.) to be usable by the Source Engine, they must be packed into VPKs and then loaded by the engine.

Over the years, Respawn ditched VPK in favor of their archive counter-part RPAK from their proprietary tech stack, RTech. Titanfall 1 exclusively used VPKs, Titanfall 2 used VPKs alongside RPAKs and Apex Legends used VPKs until Season 18, with everything except for .BSP maps (including their .bsp_lump and .ent files) and scripts being shifted to RPAKs from Season 12.

Note: although maps in the Season 3 build of Apex Legends use VPKs, many of their map geometry & static prop materials are stored inside RPAKs with the same name as the map, i.e.: mp_rr_desertlands_64k_x_64k.rpak

In the Season 3 build of Apex Legends, which R5V is based on, Valve's tech stack is used as a fallback if the engine cannot find the assets it is looking for inside Respawn's proprietary RPAKs. It is possible to activate this fallback system by force by hex editing individual assets' GUID values to be 0 before packing them into RPAKs, using RePak.

An example of using the Valve asset fallback system is presented below:

- Using a hex editor (such as 010 Editor) and the Respawn Binary Templates written by r-exx and rika, it is possible to open Respawn assets such as RMDL's and set their material / texture GUIDs to 0 / null them, prior to packing them into RPAKs, using RePak

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/vmtoverride.png)

- This will cause the Valve asset fallback system to kick in

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/vpkfallback1.png)

- The checkerboard texture indicates a missing material and is the default VPK texture for missing textures. This means that the englishclient_common VPK can be patched to include new VMT materials or that a separate VPK can be loaded with the VMT materials

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/vpkfallback2.png)

# VPK Loading

VPK Loading in R5V works in the following way:

- For player Clients, englishclient_mp_common.bsp.vpk and englishclient_frontend.bsp.vpk are loaded on game launch and kept loaded for the rest of the session. Shared base assets such as .VMTs, .VTFs, etc. are stored here. The lobby map is also stored in this VPK. The englishclient_frontend VPK is used for front end purposes, such as storing .cfg files, playlist .txt files, a particle effect manifest .txt file, menu loading VGUI VMTs and others. It is important to note that these configs are a leftover from Titanfall and are NOT the configs used for Apex Legends.
- For Dedicated Servers, englishserver_mp_common.bsp.vpk is loaded on game launch and kept loaded for the rest of the session. Shared base assets are stored here.  
- On map load, each map's corresponding VPK is loaded  
- On map unload, each map's corresponding VPK is unloaded  
- VPKs can be manually force loaded / mounted using the fs_vpk_mount Console Command, where fs stands for FileSystem  
- VPKs can be manually force unloaded / unmounted using the fs_vpk_unmount Console Command, where fs stands for FileSystem  

# ReVPKEdit

ReVPKEdit (basically a GUI for ReVPK, with ReVaMpT) can preview, pack, unpack and patch VPK archives as well as extract individual files from VPKs.

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/revpkedit1.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/revpkedit2.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/revpkedit3.png)
This is an example from Titanfall 2's englishclient_mp_common.bsp.vpk, to showcase the .MDL preview capabilities of ReVPKEdit.  
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/revpkedit4.png)


ReVPKEdit can patch existing VPK archives to include new files and folders by clicking on Edit -> Add Files... / Add Folder... or right clicking a folder and choosing the appropriate option.  

It should be noted that patching existing archives to edit included files or to add new files is buggy and can result in corruption of the entire VPK, hence one should keep backups.

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/revpkedit5.png)

ReVPKEdit can create new VPK archives from folders.

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/revpkedit6.png)

ReVPKEdit can unpack entire VPK archives or extract individual assets from VPK archives by clicking on "Edit". It is possible to export and convert assets to other types directly from ReVPKEdit.

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/revpkedit7.png)

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/revpkedit8.png)






