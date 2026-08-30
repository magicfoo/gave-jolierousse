# Baja Ha-Ha — the passage south

Written 2026-08-30. **Read the status section first: much of this is general
knowledge about the rally, not confirmed project record.** Gerald has not yet
recorded his specific entry, dates or crew.

## What the Baja Ha-Ha is
An annual cruising rally from **San Diego to Cabo San Lucas**, roughly **750
nautical miles**, organised by *Latitude 38*. It departs in **late October /
early November**, timed after the Pacific hurricane season eases and before
winter northerlies set in.

The classic format runs in three legs with two stops:
1. **San Diego → Turtle Bay (Bahía Tortugas)** — the long leg, ~360 nm
2. **Turtle Bay → Bahía Santa María** — ~240 nm
3. **Bahía Santa María → Cabo San Lucas** — ~180 nm

It is a rally, not a race: boats travel in loose company, with roll-call radio
nets and beach gatherings at the stops. Its real value is that a first offshore
passage happens **in company** rather than alone.

Typical conditions are downwind in the northwesterly Pacific swell. The stops
are remote — Turtle Bay in particular has minimal services — so self-sufficiency
in fuel, water and spares matters.

**Verify all of this against the official Latitude 38 entry material.** Dates,
legs and requirements change year to year, and this is general knowledge rather
than a checked source.

## What IS confirmed — the charts aboard
The offline chart archive on jolierion's microSD covers this route. Chart root:
`/var/lib/jolierousse/charts`.

| Source | Coverage | Status |
|---|---|---|
| NOAA CDS `ncds_19a/19b/19c` | U.S. / California portion | **official** raster → PMTiles, lossless |
| Protomaps build `20260816` | clipped `-125,22,-107,39.5`, to zoom 14 | land/coast context |
| GEBCO 2026 15-arc-second | Baja and Sea of Cortez bathymetry | **context only** |

That Protomaps bounding box spans roughly 39.5°N down to 22°N and east to
-107° — Northern California to the southern tip of Baja and into the Sea of
Cortez. It is cut for exactly this trip.

### Safety caveat — state this whenever charts come up
**GEBCO is not official Mexican charted sounding data and is NOT suitable for
shallow-water navigation.** Never describe it as a nautical chart. For the
Mexican portion of the route there is currently **no official hydrographic
product aboard** — only bathymetric context.

Official Mexican hydrographic data should be imported only when the owner
supplies a compatible product whose licence permits the use. Do not decode,
convert, redistribute or bypass controls on proprietary C-MAP downloads.

A deliberate chart-refresh procedure — source dates, free-space checks,
recoverable replacement, rollback — should exist before any refresh.

## Status and open questions for Gerald
Nothing about the trip itself is recorded yet. Worth asking, and recording as
answers arrive:
- Which year's Ha-Ha, and is entry submitted?
- Departure date from Sausalito, and the delivery leg south to San Diego —
  that is itself ~450 nm and often the harder passage
- Crew: who is aboard, and for which legs
- Will jolierion be installed and connected to the datahub before departure?
  It is currently on a desk, so voyage logging would not run
- Offshore preparation: liferaft, EPIRB, jacklines, rig check, insurance cover
  for Mexican waters, TIP (Temporary Import Permit) and clearance paperwork

## Why this matters to voyage logging
The plan is for jolierion to record NMEA data to a local SQLite store and push
hourly to Jolieweb so visitors can follow along. A Ha-Ha passage is exactly the
material that system exists to capture — and it only works if the Pi is aboard,
connected, and the sync is running before departure.
