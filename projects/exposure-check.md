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

Feature-complete: 11 checks (SPF, DKIM, DMARC, MX, TLS, HTTPS redirect, security headers, software disclosure, subdomains via certificate transparency, DNSSEC, CAA), a grading engine, CLI, web app with rate limiting and caching, and 312 tests at 99% coverage, type-checked with Pyright. Security reviews found and fixed an SSRF gap (the scanner only connects to public addresses and pins the checked IP to stop DNS rebinding) plus four DoS and injection issues. Next: proxy-aware rate limiting and a request size cap, then deployment.
