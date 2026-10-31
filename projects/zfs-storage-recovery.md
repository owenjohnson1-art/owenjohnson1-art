# ZFS storage recovery and rebuild

**Dates:** 2026-10-22 to 2026-10-23

## Problem

While mapping my homelab, I found the main ZFS pool was a **striped** pool (no redundancy) that included a USB drive, and it was degraded. One more disk problem would have meant losing every container. While investigating, the pool hung and took the network services down with it.

## What I did

1. **Stabilised first.** Recovered from the ZFS hang and the network outage, then took stop-mode backups of every container.
2. **Copied data off before touching the pool.** I didn't try snapshots or repairs on a damaged pool. Backups were integrity-checked (`zstd -t`) and copied to a second machine, along with Docker volumes and the Proxmox config.
3. **Rebuilt properly.** Destroyed the old pool and created a **mirror** on the two enterprise SSDs, keeping the same pool name so paths didn't change.
4. **Repurposed the USB drive** as a dedicated backup disk instead of part of live storage.
5. **Restored everything** and confirmed services and their settings survived.
6. **Automated backups** with retention, and monthly scrubs.

## Result

- No data loss. All services restored with their settings.
- Storage went from no redundancy to surviving a full disk failure.
- Nightly, tested backups on a separate disk, plus an off-server copy.

## Lessons

- Don't run snapshot or repair operations on a damaged pool. Get the data off first.
- A backup isn't a backup until you've restored from it.
- Make sure you have a way back in (out-of-band console) before any change that could take the network down.
