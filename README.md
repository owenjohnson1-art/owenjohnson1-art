# Owen Johnson: Homelab and Security Portfolio

I work in cybersecurity and I'm studying for a bachelor's in cybersecurity. This repo is a running record of what I build, break and fix in my homelab: real infrastructure, real incidents, and the security decisions behind them.

Everything here is written from my own lab notes. Details like addresses, network names and credentials are redacted on purpose.

## Skills shown here

| Area | What I've done | Where |
|---|---|---|
| Virtualization | Run a Proxmox VE host with containers and VMs; backup, restore and rebuild | [Homelab infrastructure](projects/homelab-infrastructure.md) |
| Storage and recovery | Recovered from a degraded ZFS pool and rebuilt it as a mirror with no data loss | [ZFS recovery](projects/zfs-storage-recovery.md) |
| Network security | FortiGate recovery over the serial console, VLAN segmentation, firewall policy cleanup | [Firewall and segmentation](projects/firewall-and-segmentation.md) |
| Monitoring | Twice-daily health checks, disk health, rogue device detection, phone alerts | [Monitoring and alerting](projects/monitoring-and-alerting.md) |
| Least privilege | Read-only API access for an automation agent across 4 platforms, verified by testing writes | [Least-privilege automation](projects/least-privilege-automation.md) |
| Offensive security | Isolated pentest lab with Kali, DVWA and OWASP Juice Shop, with isolation tested | [Pentest lab](projects/pentest-lab.md) |
| Databases | MariaDB server with role-separated accounts for SQL practice | [SQL lab](projects/sql-lab.md) |
| Development | Passive domain security scanner for small businesses (in progress) | [Exposure Check](projects/exposure-check.md) |

## Recent activity

See the full [activity log](LOG.md).

- **2026-10-30:** Started Exposure Check, a passive security report for small-business domains, on an isolated build container.
- **2026-10-29:** Built an isolated pentest lab on its own VLAN; verified the lab can't reach the home network.
- **2026-10-29:** Built a MariaDB server for SQL coursework with least-privilege accounts.
- **2026-10-25:** Regained admin access to a locked-out FortiGate over the serial console and cleaned up the VLAN layout.
- **2026-10-23:** Rebuilt failing ZFS storage as a mirror and set up automated, tested backups.

## How I work

I use AI-assisted tooling (Claude Code) as part of my workflow. I plan the work, make the design and security decisions, and verify the results; the AI helps run commands and write up notes. Using these tools safely is a skill too: the agent runs with read-only keys, can't see secrets, and is isolated from anything it doesn't need.

## Next steps

Next in the lab: Metasploitable and VulnHub targets, a vulnerability scan of the home network, and an Active Directory attack and detection lab.
