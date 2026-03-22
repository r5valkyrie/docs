# VGUI (Valve GUI)

VGUI is Valve's proprietary Graphical User Interface (GUI) format for Source Engine (1) games.   
As Respawn had not finished developing their proprietary tech stack (RTech) by the release of Titanfall 1, the game almost exclusively used Valve's tech stack. VGUI is an incredibly powerful technology which allows not only for the creation of dynamic menus, but also 2D UI projected onto 3D meshes / map geometry. All that being said, VGUI was fully superseded by Respawn's proprietary RUI in Apex Legends, while Titanfall 2 used a mix which contained mostly RUI (with a few exceptions, such as some Smart Pistol VGUI elements and the Archer Rocket Launcher's VGUI).  

All VGUI Screens and Menus have .res / .menu layout dependencies which MUST be Precached prior to use, using:  

```
void function PrecacheRes( string resFile )

i.e.: PrecacheRes( "vgui_fullscreen_pilot" )

Or

Add a line to Precache inside cl_mapspawn.gnut, line 156: void function PrecacheResFiles()
```

A toggle-able console command exists to allow for only VGUI elements to be drawn, while hiding RUI elements: rui_drawEnable   

# VGUI Menus

It is possible to create entire game menus with VGUI, as illustrated in Titanfall 1, which solely uses VGUI. The menus can have animated elements and text that changes depending on certain variables. Menu elements can have callback functions attached. All menus require a layout file (.res or .menu) located in platform/resource or platform/scripts/resource.  

# VGUI Screens

VGUI supports projecting 2D screens onto 3D meshes with the vgui_screen Point Entity. An example of a projected 2D screen is the Archer Rocket Launcher's pop-out display. Every element featured on that display is VGUI. 

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/archerscreen.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/archerscreen2.png)

VGUI Screens must be registered in platform/scripts/vgui_screens.txt. VGUI Screens also require a .res / .menu file which define their layouts, placed in either platform/resource or platform/scripts/resource.   

The Valve Developer Wiki has [a guide on VGUI Screen Creation](https://developer.valvesoftware.com/wiki/VGUI_Screen_Creation).  


The following are functions for controlling VGUI elements which exist in R5V:  

```
entity function CreateClientsideVGuiScreen( string resFileName, VGUI_SCREEN_PASS_X, vector coords, vector angles, int width, int height)  

where VGUI_SCREEN_PASS_X can be one of the following:

VGUI_SCREEN_PASS_COCKPIT
VGUI_SCREEN_PASS_HUD
VGUI_SCREEN_PASS_VIEWMODEL
VGUI_SCREEN_PASS_VIEWMODEL_NOZOOM
VGUI_SCREEN_PASS_WORLD

void function Create_Display( entity panel )

void function VGUIUpdateSpectre( entity panel )

function VGUISetupGeneric( entity panel )

function VGUIUpdateGeneric( entity panel )

function VGUISetupForRemoteTurret( entity panel )

function VGUIUpdateForRemoteTurret( entity panel )
```

# VGUI HUDs (Heads-Up Displays)

VGUI HUDs can be implement in one of two ways:  
1) VGUI Screens attached to the local client player proxy's Cockpit entity (all of Titanfall 1 and a few elements in Titanfall 2)  
2) A static HUD - drawn at the top of the local client's screen - which is not attached to the Cockpit and does not move with it (most Titanfall 2 and all of Apex Legends)  

![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/smartpistollockon.png)
![Alt text](https://raw.githubusercontent.com/r5valkyrie/docs/refs/heads/main/docs/smartpistollockon2.png)

# VGUI Videos

VGUI also supports playing videos on UI panels. This is how Titanfall 1 played VDU (Virtual Display Unit) videos in the top corners of the HUD.  














setting z 