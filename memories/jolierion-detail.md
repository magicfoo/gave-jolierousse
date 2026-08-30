# Jolierion — the boat's computer, in detail

Verified 2026-08-30 by direct inspection.

## Hardware and OS
**Raspberry Pi 4 Model B Rev 1.5**, running **Debian GNU/Linux 13 (trixie)**.
Hostname `jolierion`. Single user account with a shell: `ggainant`.

Storage includes a microSD carrying the offline chart archive.

## Its mission
The interface between Gerald and the boat, and a resource for the crew.
Collects realtime data from the **NMEA 2000 bus**, stores it locally, and
publishes a curated view. It is the **only** machine with bus access.

## Services
| Service | Port | Role |
|---|---|---|
| `signalk.service` (v2.31.1) | 3000 | reads NMEA 2000; source of all vessel data |
| `watchkeeper.service` | 3100 | the vessel dashboard (Next.js) |
| `nginx` | 8080 | single front door — `/` dashboard, `/signalk/` API, `/charts/` tiles |
| `boat-facts.service` | 127.0.0.1:8787 | read-only snapshot broker |
| NMEA-over-TCP | 10110 | instrument feed |

**boat-facts** is purpose-built so an agent can read vessel state without
touching SignalK or the bus. It exposes one fixed endpoint, accepts no URL,
path, command or write operation, and **refuses to serve a payload unless it
declares `readOnly: true` and `controlAvailable: false`.**

## Network posture (hardened 2026-08-30)
`ufw` policy, default deny incoming:
- **Tailnet (`tailscale0`)** — full access. Tailnet IP `100.70.123.72`.
- **Local LAN** (RFC1918 + IPv6 ULA/link-local) — SSH (22) and the dashboard
  (8080) only. Crew on the boat wifi use the dashboard.
- **Public internet** — nothing.

This followed the discovery that the Pi had been reachable from the open
internet over IPv6, with SSH, SignalK and the dashboard all bound to `0.0.0.0`
and no firewall installed. The bindings are unchanged; the firewall is what
closes it, so the protection travels with the machine to any network.

SSH is key-only: `passwordauthentication no`, `permitrootlogin no`.

## What is NOT on jolierion any more
A Hermes agent ran aboard from 2026-08-16 until 2026-08-27, then was removed —
its costly traffic is LLM API calls, which belong on a datacentre link rather
than the boat's connection. That is why this agent runs on Jolieweb instead.

The design is archived at `magicfoo/jolierion` → `boat-hermes-archive/`.

## Current status
As of 2026-08-30 the Pi is **on Gerald's desk, not aboard, and not connected to
the datahub**. SignalK therefore holds no vessel data (`vessels: {}`) and
boat-facts correctly returns `503 signalk_unavailable`.

**Expected, not a fault.** Until it is installed aboard and connected there is
no live vessel data. Say so plainly rather than inferring anything about the
boat's state.
