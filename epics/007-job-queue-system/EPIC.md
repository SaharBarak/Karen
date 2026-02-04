# Epic 007: Backend Job Queue System

**Priority:** P1 - HIGH  
**Status:** Draft (Pending Approval)  
**Estimated Duration:** 2-3 days  

---

## Problem Statement

The backend has **no job queue infrastructure**. Audits run synchronously, blocking requests and causing timeouts. There's no retry logic, progress tracking, or failure handling.

This is required to properly fix Epic 005 (Audit Job Trigger).

---

## Scope

### In Scope
- [ ] Set up Bull queue with Redis
- [ ] Create audit job processor
- [ ] Implement job retry logic (3 attempts, exponential backoff)
- [ ] Add job progress events via WebSocket
- [ ] Dead letter queue for failed jobs

### Out of Scope
- Notification job queue (future)
- Scheduled/recurring jobs
- Multi-region queue distribution

---

## Technical Approach

### Files to Create
```
KarenCLI/packages/karen-backend/src/queue/
├── queue.module.ts       # BullModule configuration
├── audit.processor.ts    # Job processing logic
└── audit.producer.ts     # Job creation service
```

### Files to Modify
- `src/audits/audits.module.ts` - Import QueueModule
- `src/app.module.ts` - Register BullModule with Redis

### Queue Configuration
```typescript
BullModule.forRoot({
  redis: process.env.REDIS_URL,
});

BullModule.registerQueue({
  name: 'audit-jobs',
  defaultJobOptions: {
    attempts: 3,
    backoff: { type: 'exponential', delay: 1000 },
    removeOnComplete: 100,
    removeOnFail: 50,
  },
});
```

### Job Lifecycle
1. API creates job → returns jobId immediately
2. Processor picks up job → updates status to "processing"
3. Progress events sent via WebSocket (0-100%)
4. Completion → status "completed" with results
5. Failure after 3 retries → dead letter queue

---

## Tasks

| # | Task | Assignee | Status |
|---|------|----------|--------|
| 1 | Add Bull and Redis dependencies | TBD | Not Started |
| 2 | Create queue.module.ts with config | TBD | Not Started |
| 3 | Implement audit.processor.ts | TBD | Not Started |
| 4 | Implement audit.producer.ts | TBD | Not Started |
| 5 | Add WebSocket progress events | TBD | Not Started |
| 6 | Implement dead letter queue handling | TBD | Not Started |
| 7 | Add Redis health check | TBD | Not Started |
| 8 | Integration tests | TBD | Not Started |

---

## Infrastructure Required

- Redis server (local dev: Docker, prod: managed Redis)

```yaml
# docker-compose.yml addition
redis:
  image: redis:7-alpine
  ports:
    - "6379:6379"
```

---

## Definition of Done

- [ ] Jobs queue and process asynchronously
- [ ] Failed jobs retry 3 times with backoff
- [ ] Progress updates visible in real-time
- [ ] Dead jobs captured for debugging
- [ ] Redis connection health monitored
- [ ] Tests pass

---

## Spec Reference

`specs/backend-api.md` lines 206-244

---

## Dependencies

**Blocked by:** None  
**Blocks:** Epic 005 proper implementation (currently using temp sync approach)
