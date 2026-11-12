# Book Watch

**Status: designed, build not started (2026-11-12).**

A monitor that streams live crypto trades and order books from public exchange feeds, compares each minute with a historical baseline, and sends anomaly alerts to my phone. It then records what the price did 5 minutes, 1 hour and 4 hours after each alert, so I can see whether the signals mean anything. It is alerts only: it never trades and holds no exchange keys.

## Design

- **Pipeline:** exchange adapters, validated order books, 1-minute bars, baselines, detectors, an alert router, phone notifications.
- **Baselines:** rolling median and MAD (robust to outliers), compared by hour of day once a week of history exists, with a warm-up period and a floor so quiet periods don't create false alarms.
- **Detectors:** price jump, volume spike, spread blowout, order-book imbalance, cross-exchange price gap, feed stall.
- **Alert hygiene:** cooldowns and an hourly cap; suppressed signals are still stored and measured.
- **Stack:** Python, asyncio, WebSockets, SQLite. Gates: linting, strict type checking and a 95% test-coverage floor.

## Security and reliability decisions

- Exchange data is treated as untrusted: validated, bounded, and never placed into notification text or SQL strings.
- Order books are verified by checksum or sequence number. Any gap or reconnect resets the book and mutes its detectors briefly, because a corrupt book produces fake alerts.
- Outbound connections only, bounded queues and frame sizes, so memory stays flat over weeks.
- The build runs in an isolated container with no infrastructure credentials, one small task per run, and its rules forbid adding trading or authentication code.

## How it was made

I chose the market and the alerts-only scope. An AI assistant drafted the spec, I had a second AI model review it, and the assistant adopted or corrected each point with reasons. The code will be written by an unattended AI build loop against that spec: a builder agent does one task per run, and a separate reviewer agent with no edit tools checks each commit and sends problems back. I review the result.
