<p align="center"><img src="logo.svg" alt="offsiteiq" width="320"></p>

# offsiteiq

A company trip planner for distributed teams. Given a destination, a date range, and trip and per-person budgets, offsiteiq finds flight and hotel options for every participating employee, suggests local meeting and entertainment venues, and builds a coordinated itinerary for the trip.

## Status

Early planning. No code yet. The draft requirements are in [REQUIREMENTS.md](REQUIREMENTS.md), and the ticket backlog with readiness scores is in [BACKLOG.md](BACKLOG.md). Agent instructions are in [AGENTS.md](AGENTS.md). Items marked `OPEN` in the requirements need stakeholder confirmation before they are implemented.

The design system (brand, color, typography, components, and key screens) is in [DESIGN.md](DESIGN.md). The logo is [logo.svg](logo.svg).

The system architecture (React, Django, FastAPI, PostgreSQL, AWS) is in [ARCHITECTURE.md](ARCHITECTURE.md).

## Planned features

- **Trip setup:** destination, dates, trip budget and per-person budget, in USD for v1
- **Participants:** an employee roster with home locations, used as the origin for each person's travel search
- **Travel and lodging search:** availability and rates from several flight and hotel providers, with checks against both budgets
- **Local venues:** meeting spaces and entertainment options at the destination
- **Itinerary:** a chronological schedule for the whole trip, a view for each participant, and conflict detection

## Design

Calm, friendly enterprise SaaS: light neutral surfaces, navy text, a small set of functional accents (blue, teal, orchid), Inter type, and rounded cards. Every price shows its source and freshness, budget status is never shown by color alone, and the UI targets WCAG 2.2 AA. See [DESIGN.md](DESIGN.md).

## Security

Security requirements follow OWASP ASVS 5.0. See section 5 of [REQUIREMENTS.md](REQUIREMENTS.md).
