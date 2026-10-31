# SQL lab

**Started:** 2026-10-29

## Goal

A real database server for my SQL coursework, set up the way a production one should be.

## What I built

- MariaDB 11.8 in its own container, included in nightly backups
- Sample databases: `world` (countries and cities) and `sakila` (DVD rental)
- A practice account with:
  - Full rights on its own `practice` database
  - **Read-only** access to the sample databases, so they can't be damaged
  - Logins allowed only from the home LAN
- The password lives in a secrets file, not in notes or code

## Next

- Implement data structures in Python (linked lists, hash tables, trees, B-trees) and use them on real data from the server, to understand how database indexes work
- Practise SQL injection against DVWA and Juice Shop in the [pentest lab](pentest-lab.md), connecting both sides of what I'm learning
