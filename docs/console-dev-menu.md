
# Console Dev Menu (Top Bar Presets)

The **Console Dev Menu** in Valkyrie allows developers to create a customizable tabbed menu in the in-game console’s top bar.  
It provides a quick-access UI for spawning weapons, switching maps, testing abilities, toggling debug tools, and more.

The system works by registering "preset commands" that are displayed hierarchically as tabs and buttons. These presets can be added dynamically from mod config files (`MODNAME/cfg/autoload/configname.cfg`) or entered live through console commands.

----------

## Console Commands

### `dev_menu_add`

`dev_menu_add  "<Path/Button>"  "<Command>"` 

Adds a new entry to the dev menu.

-   **Path/Button**:  
    A hierarchical string that defines the structure of the tab menu.
    
    -   Bracketed names (e.g., `[ Give ]`) are recommended as **top-level tabs**.
        
    -   Slashes (`/`) create subcategories and buttons.
        
    -   A path segment may either represent a **menu folder** or a **command button**.
        
-   **Command**:  
    The console command that will run when the button is pressed.  
    Can be any valid command, script, or alias.
    

**Example:**

`dev_menu_add  "[ Give ]/Weapon/Assault Rifle/Flatline"  "give mp_weapon_vinson"` 

This creates:

-   A top-level tab called `[ Give ]`

-   Inside it, a submenu `Weapon > Assault Rifle > Flatline`

-   A button labeled `Flatline` that runs `give mp_weapon_vinson`
    

----------

### `dev_menu_remove`

`dev_menu_remove  "<Path/Button>"` 

Removes a single entry from the dev menu, identified by its full path.  
Doing `dev_menu_remove "[ Give ]"`  will remove that path and all entries below it.

**Example:**

- `dev_menu_remove  "[ Give ]/Weapon/Assault Rifle/Flatline"` 

----------

### `dev_menu_clear`

`dev_menu_clear` 

Removes **all entries** from the dev menu, clearing the entire tab system.  
Not recommended in mod cfg as this may clear all commands added by other mods.

----------

## Example Usage

Here’s a structured example showing how the system can be used to organize common developer tools:

### Weapons

 - `dev_menu_add "[ Give ]/Weapon/Assault Rifle/Flatline"  "give mp_weapon_vinson"`
 - `dev_menu_add "[ Give ]/Weapon/Assault Rifle/Hemlok"  "give mp_weapon_hemlok"`
 - `dev_menu_add "[ Give ]/Weapon/Assault Rifle/R-301"  "give mp_weapon_rspn101"`
 - `dev_menu_add "[ Give ]/Weapon/Assault Rifle/Havoc AR"  "give mp_weapon_energy_ar"`

### Abilities

 - `dev_menu_add "[ Give ]/Ability/Wraith Tactical"  "give mp_ability_phase_walk"`
 - `dev_menu_add "[ Give ]/Ability/Pathfinder Ultimate"  "give mp_weapon_zipline"` 

### Legends

 - `dev_menu_add "[ Legend ]/Wraith"  "loadouts_devset character_selection character_wraith"`
 - `dev_menu_add "[ Legend ]/Valkyrie"  "loadouts_devset character_selection character_valkyrie"` 

### UI Toggles

- `dev_menu_add "[ UI ]/Show FPS"  "cl_showfps 1"`
- `dev_menu_add "[ UI ]/Hide FPS"  "cl_showfps 0"` 

### Maps

- `dev_menu_add "[ Change Level ]/Worlds Edge S3"  "map mp_rr_desertlands_64k_x_64k"` 
- `dev_menu_add "[ Change Level ]/Olympus S7"  "map mp_rr_olympus"` 

----------

## Notes & Best Practices

-   **Organization**: Use `[ Brackets ]` for top-level tabs and clear naming conventions to keep the menu navigable.
    
-   **Separators**: Adding a line with `"---------"` as a path and `""` as the command creates a visual divider.
    
    `dev_menu_add  "[ Bots ]/-------------"  ""` 
    
-   **Dynamic Updates**: Presets can be reloaded at runtime by clearing (`dev_menu_clear`) and re-executing a config file.
    
-   **Safety**: Avoid binding destructive or game-crashing commands unless clearly labeled.
    

----------

### This system is especially useful for:

-   Quickly giving weapons or abilities during testing
-   Switching between legends
-   Enabling/disabling debug tools like `cl_showfps`
-   Rapidly swapping maps
