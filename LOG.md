# Activity log

Newest first. Each entry: what I did, and the skills involved.

## 2026-11-08

- Self-hosted the notification server so the private briefing never leaves home: login required, deny-all default, separate read-only and post-only accounts, and access rules verified. Kept outage alerts on the public service so they still arrive if the server is down. *Privacy, self-hosting, Docker, access control, alert design*
- Added a scheduled 6am briefing (calendar, email, shared files) to the homelab's monitoring agent. Because email is attacker-controlled input, limited the run to read-only tools and blocked every send, delete, share and shell tool, so prompt injection has nothing to act with. *Prompt-injection defence, least privilege, automation*

## 2026-11-02

- Built the first shot of a GPU-rendered 3D shot library: a scripted Blender scene (volumetric atmosphere, layered clouds, camera move), reviewed through preview stills before a 97-minute full render, then encoded on the server for the video template. *Blender/Python, 3D lighting, render pipelines*
- Benchmarked 3D rendering for the video pipeline: one cinematic Blender frame took 59 s on the server's CPU against 9–14 s on a laptop RTX GPU. Found the GPU was shared with other running apps, and decided to render a reusable shot library on the GPU while the server keeps compositing. *Performance testing, capacity planning*
- Upgraded the video template with real-time 3D (lander and planet shots in three.js, CPU-rendered) and a public-domain footage pipeline (NASA image/video library and Goddard SVS, with automatic credits). Diagnosed an out-of-memory kill from host logs and resized the container. *3D graphics, shaders, media licensing, Linux troubleshooting*

## 2026-11-01

- Planned a science channel (format, schedule, visual style) and had the AI agent rebuild the video template around an animated, data-driven "descent" with a live altitude/temperature/pressure HUD, NASA imagery with automatic credits, face-cam cuts and a synthesised sound bed. Fact-checked the first script against sources and cut render file size 6×. *TypeScript/React, Python (NumPy/SciPy), data visualisation, research, performance tuning*
- Started a [self-hosted video pipeline](projects/video-pipeline.md): one voiceover becomes a captioned explainer video and vertical shorts. Built a new container with local speech-to-text, a React video template and a self-hosted scheduler; fixed a caption layout bug and made renders about 5× faster. *Linux, Docker, FFmpeg, Python, TypeScript/React, performance tuning*
- Wrapped up Exposure Check as a finished prototype: it proved the AI-assisted workflow (isolated build box, task loop, test/type/security gates), and I decided not to launch it. *Project scoping, prioritisation*
- Set up an engineering toolchain for AI-assisted development: static type checking (Pyright) and a 95% test-coverage floor as merge gates, language-server navigation and a code knowledge graph to cut token use, a security-warning plugin, and a cheaper model for routine tasks. *Developer tooling, quality engineering, cost control*
- Ran a full security review of Exposure Check. Four issues fixed, each with a regression test: unbounded memory use on large certificate-log replies, a slow-read connection that could pile up, an unbounded SPF redirect chain, and terminal escape injection in CLI output. *Application security, DoS hardening, secure code review*
- Decided to enforce an 8 KB request-size cap in the app as well as at the proxy (defence in depth); verified oversized and chunked uploads get HTTP 413 while normal reports still work. *Defence in depth, DoS hardening*
- Made Exposure Check deploy-ready behind a Caddy reverse proxy (Docker Compose, automatic HTTPS, the app not exposed directly). The app trusts forwarded client IPs only from the proxy's fixed address; verified through a live proxy that spoofed `X-Forwarded-For` headers are ignored and each visitor gets their own rate limit. *Reverse proxies, Docker, header spoofing, rate limiting*
- Reviewed the finished Exposure Check build: ran the test suite and linter, checked results against real domains, and audited the network code against the passive-only rule. *Code review, QA*
- Found a server-side request forgery (SSRF) gap: a domain pointing at a private address would make the server fetch internal hosts or cloud metadata. Specified the fix (allow only public addresses, connect to the checked IP to stop DNS rebinding) and verified it against loopback, private and link-local test domains. *Application security, SSRF, secure design*

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
