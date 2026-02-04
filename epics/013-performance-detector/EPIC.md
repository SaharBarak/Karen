# Epic 013: Enable Performance Detector

**Priority:** P1 - HIGH  
**Status:** Draft (Pending Approval)  
**Estimated Duration:** 0.5 days  

---

## Problem Statement

The performance detector **exists but is never called**. Core Web Vitals (LCP, CLS, FID) are never analyzed despite the detector being fully implemented.

**Location:** `karen-cli/src/core/audit-engine.ts`  
**Issue:** `detectPerformance` is imported but never invoked

---

## Scope

### In Scope
- [ ] Add performance detector call in audit-engine.ts
- [ ] Verify detector works correctly
- [ ] Add to default feature set

### Out of Scope
- Performance detector implementation (already done)
- New performance metrics

---

## Technical Approach

### File to Modify
`karen-cli/src/core/audit-engine.ts` around line 75

### Code to Add
```typescript
// Add with other detector calls:
if (this.config.features.includes('performance')) {
  allIssues.push(...detectPerformance(snapshots, this.config));
}
```

### Detector Already Implemented
- File: `karen-cli/src/detectors/performance.ts` (342 lines)
- Metrics: LCP, CLS, FID analysis
- Spec: `specs/core-detectors.md` lines 206-237

---

## Tasks

| # | Task | Assignee | Status |
|---|------|----------|--------|
| 1 | Add detectPerformance call in audit-engine.ts | TBD | Not Started |
| 2 | Add 'performance' to default features | TBD | Not Started |
| 3 | Verify detector output format | TBD | Not Started |
| 4 | Test with sample sites | TBD | Not Started |

---

## Definition of Done

- [ ] Performance detector called during audits
- [ ] Core Web Vitals issues appear in results
- [ ] No breaking changes to existing output

---

## Spec Reference

`specs/core-detectors.md` lines 206-237
