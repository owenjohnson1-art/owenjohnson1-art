# Exposure Check (in progress)

**Started:** 2026-10-30

## What it is

A small business enters its domain and gets a plain-English security report: a grade from A to F, what's wrong, and how to fix it. Aimed at owners who don't have an IT team.

## Checks

All checks are **passive**: public DNS, certificate transparency logs and a normal request to the homepage over HTTP and HTTPS (following at most 3 same-site redirects). No port scanning or probing, so it's safe and legal to run against any domain.

- Email spoofing protection: SPF, DKIM, DMARC
- Mail servers
- TLS certificate validity and expiry
- HTTPS redirect and security headers
- Software versions the site gives away
- Subdomains found in certificate logs (flagging names like dev, staging, admin)
- DNSSEC and CAA records

## How it's built

- Python, FastAPI, pytest
- Developed in an isolated container with **no access to my homelab credentials**
- An AI coding agent works through a task list one item at a time. Each task must pass its tests and linting before it's committed, and the agent can't push code itself.

## Status

8 of 17 tasks done: SPF, DKIM, DMARC, MX, TLS certificate, HTTPS redirect and security headers checks, each with tests. Build in progress.
