# Homelab infrastructure

## Overview

A rack server that runs my home network services, a learning lab and my own projects.

- **Hardware:** Dell PowerEdge R430 (Xeon E5-2640 v4, 40 threads, 32 GB RAM), with iDRAC for out-of-band management
- **Hypervisor:** Proxmox VE 9
- **Storage:** ZFS mirror on two enterprise SSDs, plus a dedicated backup disk
- **Network:** FortiGate 30E firewall, UniFi switch and access points
- **Remote access:** Tailscale, so nothing is port-forwarded to the internet

## What runs on it

| Workload | Type | Purpose |
|---|---|---|
| UniFi OS Server | Container | Manages the switch and access points |
| Automation agent | Container | Runs health checks with read-only API keys |
| SQL server | Container | MariaDB for coursework |
| Build box | Container | Isolated development environment |
| Portainer, Uptime Kuma | Docker | Container management and uptime monitoring |
| Kali Linux | VM, lab VLAN | Attacker machine |
| Vulnerable targets | VM, lab VLAN | DVWA and OWASP Juice Shop |

## Backups

- Nightly Proxmox backups of every container and VM except disposable lab machines, kept 7 daily, 4 weekly and 3 monthly
- A separate nightly script backs up Docker volumes and the Proxmox cluster config, because Proxmox backups don't cover Docker running on the host
- Restores were tested for real during the [storage rebuild](zfs-storage-recovery.md)

## Design choices

- **No inbound ports.** Remote access goes through Tailscale instead of port forwards.
- **Out-of-band access first.** Before any risky change I confirm I can still reach the iDRAC console.
- **Separate boxes for separate trust levels.** The container with monitoring keys never runs untrusted code; development happens in its own container with no keys.
