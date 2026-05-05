# Performance Audit · VietPay MVP

**Date:** 2026-06-05 · **Tool:** k6 + Pyroscope

## Bottlenecks Identified

### [1] GET /api/v1/transfers — N+1 query
- 1 SELECT transfers + N SELECT users
- Current: ~840ms p95 với 50 transfers
- **Fix:** JOIN users + cursor pagination
- **Target:** < 150ms p95 ✓ achieved (132ms)

### [2] Wallet balance lookup — no cache
- Hot path 100% DB hit
- Current: ~95ms p95
- **Fix:** Redis cache 30s TTL + invalidate on tx
- **Target:** < 20ms p95 ✓ achieved (12ms)

### [3] FCM push fan-out — sequential send
- 200 push notifications take 18s sequentially
- **Fix:** parallel batches of 50
- **Target:** < 3s ✓ achieved (2.4s)

## Load Test Results (k6)

| Endpoint | RPS sustained | p50 | p95 | p99 |
|---|---|---|---|---|
| POST /transfers | 800 | 240ms | 720ms | 1.4s |
| GET /transfers | 1500 | 45ms | 132ms | 280ms |
| GET /wallet/balance | 3000 | 4ms | 12ms | 35ms |
| POST /payments/qr | 500 | 380ms | 920ms | 1.8s |

## SLA Compliance

- ✅ Transfer p95 < 3s end-to-end (actual 720ms API + 1.2s Sepay)
- ✅ 99.9% availability target (Q1 trial: 99.94%)

## Decision

**PASS** · production-ready.
