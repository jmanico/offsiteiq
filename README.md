# offsiteiq

A company trip planner for distributed teams. Given a destination, a date range, and trip and per-person budgets, offsiteiq finds flight and hotel options for every participating employee, suggests local meeting and entertainment venues, and builds a coordinated itinerary for the trip.

## Status

Early planning. No code yet. The draft requirements are in [REQUIREMENTS.md](REQUIREMENTS.md). Items marked `OPEN` there need stakeholder confirmation before they are implemented.

## Planned features

- **Trip setup:** destination, dates, trip budget and per-person budget, with multi-currency support
- **Participants:** an employee roster with home locations, used as the origin for each person's travel search
- **Travel and lodging search:** availability and rates from several flight and hotel providers, with checks against both budgets
- **Local venues:** meeting spaces and entertainment options at the destination
- **Itinerary:** a chronological schedule for the whole trip, a view for each participant, and conflict detection

## Security

Security requirements follow OWASP ASVS 5.0. See section 5 of [REQUIREMENTS.md](REQUIREMENTS.md).
