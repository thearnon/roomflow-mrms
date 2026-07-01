# 2026-07-01 MIT License

## Summary

เพิ่ม MIT License ให้ RoomFlow เพื่อให้โปรเจคพร้อม public สำหรับ portfolio,
learning, และ reuse โดยคนที่สนใจนำไปต่อ.

## Interesting Topics

- License เป็น public distribution decision ที่ควรบันทึกไว้ ไม่ใช่แค่เพิ่มไฟล์
  `LICENSE`.
- ใช้ `RoomFlow contributors` เป็น copyright holder เพื่อลดการใส่ข้อมูลส่วนตัว
  ลง public repo.

## Decisions

- RoomFlow ใช้ MIT License.
- บันทึก decision นี้ใน `docs/adr/0003-adopt-mit-license.md`.
- เพิ่ม License section ใน `README.md` และ `README.th.md`.

## Alternatives Considered

- No license: public ได้ แต่ reuse ไม่ชัดเจนและ default เป็น all rights
  reserved.
- MIT License: เหมาะกับ portfolio, learning, และเปิดให้คนอื่นนำไปใช้ต่อได้ง่าย.

## Changes Made

- Created `LICENSE`.
- Updated `README.md`.
- Updated `README.th.md`.
- Created `docs/adr/0003-adopt-mit-license.md`.
- Created this session log.

## Desired Outcome

RoomFlow public repository should clearly communicate that others may use,
modify, distribute, and build on the project under MIT terms.

## Alignment Check

The license choice matches the user's intent to make RoomFlow public, portfolio
ready, and reusable by interested developers.

## Open Questions

- Should the first commit be created now with message
  `docs: initialize RoomFlow project foundation`?
- Should a GitHub remote be created manually first or through GitHub CLI later?

## Next State

Repository is ready for final first-commit verification and then the first
documentation foundation commit.
