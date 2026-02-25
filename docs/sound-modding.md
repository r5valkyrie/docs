## R5VALK FMOD Modding Guide

This explains how to author and package FMOD Studio content for R5Valk using the built‑in FMOD backend and mod system.

### What the FMOD backend does

- Auto‑loads FMOD Studio banks from the base game and enabled mods
- If an FMOD event exists with the exact name the game requests, FMOD plays it instead of Miles
- 3D spatialization, sound occlusion and attenuation are handled ONLY if the FMOD Spatializer effect is added to the audio event(s)!

### Requirements

- Use the shared FMOD Studio project provided with the game/mod SDK (located in the bin folder)
  - Build your mod bank within this project (same FMOD Studio version)

### Critical rules (read first)

- Do NOT place new events inside folders in FMOD. Keep events at the Events root.
  - R5 resolves by exact path/name. A folder changes the path (e.g., `event:/MyFolder/MyEvent_1p`), which will not match the game’s request (e.g., `event:/MyEvent_1p`).
- Do NOT rebuild or ship `Master.bank`.

### Authoring steps

1) Open the shared FMOD Studio project
2) Create a new bank for your mod (e.g., `MyMod`)
3) Right click on your new bank and mark it as a 'Master Bank'
4) Create events at the Events root (no folders) and assign them to your mod bank
5) Name events either to match existing game requests (to override) or use new names for new sounds
   - For overrides (match existing names), common suffixes the game uses:
     - First‑person: `*_1p`
     - Third‑person: `*_3p` or `*_3p_enemy`
   - Override example: `weapons_rifle_fire_1p`
6) Build your bank
   - FMOD outputs two files for your bank:
     - `MyMod.bank`
     - `MyMod.strings.bank`

### Packaging (mods)

Place the two output files in your mod:

```
<YourModRoot>/audio/fmod/
  MyMod.bank
  MyMod.strings.bank
```

Notes:
- Do not include `Master.bank`
- Do not include `Master.strings.bank`

### How overrides work

- When the game asks to play a sound by name, the backend checks FMOD first
- If an FMOD event with that exact name exists, the FMOD version plays and Miles is bypassed

### Verifying in‑game

Console commands:

- List loaded banks
  - `fmod_list_banks`

- Check an event exists in loaded FMOD banks
  - `fmod_event_exists weapons_rifle_fire_1p`

Debug output:

- Enable FMOD/Miles logs as needed
  - `fmod_debug 1`
  - `miles_debug 1`

### Best practices

- Keep base game banks unchanged; put all your content in your own bank(s)
- Avoid renaming or moving existing base events unless you are intentionally overriding with the exact same name
- Prefer short one‑shot events; current stop‑by‑event support in the backend is limited
- Keep event names unique across mods to reduce ambiguity

### Troubleshooting

Event not found:
- Event name does not match the game’s requested name
- Event placed inside a folder (path mismatch)
- Event was not assigned to your bank
- `MyMod.strings.bank` missing from your mod package

Nothing plays:
- Bank files are not in `<YourModRoot>/audio/fmod/`
- The FMOD event has no audio routing/content configured

### Notes on 3D audio and listener

- Listener position and 3D attributes are synchronized with the engine

### Limitations (current backend)

- No exposed API for runtime parameter control or querying instances
- Raw PCM streaming is not supported by the FMOD backend
- Stop‑by‑event is rudimentary. The audio modding system now properly handles STOP events called by Miles, however custom implementations are lacking and often require squirrel_re scripting.
- It is not possible to work with Miles Sound System directly
- FMOD spatialization and attenuation are very primitive compared to Wwise's
- Complex, compound event systems are much more difficult to implement, if at all possible, compared to Wwise
- FMOD does not support adding pre- or post- delays to audio events
- It is not possible to categorize audio events using folders. Categorization can only be done by loading multiple banks
- Each mod requires its own bank, leading to a high amount of banks necessary to be loaded into memory if using multiple audio mods or mods that feature audio changes

### Quick checklist

- [ ] Use the shared FMOD Studio project
- [ ] Create/assign events at the Events root (no folders)
- [ ] Match names exactly (e.g., `*_1p`, `*_3p`, `*_3p_enemy`)
- [ ] Build your bank → get `MyMod.bank` and `MyMod.strings.bank`
- [ ] Put both files in `<YourModRoot>/audio/fmod/`
- [ ] Verify with `fmod_list_banks` and `fmod_event_exists`