# notes/

Written by hand, for humans and for future sessions.

This directory sits **outside** the gave-broker sync allowlist
(`memories sessions plans workspace`), so files here survive. Anything written
into `memories/` from this side is deleted and rebuilt from
`/var/lib/hermes-jolierousse/memories` on the next hourly broker run — the sync
is one-way, agent to repo.

So: nothing in this repo reaches GaveJolieRousse's own memory. To teach the
agent something, the file has to land in its data directory on jolieweb.
