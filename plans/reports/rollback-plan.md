# Rollback Plan · VietPay MVP

**Date:** 2026-06-05

## Triggers

Rollback trong vòng 15 phút nếu một trong các điều sau:

- Error rate > 5% trên critical endpoints (transfer, KYC, login) liên tục 5 phút
- p95 latency > 2x SLA liên tục 5 phút
- DB connection pool exhaustion
- Sepay/VNPT integration failure rate > 20%
- Data integrity issue (failed reconciliation between wallet + ledger)

## Rollback Steps

### Step 1 · Backend (ECS)
- Switch ALB target group to previous task definition (1-click in console)
- ETA: 2-3 minutes
- Verify: health check green on old version

### Step 2 · Database
- No schema migration in this release → no DB rollback needed
- If schema changed: run `migrations/rollback.sql` (pre-tested in staging)

### Step 3 · Mobile App
- iOS: cannot force-rollback published build → server force-version-bump banner
- Android: staged rollout halt + previous APK promotion

### Step 4 · Communication
- Status page update within 5 min
- In-app banner if degraded
- Twitter/Zalo OA post within 15 min

## Decision Authority

- Rollback decision: **Tech Lead** + **PM** (joint approval)
- Execution: SRE on-call
- Post-mortem: required within 48h

## Verification After Rollback

- [ ] All endpoints HTTP 200
- [ ] Error rate < baseline
- [ ] p95 within SLA
- [ ] No data loss (reconcile wallet vs ledger)
- [ ] Customer support volume returning to baseline
