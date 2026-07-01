# AGENTS.md

Guidance for AI coding agents working in this repository.

## Project Overview

`RoomFlow` is a learning-focused meeting room management and booking system.
The selected system level is **Level 2: Department Internal System**.

This project should become a practical internal web application for managing
meeting rooms, room availability, bookings, participants, basic admin work,
basic reporting, and audit-friendly records. The purpose is not only to ship a
working app, but also to help the human developer understand the fundamentals
behind room booking systems: domain modeling, conflict detection, validation,
database design, frontend UX, backend logic, testing, and safe agentic AI
workflow.

The human is the product owner, software architect, reviewer, and decision
maker. The agent is a researcher, analyst, planner, implementation assistant,
tester, and reviewer.

Keep the project practical. Avoid over-engineering and avoid turning v1 into a
large enterprise platform.

## Current Repository State

- This repository is in documentation-foundation stage.
- No application stack has been selected yet.
- No `package.json`, framework, database, ORM, Docker setup, authentication
  provider, or deployment target should be assumed until a later decision.
- `CLAUDE.md` imports this file with `@AGENTS.md`; keep this file as the
  canonical agent instruction source.
- Local agent skill directories such as `.agents/skills/` and `.claude/skills/`
  may exist on a developer machine, but they are local-only and ignored for the
  public repository unless the human explicitly decides to publish project-local
  skills later.

## Language Rules

- Use Thai-first explanations when talking with the human unless the human asks
  for another language.
- Keep public and canonical project docs in English by default.
- Use `.th.md` companion files for Thai versions of public or canonical docs
  when Thai learning context is valuable.
- Keep ADRs in English unless the human explicitly requests a Thai-only
  decision record.
- Keep session logs Thai-first because they are working memory for the human and
  agent.
- Keep common technical terms in English when clearer, such as `frontend`,
  `backend`, `database`, `API`, `business rules`, `validation`, `conflict
  detection`, `module`, `entity`, `service`, `repository`, `state`, `workflow`,
  and `testing`.
- Do not casually rewrite Thai content. Treat localized copy and mixed
  Thai-English technical wording as meaningful project context.

## Canonical Documentation

- `README.md`: English human-facing overview and project entry point.
- `README.th.md`: Thai companion overview.
- `CONTEXT.md`: English domain glossary only. Keep implementation decisions out
  of it.
- `CONTEXT.th.md`: Thai companion glossary.
- `docs/project-brief.md`: English project analysis, scope, model, rules, UX,
  risks, and next decisions.
- `docs/project-brief.th.md`: Thai companion project brief.
- `docs/adr/`: architectural decision records for hard-to-reverse,
  non-obvious trade-offs.
- `docs/session-log/`: stateful session summaries for meaningful planning,
  design, implementation, and review sessions.
- `docs/session-log/TEMPLATE.md`: required format for new session logs.

When changing project scope, update the relevant docs together. When changing
domain terms, update `CONTEXT.md` and `CONTEXT.th.md` together when the Thai
companion exists. When making an important architectural or scope decision,
consider whether it deserves an ADR.

## V1 Scope

RoomFlow v1 should stay at department-level internal system complexity.

### Included in Scope

- User login or mock authentication.
- Simple user roles.
- Room list and room detail.
- Room capacity and room equipment.
- Room active or unavailable state.
- Room availability checks.
- Create, edit, and cancel bookings.
- Booking purpose.
- Booking participants.
- Booking status.
- Conflict detection for overlapping bookings.
- Basic admin room management.
- Basic notification placeholder.
- Basic dashboard or summary.
- Basic reporting.
- Audit-friendly logging for important actions where appropriate.

### Out of Scope for V1 Unless Explicitly Approved

- Real SSO integration.
- Microsoft Outlook integration.
- Google Calendar integration.
- Multi-branch organization support.
- Complex approval workflow.
- Multi-step approval matrix.
- Recurring booking engine.
- IoT sensor integration.
- Door access integration.
- QR check-in.
- Advanced analytics.
- Multi-tenant architecture.
- Payment or billing.
- Smart building features.

## Expected Roles

Start with a simple role model unless the human chooses otherwise.

- `Requester`: can view rooms, check availability, create bookings, and edit or
  cancel their own bookings.
- `Department Admin`: can manage room master data, view all department
  bookings, and manage bookings when needed.
- `System Admin`: can manage users, roles, rooms, and system settings.

Analyze whether all three roles are needed before implementation. Do not add
role complexity just because it is common in larger systems.

## Expected Booking Statuses

Prefer a simple booking lifecycle for v1:

- `BOOKED`
- `CANCELLED`
- `COMPLETED`

Evaluate optional statuses before adding them:

- `DRAFT`
- `EXPIRED`
- `NO_SHOW`

Optional statuses should be added only if they improve real workflows enough to
justify the extra business rules and tests.

## Core Domain Rules

Before implementation, explain and validate these concepts:

- A room is a shared `Resource`.
- A `Booking` reserves one room for one `Time Slot`.
- The same room cannot have overlapping active bookings.
- Conflict detection uses the rule:
  `new_start < existing_end AND new_end > existing_start`.
- Different rooms may be booked at overlapping times.
- Users cannot book in the past.
- Start time must be before end time.
- A booking must have a purpose.
- A room must be active to be booked.
- A user may edit or cancel their own booking unless an admin permission allows
  broader management.

Mark business rules as `Required`, `Optional`, or `Needs Decision` in planning
docs before implementing them.

## Agent Workflow Rules

- Inspect the repository before editing. Read local docs before assuming
  project direction.
- Check whether the directory is a git repository before relying on git status
  or commits.
- Do not start scaffolding or implementation until the human has reviewed and
  approved the relevant analysis or plan.
- Before writing code for a major feature, produce or update a planning note
  that covers project understanding, fundamental concepts, architecture,
  feature scope, domain model, business rules, UX flow, risks, and questions.
- Keep changes scoped to the current request.
- For documentation-only tasks, do not edit runtime source code.
- Do not add package manager, framework, database, ORM, Docker, auth provider,
  deployment, or production domain assumptions until they are explicitly
  selected.
- Do not edit `.agents/skills/` or `.claude/skills/` unless the task is
  explicitly about local agent skill maintenance. These directories are
  local-only by default and should not be treated as RoomFlow product source.
- Do not delete files or run destructive git commands unless explicitly asked.
- Preserve user changes. If unexpected changes appear, work with them and ask
  only when they make the task impossible.

## Session Summary Rules

For every meaningful planning, design, implementation, debugging, or review
session, create or update a session log under `docs/session-log/`.

Each session log must include:

- Summary.
- Interesting Topics.
- Decisions.
- Alternatives Considered.
- Changes Made.
- Desired Outcome.
- Alignment Check.
- Open Questions.
- Next State.

Use `docs/session-log/TEMPLATE.md` for new logs. The goal is to preserve why
choices were made, what changed, what was not chosen, and what state the project
should be in after the session.

## ADR Rules

Use ADRs sparingly. Create an ADR only when the decision is:

- Hard to reverse.
- Non-obvious without context.
- A real trade-off among meaningful alternatives.

ADRs live in `docs/adr/` and use sequential filenames such as
`0001-select-level-2-department-internal-system.md`.

## Testing and Validation

Because the app has not been scaffolded yet, there are no project test commands
to run. For documentation changes, validate by reading the changed files and
using `rg` to check headings, links, stale project names, and key terms.

After a stack is selected and project scripts exist, update this section with
the actual commands declared by the project. Do not invent commands that are not
present in local config.

## Security and Secrets

- Never read, print, commit, or modify secret-bearing env files.
- Do not add real email addresses, private contact links, analytics IDs,
  credentials, or deployment secrets unless the human explicitly provides and
  approves them.
- Use placeholders for notification providers and integrations until the
  relevant decisions are made.

## Final Response Requirements

When finishing a task, report:

- Whether files were created or updated.
- The key project behavior, rule, or decision affected.
- Commands run and their results.
- Checks not run and why.
- Assumptions, uncertainties, or recommended next improvements.
