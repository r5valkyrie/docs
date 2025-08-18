## Console Reference

Quick reference of useful commands, convars, and launch options.

### Commands

| Command | Description |
| --- | --- |
| `modsystem_list` | Lists loaded mods with author, name, id, version, path, and state. |
| `modsystem_reload` | Reloads the mod system (rebuilds state, reparses scripts, refreshes level mod rPaks). |

#### Scripting (Squirrel VMs)

| Command | Description |
| --- | --- |
| `script` | Run input as SERVER script on the VM. |
| `script_client` | Run input as CLIENT script on the VM. |
| `script_ui` | Run input as UI script on the VM. |

#### Networking keys

| Command | Description |
| --- | --- |
| `net_getkey` | Show the installed base64 net key. |
| `net_setkey` | Set user-specified base64 net key. |
| `net_generatekey` | Generate and set a random base64 net key. |

#### Client and connection

| Command | Description |
| --- | --- |
| `cl_setname` | Set the client's persona name. |
| `reconnect` | Reconnect to the current server. |

#### File system and VPK

| Command | Description |
| --- | --- |
| `fs_vpk_mount` | Mount a VPK file for FileSystem usage. |
| `fs_vpk_unmount` | Unmount a VPK file and clear its cache. |
| `fs_vpk_pack` | Pack a VPK from the current workspace. |
| `fs_vpk_unpack` | Unpack all files from a VPK file. |

#### RPAK (RTech) utilities

| Command | Description |
| --- | --- |
| `pak_listpaks` | List loaded RPAK files. |
| `pak_listtypes` | List registered asset types. |
| `pak_emulateremount` | Emulate BSP remount to refresh content (dev). |
| `pak_stringtoguid` | Compute GUID from input text (dev). |
| `pak_compress` | Compress an RPAK file (dev). |
| `pak_decompress` | Decompress an RPAK file (dev). |
| `pak_requestload` | Request async load for an RPAK (dev). |
| `pak_requestunload` | Request async unload for an RPAK (dev). |
| `pak_requestswap` | Request swap for an RPAK (dev). |

#### Server administration

| Command | Description |
| --- | --- |
| `kick` | Kick a client by user name. |
| `kickid` | Kick by handle, nucleus id, or IP address. |
| `ban` | Ban by user name. |
| `banid` | Ban by handle, nucleus id, or IP address. |
| `unban` | Unban by nucleus id or IP address. |
| `banlist_reload` | Reload the banned list. |

#### Playlists

| Command | Description |
| --- | --- |
| `playlist_reload` | Reload the playlists file. |

#### UI and console

| Command | Description |
| --- | --- |
| `toggleconsole` | Show/hide the developer console. |
| `con_history` | Show console submission history. |
| `con_removeline` | Remove a range of lines from the console. |
| `con_clearlines` | Clear all lines from the console. |
| `con_clearhistory` | Clear the console submission history. |
| `togglebrowser` | Show/hide the server browser. |
| `con_help` | Show colors and description of each console context. |

#### Bots

| Command | Description |
| --- | --- |
| `sv_addbot` | Create a bot on the server. |

#### Debug drawing and overlays (dev/cheat)

| Command | Description |
| --- | --- |
| `bhit` | Bullet-hit trajectory debug. |
| `line` | Draw a debug line. |
| `triangle` | Draw a debug triangle. |
| `sphere` | Draw a debug sphere. |
| `capsule` | Draw a debug capsule. |
| `stream_dumpinfo` | Dump texture streaming debug info. |

### Mod system convars

| ConVar | Default | Description |
| --- | --- | --- |
| `modsystem_enable` | `1` | Master toggle for the mod system (changing triggers a reload). |

### Audio mods (Miles) convars

| ConVar | Default | Description |
| --- | --- | --- |
| `enable_audio_mods` | `1` | Enable custom audio override system. |
| `audio_override_debug` | `0` | Log override resolution (exact/regex matches). |
| `wav_debug` | `0` | Verbose logs for custom WAV playback and cleanup. |
| `miles_wav_auto_stop_on_level_change` | `1` | Stop custom audio automatically on level change. |
| `miles_debug` | `0` | General Miles debug logs. |
| `miles_warnings` | `0` | Print Miles warnings. |
| `miles_pcm_log` | `0` | Log raw sample injection parameters. |

Volume/attenuation tuning (global defaults; overridden by JSON unless `wav_force_convars 1`):

| ConVar | Default | Description |
| --- | --- | --- |
| `wav_volume_base` | `1.0` | Base volume multiplier. |
| `wav_volume_min` | `0.0` | Minimum volume floor. |
| `wav_distance_start` | `100.0` | Distance at which attenuation begins. |
| `wav_falloff_power` | `1.0` | Falloff curve exponent (higher = stronger attenuation). |
| `wav_volume_update_rate` | `0.1` | Seconds between runtime volume updates. |
| `wav_allow_silence` | `1` | Allow becoming fully silent at distance. |
| `wav_silence_cutoff` | `0.001` | Threshold below which volume becomes 0. |
| `wav_force_convars` | `0` | When `1`, ignore per-override JSON audio settings. |

### Launch options

| Option | Description |
| --- | --- |
| `-modsystem_disable` | Start with the mod system disabled. |
| `-modsystem_debug` | Enable verbose logging for mod discovery and state. |

### Examples

```text
// Show installed mods
modsystem_list

// Toggle audio mods
enable_audio_mods 0
enable_audio_mods 1

// Tweak volume and falloff
wav_volume_base 0.9
wav_distance_start 120
wav_falloff_power 1.25

// Debug override matching and playback
audio_override_debug 1
wav_debug 1
```


