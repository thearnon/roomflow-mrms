# 2026-07-02 Solo Agent Workflow and Role Hats

## Summary

ขยายแนวทางการทำงานแบบ solo developer + agent สำหรับ RoomFlow ตั้งแต่ได้รับ
requirement คร่าว ๆ จากห้องประชุม ไปจนถึงการจัดลำดับ discovery, shaping,
design, implementation, testing, review, และ session memory.

## Interesting Topics

- ใช้ `Role Hat` เป็นมุมคิดและ checklist ไม่ใช่โครงสร้าง folder หรือทีมจริง
  ในโปรเจกต์.
- ใช้ feature slice เป็นหน่วยงานหลัก เช่น room discovery, create booking,
  conflict detection, admin room management, และ reporting.
- ใช้ `Shape -> Ready -> Build -> Review -> Done` เป็น workflow เบา ๆ สำหรับ
  solo work และ agentic workflow.
- แยก `User Role` ของระบบ เช่น Requester หรือ Department Admin ออกจาก
  `Role Hat` ของคนทำงาน เช่น BA, UX, SA, Dev, QA.

## Decisions

- ยังไม่เปลี่ยน project scope, architecture, stack, database, ORM, auth, หรือ
  deployment.
- ใช้แนวทางเบาไว้ก่อน: session logs และ existing canonical docs เป็น memory
  หลัก แทนการเพิ่ม docs tree ขนาดใหญ่ตั้งแต่ตอนนี้.

## Alternatives Considered

- แยกงานตาม role เต็มรูปแบบ: ไม่เลือก เพราะทำให้ context กระจายและหนักเกิน
  สำหรับ solo developer.
- เพิ่ม `project-state.md` ทันที: ยังไม่เลือก เพราะ repository มี README,
  project brief, ADRs, และ session logs เป็น project memory อยู่แล้ว.
- ทำ docs tree เต็มแบบ discovery/requirements/ux/architecture/testing:
  ยังไม่เลือก เพราะโปรเจกต์ยังอยู่ documentation-foundation stage.

## Changes Made

- Created this session log.

## Desired Outcome

ให้ human และ future agents มี working model ที่ชัดขึ้นว่าเมื่อได้ requirement
คร่าว ๆ แล้วควรจัดระเบียบงานอย่างไรโดยไม่รีบ scaffold code หรือ over-engineer
workflow.

## Alignment Check

แนวทางนี้สอดคล้องกับ RoomFlow v1 เพราะช่วยคุม scope ให้อยู่ระดับ department
internal system, สนับสนุน learning goal, และยังไม่เพิ่ม technical assumption
ก่อนการตัดสินใจ.

## Open Questions

- จะเลือก auth style, role model, participant model, booking UX flow, stack,
  database, ORM, testing approach, และ Docker policy อย่างไร.
- จะเพิ่มเอกสาร workflow ถาวร เช่น `docs/planning/solo-agent-workflow.md`
  หรือใช้ session logs ต่อไปก่อน.

## Next State

Project remains in documentation-foundation stage. Next useful step is to walk
through the end-to-end solo workflow, then decide the first implementation
foundation before scaffolding code.
