# 2026-07-01 Docs Language Split

## Summary

ปรับ documentation structure ให้ public/canonical docs เป็น English-first และแยก
Thai companion docs เป็น `.th.md` เพื่อให้ repo อ่านง่ายในระดับสากล แต่ยังเก็บ
learning context ภาษาไทยไว้ครบ.

## Interesting Topics

- `README.md` เป็นหน้าแรกของ repo จึงควรเป็น English canonical.
- `CONTEXT.md` เป็น domain glossary ที่ agent และ future contributors ต้องอ่าน
  จึงควรเป็น English canonical เพื่อให้ terminology นิ่ง.
- Thai companion files ยังสำคัญ เพราะโปรเจคนี้มี learning goal เป็นภาษาไทย.

## Decisions

- `README.md`, `CONTEXT.md`, และ `docs/project-brief.md` เป็น English canonical.
- เพิ่ม `README.th.md`, `CONTEXT.th.md`, และ `docs/project-brief.th.md` เป็น Thai
  companion docs.
- `AGENTS.md` ระบุ policy ว่า public/canonical docs ใช้ English, Thai companion
  ใช้ `.th.md`, ADRs ใช้ English, session logs ใช้ Thai-first.
- ไม่สร้าง ADR สำหรับ language split รอบนี้ เพราะเป็น documentation policy ที่
  reversible ได้และไม่ใช่ architectural lock-in.

## Alternatives Considered

- เก็บไฟล์หลักเป็น Thai ต่อไป: อ่านง่ายสำหรับ learning แต่ไม่เหมาะกับ repo entry
  point แบบสากล.
- ทำทุกไฟล์ bilingual ในไฟล์เดียว: ลดจำนวนไฟล์ แต่ทำให้ canonical source ยาวและ
  เสี่ยง drift ภายในไฟล์เดียว.
- แยกทุกไฟล์ทุกประเภทเป็น English/Thai: เป็นระบบที่สุด แต่เกินจำเป็นสำหรับ ADR
  และ session logs.

## Changes Made

- Rewrote `README.md` in English.
- Created `README.th.md`.
- Rewrote `CONTEXT.md` in English.
- Created `CONTEXT.th.md`.
- Rewrote `docs/project-brief.md` in English.
- Created `docs/project-brief.th.md`.
- Updated `AGENTS.md` language and canonical documentation rules.
- Created this session log.

## Desired Outcome

Future readers should see English as the canonical public documentation while
the human developer still has Thai learning material available beside it.

## Alignment Check

This keeps RoomFlow practical and agent-friendly without changing product scope,
tech stack, database, ORM, Docker, auth, or deployment assumptions.

## Open Questions

- Future diagrams should use English canonical labels only, or include Thai
  companion diagrams?
- Should future PRD/spec docs follow the same `.th.md` companion pattern?

## Next State

The next planning round can choose implementation foundation while following the
new docs language policy.
