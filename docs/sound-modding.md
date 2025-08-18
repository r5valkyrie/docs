## Custom Audio Override System

A lightweight mod-side system to replace Miles audio events with user-provided WAV samples, with optional per-event JSON configuration.

### Folder layout (per mod)

Put your audio assets under the mod's `audio/` folder. Supported layouts:

- Single file per event
  - `audio/<EventName>.wav`

- Folder per event (all `.wav` inside are candidates)
  - `audio/<EventName>/*.wav`

- JSON manifest + sibling samples folder (Northstar-style)
  - Manifest: `audio/<Name>.json`
  - Samples: `audio/<Name>/*.wav` (folder base name must match the JSON file name)

Notes:
- The event name is the basename of the file/folder (e.g., `audio/weapon_pistol_fire.wav` → event `weapon_pistol_fire`).
- WAVs must be PCM (8 or 16-bit). Files are loaded into memory.

### JSON manifest schema

Location: `audio/<Name>.json`

Fields:
- `EventId`: string | string[] — exact event id(s) to override
- `EventIdRegex`: string | string[] — regex(es) matching event names
- `AudioSelectionStrategy`: "sequential" | "random" — choose next sample

Optional per-override audio settings (used only if present and `wav_force_convars == 0`):
- `VolumeBase`: number (default 1.0)
- `VolumeMin`: number (default 0.0)
- `DistanceStart`: number (default 100.0)
- `FalloffPower`: number (default 1.0)
- `VolumeUpdateRate`: number (default 0.1)
- `AllowSilence`: bool (default true)
- `SilenceCutoff`: number (default 0.001)

If `EventId`/`EventIdRegex` are omitted, the system falls back to the JSON filename stem for the event id.

Example:

```json
{
  "EventId": "weapon_pistol_fire",
  "AudioSelectionStrategy": "random",
  "VolumeBase": 0.9,
  "VolumeMin": 0.05,
  "DistanceStart": 150.0,
  "FalloffPower": 1.3,
  "VolumeUpdateRate": 0.05,
  "AllowSilence": true,
  "SilenceCutoff": 0.0005
}
```

### Load timing and hot reload

- Overrides are loaded once during Miles initialization (`CSOM_Initialize`).
- If you hot-reload mods at runtime, also call `GetAudioOverrideManager()->LoadFromMods()` in your mod reload path to refresh overrides.

### Selection and priority

- If multiple sources target the same event (e.g., both JSON and direct file), the last registered wins (mods are scanned in mod list order).
- When multiple samples exist for an event:
  - `sequential`: cycles through samples in order
  - `random`: randomly picks a sample

### ConVars

Global behavior can be tuned via convars. JSON can override these per-event if `wav_force_convars == 0`.

- `wav_force_convars` (default 0): when 1, ignore JSON per-override settings and use only convars below
- `wav_volume_base` (default 1.0)
- `wav_volume_min` (default 0.0)
- `wav_distance_start` (default 100.0)
- `wav_falloff_power` (default 1.0)
- `wav_volume_update_rate` (default 0.1)
- `wav_allow_silence` (default 1)
- `wav_silence_cutoff` (default 0.001)
- `wav_debug` (default 0): extra logs for override playback
- `miles_debug` (default 0): general Miles logs

When `wav_volume_update_rate` is set in JSON and `wav_force_convars == 0`, the system updates `wav_volume_update_rate` to that value.

### Minimal examples

Single file:
```
audio/weapon_pistol_fire.wav
```

Folder-based:
```
audio/weapon_pistol_fire/
  shot1.wav
  shot2.wav
  shot3.wav
```

JSON + folder-based:
```
audio/footsteps.json
audio/footsteps/
  hard/step1.wav
  hard/step2.wav
  soft/step1.wav
```
```json
{ "EventId": "footsteps", "AudioSelectionStrategy": "sequential" }
```

### Troubleshooting

- Not replacing:
  - Enable logs: `miles_debug 1`, `wav_debug 1`
  - Check for "Loaded X audio overrides" at startup
  - Verify the event name matches exactly
  - Ensure JSON has a sibling samples folder with the same basename
  - Validate your WAVs are PCM 8/16-bit

- JSON settings ignored:
  - Ensure `wav_force_convars` is `0`
  - Confirm JSON value types (numbers vs strings)

- Performance:
  - Large/long WAVs are held in memory; prefer short, optimized samples.


