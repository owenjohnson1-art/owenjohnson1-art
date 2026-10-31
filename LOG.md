# Activity log

Newest first. Each entry: what I did, and the skills involved.

## 2026-10-31

- Decided the scan scope for Exposure Check's HTTPS check: one plain HTTP request to test the redirect, and up to 3 same-site redirects so headers are judged on the real homepage. Kept the tool strictly passive. *Security assessment, scoping*
- Reviewed third-party AI coding plugins before installing them on the build container; installed two and declined a stale fork of a proxy that would have handled account credentials. *Supply-chain risk, least privilege*

## 2026-10-30

- Chose a side project, [Exposure Check](projects/exposure-check.md): a passive security report for small-business domains. Wrote the spec and a 17-step task list. *Product scoping, security assessment*
- Built an isolated development container with no homelab credentials, plus an automated build loop with tests as the quality gate. *Linux, containers, CI-style automation, least privilege*
- Started this portfolio, with an automated check that blocks any push containing internal addresses or secrets. *Git, OPSEC*

## 2026-10-29

- Built a [pentest lab](projects/pentest-lab.md) on its own VLAN with Kali, DVWA and OWASP Juice Shop. Verified the lab can reach the internet but not the home network. *Network segmentation, firewall policy, Docker*
- Troubleshot lab DHCP with firewall debug and packet captures; worked around it with static addressing. *Network troubleshooting*
- Built a MariaDB [SQL lab](projects/sql-lab.md) with read-only access to sample databases. *Databases, access control*

## 2026-10-25

- Regained admin access to the FortiGate over the serial console. Created a read-only API admin and verified writes are refused. *Firewall administration, incident recovery*
- Removed unused VLANs and policies; moved guest Wi-Fi to an isolated VLAN. *Network security*
- Added disk SMART, scrub and new-device checks to [monitoring](projects/monitoring-and-alerting.md), plus Proxmox and Uptime Kuma alerts. *Monitoring*

## 2026-10-24

- Built a locked-down container for an AI automation agent with [read-only access](projects/least-privilege-automation.md) and a twice-daily health check that alerts my phone. *Automation, least privilege*

## 2026-10-23

- [Rebuilt storage](projects/zfs-storage-recovery.md) as a ZFS mirror, restored all services, and set up nightly backups with retention. *Storage, backup and recovery*
- Set up read-only API access for Proxmox, UniFi and Docker. *Identity and access management*

## 2026-10-22

- Mapped the homelab and found a degraded, non-redundant ZFS pool. Recovered from a storage hang and network outage, and backed up every container. *Incident response*
