---
description: Remote server management with RCON
---

# RCON

RCON lets you administer an R5Reloaded server via a remote console connection.

> Important: Keep your server secure. Do not share your RCON password or encryption key. Avoid opening RCON to the public internet unless necessary. If you do, never expose game ports over TCP beyond what you need.

## Prerequisites

- Set an RCON password or RCON is disabled.
  - Edit the appropriate config in `platform/cfg/tools/` on the server:
    - `rcon_server` (default)
    - `rcon_server_dev` (used when launching with `-devsdk` or "Enable development" in the launcher)
- If accessing from outside your network, open the server's TCP port. For multiple servers, consider opening a range like `37015–37020`.
- Whitelist your admin IP with `sv_rcon_whitelistaddress` to prevent lockouts.
- Optional: set a fixed encryption key with `rcon_key` in the server RCON config. This is a Base64‑encoded AES‑128 key. If unsure, let the server generate a key and reuse it.

You’ll know RCON is active when the server console prints: “Remote server access initialized”.

## Connecting

Two common ways to connect: Netcon (tool) and the game client.

### 1) Using Netcon

Netcon is in the `bin/` folder of your R5Reloaded install.

Connect using: `[IP]:Port Key`

Example:
```text
[::ffff:127.0.0.1]:37015 WDNWLmJYQ2ZlM0VoTid3Yg==
```

Notes:
- IP must be IPv6 or IPv4 in IPv6 notation (as above).
- The “Key” is the RCON encryption key from the server’s `rcon_server` config (not the game connection key). If not set manually, copy it from the server console.

Authenticate after connecting:
```text
PASS your_password_here
```

You can now execute server console commands.

### 2) Using the game client

Set these ConVars via the in‑game console or the client configs `platform/cfg/tools/rcon_client` and `rcon_client_dev`:

| Name              | Note                                                            | Example                    |
| ----------------- | --------------------------------------------------------------- | -------------------------- |
| `cl_rcon_address` | Server IP and port to access                                   | `[::ffff:127.0.0.1]:37015` |
| `rcon_key`        | Encryption key from the server’s RCON config                    | `WDNWLmJYQ2ZlM0VoTid3Yg==` |

Then authenticate when issuing a command by prefixing with `rcon`:
```text
rcon PASS your_password_here
```

If successful, you’ll see:
```text
Netcon(X):Authentication successful.
```

Prefix all remote commands with `rcon`. For example:
```text
rcon launchplaylist survival_dev
```


