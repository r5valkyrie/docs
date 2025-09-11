# Mod Playlist System

The R5SDK mod system supports custom playlists, allowing mods to define their own game modes, rules, and map rotations. This system provides a safe and validated way to extend the game with custom content.

## Overview

The mod playlist system allows you to:
- ✅ Create custom playlists with unique settings
- ✅ Define custom gamemodes (advanced)
- ✅ Override mod-specific playlists when updated
- ✅ Automatically clean up orphaned playlists when mods are disabled
- ❌ **Cannot override base game playlists** (protected for stability)

## Quick Start

### 1. Create Your Playlist File

Create a file named `playlists_r5_patch.txt` in your mod's root directory:

```
YourMod/
├── mod.json
├── playlists_r5_patch.txt  ← Your playlist file
└── scripts/
    └── ...
```

### 2. Basic Playlist Structure

```keyvalues
playlists
{
    Playlists
    {
        your_custom_mode
        {
            inherit defaults
            vars
            {
                for_mod         "YourModName"  // REQUIRED: Must match your mod name exactly
                visible         1
                name           "Your Custom Mode"
                description    "A custom game mode"
                
                // Your custom settings here...
            }
            gamemodes { survival { maps {
                mp_rr_desertlands_64k_x_64k    1
            } } }
        }
    }
}
```

### 3. Apply Changes

Use the console command to reload playlists:
```
playlist_reload
```

## Required Fields

### `for_mod` Variable
**This is mandatory for all mod playlists and gamemodes.**

```keyvalues
vars
{
    for_mod    "YourModName"  // Must exactly match your mod's name
}
```

**What happens if missing:**
- ⚠️ Warning logged: `"Playlist 'name' missing required 'for_mod' variable - skipping"`
- ❌ Playlist will be rejected and not added

**What happens if incorrect:**
- ⚠️ Warning logged: `"Playlist 'name' has for_mod='Wrong' but should be 'YourMod' - skipping"`
- ❌ Playlist will be rejected

## File Naming

Place your playlist file in your mod's **root directory** with one of these names:

| Filename | Status |
|----------|--------|
| `playlists_r5_patch.txt` | ✅ **Recommended** |
| `playlist_r5_patch.txt` | ✅ Alternative |

> **Note:** The system checks for both names and uses the first one found.

## Common Playlist Settings

### Team & Player Configuration
```keyvalues
vars
{
    max_team_size        1      // Players per team (1 = FFA, 3 = squads)
    max_players         30      // Total players in match
    min_players          1      // Minimum players to start
}
```

### Skip Intros (Recommended for Testing)
```keyvalues
vars
{
    waiting_for_players_spawning_enabled     1
    waiting_for_players_countdown_seconds    0
    waiting_for_players_timeout_seconds      1
    character_select_time_max                0.0
    character_select_time_min                0.0
    survival_enable_gladiator_intros         0
    jump_from_plane_enabled                  0
}
```

### Ring/Circle Control
```keyvalues
vars
{
    sur_circle_start_paused    1    // 1 = paused/disabled, 0 = normal
}
```

### Loadout System
```keyvalues
vars
{
    dev_loadout_changeable_at_any_time        1
    sur_dev_unrestricted_character_changes    1
}
```

### Loot Settings
```keyvalues
vars
{
    ground_loot_enable           0    // 1 = enable ground loot spawns
    lootbin_loot_enable          0    // 1 = enable loot bin contents
    deathbox_ondeath_enabled     0    // 1 = drop deathbox on death
    drop_loot_on_death           0    // 1 = drop items on death
}
```

## Map Configuration

### Available Maps
```keyvalues
gamemodes { survival { maps {
    // Main Battle Royale Maps
    mp_rr_desertlands_64k_x_64k        1    // King's Canyon
    mp_rr_olympus                      1    // Olympus  
    mp_rr_canyonlands_64k_x_64k        1    // World's Edge
    
    // Map Variants
    mp_rr_desertlands_64k_x_64k_nx     1    // King's Canyon (Night)
    mp_rr_desertlands_64k_x_64k_tt     1    // King's Canyon (Season variant)
    mp_rr_olympus_tt                   1    // Olympus (Season variant)
    mp_rr_canyonlands_mu1              1    // World's Edge (Variant)
    
    // Special Maps
    mp_rr_party_crasher               1    // Party Crasher
    mp_rr_aqueduct                    1    // Arenas map
    mp_rr_arena_phase_runner          1    // Arenas map
    mp_rr_arena_composite             1    // Arenas map
} } }
```

## Advanced: Custom Gamemodes

You can define custom gamemodes for advanced mod functionality:

```keyvalues
playlists
{
    Gamemodes
    {
        my_custom_gamemode
        {
            for_mod        "YourModName"  // REQUIRED
            inherit defaults
            vars
            {
                name       "My Custom Gamemode"
                // Add custom gamemode variables here
            }
        }
    }
}
```

## Multiple Playlists

You can define multiple playlists in a single file:

```keyvalues
playlists
{
    Playlists
    {
        mode_ffa
        {
            inherit defaults
            vars
            {
                for_mod           "YourMod"
                name             "Free For All"
                max_team_size     1
            }
            // ... settings and maps
        }
        
        mode_squads
        {
            inherit defaults
            vars
            {
                for_mod           "YourMod"
                name             "Squad Mode"
                max_team_size     3
            }
            // ... settings and maps
        }
    }
}
```

## System Behavior

### Startup Integration
- Playlists are automatically merged at game startup
- Called from `CHostState::Setup` - happens before playlist system initializes

### Hot Reloading
Use `playlist_reload` command to:
1. Merge current mod playlists into base file
2. Clean up orphaned playlists from disabled mods
3. Reload the updated playlist file

### Orphan Cleanup
The system automatically removes playlists when:
- ❌ Mod is disabled or uninstalled
- ❌ Mod no longer provides that specific playlist (e.g., you renamed it)
- ❌ Mod's playlist file is deleted or corrupted

### Base Game Protection
- 🔒 **Base game playlists cannot be overridden** by mods
- ⚠️ Warning logged if attempted: `"Cannot override base game playlist 'survival' - skipping"`

### Mod Playlist Replacement
- ✅ **Mod playlists can replace other mod playlists** with the same name
- 📝 Log message: `"Replacing existing mod playlist 'custom_mode' with version from mod 'NewMod'"`

## Debugging

### Enable Debug Logging
```
playlist_debug 1
```

### Debug Output Examples
```
[ENGINE] Checking for orphaned playlists in base file...
[ENGINE] Playlist #1: 'survival', for_mod='(null)'
[ENGINE] Playlist #2: 'my_custom', for_mod='MyMod'
[ENGINE] Mod 'MyMod' still provides playlist 'my_custom', keeping it
[ENGINE] Merged playlist patch from mod 'MyMod' into base file
[ENGINE] Successfully updated playlist file with mod patches
```

### Common Error Messages

| Error | Cause | Solution |
|-------|-------|----------|
| `"missing required 'for_mod' variable"` | No `for_mod` in playlist vars | Add `for_mod "YourModName"` |
| `"has for_mod='Wrong' but should be 'YourMod'"` | Incorrect mod name | Fix the `for_mod` value |
| `"Cannot override base game playlist"` | Trying to replace base playlist | Rename your playlist |
| `"no longer provides it"` | Playlist removed/renamed | Expected behavior during cleanup |

## Example Template

Here's a complete example you can copy and modify:

```keyvalues
// Example Mod Playlist File
// Place this file in your mod's root directory as:
// - playlists_r5_patch.txt

playlists
{
    Playlists
    {
        my_custom_mode
        {
            inherit defaults
            vars
            {
                // REQUIRED: Must match your mod's exact name
                for_mod                                     "YourModName"
                
                // Basic playlist settings
                visible                                     1
                name                                        "My Custom Mode"
                description                                 "A custom game mode created by my mod"
                
                // Team and player settings
                max_team_size                               1       // Players per team
                max_players                                 30      // Total players in match
                min_players                                 1       // Minimum to start
                
                // Skip lengthy intros for faster testing
                waiting_for_players_spawning_enabled        1
                waiting_for_players_countdown_seconds       0
                waiting_for_players_timeout_seconds         1
                character_select_time_max                   0.0
                character_select_time_min                   0.0
                survival_enable_gladiator_intros            0
                jump_from_plane_enabled                     0
                
                // Ring/Circle settings
                sur_circle_start_paused                     1       // 1 = paused, 0 = normal
                
                // Loadout and character settings
                dev_loadout_changeable_at_any_time          1
                sur_dev_unrestricted_character_changes      1
                
                // Bot settings
                sur_bots_spawn_with_random_weapons           1
                
                // Match flow
                match_ending_enabled                         0       // 0 = endless, 1 = normal ending
                
                // Communication
                enable_global_chat                           1
                
                // Gameplay mechanics
                enable_bullet_slow                           1
                activate_passive_abilities                   1
                dodge_dash_enabled                           0
                
                // Loot settings
                custom_loot                                  0
                ground_loot_enable                           0
                lootbin_loot_enable                          0
                deathbox_ondeath_enabled                     0
                drop_loot_on_death                           0
                
                // Misc features
                enable_motd                                  0
                skydive_ziplines_enabled                     0
            }
            
            // Map rotation for this playlist
            gamemodes 
            { 
                survival 
                { 
                    maps 
                    {
                        // Battle Royale maps
                        mp_rr_desertlands_64k_x_64k             1   // King's Canyon
                        mp_rr_olympus                           1   // Olympus
                        mp_rr_canyonlands_64k_x_64k             1   // World's Edge
                    } 
                } 
            }
        }
    }
}
```

## Console Commands

| Command | Description |
|---------|-------------|
| `playlist_reload` | Merge mod playlists and reload the playlist system |
| `playlist_debug 1` | Enable verbose debug logging |
| `playlist_debug 0` | Disable debug logging (default) |

## Best Practices

### Development
1. **Use `playlist_debug 1`** during development for detailed logging
2. **Use `playlist_reload`** for quick testing of playlist changes
3. **Skip intros** in development playlists for faster iteration
4. **Start with minimal settings** and add complexity gradually

### Distribution
1. **Always include `for_mod`** with your exact mod name
2. **Use descriptive playlist names** that won't conflict with other mods
3. **Test with multiple maps** to ensure compatibility
4. **Document your custom settings** for users

### Troubleshooting
1. **Check console logs** for validation errors
2. **Verify mod name** matches exactly between `mod.json` and `for_mod`
3. **Test playlist loading** with `playlist_reload`
4. **Use debug mode** to trace playlist processing

## Technical Notes

- Playlists are merged into `platform/playlists_r5_patch.txt` on disk
- The system preserves base game playlists and only manages mod content
- File validation happens at both parse time and runtime
- Orphan cleanup ensures no leftover content from disabled mods
- Multiple mods can coexist with different playlist names

## Limitations

- Cannot override base game playlists (by design for stability)
- Playlist names must be unique across all enabled mods
- Custom gamemode functionality may require additional script support
- File must be valid KeyValues format (syntax errors will cause rejection)
