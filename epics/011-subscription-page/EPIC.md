# Epic 011: Web Subscription Management Page

**Priority:** P1 - HIGH  
**Status:** Draft (Pending Approval)  
**Estimated Duration:** 2 days  

---

## Problem Statement

The user menu links to a subscription page that **returns 404**. Users cannot manage their billing.

**Missing page:** `karen-cli-web/app/dashboard/subscription/page.tsx`

---

## Scope

### In Scope
- [ ] Create subscription management page
- [ ] Display current plan and features
- [ ] Show usage meters (audits used/remaining)
- [ ] Billing cycle dates
- [ ] Upgrade/downgrade CTAs
- [ ] Link to Stripe billing portal
- [ ] Payment history (last 10 invoices)
- [ ] Cancel subscription option

### Out of Scope
- Plan comparison page
- Proration previews
- Team seat management

---

## Technical Approach

### File to Create
`karen-cli-web/app/dashboard/subscription/page.tsx`

### Page Sections

**1. Current Plan**
```
┌─────────────────────────────┐
│ 🏆 Pro Plan                 │
│ $29/month                   │
│                             │
│ ✓ 100 audits/month          │
│ ✓ AI recommendations        │
│ ✓ GitHub integration        │
│                             │
│ [Upgrade to Team]           │
└─────────────────────────────┘
```

**2. Usage Meter**
```
Audits this month: 47/100
████████████░░░░░░░░ 47%
Resets: Feb 15, 2026
```

**3. Billing Actions**
- "Manage Payment Methods" → Stripe portal
- "View Invoices" → expandable list
- "Cancel Subscription" → confirmation modal

### API Endpoints Needed
- `GET /api/subscriptions/current` - plan details
- `GET /api/subscriptions/usage` - usage stats (exists)
- `GET /api/subscriptions/invoices` - payment history
- `POST /api/subscriptions/portal` - Stripe portal link

---

## Tasks

| # | Task | Assignee | Status |
|---|------|----------|--------|
| 1 | Create page layout and routing | TBD | Not Started |
| 2 | Build current plan display component | TBD | Not Started |
| 3 | Build usage meter component | TBD | Not Started |
| 4 | Implement Stripe portal redirect | TBD | Not Started |
| 5 | Build invoice history component | TBD | Not Started |
| 6 | Add cancel subscription flow | TBD | Not Started |
| 7 | Mobile responsive design | TBD | Not Started |

---

## Definition of Done

- [ ] Page accessible from user menu
- [ ] Current plan displayed correctly
- [ ] Usage meter accurate
- [ ] Stripe portal link works
- [ ] Invoice history loads
- [ ] Cancel flow with confirmation
- [ ] Mobile responsive

---

## Dependencies

**Blocked by:** Epic 002 (Stripe Webhooks) ✅ merged  
**Blocks:** Billing UX
