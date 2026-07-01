# RoomFlow Initial Analysis

Thai version: [project-brief.th.md](project-brief.th.md)

## 1. Project Understanding

RoomFlow is a Meeting Room Management System for managing department-level
meeting rooms and bookings. The system should help users discover rooms, plan
time, create bookings, edit or cancel bookings, and help administrators manage
room master data and booking activity within a practical internal scope.

This is a learning-focused internal web application. The goal is not only to
produce working software, but also to understand the foundations of the domain,
business rules, database design, UX, testing, and agentic AI workflow.

## 2. Selected System Level: Level 2 Department Internal System

Level 2 fits RoomFlow because it gives enough real complexity to learn resource
booking, roles, conflict detection, validation, and reporting without forcing
enterprise integrations, multi-tenant design, recurring bookings, or approval
matrices into v1.

The practical consequence is that v1 should prioritize correctness and
maintainability of the core booking flow over a large number of integrations.

## 3. Fundamental Concepts

- **Resource booking**: a meeting room is a shared resource that many people may
  want to use, but the same room should not be booked for overlapping time.
- **Time Slot**: a booking must have a clear start time and end time, and the
  start time must be before the end time.
- **Conflict Detection**: a new booking conflicts with an existing booking when
  `new_start < existing_end AND new_end > existing_start` for the same room.
- **Availability**: a room is available when it is active and has no active
  booking that overlaps the requested time slot.
- **User Roles**: the role model should start simple so permissions do not
  become more complex than v1 needs.
- **Validation**: business rules should live close to booking logic, not only in
  the UI.
- **Booking Lifecycle**: start with `BOOKED`, `CANCELLED`, and `COMPLETED`,
  avoiding extra statuses until they have clear workflow value.

## 4. Foundational Architecture

No technology stack has been selected yet. The system should still be designed
with clear separation of concerns:

- **Frontend**: internal UX for finding rooms, checking availability, and
  creating bookings.
- **Backend**: API or server-side workflow for business rules, permissions,
  conflict detection, and persistence.
- **Database**: stores users, roles, rooms, equipment, bookings, participants,
  notifications, and audit logs.
- **Domain logic**: conflict detection and booking validation should be testable
  without coupling to UI.
- **Testing**: start with unit tests for time rules and conflict detection, then
  add integration tests for booking flow.
- **Documentation**: use `CONTEXT.md`, ADRs, and session logs to preserve
  terminology, decisions, and project state.

## 5. Proposed Feature Scope

### Must Have

- Mock authentication or simple login.
- Simple roles.
- Room list and room detail.
- Room capacity and room equipment.
- Room active/inactive state.
- Create booking.
- Edit booking.
- Cancel booking.
- Booking purpose.
- Booking participants.
- Booking status.
- Conflict detection.
- View my bookings.
- View room schedule or availability.

### Should Have

- Basic admin room management.
- Basic dashboard or summary.
- Basic reporting such as total bookings, room usage summary, popular rooms,
  and upcoming bookings.
- Audit-friendly records for important booking actions.

### Could Have

- Notification placeholder.
- Participant count validation.
- Room maintenance state.
- Working-hours validation.
- Future booking limit.
- Maximum booking duration.

### Out of Scope for V1

- Real SSO.
- Outlook or Google Calendar integration.
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

## 6. Initial Domain Model

- **User**: a person who uses the system.
- **Role**: permissions assigned to a user, such as Requester, Department Admin,
  or System Admin.
- **Room**: a meeting room that can be booked.
- **RoomEquipment**: equipment or capabilities attached to a room.
- **Booking**: a room reservation for one time slot.
- **BookingParticipant**: a participant or participant record related to a
  booking.
- **Notification**: a placeholder for notification events.
- **AuditLog**: a record of important actions in the system.

## 7. Initial Business Rules

| Rule | Status | Why it matters |
| --- | --- | --- |
| User cannot book in the past. | Required | Prevents booking records that have no workflow value. |
| Start time must be before end time. | Required | Establishes the basic invariant for a time slot. |
| Booking must have a purpose. | Required | Gives bookings context and makes them reviewable later. |
| Room must be active to be booked. | Required | Prevents booking rooms that are disabled or unavailable. |
| Room cannot be double-booked. | Required | This is the core correctness rule of the system. |
| Cancelled bookings do not block new bookings. | Required | Keeps the booking lifecycle aligned with availability. |
| Users can edit or cancel only their own bookings. | Required | Provides the basic permission rule. |
| Admin can manage bookings when needed. | Needs Decision | Admin authority must be clearly scoped. |
| Maximum booking duration. | Needs Decision | Helps control room usage, but needs a policy decision. |
| Future booking limit. | Needs Decision | Reduces excessively long-ahead booking, but adds policy complexity. |
| Working-hours validation. | Needs Decision | Useful for some organizations, but should not be assumed. |
| Participant count cannot exceed room capacity. | Optional | Useful, but depends on the participant model. |
| No-show handling. | Optional | Adds operational value, but also adds statuses and rules. |

## 8. Suggested UX Flow

1. User opens room list.
2. User filters or scans by capacity, equipment, or availability.
3. User selects a room.
4. User checks available time.
5. User enters purpose, time slot, and participants.
6. System validates time, permission, room state, and conflict detection.
7. Booking is created as `BOOKED`.
8. User can view booking in my bookings or room schedule.
9. User can edit or cancel the booking if allowed.
10. Admin can review room usage and manage room data.

## 9. Risks and Scope Creep

- Calendar integrations can dominate the project and should wait until core
  booking logic is correct.
- Recurring bookings make conflict detection much harder and should not be in
  v1.
- Approval workflows can multiply roles, statuses, and edge cases.
- Multi-tenant or multi-branch design would deeply change the data model.
- Advanced analytics should wait until basic reporting and data quality are
  reliable.
- Choosing technology too early can distract from domain learning.

## 10. Questions for Human Review

### Scope

- Should v1 support only one department?
- Should authentication be mocked first?
- Should booking be instantly confirmed?
- Should participants be free text/email records or linked users?

### Room Management

- What room attributes are required?
- Should room status include active, inactive, and maintenance?
- Should equipment be master data or free text?
- Should room capacity affect validation?

### Booking Rules

- What is minimum and maximum booking duration?
- How far in advance can users book?
- Can users edit booking after it starts?
- Can users cancel anytime?
- Should booking outside working hours be blocked?

### UX

- Should the main flow start room-first or time-first?
- Should the first UI be calendar-based, form-based, or both?
- Should dashboard target users, admins, or both?
- What information must be visible on the room list?

### Technical

- Which frontend/backend stack should be used?
- Which database should be used?
- Should an ORM be used?
- Should Docker be included from the beginning?
- What testing levels are required in v1?

### Documentation

- Should public and canonical docs stay English-first?
- Should every phase include learning notes?
- Which decisions deserve ADRs?
- Which diagrams should be created first: ERD, workflow, or architecture?

## 11. Recommended Next Step

Decide the first implementation foundation before creating project files:

1. Choose v1 authentication style.
2. Choose role model complexity.
3. Choose participant model.
4. Choose primary booking UX flow.
5. Choose stack, database, ORM, testing approach, and Docker policy.

After those decisions, create a focused implementation plan before scaffolding
the app.
