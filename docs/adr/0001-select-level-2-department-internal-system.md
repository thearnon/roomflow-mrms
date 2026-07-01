# Select Level 2 Department Internal System

RoomFlow will start as a Level 2 Department Internal System. This keeps the
project practical enough to learn real room booking concepts such as
availability, conflict detection, roles, validation, reporting, and audit
records, while deliberately excluding enterprise concerns like SSO, calendar
integrations, recurring bookings, complex approvals, multi-tenant architecture,
and IoT features from v1.

## Considered Options

- **Level 1 simple demo**: easier to build, but too small to teach the real
  domain decisions.
- **Level 2 department internal system**: enough realism without enterprise
  complexity.
- **Level 3 enterprise platform**: more complete, but too broad for the current
  learning goal and likely to cause scope creep.

## Consequences

Future work should prioritize core booking correctness before integrations.
Features that move the project toward enterprise scope need explicit approval.
