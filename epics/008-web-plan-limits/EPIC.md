# Epic 008: Fix Web Dashboard Plan Limits

**Priority:** P1 - HIGH  
**Status:** Draft (Pending Approval)  
**Estimated Duration:** 1 day  

---

## Problem Statement

The web dashboard has **wrong plan limits** and is missing tier definitions. Users get incorrect quotas.

**Location:** `karen-cli-web/app/api/subscriptions/usage/route.ts` lines 4-8

**Current (WRONG):**
```typescript
const PLAN_LIMITS = {
  free: 3,        // WRONG - should be 5
  managed: 1,     // NOT IN SPEC - remove
  pro: -1,        // WRONG - should be 100
}
```

**Spec says (`specs/billing.md` lines 7-12):**
```typescript
const PLAN_LIMITS = {
  free: 5,        // 5 audits/month
  pro: 100,       // 100 audits/month
  team: 500,      // 500 audits/month
  enterprise: -1, // unlimited
}
```

---

## Scope

### In Scope
- [ ] Fix plan limit values to match spec
- [ ] Add missing tiers (team, enterprise)
- [ ] Remove non-spec tier (managed)
- [ ] Update usage display logic

### Out of Scope
- Stripe plan creation
- Plan upgrade/downgrade flow

---

## Technical Approach

### File to Modify
`karen-cli-web/app/api/subscriptions/usage/route.ts`

### Change Required
```typescript
// REPLACE lines 4-8 with:
const PLAN_LIMITS: Record<string, number> = {
  free: 5,
  pro: 100,
  team: 500,
  enterprise: -1, // unlimited
};
```

### Validation
- [ ] Verify free users see 5 audit limit
- [ ] Verify pro users see 100 audit limit
- [ ] Verify team users see 500 audit limit
- [ ] Verify enterprise users see "unlimited"

---

## Tasks

| # | Task | Assignee | Status |
|---|------|----------|--------|
| 1 | Update PLAN_LIMITS constant | TBD | Not Started |
| 2 | Test with each plan type | TBD | Not Started |
| 3 | Update any UI showing limits | TBD | Not Started |

---

## Definition of Done

- [ ] Plan limits match spec exactly
- [ ] All 4 tiers defined
- [ ] No invalid "managed" tier
- [ ] UI displays correct limits per plan

---

## Spec Reference

`specs/billing.md` lines 7-12

---

## Dependencies

**Blocked by:** None  
**Blocks:** Accurate billing UX
