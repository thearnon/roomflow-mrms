# RoomFlow Context

Domain glossary for the RoomFlow meeting room management system. This file
defines project vocabulary only; implementation choices belong in planning docs
or ADRs.

Thai version: [CONTEXT.th.md](CONTEXT.th.md)

## System Scope

**RoomFlow**:
A department-level meeting room management and booking system focused on room
availability, booking lifecycle, and conflict detection.
_Avoid_: generic calendar app, enterprise facility platform

**Department Internal System**:
An internal system for one department-level operating context, realistic enough
for real workflows without enterprise integration or multi-tenant complexity.
_Avoid_: enterprise platform, multi-branch system

## Booking Domain

**Resource**:
Something shared that must be reserved before use. In RoomFlow, the primary
resource is a meeting room.
_Avoid_: asset, facility item

**Room**:
A meeting room that users can view and book when it is active and available for
the requested time slot.
_Avoid_: venue, location

**Room Equipment**:
Equipment or capability attached to a room that helps users choose a suitable
room for a meeting.
_Avoid_: amenities, room features

**Time Slot**:
A booking time range with a clear start time and end time.
_Avoid_: schedule block, calendar range

**Booking**:
A reservation of one room for one time slot, with a purpose, owner,
participants, and status.
_Avoid_: reservation, appointment

**Booking Purpose**:
The reason or subject for a booking that gives context to the requester and
administrators.
_Avoid_: title only, description only

**Participant**:
A person identified as related to a booking. Participants may later be modeled
as linked users or text/email records.
_Avoid_: attendee

**Availability**:
Whether a room can be booked for a requested time slot, considering room state
and existing bookings.
_Avoid_: free time

**Conflict Detection**:
The rule that checks whether a new booking overlaps an existing booking for the
same room.
_Avoid_: collision check, schedule validation

## Users and Roles

**User**:
A person who uses RoomFlow to view rooms, book rooms, or manage data according
to permissions.
_Avoid_: account, employee

**Requester**:
A standard user who can view rooms, check availability, create bookings, and
edit or cancel their own bookings.
_Avoid_: normal user, booker

**Department Admin**:
A department-level administrator who manages room master data and bookings
within the department scope.
_Avoid_: room manager, local admin

**System Admin**:
An administrator who manages users, roles, rooms, and system settings.
_Avoid_: superuser

## Lifecycle and Records

**Booking Status**:
The lifecycle state of a booking, such as `BOOKED`, `CANCELLED`, or
`COMPLETED`.
_Avoid_: state when referring only to booking lifecycle

**Cancelled Booking**:
A booking that has been cancelled and should no longer block availability for a
new booking.
_Avoid_: deleted booking

**Notification Placeholder**:
A record or simulation showing that a notification should happen, without
connecting to a real email provider in v1.
_Avoid_: real email integration

**Audit Log**:
A record of an important action, such as booking created, updated, or cancelled,
used to make the system reviewable after the fact.
_Avoid_: debug log, application log
