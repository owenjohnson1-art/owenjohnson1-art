# Least-privilege access for an automation agent

**Dates:** 2026-10-23 to 2026-10-25, extended 2026-11-08

## Goal

Let an AI automation agent (Claude Code) monitor the homelab around the clock without giving it the power to change or break anything.

## Approach

The agent runs in its own container, as a non-root user with no sudo. Every platform it can reach gets a **read-only** credential, and each one was tested by attempting a write.

| Platform | Access | How it's limited | Verified |
|---|---|---|---|
| Proxmox | API token | Auditor role only | Audit privileges only |
| UniFi | API key | Key can't be scoped, so usage is limited to GET requests in the agent's rules | Reads work |
| Docker | Socket proxy | POST disabled; listens on the VPN interface only, not the LAN | Writes return 403 |
| FortiGate | API admin | Custom read-only profile, two trusted source hosts | Writes return 403 |

## Keeping secrets away from the agent

- Keys live in a file outside the agent's working folder and outside cloud-synced storage.
- The agent's own permission settings **block reading that file** and **block editing the health check scripts**. The one pre-approved command is the fixed health check script, which loads the keys itself.
- Anything that does need to change something (development work) runs in a **separate container that has no keys at all**.

## Daily briefing from untrusted input (2026-11-08)

The same agent now sends a 6am briefing to my phone: today's calendar, emails that need me, and new shared files. Email is untrusted input, since anyone can send text that tries to instruct the agent (prompt injection). So the scheduled run gets only the read tools for mail, calendar and files. Every send, reply, forward, delete, share, calendar-edit and shell tool is explicitly blocked, so a hostile email has nothing to act with. The prompt also says not to follow instructions found in content. Tested with a live run.

## Web research with web-only tools (2026-11-12)

I moved a daily web-research job from my PC to the server agent so it runs whether or not the PC is on. Web pages are untrusted input too, and this container holds the read-only infrastructure keys. So that run gets only web search and web fetch: no shell and no file tools. The agent returns JSON, and a separate script validates it, writes the files and sends the phone message. A hostile page can at worst put a wrong line of text in a report. Tested with a live run.

## Why it matters

This is the same thinking as service accounts at work: give automation exactly the access it needs, assume it could be misused, and test the limits instead of trusting the label.
