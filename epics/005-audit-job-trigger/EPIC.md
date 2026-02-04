# Epic 005: Audit Job Trigger

**Priority:** P0 - CRITICAL (Broken Core Feature)  
**Status:** Draft (Pending Approval)  
**Estimated Duration:** 2 days  

---

## Problem Statement

The web dashboard **creates audit records but never executes them**. Users click "Run Audit" and see a pending status forever.

**Location:** `karen-cli-web/app/api/audits/create/route.ts`  
**Line:** 64  
**Current broken code:**
```typescript
// TODO: Trigger actual audit job here
```

The database record is created with "pending" status, but no audit ever runs.

---

## Scope

### In Scope
- [ ] Connect audit creation to backend job queue
- [ ] Implement WebSocket/SSE for progress updates
- [ ] Update UI to show real-time progress
- [ ] Handle failures with retry option

### Out of Scope
- Job queue implementation (Epic 006 - P1)
- CLI changes

---

## Technical Approach

### Option A: Backend Job Queue (Recommended)
- Call backend `/audits/trigger` endpoint
- Requires job queue infrastructure (P1-BE-1)
- Best for production reliability

### Option B: Direct CLI Invocation (Temporary)
- Use child_process to invoke CLI
- Add timeout handling
- Simpler but less scalable

### Implementation (Option B for now, upgrade to A later)

**File:** `karen-cli-web/app/api/audits/create/route.ts`

```typescript
// After creating audit record (line 64):
import { spawn } from 'child_process';

// Trigger CLI audit
const process = spawn('karen', ['audit', targetUrl, '--json']);

// Update status on completion
process.on('close', async (code) => {
  await supabase.from('audits').update({
    status: code === 0 ? 'completed' : 'failed',
    results: parsedOutput
  }).eq('id', auditId);
});
```

### Progress Updates
- Use Server-Sent Events for real-time progress
- Or polling with `/api/audits/[id]/status` endpoint

---

## Tasks

| # | Task | Assignee | Status |
|---|------|----------|--------|
| 1 | Implement CLI invocation in create route | TBD | Not Started |
| 2 | Add timeout handling (5 min default) | TBD | Not Started |
| 3 | Create status polling endpoint | TBD | Not Started |
| 4 | Update dashboard UI for progress | TBD | Not Started |
| 5 | Add retry button for failed audits | TBD | Not Started |
| 6 | Test with various target URLs | TBD | Not Started |

---

## Definition of Done

- [ ] Clicking "Run Audit" actually executes an audit
- [ ] Progress visible in UI (polling or SSE)
- [ ] Completed audits show results
- [ ] Failed audits show error + retry option
- [ ] Timeout after 5 minutes with graceful failure

---

## Spec Reference

`specs/backend-api.md` - Audit triggering flow

---

## Dependencies

**Blocked by:** None (can use temporary CLI approach)  
**Blocks:** Dashboard usability  
**Future upgrade:** Epic 006 (Job Queue) will improve reliability
