# CLAUDE.md

@AGENTS.md

## Claude Code

Claude Code reads `CLAUDE.md` at session start and expands the `@AGENTS.md`
import, so project instructions stay in one canonical source of truth.

Local Claude Code runtime files may exist under `.claude/`, but they are
developer-local by default and are ignored for the public repository until the
project explicitly decides to publish Claude-specific configuration.
