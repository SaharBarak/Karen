# Epic 006: CLI Login Command

**Priority:** P1 - HIGH  
**Status:** Draft (Pending Approval)  
**Estimated Duration:** 2-3 days  

---

## Problem Statement

The CLI has **no authentication commands**. Users cannot log in, which blocks all cloud features: sync, team collaboration, and CI integration.

**Missing commands:**
- `karen login` - Email/password authentication
- `karen login --github` - GitHub OAuth device flow
- `karen logout` - Clear credentials
- `karen whoami` - Show current user

---

## Scope

### In Scope
- [ ] Create `karen login` command (email/password)
- [ ] Create `karen login --github` command (OAuth device flow)
- [ ] Create `karen logout` command
- [ ] Create `karen whoami` command
- [ ] Secure credential storage in `~/.karen/credentials.json`

### Out of Scope
- Password reset flow
- Multi-account support
- SSO/SAML

---

## Technical Approach

### Files to Create
```
karen-cli/src/
├── commands/
│   ├── login.ts      # Login command
│   ├── logout.ts     # Logout command
│   └── whoami.ts     # Show current user
└── core/
    └── credentials.ts # Secure credential storage
```

### Files to Modify
- `karen-cli/src/cli.ts` - Register new commands

### GitHub OAuth Device Flow
1. Request device code from GitHub
2. Display user code and verification URL
3. Poll for token completion
4. Store token securely

### Credential Storage
- Location: `~/.karen/credentials.json`
- Permissions: `chmod 600`
- Contents: `{ karenToken, githubToken, expiresAt }`

---

## Tasks

| # | Task | Assignee | Status |
|---|------|----------|--------|
| 1 | Create credentials.ts storage module | TBD | Not Started |
| 2 | Implement `karen login` (email/password) | TBD | Not Started |
| 3 | Implement GitHub OAuth device flow | TBD | Not Started |
| 4 | Implement `karen logout` | TBD | Not Started |
| 5 | Implement `karen whoami` | TBD | Not Started |
| 6 | Add token refresh logic | TBD | Not Started |
| 7 | Register commands in cli.ts | TBD | Not Started |
| 8 | Add tests | TBD | Not Started |

---

## Definition of Done

- [ ] `karen login` authenticates and stores token
- [ ] `karen login --github` completes OAuth flow
- [ ] `karen logout` clears all credentials
- [ ] `karen whoami` shows user info and plan
- [ ] Credentials file has 600 permissions
- [ ] Tests pass

---

## Spec Reference

`specs/github-integration.md` lines 12-51

---

## Dependencies

**Blocked by:** Epic 001 (Auth System) - needs backend auth endpoints  
**Blocks:** All cloud CLI features
