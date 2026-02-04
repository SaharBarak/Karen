# Epic 014: Fix Non-functional Dashboard Buttons

**Priority:** P1 - HIGH  
**Status:** Draft (Pending Approval)  
**Estimated Duration:** 2 days  

---

## Problem Statement

Multiple dashboard buttons have **no onClick handlers** — they render but do nothing when clicked.

---

## Scope

### Audit Detail Page (`app/dashboard/audits/[auditId]/page.tsx`)
- [ ] "Export Report" (~line 111) - Generate PDF/JSON download
- [ ] "Re-run Audit" (~line 114) - Call rerun API endpoint
- [ ] "View Screenshot" (~line 238) - Open modal with full screenshot
- [ ] "View Fix" (~line 243) - Show suggested CSS fix
- [ ] "Ignore" (~line 247) - Mark issue as ignored, update DB
- [ ] "View Diff" (~line 291) - Open visual diff component
- [ ] "Approve" (~line 292) - Apply fix and create PR/commit

### Repos Page (`app/dashboard/repos/page.tsx`)
- [ ] "Connect GitHub" (line 69) - Initiate OAuth flow
- [ ] "Delete Repository" - Confirm and remove from monitoring

### Out of Scope
- Visual diff viewer component (separate epic)
- PDF generation library selection

---

## Technical Approach

### Export Report
```typescript
const handleExport = async (format: 'pdf' | 'json') => {
  const response = await fetch(`/api/audits/${auditId}/export?format=${format}`);
  const blob = await response.blob();
  // Trigger download
};
```

### Re-run Audit
```typescript
const handleRerun = async () => {
  await fetch(`/api/audits/${auditId}/rerun`, { method: 'POST' });
  router.refresh();
};
```

### Ignore Issue
```typescript
const handleIgnore = async (issueId: string) => {
  await supabase.from('audit_issues')
    .update({ ignored: true })
    .eq('id', issueId);
};
```

---

## Tasks

| # | Task | Assignee | Status |
|---|------|----------|--------|
| 1 | Implement Export Report (PDF/JSON) | TBD | Not Started |
| 2 | Implement Re-run Audit | TBD | Not Started |
| 3 | Implement View Screenshot modal | TBD | Not Started |
| 4 | Implement View Fix display | TBD | Not Started |
| 5 | Implement Ignore issue | TBD | Not Started |
| 6 | Implement View Diff (basic) | TBD | Not Started |
| 7 | Implement Approve flow | TBD | Not Started |
| 8 | Implement Connect GitHub | TBD | Not Started |
| 9 | Implement Delete Repository | TBD | Not Started |

---

## Definition of Done

- [ ] All listed buttons have working handlers
- [ ] Export downloads file successfully
- [ ] Re-run triggers new audit
- [ ] Ignore persists to database
- [ ] GitHub OAuth flow initiates
- [ ] No console errors on click

---

## Dependencies

**Blocked by:** None  
**Blocks:** Dashboard usability
