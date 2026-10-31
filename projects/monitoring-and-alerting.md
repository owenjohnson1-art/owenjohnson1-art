# Monitoring and alerting

**Dates:** 2026-10-24 to 2026-10-25

## Goal

Find out about problems from my phone before they become outages, without opening anything to the internet.

## What I built

- **Twice-daily health check** (08:00 and 20:00) covering:
  - Hypervisor, container and VM status
  - ZFS pool health and last scrub
  - Disk SMART status and SSD wear
  - Backup job results
  - **New device detection:** compares connected network clients against an approved list and flags unknown MAC addresses
- **Push alerts with ntfy:** anything that isn't "OK" goes to my phone. Reports are kept for 30 days.
- **Proxmox notifications** go to the same ntfy topic through a webhook, filtered to warnings and errors.
- **Uptime Kuma** monitors for the main services.

## Security choices

- The health check is a **fixed, read-only script**. The automation agent may run that script and nothing else, and it can't read the file that holds the API keys (see [Least-privilege automation](least-privilege-automation.md)).
- Approving a new device is a deliberate manual step with its own script.

## Known gap

Uptime Kuma runs on the same server it monitors. An external "dead man's switch" is on the to-do list.
