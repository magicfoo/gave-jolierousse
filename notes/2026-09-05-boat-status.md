# Boat status: Jolierion is aboard, DataHub still not connected

2026-09-05

## What changed

Jolierion — the boat's computer — was moved from Gerald's desk to Jolie Rousse
on the evening of **2026-09-03**. It is running aboard and reachable over the
tailnet. Before that date it had never left the house.

## Correction to `memories/jolierion-detail.md`

That file states the Pi "collects realtime data from the **NMEA 2000 bus**" and
lists `signalk.service` as "reads NMEA 2000; source of all vessel data".

That is the intent, not the current state. **Signal K has never received live
vessel data.** Its configured provider has failed to connect 11,402 consecutive
times. The vessel data path is:

    PredictWind DataHub    10.10.10.1:11102
    Signal K provider      predictwind-datahub    (NMEA 0183 over TCP)

Two things follow. First, this is a **network** feed, not a CAN bus — no CAN
adapter or HAT is required, contrary to advice given before the move. Second,
the 11,402 failures are expected behaviour rather than a fault: the Pi was
retrying `10.10.10.1:11102` from a desk where that network does not exist.

The link has still not been made. Until it is, any vessel telemetry the agent
might be asked for does not exist, and `boat-facts` has nothing real to serve.

## What connecting it requires

The Pi has both interfaces free, so it can be dual-homed:

    eth0   → Peplink      internet, tailnet, remote access
    wlan0  → DataHub      10.10.10.x, instrument data only

The DataHub has no internet. If wlan0 takes the default route, Tailscale exits
through it and the Pi leaves the tailnet — so the wlan0 profile must carry
`ipv4.never-default yes`.

Validation is observe-only: confirm the provider stays input-only, generate no
traffic toward the DataHub beyond the TCP read Signal K already performs, and
make no network changes while the vessel is under way.

## Why this has not been done

Agent SSH to both jolierion and jolieweb was blocked by a managed corporate
policy on 2026-09-03 at 21:47. The work is planned and specified but cannot be
executed from Gerald's laptop. Full detail in `magicfoo/jolieweb`
`notes/2026-09-05-ssh-blocked-by-policy.md`; the pre-move audit that produced
the plan is in `magicfoo/jolierion`
`notes/2026-09-05-move-to-boat-and-pre-move-audit.md`.

## Unverified

Nothing about the Pi's post-move state has been confirmed, for the same reason.
In particular: whether eth0 actually negotiated a link (it never had one before
the move), whether the Peplink wifi profile was added, and whether the chronic
undervoltage — 13,582 brownouts in 18 days, `get_throttled 0x50000` — got worse
on boat power.

## Deadline

Baja Ha-Ha XXXII starts **2026-11-02** off Coronado Roads, per
`memories/baja-haha-passage.md`. The vessel data path needs to be working and
proven well before then.
