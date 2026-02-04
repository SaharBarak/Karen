# Epic 009: CLI Watch Mode

**Priority:** P1 - HIGH  
**Status:** Draft (Pending Approval)  
**Estimated Duration:** 3-4 days  

---

## Problem Statement

The entire watch mode spec is **not implemented**. Developers cannot get live feedback while editing CSS — they must manually re-run audits.

**Spec:** `specs/watch-mode.md` (entire spec unimplemented)

---

## Scope

### In Scope
- [ ] File watcher with chokidar
- [ ] Debounced re-auditing (500ms default)
- [ ] Terminal UI with Ink (React for CLI)
- [ ] Keyboard shortcuts (q/r/c/o/h)
- [ ] Browser overlay for visual feedback
- [ ] `--watch` flag for audit command

### Out of Scope
- IDE integrations
- Browser extension

---

## Technical Approach

### Files to Create
```
karen-cli/src/
├── commands/watch.ts       # Watch command
├── watch/
│   ├── watcher.ts          # Chokidar file watcher
│   ├── ui.tsx              # Ink terminal UI
│   ├── overlay-server.ts   # WebSocket server for browser
│   └── keyboard.ts         # Keyboard shortcuts
```

### File Watcher
- Use `chokidar` for cross-platform watching
- Debounce: 500ms (configurable)
- Incremental analysis (only changed files)
- Respect `.karenignore` patterns

### Terminal UI (Ink)
- Current issue count with severity breakdown
- Last audit timestamp
- Files being watched
- Progress indicator during re-audit

### Keyboard Shortcuts
- `q` - quit watch mode
- `r` - force full re-audit
- `c` - clear terminal
- `o` - open browser overlay
- `h` - show help

### Browser Overlay
- WebSocket server on configurable port
- Inject overlay script into target page
- Highlight issue locations visually

---

## Tasks

| # | Task | Assignee | Status |
|---|------|----------|--------|
| 1 | Create watch command skeleton | TBD | Not Started |
| 2 | Implement chokidar file watcher | TBD | Not Started |
| 3 | Add debouncing logic | TBD | Not Started |
| 4 | Build Ink terminal UI | TBD | Not Started |
| 5 | Implement keyboard shortcuts | TBD | Not Started |
| 6 | Create WebSocket overlay server | TBD | Not Started |
| 7 | Add `--watch` flag to audit command | TBD | Not Started |
| 8 | Tests | TBD | Not Started |

---

## Definition of Done

- [ ] `karen audit --watch` starts watching
- [ ] File changes trigger re-audit
- [ ] Terminal shows live issue updates
- [ ] Keyboard shortcuts work
- [ ] Browser overlay highlights issues
- [ ] Tests pass

---

## Spec Reference

`specs/watch-mode.md` (entire spec)

---

## Dependencies

**Blocked by:** None  
**Blocks:** Developer workflow feature
