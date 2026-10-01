# offsiteiq

offsiteiq plans company trips for distributed teams. From a destination, a date range, a trip budget, and a per-person budget, it finds flight and hotel options for each participant's home location, suggests meeting and entertainment venues at the destination, and builds one coordinated itinerary. v1 searches and compares only; it does not book or pay.

## Where the rules live

Read the file that owns the domain you are touching. Do not restate its content elsewhere.

| File | Domain | Read before |
|---|---|---|
| `REQUIREMENTS.md` | WHAT the system does | changing observable behavior |
| `DESIGN.md` | Design language; `style-guide.html` is its rendered reference | UI work |
| `ARCHITECTURE.md` | Components, boundaries, data flow, dependency rules | structural changes |
| `SECURITY.md` | Security rules and threat model | touching auth, input handling, data protection, or any trust boundary |
| `REQUIREMENT_TEMPLATE.md` | Required structure for new GitHub issues | opening an issue |

## Rules

- Every new GitHub issue MUST follow `REQUIREMENT_TEMPLATE.md`, so each issue is a structured, testable requirement.
- If two spec files conflict and the files themselves don't say which wins, stop and raise it. Don't pick one silently.
- When a change alters what a spec file says, update that file in the same pull request.
- Don't implement an item marked `OPEN` until it is resolved; ask instead.

## Commands

Build, test, and run commands: TO BE DECIDED (no code yet).

## Workflow

Branching, commit conventions, and release process: TO BE DECIDED.
