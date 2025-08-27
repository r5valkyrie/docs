### Developer Menu (Dev Menu window + Top Bar)

The Developer Menu provides quick access to common tools and commands in two UIs:
- Dev Menu window: a standalone ImGui window with a hierarchical “Commands” tree and a “Search” tab.
- Top Bar “Dev” menu: a lightweight hierarchical menu always available in the top bar.

Both UIs share the same preset command data. Add/remove/clear presets once; they appear in both.

----------

### Console Commands

#### dev_menu_add
Syntax:
`dev_menu_add "<Path/Button>" "<Command>"`

- Path/Button:
  - Use [ Brackets ] for top-level categories (e.g., [ Give ]).
  - Use slashes (/) to build folder structure and final button labels.
  - A path segment represents either a folder or a terminal button.
- Command:
  - Any valid console command, script, alias, etc.

Example:
`dev_menu_add "[ Give ]/Weapon/Assault Rifle/Flatline" "give mp_weapon_vinson"`

Creates:
- Top-level tab [ Give ]
- Submenu Weapon > Assault Rifle > Flatline
- Button “Flatline” that runs give mp_weapon_vinson

#### dev_menu_remove
Syntax:
`dev_menu_remove "<Path/Button>"`

Removes a single entry by full path. Removing a top-level like [ Give ] removes all its children.

Example:
`dev_menu_remove "[ Give ]/Weapon/Assault Rifle/Flatline"`

#### dev_menu_clear
Syntax:
`dev_menu_clear`

Clears all dev menu entries. Avoid using this in cfgs unless you intend to clear other mods’ entries too.

----------

### Example Usage

#### Weapons
- `dev_menu_add "[ Give ]/Weapon/Assault Rifle/Flatline" "give mp_weapon_vinson"`
- `dev_menu_add "[ Give ]/Weapon/Assault Rifle/Hemlok" "give mp_weapon_hemlok"`
- `dev_menu_add "[ Give ]/Weapon/Assault Rifle/R-301" "give mp_weapon_rspn101"`
- `dev_menu_add "[ Give ]/Weapon/Assault Rifle/Havoc AR" "give mp_weapon_energy_ar"`

#### Abilities
- `dev_menu_add "[ Give ]/Ability/Wraith Tactical" "give mp_ability_phase_walk"`
- `dev_menu_add "[ Give ]/Ability/Pathfinder Ultimate" "give mp_weapon_zipline"`

#### Legends
- `dev_menu_add "[ Legend ]/Wraith" "loadouts_devset character_selection character_wraith"`
- `dev_menu_add "[ Legend ]/Valkyrie" "loadouts_devset character_selection character_valkyrie"`

#### UI Toggles
- `dev_menu_add "[ UI ]/Show FPS" "cl_showfps 1"`
- `dev_menu_add "[ UI ]/Hide FPS" "cl_showfps 0"`

#### Maps
- `dev_menu_add "[ Change Level ]/Worlds Edge S3" "map mp_rr_desertlands_64k_x_64k"`
- `dev_menu_add "[ Change Level ]/Olympus S7" "map mp_rr_olympus"`

----------

### Notes & Best Practices

- Organization: Use [ Brackets ] for top-level categories; keep names concise and clear.
- Separators: Use a visual divider by adding a path with dashes and an empty command:
  `dev_menu_add "[ Bots ]/-------------" ""`
- Dynamic updates: You can clear and re-add presets at runtime from cfgs (dev_menu_clear then exec your cfg).
- Safety: Avoid destructive or crash-prone commands unless clearly labeled.

----------

### UI Behavior

- Dev Menu window:
  - Commands tab: hierarchical tree. Clicking a leaf executes its command.
  - Search tab: case-insensitive filter over all commands; shows “Exec” button to run.
- Top Bar “Dev” menu:
  - Compact hierarchical menu mirroring the same presets.

Hotkeys:
- Dev Menu window toggles via DevMenu hotkeys (default F3 / Delete). You can change these in ImGui settings.