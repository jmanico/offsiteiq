# offsiteiq

Agent operating rules for this repository. What the system does is in `REQUIREMENTS.md`.

## Where the rules live

Read the file that owns the domain you are touching. Do not restate its content elsewhere.

| File | Domain | Read before |
|---|---|---|
| `REQUIREMENTS.md` | WHAT the system does | changing observable behavior |
| `DESIGN.md` | Design language; `style-guide.html` is its rendered reference | UI work |
| `ARCHITECTURE.md` | Components, boundaries, data flow, data model, technology choices | structural changes |
| `SECURITY.md` | Security requirements (`SEC-*`), controls, threat model; wins on any security conflict | touching auth, input handling, data protection, or any trust boundary |
| `BACKLOG.md` | Tickets (`OIQ-*`) decomposed from the requirements, with acceptance criteria, dependencies, readiness scores, and the definition of done | picking up a ticket |
| `REQUIREMENT_TEMPLATE.md` | Required structure for new GitHub issues | opening an issue |

## Rules

- Every new GitHub issue MUST follow `REQUIREMENT_TEMPLATE.md`, so each issue is a structured, testable requirement.
- If two spec files conflict and the files themselves don't say which wins, stop and raise it. Don't pick one silently.
- When a change alters what a spec file says, update that file in the same pull request.
- Don't implement an item marked `OPEN` until it is resolved; ask instead.
- One ticket per branch and PR. Name the ticket and requirement IDs in the PR description.
- Every ticket meets the definition of done in `BACKLOG.md`, which includes the cross-cutting security requirements.
- Provider integrations are tested against mocks with no network access (NFR-TEST-02). Never call a real Provider from CI.
- Never commit credentials, API keys, or `.env` files (SEC-INTEG-01).
- AI-generated code needs a passing test gate and human review before merge (NFR-REVIEW-01). An agent does not merge, deploy, or close tickets.
- A commit made with AI help carries a co-author trailer naming the tool. An agent never adds `Signed-off-by`.

## Declared checks

None yet. The stack is set in `ARCHITECTURE.md`, but nothing is scaffolded. OIQ-002 adds these commands; update this section in the same PR:

- unit and contract tests, with the 80% line coverage gate (NFR-TEST-01)
- SAST and dependency scan, blocking on high or critical findings (NFR-SEC-01)
- SBOM generation (SEC-INTEG-05)

## Acceptance boundary

Nothing is deployed. Until there is an environment, the boundary is the automated test suite with mocked Providers. A green build does not prove that any real Provider works. Real Provider acceptance needs sandbox credentials and is blocked until open questions 2 and 3 in `REQUIREMENTS.md` are answered.

## Rollback

Every change can be undone with `git revert` until a deployed environment exists. When the first environment exists, this section must name its rollback path.
