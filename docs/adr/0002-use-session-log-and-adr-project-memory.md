# Use Session Log and ADR Project Memory

RoomFlow will use session logs for recurring project state and ADRs for durable
decisions. Session logs capture what happened in each meaningful planning,
design, implementation, or review session, while ADRs record hard-to-reverse,
non-obvious trade-offs so future agents and humans can understand why a choice
was made.

## Considered Options

- **Single journal**: simple, but important decisions become hard to find as the
  file grows.
- **ADR only**: concise, but loses per-session state and learning notes.
- **Session logs plus ADRs**: slightly more structure, but clearer for agentic
  workflow and long-running learning.

## Consequences

Agents must update `docs/session-log/` after meaningful work and create ADRs
only when the decision meets the ADR criteria in `AGENTS.md`.
