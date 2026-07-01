# RoomFlow

RoomFlow is a learning-focused Meeting Room Management System for building and
understanding a practical internal room booking application.

The project is intentionally scoped as a **Level 2: Department Internal
System**. That means RoomFlow should be realistic enough for department-level
work, but should not become a large enterprise platform in v1.

Thai version: [README.th.md](README.th.md)

## Purpose

RoomFlow is not only about producing an app that can book rooms. It should help
the developer understand the core ideas behind shared-resource booking systems:

- core domain of a meeting room booking system
- room availability and conflict detection
- relationships between users, rooms, bookings, and participants
- business rules and validation
- maintainable database schema design
- frontend UX for internal enterprise users
- separation of frontend, backend, database, validation, and business logic
- testing from the beginning
- agentic AI workflow with human review and decision records

## Current Status

The repository is in documentation-foundation stage:

- no frontend or backend stack has been selected
- no database or ORM has been selected
- no application code has been scaffolded
- no package manager or development commands have been defined
- project memory starts with `CONTEXT.md`, `docs/adr/`, and
  `docs/session-log/`

## V1 Direction

RoomFlow v1 should start with department-level scope:

- mock authentication or simple login
- simple roles
- room list and room detail
- room capacity and equipment
- create, edit, and cancel booking
- booking purpose and participants
- booking status
- availability and overlapping booking conflict detection
- basic admin room management
- basic notification placeholder
- basic dashboard and reporting
- audit-friendly structure for important actions

## Out of Scope for V1

These features should stay out of v1 unless a later decision explicitly approves
them:

- real SSO
- Microsoft Outlook or Google Calendar integration
- recurring booking engine
- complex approval workflow
- multi-tenant architecture
- multi-branch organization support
- IoT, QR check-in, or door access
- advanced analytics
- payment or billing

## Documentation Map

- `AGENTS.md`: canonical instructions for AI coding agents
- `CONTEXT.md`: canonical English domain glossary
- `CONTEXT.th.md`: Thai companion glossary
- `docs/project-brief.md`: canonical English project analysis and scope
- `docs/project-brief.th.md`: Thai companion project brief
- `docs/adr/`: architectural decision records
- `docs/session-log/`: session state and work summaries
- `CLAUDE.md`: Claude Code entry point that imports `AGENTS.md`

## License

RoomFlow is released under the [MIT License](LICENSE). You may use, copy,
modify, distribute, and build on this project as long as the license notice is
included.

## Next Decisions

Before scaffolding code, decide:

- whether v1 uses mock authentication or simple authentication
- whether the role model starts with two roles or three roles
- whether participants are text/email records or linked users
- whether the booking UI starts room-first, time-first, calendar-based, or
  form-based
- stack, database, ORM, testing approach, and Docker policy
