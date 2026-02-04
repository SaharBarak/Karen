# Epic 001: Authentication System

**Priority:** P0 - CRITICAL SECURITY  
**Status:** Draft (Pending Approval)  
**Estimated Duration:** 2-3 days  

---

## Problem Statement

The backend has a **critical security vulnerability**: all protected routes fall back to a hardcoded `test-user-id` when no authentication is provided. This means any unauthenticated request gets access to test user data.

**Location:** `KarenCLI/packages/karen-backend/src/audits/audits.controller.ts`  
**Lines:** 27, 49, 63  
**Current vulnerable code:**
```typescript
const userId = req.user?.id || 'test-user-id';
```

---

## Scope

### In Scope
- [ ] Create NestJS authentication module with JWT strategy
- [ ] Remove hardcoded `test-user-id` fallback from all controllers
- [ ] Implement JWT guard for protected routes
- [ ] Add login/register endpoints
- [ ] Proper 401 Unauthorized responses for unauthenticated requests

### Out of Scope
- OAuth providers (GitHub OAuth is separate epic)
- Password reset flow
- Email verification

---

## Technical Approach

### Files to Create
```
KarenCLI/packages/karen-backend/src/auth/
├── auth.module.ts           # NestJS module definition
├── auth.controller.ts       # Login/logout/register endpoints
├── auth.service.ts          # Token generation/validation
├── guards/jwt.guard.ts      # Route protection guard
├── strategies/jwt.strategy.ts # Passport JWT strategy
└── dto/
    ├── login.dto.ts         # Login request validation
    └── register.dto.ts      # Registration validation
```

### Files to Modify
- `audits.controller.ts` - Remove test-user-id, add @UseGuards(JwtAuthGuard)
- `github.controller.ts` - Add guards (except webhook endpoint)
- `stripe.controller.ts` - Add guards (checkout/portal methods)
- `app.module.ts` - Import AuthModule

### Environment Variables Required
```
JWT_SECRET=<min 32 characters>
JWT_EXPIRY=7d
```

---

## Tasks

| # | Task | Assignee | Status |
|---|------|----------|--------|
| 1 | Create auth module skeleton | TBD | Not Started |
| 2 | Implement JWT strategy with Passport | TBD | Not Started |
| 3 | Create JwtAuthGuard | TBD | Not Started |
| 4 | Add login/register endpoints | TBD | Not Started |
| 5 | Apply guards to audits.controller.ts | TBD | Not Started |
| 6 | Apply guards to github.controller.ts | TBD | Not Started |
| 7 | Apply guards to stripe.controller.ts | TBD | Not Started |
| 8 | Remove all test-user-id fallbacks | TBD | Not Started |
| 9 | Add unit tests for auth service | TBD | Not Started |
| 10 | Integration test: unauthorized access returns 401 | TBD | Not Started |

---

## Definition of Done

- [ ] No routes use hardcoded user IDs
- [ ] All protected routes require valid JWT
- [ ] Unauthenticated requests return 401 Unauthorized
- [ ] Unit tests for auth service pass
- [ ] Integration test confirms auth bypass is fixed
- [ ] Code review approved

---

## Spec Reference

`specs/backend-api.md` lines 111-173

---

## Dependencies

**Blocks:**
- Epic: Stripe Webhooks (needs auth for portal)
- Epic: GitHub Integration (needs auth for token storage)
- All cloud features

**Blocked by:** None - can start immediately
