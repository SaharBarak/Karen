# Epic 003: GitHub Token Security

**Priority:** P0 - SECURITY  
**Status:** Draft (Pending Approval)  
**Estimated Duration:** 1-2 days  

---

## Problem Statement

GitHub OAuth implementation has multiple security vulnerabilities:

1. **Tokens in query strings** - Logged to server logs, browser history, referrer headers
2. **Token displayed in HTML** - Partially shown in OAuth success page
3. **Weak OAuth state** - Uses `Date.now().toString()` which is predictable (CSRF risk)

**Location:** `KarenCLI/packages/karen-backend/src/github/github.controller.ts`

---

## Scope

### In Scope
- [ ] Move token from query string to Authorization header
- [ ] Remove token display from HTML response
- [ ] Fix OAuth state generation with crypto.randomBytes
- [ ] Implement secure token storage (encrypted in database)

### Out of Scope
- GitHub App installation flow
- GitHub webhook implementation (separate epic)
- Rate limiting for GitHub API

---

## Technical Approach

### Security Fixes

**1. Token from Query String → Header (Line 136)**
```typescript
// Current (VULNERABLE):
@Get('repos')
async getUserRepos(@Query('access_token') accessToken: string)

// Fix:
@Get('repos')
async getUserRepos(@Headers('authorization') authHeader: string)
```

**2. OAuth State Generation (Line 24)**
```typescript
// Current (VULNERABLE):
const state = Date.now().toString();

// Fix:
const state = crypto.randomBytes(32).toString('hex');
```

**3. Token Display (Line 70)**
- Store token server-side in session/database
- Return session cookie instead of token in HTML

**4. Encrypted Token Storage**
- Add encryption key to config
- Encrypt tokens before database storage
- Decrypt only when making GitHub API calls

---

## Tasks

| # | Task | Assignee | Status |
|---|------|----------|--------|
| 1 | Replace query token with Authorization header | TBD | Not Started |
| 2 | Fix OAuth state to use crypto.randomBytes | TBD | Not Started |
| 3 | Remove token from HTML response | TBD | Not Started |
| 4 | Implement encrypted token storage | TBD | Not Started |
| 5 | Update frontend to use new auth pattern | TBD | Not Started |
| 6 | Security audit of OAuth flow | TBD | Not Started |

---

## Definition of Done

- [ ] No tokens in URLs or query strings
- [ ] OAuth state is cryptographically random
- [ ] Tokens encrypted at rest in database
- [ ] Security review passed
- [ ] No token exposure in logs

---

## Spec Reference

`specs/github-integration.md` lines 40-51

---

## Dependencies

**Blocked by:** Epic 001 (Auth System)  
**Blocks:** Epic: GitHub Webhooks
