# RoomFlow

RoomFlow คือโปรเจค Meeting Room Management System สำหรับเรียนรู้และสร้างระบบ
จองห้องประชุมแบบ practical internal web application.

โปรเจคนี้ถูกกำหนดให้เป็น **Level 2: Department Internal System** หมายความว่า
ระบบควรพอจริงจังสำหรับใช้งานภายในแผนก แต่ยังไม่ควรขยายเป็น enterprise platform
ขนาดใหญ่ตั้งแต่ v1.

English version: [README.md](README.md)

## Purpose

เป้าหมายของ RoomFlow ไม่ใช่แค่สร้างแอปที่จองห้องได้ แต่ต้องช่วยให้ผู้พัฒนาเข้าใจ
พื้นฐานสำคัญของระบบจองทรัพยากรร่วม:

- core domain ของ meeting room booking system
- room availability และ conflict detection
- ความสัมพันธ์ของ users, rooms, bookings, participants
- business rules และ validation
- database schema ที่ดูแลง่าย
- UX สำหรับ internal enterprise users
- การแยก frontend, backend, database, validation, business logic
- testing ตั้งแต่ช่วงต้น
- agentic AI workflow ที่มี human review และ decision records

## Current Status

ตอนนี้ repository อยู่ในช่วง documentation foundation:

- ยังไม่เลือก frontend/backend stack
- ยังไม่เลือก database หรือ ORM
- ยังไม่ scaffold application code
- ยังไม่กำหนด package manager หรือ development commands
- เริ่มตั้ง project memory ผ่าน `CONTEXT.md`, `docs/adr/`, และ
  `docs/session-log/`

## V1 Direction

RoomFlow v1 ควรเริ่มจาก department-level scope:

- mock authentication หรือ simple login
- simple roles
- room list และ room detail
- room capacity และ equipment
- create, edit, cancel booking
- booking purpose และ participants
- booking status
- availability และ overlapping booking conflict detection
- basic admin room management
- basic notification placeholder
- basic dashboard/reporting
- audit-friendly structure สำหรับ action สำคัญ

## Out of Scope for V1

ฟีเจอร์เหล่านี้ไม่ควรเข้ามาใน v1 เว้นแต่มีการตัดสินใจใหม่อย่างชัดเจน:

- real SSO
- Microsoft Outlook หรือ Google Calendar integration
- recurring booking engine
- complex approval workflow
- multi-tenant architecture
- multi-branch organization support
- IoT, QR check-in, door access
- advanced analytics
- payment หรือ billing

## Documentation Map

- `AGENTS.md`: instruction หลักสำหรับ AI coding agents
- `CONTEXT.md`: canonical English domain glossary
- `CONTEXT.th.md`: glossary เวอร์ชั่นไทย
- `docs/project-brief.md`: canonical English project analysis และ scope
- `docs/project-brief.th.md`: project brief เวอร์ชั่นไทย
- `docs/adr/`: architectural decision records
- `docs/session-log/`: session state และ summary รายรอบ
- `CLAUDE.md`: Claude Code entry point ที่ import `AGENTS.md`

## License

RoomFlow ใช้ [MIT License](LICENSE) สามารถนำไปใช้ copy, modify, distribute,
หรือ build ต่อได้ โดยต้องคง license notice ไว้ตามเงื่อนไขของ MIT License.

## Next Decisions

ก่อน scaffold โค้ด ควรตัดสินใจเรื่องเหล่านี้:

- v1 จะใช้ mock authentication หรือ simple authentication
- role model จะใช้ 2 roles หรือ 3 roles
- participant จะเป็น text/email records หรือ linked users
- booking UI จะเริ่มจาก room-first, time-first, calendar, หรือ form-based flow
- stack, database, ORM, testing approach, และ Docker policy
