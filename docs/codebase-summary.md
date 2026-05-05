# VietPay V1 — Codebase Summary

**Generated:** 2026-05-05 · **Source:** Repomix Agent + Dependency Graph Analyzer
**Scope:** Pre-V2 reverse engineering · pre-condition cho gap analysis & migration plan

---

## 1. Tổng quan

| Metric | Value |
|--------|-------|
| Total LOC | 124,000 |
| Languages | TypeScript (82%), SQL (11%), YAML (7%) |
| Services | 6 NestJS microservices |
| API endpoints | 89 |
| DB tables | 47 (1 shared schema — anti-pattern) |
| Test coverage (line) | 71.2% (target ≥ 80%) |
| Codebase age | 4 năm (Q3/2022 first commit) |
| Active contributors (90d) | 12 |
| Last deploy production | 6 days ago (v1.18.4) |

---

## 2. Service Inventory

| Service | LOC | Last touched | Test cov | Status |
|---------|-----|--------------|----------|--------|
| `auth-service` | 12,400 | 3 days ago | 88% | ✅ stable |
| `kyc-service` | 8,100 | 21 days ago | 79% | ✅ stable |
| `wallet-service` | 18,200 | yesterday | 64% | ⚠ god module |
| `transfer-service` | 26,300 | 2 days ago | 81% | ⚠ hot path · complex |
| `payment-service` | 22,800 | 5 days ago | 73% | ✅ Sepay integration |
| `notification-service` | 9,600 | 14 days ago | 86% | ✅ stable |

**Shared infra**:
- `shared/lib/` (8.4k LOC) — domain types, error classes, validators
- `infra-cdk/` (12k LOC TypeScript) — AWS CDK stacks
- `tests/` (~24k LOC test files, fixtures, factories)

---

## 3. Dependency Graph Highlights

### Cross-service dependencies

```
auth → kyc                       (consume kyc.completed event)
wallet ↔ transfer  (CYCLE)       (anti-pattern · shared DB)
transfer → payment               (Sepay relay)
notification ← all services      (Kafka consumer)
```

**Detected cycles**: 2
- `wallet ↔ transfer` (shared DB schema `vietpay`, both services read/write same tables)
- `wallet → ledger-helper → wallet` (internal helper imports back wallet)

**Boundary violations**: 18 cross-service direct imports vi phạm DDD pattern (e.g., `transfer-service/src/...` import `wallet-service/src/...` thay vì gọi qua API).

---

## 4. Tech Stack

### Runtime
- Node.js 20 LTS
- NestJS 10
- TypeScript 5.3
- React Native 0.74 + Expo SDK 51 (mobile)

### Data
- PostgreSQL 15 (RDS Multi-AZ, ap-southeast-1)
- Redis 7 (ElastiCache, cluster mode)
- Kafka 3.7 (MSK, 3 brokers)
- S3 (KYC docs, receipts) + KMS

### Integrations
- Sepay (VietQR + bank transfer aggregator)
- VNPT eKYC (CMND/CCCD scan + liveness)
- FCM + APNs (push notification)
- Stringee (SMS OTP)

### Infra
- AWS CDK (TypeScript) for IaC
- ECS Fargate (no k8s)
- CloudFront CDN
- DataDog (metrics + logs + APM)
- Sentry (mobile + backend errors)

---

## 5. Hot Spots (cần đặc biệt chú ý khi refactor)

| File | LOC | Cyclomatic complexity | Touch frequency | Risk |
|------|-----|----------------------|-----------------|------|
| `wallet-service/wallet.controller.ts` | 814 | 47 | high | 🔴 god class |
| `transfer-service/transfer.service.ts` | 612 | 38 | very high | 🔴 critical path |
| `transfer-service/legacy-mapper.ts` | 478 | 29 | medium | 🔴 zero tests |
| `payment-service/sepay-adapter.ts` | 392 | 22 | medium | 🟡 retry logic complex |
| `wallet-service/ledger.service.ts` | 348 | 18 | high | 🟡 invariants không doc |

---

## 6. DB Schema Snapshot

47 tables across 1 shared schema `vietpay` (anti-pattern — V2 sẽ split):

```
auth tables       (8): users, sessions, device_keys, otp_log, ...
kyc tables        (5): kyc_records, kyc_documents, manual_review, ...
wallet tables     (7): wallets, wallet_balances, ledger_entries, ...
transfer tables   (9): transfers, idempotency_keys, daily_limits, ...
payment tables    (8): payments, qr_codes, webhooks, linked_banks, ...
notification (4): notifications, prefs, device_tokens, ...
audit / shared (6): audit_log, feature_flags, system_events, ...
```

**Foreign keys**: 64. Cross-domain FKs (anti-pattern): 12 (wallet → users, transfer → wallets, etc.) — V2 thay bằng eventual consistency qua events.

---

## 7. API Surface

**89 REST endpoints** across 6 services:

```
auth-service:           14 endpoints (signup, otp, login, biometric, ...)
kyc-service:            8  endpoints (submit, status, documents, review, ...)
wallet-service:         11 endpoints (balance, history, ledger, ...)
transfer-service:       18 endpoints (transfer, cancel, history, limits, ...)
payment-service:        24 endpoints (banks, topup, qr, webhooks, ...)
notification-service:   14 endpoints (inbox, prefs, devices, ...)
```

**Public-facing** (mobile client): 67 endpoints
**Internal admin**: 22 endpoints

OpenAPI spec at `docs/openapi-v1.18.yaml` (auto-generated).

---

## 8. Build & Deploy

| | |
|---|---|
| Monorepo | Turborepo |
| Package manager | pnpm 9 |
| CI | GitHub Actions (lint + test + build container per service) |
| CD | GitHub Actions → CodeDeploy → ECS blue/green |
| Mobile build | Expo EAS |
| Build time | ~14 min full pipeline |
| Deploy time | ~8 min staging, ~12 min prod |

---

## 9. Production Footprint

| | |
|---|---|
| MAU | 312,000 (May 2026) |
| Daily active | 41k |
| Daily transactions | 78k (avg) · peak 145k |
| Bank linked accounts | 287k |
| Storage (KYC docs S3) | 3.4 TB |
| Daily Sepay calls | ~210k |
| Cost (monthly AWS) | ~$8,400 |

---

## 10. Known Issues / Tech Debt

(Full inventory ở `docs/tech-debt-inventory.md`)

- 14 god classes (>500 LOC, >25 cyclomatic) — top 3 listed in §5
- Shared DB schema giữa wallet + transfer (must split for V2)
- 23 outdated dependencies (4 với CVE high)
- 0% test coverage cho 4 critical modules: `legacy-mapper`, `audit-shipper`, `slo-emitter`, `compliance-classifier`
- 6 outstanding TODOs trong code mention "Refund" feature (architecture chưa ready)
- Manual review queue cho eKYC chưa có SLA tracking

---

## 11. Reverse Engineering Notes (AI-extracted)

Findings từ Reverse Engineer Agent đọc code:

### Architectural decisions (inferred từ code)

- **Idempotency**: Redis SETNX với 24h TTL, fallback DB lookup. Pattern lặp lại 6 places — nên extract helper.
- **Audit log**: Mọi write operation gửi event tới `audit-log` Kafka topic. Consumer ship to S3 Object Lock 7 năm (compliance SBV).
- **Feature flags**: dùng LaunchDarkly. 23 flags currently active in production.
- **Multi-region**: chỉ `ap-southeast-1` active. `eu-central-1` provisioned nhưng inactive (dormant DR).

### Surprising / Undocumented behaviors (cần char. tests pin)

- `transfer-service/legacy-mapper.ts:142` — V1 returns HTTP 200 even khi DB transaction rollback (gotcha). Mobile app dependent on this. Char. test must pin.
- `wallet-service/balance.service.ts:88` — Cache 30s nhưng invalidate trên transfer event chỉ async (race window ~50ms). Mobile UI handle stale data gracefully.
- `payment-service/sepay-adapter.ts:201` — Retry 3 attempts với exponential backoff, nhưng attempt 3 dùng different endpoint (legacy v0 API). Sepay vendor confirmed in support thread #4421.

---

## 12. Recommendations cho V2

**Architectural**:
- Split shared DB schema: separate `wallet_db` và `transfer_db` (high risk migration → strangler fig + dual-write)
- Break wallet god class thành 4 modules: `wallet-balance`, `wallet-ledger`, `wallet-policy`, `wallet-history`
- Extract idempotency pattern thành shared library

**Quality**:
- Add char. tests cho 4 zero-coverage modules trước khi touch
- Upgrade 23 outdated deps (4 critical CVE)
- Pin surprising behaviors (3 undocumented gotchas above) bằng char. tests

**Process**:
- Migration via strangler fig thay vì big bang (lower risk cho 312k MAU)
- 72h hyper-care on-call sau full rollout
- Customer SME (PO) phải sign-off trước cutover

---

**Next step**: feed vào Step 02 Gap Analysis (compare with customer V2 requirements brief) → sinh gap-analysis.md.
