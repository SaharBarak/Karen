# Epic 010: GitHub Webhook Implementation

**Priority:** P1 - HIGH  
**Status:** Draft (Pending Approval)  
**Estimated Duration:** 2-3 days  

---

## Problem Statement

GitHub webhook handler is a **stub** — webhooks are received but not processed. This blocks CI/CD integration and PR comments.

**Location:** `KarenCLI/packages/karen-backend/src/github/github.controller.ts` lines 117-129

**Current (stub):**
```typescript
@Post('webhook')
async handleWebhook(@Body() payload: any) {
  // TODO: Verify signature and process webhook
  return { success: true, message: 'Webhook received (not yet implemented)' };
}
```

---

## Scope

### In Scope
- [ ] Webhook signature verification (HMAC SHA-256)
- [ ] Handle `push` events → trigger audit on default branch
- [ ] Handle `pull_request` events → trigger PR audit
- [ ] Handle `installation` events → app install/removal
- [ ] Post PR comments with audit results
- [ ] Update commit status (pending/success/failure)

### Out of Scope
- GitHub App marketplace listing
- GitHub Actions integration
- Check Runs API (using simpler commit status)

---

## Technical Approach

### Signature Verification
```typescript
import * as crypto from 'crypto';

function verifySignature(payload: string, signature: string, secret: string): boolean {
  const expected = 'sha256=' + crypto
    .createHmac('sha256', secret)
    .update(payload)
    .digest('hex');
  return crypto.timingSafeEqual(Buffer.from(signature), Buffer.from(expected));
}
```

### Event Routing
```typescript
switch (event) {
  case 'push':
    // Trigger audit if default branch
    break;
  case 'pull_request':
    // Trigger audit, post comment, set status
    break;
  case 'installation':
    // Handle app install/uninstall
    break;
}
```

### PR Comment Format
```markdown
## 🔍 Karen CSS Audit Results

| Severity | Count |
|----------|-------|
| 🔴 Error | 3 |
| 🟡 Warning | 7 |
| 🔵 Info | 2 |

[View Full Report](https://karen.dev/audits/xxx)
```

---

## Tasks

| # | Task | Assignee | Status |
|---|------|----------|--------|
| 1 | Implement signature verification | TBD | Not Started |
| 2 | Parse and route webhook events | TBD | Not Started |
| 3 | Handle push events | TBD | Not Started |
| 4 | Handle pull_request events | TBD | Not Started |
| 5 | Handle installation events | TBD | Not Started |
| 6 | Implement PR comment posting | TBD | Not Started |
| 7 | Implement commit status updates | TBD | Not Started |
| 8 | Tests with mock payloads | TBD | Not Started |

---

## Definition of Done

- [ ] Webhook signature verified before processing
- [ ] Push to main triggers audit
- [ ] PR open/sync triggers audit + comment
- [ ] Commit status reflects audit result
- [ ] Tests pass with sample GitHub payloads

---

## Spec Reference

`specs/github-integration.md` lines 89-134

---

## Dependencies

**Blocked by:** Epic 003 (GitHub Security) ✅ merged  
**Blocks:** CI/CD integration
