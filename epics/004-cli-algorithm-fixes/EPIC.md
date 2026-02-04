# Epic 004: CLI Algorithm Bug Fixes

**Priority:** P0 - HIGH  
**Status:** Draft (Pending Approval)  
**Estimated Duration:** 1 day  

---

## Problem Statement

Two detector algorithms have bugs causing incorrect audit results:

### Bug 1: Color Threshold Too Permissive
- **Location:** `karen-cli/src/detectors/colors.ts` line 77
- **Spec says:** deltaE < 5 for near-duplicates
- **Code says:** deltaE < 15
- **Impact:** Similar colors not being flagged (3x too permissive)

### Bug 2: Spacing Tolerance Too Strict
- **Location:** `karen-cli/src/detectors/spacing.ts` line 15
- **Spec says:** Within 2px = PASS
- **Code says:** < 1px = PASS
- **Impact:** False positives - valid spacing flagged as issues

---

## Scope

### In Scope
- [ ] Fix color deltaE threshold: 15 → 5
- [ ] Fix spacing tolerance: < 1 → <= 2
- [ ] Add regression tests for both fixes

### Out of Scope
- Other detector improvements
- Performance optimizations

---

## Technical Approach

### Color Fix (colors.ts line 77)
```typescript
// Current (WRONG):
if (distance > 0 && distance < 15)

// Fix:
if (distance > 0 && distance < 5)
```

### Spacing Fix (spacing.ts line 15)
```typescript
// Current (WRONG):
return scale.some((scaleValue) => Math.abs(value - scaleValue) < 1);

// Fix:
return scale.some((scaleValue) => Math.abs(value - scaleValue) <= 2);
```

---

## Tasks

| # | Task | Assignee | Status |
|---|------|----------|--------|
| 1 | Fix color deltaE threshold | TBD | Not Started |
| 2 | Add color threshold regression test | TBD | Not Started |
| 3 | Fix spacing tolerance calculation | TBD | Not Started |
| 4 | Add spacing tolerance regression test | TBD | Not Started |
| 5 | Run full test suite | TBD | Not Started |

---

## Definition of Done

- [ ] Color threshold matches spec (deltaE < 5)
- [ ] Spacing tolerance matches spec (<= 2px)
- [ ] Regression tests pass
- [ ] Full test suite passes
- [ ] No new issues introduced

---

## Spec Reference

- Colors: `specs/core-detectors.md` line 142
- Spacing: `specs/core-detectors.md` lines 85-87

---

## Dependencies

**Blocked by:** None  
**Blocks:** Accurate audit results
