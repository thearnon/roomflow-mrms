# RoomFlow Context

Domain glossary สำหรับ RoomFlow meeting room management system. ไฟล์นี้นิยาม
คำศัพท์ของ domain เท่านั้น; implementation choices ควรอยู่ใน planning docs หรือ
ADRs.

English version: [CONTEXT.md](CONTEXT.md)

## System Scope

**RoomFlow**:
ระบบจัดการและจองห้องประชุมสำหรับใช้งานภายในแผนก โดยเน้น room availability,
booking lifecycle, และ conflict detection.
_Avoid_: generic calendar app, enterprise facility platform

**Department Internal System**:
ระบบภายในระดับแผนกที่มี scope พอสำหรับงานจริง แต่ยังไม่รวม enterprise
integration หรือ multi-tenant complexity.
_Avoid_: enterprise platform, multi-branch system

## Booking Domain

**Resource**:
สิ่งที่ถูกใช้งานร่วมกันและต้องถูกจองก่อนใช้งาน ใน RoomFlow resource หลักคือ
meeting room.
_Avoid_: asset, facility item

**Room**:
ห้องประชุมที่ผู้ใช้สามารถดูข้อมูลและจองได้ถ้าห้อง active และว่างในเวลาที่ต้องการ.
_Avoid_: venue, location

**Room Equipment**:
อุปกรณ์หรือความสามารถของห้องที่ช่วยให้ผู้ใช้เลือกห้องได้เหมาะกับการประชุม.
_Avoid_: amenities, room features

**Time Slot**:
ช่วงเวลาของ booking ที่มี start time และ end time ชัดเจน.
_Avoid_: schedule block, calendar range

**Booking**:
การจองห้องหนึ่งห้องใน time slot หนึ่ง พร้อม purpose, owner, participants, และ
status.
_Avoid_: reservation, appointment

**Booking Purpose**:
เหตุผลหรือหัวข้อของการจองที่ช่วยให้ booking มี context สำหรับผู้จองและผู้ดูแล.
_Avoid_: title only, description only

**Participant**:
บุคคลที่ถูกระบุว่าเกี่ยวข้องกับ booking อาจเป็น linked user หรือ text/email
record ตาม decision ภายหลัง.
_Avoid_: attendee

**Availability**:
สถานะว่าห้องสามารถถูกจองได้ใน time slot ที่ต้องการหรือไม่ โดยพิจารณาจาก room
state และ existing bookings.
_Avoid_: free time

**Conflict Detection**:
กฎที่ตรวจว่าการจองใหม่ทับซ้อนกับ booking เดิมของห้องเดียวกันหรือไม่.
_Avoid_: collision check, schedule validation

## Users and Roles

**User**:
บุคคลที่ใช้งานระบบ RoomFlow เพื่อดูห้อง จองห้อง หรือดูแลข้อมูลตามสิทธิ์.
_Avoid_: account, employee

**Requester**:
ผู้ใช้ทั่วไปที่สามารถดูห้อง เช็ก availability สร้าง booking และแก้ไขหรือยกเลิก
booking ของตนเอง.
_Avoid_: normal user, booker

**Department Admin**:
ผู้ดูแลระดับแผนกที่จัดการ room master data และดูแล booking ภายใน scope ของแผนก.
_Avoid_: room manager, local admin

**System Admin**:
ผู้ดูแลระบบที่จัดการ users, roles, rooms, และ system settings.
_Avoid_: superuser

## Lifecycle and Records

**Booking Status**:
สถานะ lifecycle ของ booking เช่น `BOOKED`, `CANCELLED`, และ `COMPLETED`.
_Avoid_: state when referring only to booking lifecycle

**Cancelled Booking**:
booking ที่ถูกยกเลิกและไม่ควรนับเป็น conflict สำหรับการจองใหม่.
_Avoid_: deleted booking

**Notification Placeholder**:
record หรือ simulation สำหรับแสดงว่า notification ควรเกิดขึ้น โดยยังไม่ผูกกับ
email provider จริงใน v1.
_Avoid_: real email integration

**Audit Log**:
record ของ action สำคัญ เช่น booking created, updated, หรือ cancelled เพื่อช่วย
ให้ระบบตรวจสอบย้อนหลังได้.
_Avoid_: debug log, application log
