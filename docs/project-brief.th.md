# RoomFlow Initial Analysis

English version: [project-brief.md](project-brief.md)

## 1. Project Understanding

RoomFlow คือ Meeting Room Management System สำหรับจัดการห้องประชุมและการจอง
ภายในแผนก ระบบต้องช่วยให้ผู้ใช้เห็นห้อง วางแผนเวลา จองห้อง แก้ไขหรือยกเลิก
booking และช่วยให้ admin ดูแล room master data กับ booking ภายใน scope ที่เหมาะสม.

โปรเจคนี้เป็น learning-focused internal web application ผู้พัฒนาไม่ได้ต้องการแค่
ผลลัพธ์เป็นแอป แต่ต้องการเข้าใจพื้นฐานของ domain, business rules, database,
UX, testing, และ agentic AI workflow ด้วย.

## 2. Selected System Level: Level 2 Department Internal System

Level 2 เหมาะกับ RoomFlow เพราะมี complexity จริงพอให้เรียนรู้ resource booking,
roles, conflict detection, validation, และ reporting แต่ยังไม่ใหญ่จนต้องรองรับ
enterprise integration, multi-tenant, recurring booking, หรือ approval matrix.

ผลของการเลือกระดับนี้คือ v1 ควรเน้น correctness และ maintainability ของ core
booking flow มากกว่า integration จำนวนมาก.

## 3. Fundamental Concepts

- **Resource booking**: ห้องประชุมคือ shared resource ที่หลายคนต้องการใช้ แต่
  ห้องเดียวกันไม่ควรถูกจองซ้อนเวลาเดียวกัน.
- **Time Slot**: booking ต้องมี start time และ end time ชัดเจน และ start time
  ต้องมาก่อน end time.
- **Conflict Detection**: booking ใหม่ชน booking เดิมเมื่อ
  `new_start < existing_end AND new_end > existing_start` สำหรับห้องเดียวกัน.
- **Availability**: ห้องว่างเมื่อห้อง active และไม่มี active booking ที่ทับซ้อน
  time slot ที่เลือก.
- **User Roles**: role ควรเริ่มง่ายเพื่อไม่ให้ permission model ซับซ้อนเกิน v1.
- **Validation**: business rules ต้องอยู่ใกล้ booking logic ไม่กระจายอยู่แค่ UI.
- **Booking Lifecycle**: เริ่มจาก `BOOKED`, ไปเป็น `CANCELLED` หรือ `COMPLETED`
  โดยหลีกเลี่ยง status เพิ่มเติมจนกว่าจะมีเหตุผลชัดเจน.

## 4. Foundational Architecture

ยังไม่เลือก tech stack ในรอบนี้ แต่ระบบควรถูกออกแบบให้แยก concern ชัดเจน:

- **Frontend**: internal UX สำหรับค้นหาห้อง ดู availability และสร้าง booking.
- **Backend**: API หรือ server-side workflow สำหรับ business rules, permission,
  conflict detection, และ persistence.
- **Database**: เก็บ users, roles, rooms, equipment, bookings, participants,
  notifications, และ audit logs.
- **Domain logic**: conflict detection และ booking validation ควร test ได้โดยไม่
  ผูกกับ UI.
- **Testing**: เริ่มจาก unit tests สำหรับ time rules และ conflict detection,
  แล้วเพิ่ม integration tests สำหรับ booking flow.
- **Documentation**: ใช้ `CONTEXT.md`, ADRs, และ session logs เพื่อเก็บความคิด
  decision และ state ของงาน.

## 5. Proposed Feature Scope

### Must Have

- Mock authentication หรือ simple login.
- Simple roles.
- Room list และ room detail.
- Room capacity และ room equipment.
- Room active/inactive state.
- Create booking.
- Edit booking.
- Cancel booking.
- Booking purpose.
- Booking participants.
- Booking status.
- Conflict detection.
- View my bookings.
- View room schedule หรือ availability.

### Should Have

- Basic admin room management.
- Basic dashboard หรือ summary.
- Basic reporting เช่น total bookings, room usage summary, popular rooms,
  upcoming bookings.
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
- Outlook หรือ Google Calendar integration.
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

- **User**: คนที่ใช้งานระบบ.
- **Role**: สิทธิ์ของ user เช่น Requester, Department Admin, System Admin.
- **Room**: ห้องประชุมที่จองได้.
- **RoomEquipment**: อุปกรณ์หรือความสามารถของห้อง.
- **Booking**: การจองห้องใน time slot หนึ่ง.
- **BookingParticipant**: ผู้เข้าร่วมหรือข้อมูลผู้เกี่ยวข้องกับ booking.
- **Notification**: placeholder สำหรับ notification event.
- **AuditLog**: record ของ action สำคัญที่เกิดในระบบ.

## 7. Initial Business Rules

| Rule | Status | Why it matters |
| --- | --- | --- |
| User cannot book in the past. | Required | ป้องกันข้อมูล booking ที่ไม่มีความหมายเชิง workflow. |
| Start time must be before end time. | Required | เป็น invariant พื้นฐานของ time slot. |
| Booking must have a purpose. | Required | ทำให้ booking มี context และตรวจสอบย้อนหลังได้. |
| Room must be active to be booked. | Required | ป้องกันการจองห้องที่ปิดใช้งานหรือไม่พร้อมใช้. |
| Room cannot be double-booked. | Required | เป็น core correctness ของระบบ. |
| Cancelled bookings do not block new bookings. | Required | ทำให้ lifecycle ของ booking สอดคล้องกับ availability. |
| Users can edit or cancel only their own bookings. | Required | เป็น permission rule พื้นฐาน. |
| Admin can manage bookings when needed. | Needs Decision | ต้องกำหนดขอบเขต admin authority ให้ชัด. |
| Maximum booking duration. | Needs Decision | ช่วยควบคุมการใช้งานห้อง แต่ต้องเลือก policy. |
| Future booking limit. | Needs Decision | ช่วยลดการจองยาวเกินไป แต่เพิ่ม policy complexity. |
| Working-hours validation. | Needs Decision | เหมาะกับองค์กรบางแบบ แต่ไม่ควร assume. |
| Participant count cannot exceed room capacity. | Optional | มีประโยชน์แต่ขึ้นกับข้อมูล participant ที่เลือกใช้. |
| No-show handling. | Optional | เพิ่ม operational value แต่เพิ่ม status และ rules. |

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
- Multi-tenant or multi-branch design would change the data model deeply.
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
