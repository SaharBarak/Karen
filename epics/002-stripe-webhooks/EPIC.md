# Epic 002: Stripe Webhook Implementation

**Priority:** P0 - CRITICAL (Broken Billing)  
**Status:** Draft (Pending Approval)  
**Estimated Duration:** 1-2 days  

---

## Problem Statement

Stripe webhook handlers are **stub implementations**. Payments are received but subscriptions are never activated in the database. Users pay but get no access.

**Location:** `KarenCLI/packages/karen-backend/src/stripe/stripe.service.ts`  
**Lines:** 79-92  
**Current broken code:**
```typescript
private async handleCheckoutCompleted(session: any): Promise<void> {
  // TODO: Update subscription in database
  console.log('Checkout completed:', session.id);
}
```

---

## Scope

### In Scope
- [ ] Implement `checkout.session.completed` - activate subscription
- [ ] Implement `customer.subscription.updated` - sync status changes
- [ ] Implement `customer.subscription.deleted` - handle cancellations
- [ ] Add `invoice.payment_failed` handler - dunning flow
- [ ] Add `invoice.paid` handler - reset usage counters

### Out of Scope
- Stripe checkout page customization
- Proration handling for plan changes
- Refund automation

---

## Technical Approach

### Files to Modify
```
KarenCLI/packages/karen-backend/src/stripe/
├── stripe.service.ts    # Implement webhook handlers
└── stripe.controller.ts # Verify no changes needed
```

### Webhook Implementation Details

**checkout.session.completed:**
- Extract `customer`, `subscription`, `metadata` from session
- Create/update user subscription in Supabase
- Set: `stripe_customer_id`, `stripe_subscription_id`, `plan`, `status`
- Reset usage counters

**customer.subscription.updated:**
- Update `current_period_start`, `current_period_end`
- Update `status` and `plan`

**customer.subscription.deleted:**
- Downgrade to free tier
- Clear Stripe IDs
- Send notification (future)

### Environment Variables Required
```
STRIPE_SECRET_KEY=sk_live_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx
```

---

## Tasks

| # | Task | Assignee | Status |
|---|------|----------|--------|
| 1 | Implement checkout.session.completed handler | TBD | Not Started |
| 2 | Implement subscription.updated handler | TBD | Not Started |
| 3 | Implement subscription.deleted handler | TBD | Not Started |
| 4 | Add invoice.payment_failed handler | TBD | Not Started |
| 5 | Add invoice.paid handler | TBD | Not Started |
| 6 | Add signature verification tests | TBD | Not Started |
| 7 | Integration test with Stripe CLI | TBD | Not Started |

---

## Definition of Done

- [ ] Successful checkout creates subscription record
- [ ] Plan upgrades/downgrades update database
- [ ] Cancellations downgrade to free tier
- [ ] Failed payments trigger appropriate status update
- [ ] All webhook handlers have unit tests
- [ ] Tested with `stripe trigger` CLI commands

---

## Spec Reference

`specs/billing.md` lines 76-124

---

## Dependencies

**Blocked by:** Epic 001 (Auth System) - needs auth for portal endpoints  
**Blocks:** Production billing, subscription page
