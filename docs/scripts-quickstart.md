## Scripting Quickstart

A practical intro to how scripts in `vscripts/` are organized and how Server, Client, and UI VMs work together. Use this as a quick reference when adding new features.

### Where scripts live

- `vscripts/` contains Squirrel scripts for gameplay, client presentation, and UI.
  - Shared: `sh_*.gnut|.nut` (usable by multiple VMs)
  - Server: `mp/`, `ai/`, `titan/`, server-only `_*` files
  - Client: `client/`, `cl_*.gnut|.nut`
  - UI: `ui/`, `clui_*`

### How scripts load (scripts.rson)

The engine loads files based on expressions in `vscripts/scripts.rson`. Use the right condition when registering your file:

```text
When: "SERVER || CLIENT || UI"
Scripts:
[
  sh_assert.nut
  sh_consts.gnut
  sh_networkvars.gnut
  sh_utility_all.gnut
  ...
]

When: "SERVER"
Scripts:
[
  _vscript.gnut
  _init.gnut
  mp/_codecallbacks.gnut
  ai/_ai_soldiers.gnut
  ...
]

When: "CLIENT"
Scripts:
[
  cl_vscript.gnut
  client/rui/cl_hud.gnut
  client/cl_player.gnut
  ...
]
```

Add your file path (relative to `vscripts/`) under a condition that matches where it should run.

### VM responsibilities

- Server (authoritative)
  - Game rules, entity creation/destruction, damage/heal, AI, traces, inventories, timers
  - Writes networked state; validates client actions

- Client (presentation + prediction)
  - HUD/RUI, particles, sounds, cosmetic state, mild input prediction
  - Reads networked state; must not authoritatively mutate gameplay

- UI (menus/lobby)
  - Menus, lobby panels, store, loadouts; no world entity access

### Guard code per-VM

```nut
#if SERVER
  // Spawn entities, set networkvars, apply damage
#endif

#if CLIENT
  // HUD/RUI updates, local effects, prediction checks
#endif

#if UI
  // Menu/lobby scripts only
#endif
```

### Lifecycle hooks you’ll use most

- Early compile-time hook (captures consts in DEV):
```nut
void function CodeCallback_Precompile()
{
#if DEVELOPER
  getroottable().originalConstTable <- clone getconsttable()
#endif
}
```

- Server post-entity initialization (safe to kick off systems):
```nut
global function CodeCallback_PostEntityInit

bool _initialized = false

void function CodeCallback_PostEntityInit()
{
  printl("CODE_SCRIPT: _init")
  Assert(!_initialized)
  _initialized = true
  RunCallbacks_EntitiesDidLoad()
  FlagSet("EntitiesDidLoad")
}
```

### Server↔Client communication

- Networkvars
  - Define shared vars in `sh_networkvars.gnut` and read them on both VMs
- Remote calls / net wrapper
  - Use the provided wrappers in `sh_net_wrapper.gnut` and `_remote_functions_mp.gnut`
- Principle
  - Server owns state, Client mirrors for presentation; UI is separate

### Useful shared building blocks

- `sh_consts.gnut`, `sh_consts_mp.gnut`: constants
- `sh_entitystructs.gnut` (+ `cl_entitystructs.gnut`): entity data
- `sh_utility_all.gnut`, `_utility_shared.nut`: helpers
- `weapons/_weapon_utility.nut`: weapon helpers
- `sh_mod_callbacks.gnut`: plugin-style callbacks between mods

Registering a simple callback:
```nut
void function MyMod_Init()
{
  ModCallbacks_RegisterBoolCallback("CanDropCustomLoot", function():bool { return true })
}

bool canDrop = ModCallbacks_CallBoolCallback("CanDropCustomLoot", false)
```

### Weapons and abilities (SERVER || CLIENT)

- Place files under `vscripts/weapons/` (e.g., `mp_weapon_zipline.nut`, `mp_ability_grapple.nut`)
- Add to a weapons block in `scripts.rson` guarded by `SERVER || CLIENT`
- Use attack param flags for prediction:
```nut
void function OnPrimaryAttack( WeaponPrimaryAttackParams params )
{
  if ( params.firstTimePredicted )
  {
    // client FX/sound gated by prediction
  }
  #if SERVER
    // authoritative hit/damage
  #endif
}
```

### UI panels (UI VM) and in-world screens (CLIENT)

- Menu/UI logic: `vscripts/ui/` and `client/rui/`
- In-world panels: register in `scripts/vgui_screens.txt`, create `.res` in `scripts/screens/`

Example registration (vgui_screens.txt):
```text
VGUI_Screens
{
  my_example_panel
  {
    type        vgui_screen_panel
    pixelswide  512
    pixelshigh  256
    resfile     "scripts/screens/my_example_panel.res"
  }
}
```

### Debugging basics

- Use `printl("[Tag] message")` to trace execution
- If nothing happens, verify your file is listed in `scripts.rson` under the right `When:` block
- Check load order: put shared prerequisites (consts, structs) before dependents

### Quick checklists

Add a gameplay feature
- [ ] Shared types in `sh_*` if needed
- [ ] SERVER logic in a server-only file
- [ ] CLIENT visuals/HUD in client file
- [ ] Entries added to `scripts.rson` (correct `When:`)
- [ ] Logs confirm it runs; state flows via networkvars or wrapper calls

Add a weapon/ability
- [ ] Place file in `vscripts/weapons/`
- [ ] Add to the `SERVER || CLIENT` weapons block in `scripts.rson`
- [ ] Separate server authority from client prediction/FX

Add a UI panel
- [ ] Create `.res` in `scripts/screens/`
- [ ] Register in `scripts/vgui_screens.txt`
- [ ] Drive content from CLIENT/UI scripts as appropriate

### Handy globals

```nut
void function printl( var text ) { return print(text + "\n") }

// Unique ids and map IO
string function UniqueString( string prefix = "" ) { /* see _vscript.gnut */ }
function EntFire( target, action, value = null, delay = 0.0, activator = null ) { /* see _vscript.gnut */ }
```

If you want, we can scaffold a minimal SERVER/CLIENT pair or a weapons template you can copy into `vscripts/weapons/`.