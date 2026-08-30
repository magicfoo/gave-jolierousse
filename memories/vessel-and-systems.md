# Jolie Rousse — vessel and onboard systems

Written 2026-08-30 from the project record. Facts about the boat and the
equipment that produces the data this agent receives.

## The vessel
**Jolie Rousse** — a **Lagoon 42** catamaran, currently docked in **Sausalito,
California**. MMSI 338421949.

Owner and skipper: **Gerald**.

## Onboard computer — "jolierion"
A **Raspberry Pi 4 Model B** running **Debian 13 (trixie)**, named `jolierion`.
It is the interface between Gerald and the boat, and it serves the crew.

Its role: collect realtime data from the **NMEA 2000 bus**, store it locally,
and publish a curated view. It is the only machine with bus access.

### Software aboard
- **SignalK** (`signalk.service`, v2.31.1) — reads the NMEA 2000 bus and is the
  source of all vessel data. Runs on port 3000.
- **Watchkeeper** (`watchkeeper.service`) — the vessel dashboard, a Next.js app
  on port 3100.
- **nginx** on port 8080 — the single front door. `/` proxies the dashboard,
  `/signalk/` proxies SignalK, `/charts/` serves chart tiles. Crew use this.
- **boat-facts** (`boat-facts.service`) — a read-only snapshot broker on
  `127.0.0.1:8787`, purpose-built so an agent can read vessel state without
  touching SignalK or the bus directly. It refuses to serve a payload unless it
  declares `readOnly: true` and `controlAvailable: false`.

## How this agent gets boat data
**By push, never by pull.** GaveJolieRousse runs on Jolieweb, a public VPS, and
has no network path to jolierion. The boat pushes a read-only snapshot up; the
agent reads a local copy.

This is deliberate and must not be changed. Jolieweb runs agents exposed to
untrusted Telegram input; a path from there into the boat would put that
exposure next to the NMEA bus. See the vessel boundaries in AGENTS.md.

## Network posture of jolierion (as of 2026-08-30)
Access is restricted by `ufw`:
- **Tailnet (tailscale0)** — full access
- **Local LAN** (RFC1918, IPv6 ULA/link-local) — SSH and the dashboard on 8080
- **Public internet** — nothing

This was tightened after the Pi was found reachable from the open internet over
IPv6 while services were bound to `0.0.0.0` with no firewall.

## Current status — important context
As of 2026-08-30 the Pi is **on Gerald's desk, not aboard, and not connected to
the datahub**. With no NMEA bus attached, SignalK holds no vessel data
(`vessels: {}`), and boat-facts correctly returns `503 signalk_unavailable`.

This is expected, not a fault. Until the Pi is installed aboard and connected,
there is no live vessel data to report. Say so plainly rather than inferring
anything about the boat's actual state.

## Planned
Hourly synchronisation of voyage data from jolierion to jolieweb, so external
visitors can follow the adventures — and a more frequent small-snapshot push so
this agent has near-live vessel state.
