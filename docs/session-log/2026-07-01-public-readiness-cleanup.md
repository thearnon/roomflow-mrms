# 2026-07-01 Public Readiness Cleanup

## Summary

เตรียม repo ให้เหมาะกับ first public commit มากขึ้น โดยแยก local agent runtime
files ออกจาก source ที่ควรเผยแพร่ และลบ config ที่อ้าง command ซึ่งยังไม่มีจริง.

## Interesting Topics

- `.claude/launch.json` เรียก `npm run dev` แต่โปรเจคยังไม่มี `package.json` หรือ
  dev script จึงทำให้ future reader เข้าใจผิดได้.
- `.agents/skills/` และ `.claude/skills/` เป็น local agent tooling จำนวนมาก ไม่ใช่
  core RoomFlow product source สำหรับ public first commit.
- `AGENTS.md` และ `CLAUDE.md` ควรสะท้อน repo reality มากกว่า local runtime state.

## Decisions

- เพิ่ม `.gitignore` เพื่อกัน secrets, dependencies, build output, logs, IDE
  files, และ local agent runtime directories.
- ลบ `.claude/launch.json` เพราะยังไม่มี app dev command ให้รัน.
- ปรับ `AGENTS.md` ให้ระบุว่า `.agents/skills/` และ `.claude/skills/` เป็น
  local-only by default.
- ปรับ `CLAUDE.md` ให้เหลือหน้าที่ import `AGENTS.md` และอธิบายว่า `.claude/`
  เป็น local runtime area.
- Initialize Git repository metadata with `git init -b main`.

## Alternatives Considered

- เก็บ `.claude/launch.json` ไว้: ไม่เหมาะ เพราะอ้าง `npm run dev` ก่อนมี
  package scripts จริง.
- Commit `.agents/skills/` และ `.claude/skills/`: ไม่เหมาะกับ public first commit
  เพราะใหญ่และไม่ใช่ project product docs.
- ลบ skill directories จริงจากเครื่อง: ไม่จำเป็น เพราะ ignore ก็เพียงพอสำหรับ
  public repo และยังเก็บ local tooling ไว้ใช้งานได้.

## Changes Made

- Created `.gitignore`.
- Deleted `.claude/launch.json`.
- Updated `AGENTS.md`.
- Updated `CLAUDE.md`.
- Initialized Git repository on branch `main`.
- Created this session log.

## Desired Outcome

First commit should contain RoomFlow project docs and agent instructions without
publishing local runtime bundles or stale dev configuration.

## Alignment Check

This keeps the project documentation-first and does not introduce framework,
package manager, database, ORM, Docker, auth, or deployment assumptions.

## Open Questions

- Should the public repository use MIT license, another license, or no license
  for now?

## Next State

Choose license policy, then create the first commit when ready.
