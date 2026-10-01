# offsiteiq agent instructions

This file and `CLAUDE.md` are the same file (symlinked). Edit `AGENTS.md`.

This is the repository contract a coding agent reads before triaging,
implementing, or reviewing work here. Work follows this loop:

```text
ticket → branch → inspect repo → implement → run declared checks
       → collect evidence → hand off for human review
```

## Sources of truth

1. `REQUIREMENTS.md`: what the system must do. Every change traces to a
   requirement ID (`FR-*`, `SEC-*`, `NFR-*`).
2. `BACKLOG.md`: tickets (`OIQ-NNN`) decomposed from the requirements, with
   acceptance criteria, dependencies, and readiness scores.
3. `ARCHITECTURE.md`: the system architecture and stack. `DESIGN.md`: the
   design system. Jim leads design and the frontend, so follow these files
   rather than proposing alternatives. If either disagrees with
   `REQUIREMENTS.md`, the requirements win.
4. `docs/architecture/adr/`: durable decisions made after ARCHITECTURE.md.
   None exist yet.

## Rules

- Do not implement anything marked `OPEN` in `REQUIREMENTS.md`. An `OPEN`
  item is a question for a human, not permission to pick a default.
- One ticket per branch and PR. Name the ticket and requirement IDs in the PR
  description.
- Every ticket meets the definition of done in `BACKLOG.md`, which includes
  the cross-cutting security requirements.
- Provider integrations are tested against mocks with no network access
  (NFR-TEST-02). Never call a real Provider from CI.
- Never commit credentials, API keys, or `.env` files (SEC-INTEG-01).
- AI-generated code needs a passing test gate and human review before merge
  (NFR-REVIEW-01). An agent does not merge, deploy, or close tickets.
- A commit made with AI help carries a co-author trailer naming the tool.
  An agent never adds `Signed-off-by`.

## Declared checks

None yet. The stack is set in ARCHITECTURE.md, but nothing is scaffolded.
OIQ-002 adds these commands; update this section in the same PR:

- unit and contract tests, with the 80% line coverage gate (NFR-TEST-01)
- SAST and dependency scan, blocking on high or critical findings (NFR-SEC-01)
- SBOM generation (SEC-INTEG-05)

## Acceptance boundary

Nothing is deployed. Until there is an environment, the boundary is the
automated test suite with mocked Providers. A green build does not prove that
any real Provider works. Real Provider acceptance needs sandbox credentials
and is blocked until open questions 2 and 3 are answered.

## Rollback

Every change can be undone with `git revert` until a deployed environment
exists. When the first environment exists, this section must name its
rollback path.
