# Epic 012: AI Response Caching & Rate Limiting

**Priority:** P1 - HIGH  
**Status:** Draft (Pending Approval)  
**Estimated Duration:** 2 days  

---

## Problem Statement

The CLI has **no AI response caching** and **no rate limiting**:
- Every audit makes fresh API calls, wasting money on repeated analyses
- No protection against 429 errors or API bans
- Estimated 60-80% cost reduction possible with caching

---

## Scope

### In Scope
- [ ] AI response cache with SHA256 key generation
- [ ] File-based cache in `~/.karen/cache/ai/`
- [ ] 24-hour TTL (configurable)
- [ ] LRU eviction at 100MB
- [ ] `--no-cache` bypass flag
- [ ] Rate limiter: 50 req/min, 100k tokens/min, 5 concurrent
- [ ] Exponential backoff on 429

### Out of Scope
- Distributed cache
- Cache warming
- Per-user quotas

---

## Technical Approach

### Files to Create
```
karen-cli/src/core/
├── ai-cache.ts      # Cache implementation
└── rate-limiter.ts  # Rate limiting
```

### File to Modify
- `karen-cli/src/core/claude.ts` - Integrate cache + rate limiter

### Cache Key Generation
```typescript
const key = crypto.createHash('sha256')
  .update(prompt + cssContext + JSON.stringify(config))
  .digest('hex');
```

### Cache Structure
```
~/.karen/cache/ai/
├── abc123.json  # { response, timestamp, ttl }
└── def456.json
```

### Rate Limiter
```typescript
const limiter = {
  requestsPerMinute: 50,
  tokensPerMinute: 100000,
  maxConcurrent: 5,
  backoff: { initial: 1000, max: 60000, multiplier: 2 }
};
```

---

## Tasks

| # | Task | Assignee | Status |
|---|------|----------|--------|
| 1 | Create ai-cache.ts with SHA256 keys | TBD | Not Started |
| 2 | Implement file-based storage | TBD | Not Started |
| 3 | Add TTL and LRU eviction | TBD | Not Started |
| 4 | Create rate-limiter.ts | TBD | Not Started |
| 5 | Implement exponential backoff | TBD | Not Started |
| 6 | Integrate into claude.ts | TBD | Not Started |
| 7 | Add `--no-cache` flag | TBD | Not Started |
| 8 | Add env vars for config | TBD | Not Started |

---

## Environment Variables
```
KAREN_AI_CACHE_TTL=86400        # 24 hours
KAREN_AI_CACHE_DIR=~/.karen/cache/ai
KAREN_AI_RATE_LIMIT_RPM=50
KAREN_AI_RATE_LIMIT_TPM=100000
KAREN_AI_RATE_LIMIT_CONCURRENT=5
```

---

## Definition of Done

- [ ] Repeated audits use cached responses
- [ ] Cache respects TTL and size limits
- [ ] `--no-cache` forces fresh API call
- [ ] Rate limiter queues requests
- [ ] 429 triggers exponential backoff
- [ ] Tests pass

---

## Spec Reference

- Caching: `specs/ai-analysis.md` lines 244-271
- Rate Limiting: `specs/ai-analysis.md` lines 231-242
