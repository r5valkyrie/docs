## Weapons Quickstart

This short guide helps you add or prototype a weapon/ability using `vscripts/weapons/` with the correct Server/Client split and registration in `vscripts/scripts.rson`.

### 1) Where weapon scripts go

- Place your files under: `platform/scripts/vscripts/weapons/`
- Typical naming:
  - Primary weapons: `mp_weapon_*.nut`
  - Abilities/tactical/ultimate: `mp_ability_*.nut`
  - Shared helpers: `_weapon_utility.nut`, `sh_*.gnut`

Good references in this repo:
- `vscripts/weapons/mp_weapon_zipline.nut`
- `vscripts/weapons/mp_ability_grapple.nut`
- `vscripts/weapons/_weapon_utility.nut`

### 2) Register your script in scripts.rson

Add your new file under a block that loads for both Server and Client (most weapons do), alongside other weapons.

```text
When: "SERVER || CLIENT"
Scripts:
[
  ...
  weapons/mp_weapon_myprototype.nut
]
```

If your script is helper-only, you can register it in a shared utilities block too.

### 3) Minimal weapon skeleton (SERVER/CLIENT split)

Create `vscripts/weapons/mp_weapon_myprototype.nut`:

```nut
untyped

// Initialization
global function MpWeapon_MyPrototype_Init

// Firing
global function OnWeaponActivate_MyPrototype
global function OnWeaponDeactivate_MyPrototype
global function OnPrimaryAttack_MyPrototype

void function MpWeapon_MyPrototype_Init()
{
  #if SERVER
  printl("[MyProto] Server init")
  #endif

  #if CLIENT
  printl("[MyProto] Client init")
  #endif
}

void function OnWeaponActivate_MyPrototype( entity weapon )
{
  #if CLIENT
  // client HUD or FX setup
  #endif
}

void function OnWeaponDeactivate_MyPrototype( entity weapon )
{
}

void function OnPrimaryAttack_MyPrototype( entity weapon, WeaponPrimaryAttackParams params )
{
  #if CLIENT
  if ( params.firstTimePredicted )
  {
    // local muzzle FX / sound gated by prediction
  }
  #endif

  #if SERVER
  // authoritative fire logic (traces, damage, projectile spawn)
  // Example: Fire bullets
  // WeaponFireBulletSpecialParams bullet
  // bullet.pos = params.pos
  // bullet.dir = params.dir
  // bullet.bulletCount = 1
  // FireWeaponBulletSpecial( weapon, bullet )
  #endif
}
```

Notes:
- Keep entity creation, damage application, and inventory changes on the SERVER side.
- Gate client-only visuals with `params.firstTimePredicted` where applicable.

### 4) Useful helpers you can reuse

- `weapons/_weapon_utility.nut`: common projectile/bullet helpers
- `sh_utility_all.gnut`: general helpers
- `sh_networkvars.gnut`: define/read networked variables shared by server & client

### 5) Common hooks you may implement

- Activate/Deactivate: setup/cleanup state when the weapon is selected/holstered
- Primary attack: firing logic (bullets, projectiles, traces) and client FX
- Secondary/offhand: abilities or alt-fire (similar pattern to primary attack)

Example signatures seen in repo weapons:
- `void function OnPrimaryAttack( WeaponPrimaryAttackParams params )`
- `void function OnWeaponTossPrep( WeaponTossPrepParams params )`
- `void function OnWeaponOwnerChanged( WeaponOwnerChangedParams params )`

### 6) Troubleshooting

- Nothing happens? Confirm the file is referenced in `vscripts/scripts.rson` under a `When: "SERVER || CLIENT"` block that loads for your map/mode.
- Logs: add `printl("[MyProto] ...")` in activate/attack to verify execution on SERVER/CLIENT.
- Prediction double-FX: ensure visuals are gated by `params.firstTimePredicted` on the client.

### 7) Next steps

- Add projectile types: check `WeaponFireGrenadeParams` / `WeaponFireBoltParams` / `WeaponFireMissileParams` in shared headers for the right pattern.
- Copy from a similar weapon: e.g., `mp_weapon_zipline.nut` (projectile behavior) or `mp_ability_grapple.nut` (ability logic).