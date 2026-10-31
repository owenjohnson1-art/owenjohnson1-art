# Firewall recovery and network segmentation

**Dates:** 2026-10-23 to 2026-10-29

## Problem

The FortiGate's admin access was locked to a "trusted host" subnet that no longer existed, so it refused every management connection from the LAN. The only way in was the serial console. The network also had leftover VLANs and policies from older setups that nobody used.

## What I did

**Regained access**
- Connected over the serial console (USB-to-RJ45, 9600 baud) and reset the admin trusted host to the current management subnet.
- Kept a config backup before making changes.

**Least-privilege API access**
- Created a read-only API admin with a custom read-only profile, restricted to two trusted source hosts.
- Verified it: reads work, writes return 403.

**Cleaned up and segmented**
- Removed three unused VLANs and an old captive-portal policy.
- Moved guest Wi-Fi to its own VLAN with client isolation, filtered DNS, and a deny policy to the LAN.
- Restricted admin access (HTTPS, SSH, ping) to the main LAN interface only.
- No port forwards; the WAN answers ping only.

**Isolated lab VLAN** (see [Pentest lab](pentest-lab.md))
- New VLAN carried from the firewall to the hypervisor over a trunk, with no Wi-Fi attached.
- Policies: lab → internet allowed with NAT; only my PC → lab, for SSH/RDP/HTTP; lab → LAN explicitly denied and logged.

## Troubleshooting note

DHCP worked on the guest VLAN but not the new lab VLAN. Firewall debug showed it sending the DHCP Offer, but a packet capture showed the Offer never leaving the interface. I worked around it with static addressing in the lab and documented the issue for a later firewall reboot.

## Lessons

- Trusted-host restrictions are good security, but keep them current or you lock yourself out.
- Prove "read-only" by actually trying a write.
