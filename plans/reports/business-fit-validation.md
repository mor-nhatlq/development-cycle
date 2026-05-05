# Business Fit Validation · VietPay MVP

**Date:** 2026-06-05 · **Owner:** PO Linh M.

## PRD Alignment Check

| PRD Goal | Implementation | Match |
|---|---|---|
| Social-first P2P transfers | P2P engine + future split-bill hooks | ✅ |
| AI insights (Phase 4) | Foundation event log in place | ✅ |
| VietQR payment | Sepay primary integration | ✅ |
| Sub-3s transfer SLA | p95 720ms API + 1.2s Sepay | ✅ |
| 50k MAU 6mo target | Capacity tested 800 RPS sustained | ✅ |

## Gen Z + Millennial Validation

User testing 2026-05-20 with 24 participants (18-35, urban):

- **NPS:** 58 (good for fintech MVP)
- **Onboarding completion:** 89% (industry avg 60%)
- **First-transfer success:** 94%
- **Top loved:** sub-3s transfer, biometric login, dark mode default
- **Top friction:** bank linking flow (clunky OTP UX) — backlogged for V2

## Differentiation vs MoMo/ZaloPay/ShopeePay/ViettelPay

- ✅ Lightest onboarding (3 steps vs 5-7 competitors)
- ✅ Modern UI (only one with dark mode)
- ✅ Faster transfer (720ms vs 1.5-3s competitors)
- ⚠️ Smaller bank network (3 banks vs 20+ competitors) — V2 priority
- ⚠️ No bill payment yet — V2 priority

## Decision

**PASS** · MVP fits PRD vision · launch approved.
