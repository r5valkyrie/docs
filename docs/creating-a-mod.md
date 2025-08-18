## Creating a normal mod — Quick Guide

### Overview
The mod system discovers mods under `mods/`, loads their `mod.vdf` metadata, and adds each mod's folder to the engine search paths. Mods can ship assets, scripts, audio, and more. Enable/disable state is tracked in `mods/mods.vdf`.

Key facts:
- Mods live under `mods/<YourMod>/` (recursive discovery of `mod.vdf`).
- Metadata file is `mod.vdf` (Valve KeyValues format).
- New mods default to enabled unless listed in `mods/mods.vdf`.
- You can list or reload mods at runtime via console.

### Minimal folder layout
```
mods/
  MyFirstMod/
    mod.vdf
    <your assets and subfolders>
```

### `mod.vdf` required fields
These are mandatory. If any are missing/empty, the mod is skipped.

```text
"Mod"
{
    "author"       "YourName"
    "name"         "My First Mod"
    "id"           "myorg.MyFirstMod"    // letters, '.' and '_' only; min length 4
    "description"  "Short description of what this mod does."
    "version"      "1.0.0"
}
```

ID rules:
- Allowed characters: letters A–Z/a–z, period (.) and underscore (_)
- Minimum length: 4
- The engine normalizes the ID internally (periods become underscores) to derive unique symbols. Keep IDs unique across all mods.

### Optional: ConVars
Define console variables your mod provides. Each subkey name is the ConVar name.

```text
"Mod"
{
    ...
    "ConVars"
    {
        "my_mod_speed"
        {
            "flags"      "RELEASE|ARCHIVE"   // same flag strings the engine uses
            "helpText"   "Player speed multiplier"
            "usageText"  "Set to a value between 0.5 and 2.0"
            "Values"
            {
                "default" "1.0"
                "min"     "0.5"
                "max"     "2.0"
            }
        }
        // more convars...
    }
}
```

Notes:
- If a ConVar of that name already exists, the new one is skipped with a warning.
- Flags must be valid; see engine ConVar flags strings.

### Optional: Localization files
Register localization files shipped in your mod. Only the subkey names are read; values are ignored.

```text
"Mod"
{
    ...
    "LocalizationFiles"
    {
        "resource/localization/my_mod_english.txt"  "1"
        "resource/localization/my_mod_french.txt"   "1"
    }
}
```

Place these files relative to your mod root. The engine adds your mod directory to the `GAME` search path so these paths resolve.

### Optional: Conditional pak loading by playlist
Control when your mod's rPaks should be loaded, based on playlist names.

```text
"Mod"
{
    ...
    "PakLoadOnPlaylists"
    {
        "mp_public"   "1"
        "fs_coop"     "1"
        // omit or set to "0" to skip loading on other playlists
    }
}
```

If this block is omitted or empty, paks load on all playlists.

### Enable/disable and runtime commands
- Default: New mods are enabled automatically.
- Per‑mod state file: `mods/mods.vdf` stores enabled flags by `id`.
- Console commands:
  - `modsystem_list`: print loaded mods and their states.
  - `modsystem_reload`: reload the mod system (rebuild state, reparse scripts, refresh level mod rPaks).
- Console variables and flags:
  - `modsystem_enable 1|0`: master toggle (changing triggers reload).
  - Launch option `-modsystem_disable`: start with mod system disabled.
  - Launch option `-modsystem_debug`: verbose logging for mod discovery/state.

### Asset loading and precedence
- The engine adds each enabled mod’s directory to the `GAME` search path (appended). Be mindful of filename collisions with other mods or base game content.
- Prefer unique subpaths/file names to avoid clashes.

### Packaging content
- Place files under your mod like you would in the game’s directory (scripts, materials, models, audio, etc.).
- For audio overrides, see `r5sdk/docs/audio_mods.md` for the `audio/` layouts and JSON schema.
- Standard Miles banks can also be added via `scripts/audio/banks.rson` inside your mod (merged at startup).

### Troubleshooting
- Nothing shows up in `modsystem_list`: check that `mods/<YourMod>/mod.vdf` exists and is valid KeyValues.
- Mod skipped as invalid: verify required keys, `id` rules, and non‑empty values.
- Files not loading: confirm paths relative to your mod root and avoid name clashes. Use `-modsystem_debug` to inspect search paths and state.


