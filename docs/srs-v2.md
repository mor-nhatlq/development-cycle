# Software Requirements Specification — VietPay V2

## Overview

V2 extends V1 (P2P + QR) với bill payment + mobile topup, đồng thời migrate khỏi shared-DB anti-pattern.

## Functional Requirements (delta from V1)

### FR-V2-001 · Bill Payment
- Pay EVN electricity, internet (Viettel/FPT/VNPT), water (SAWACO), TV/cable
- Lookup by customer code (provider-specific format)
- Display bill amount + due date trước khi confirm
- **Idempotency:** required (Idempotency-Key header)
- **Limit:** 50M VND/giao dịch · 200M/ngày

### FR-V2-002 · Mobile Topup
- Carriers: Viettel, Vinaphone, Mobifone, Vietnamobile
- Denominations: 10k / 20k / 50k / 100k / 200k / 500k VND
- **Latency:** topup applied < 30s SLA
- **Refund:** auto-refund nếu telco confirm fail trong 5 min

### FR-V2-003 · Multi-currency Wallet (foundation only)
- Schema support cho USD, EUR (no UI yet — Phase 3 deliverable)
- Wallet table includes `currency CHAR(3)` column

## Non-Functional Requirements (delta)

### NFR-V2-001 · Migration Safety
- Zero downtime cutover
- Dual-write window 1-2 weeks
- Read traffic ramp 5% → 25% → 100%
- Rollback window 7 days

### NFR-V2-002 · Performance
- Bill lookup p95 < 800ms (incl. provider API call)
- Topup p95 < 5s end-to-end
- Existing P2P/QR p95 SLA unchanged

### NFR-V2-003 · Compliance (delta)
- SBV tracking: bill payments require beneficiary KYC level 2+
- AML threshold check: bill amounts > 10M flagged for review

## Out of Scope (V2)

- ❌ International remittance (Phase 3)
- ❌ Virtual cards (Phase 3)
- ❌ Group wallets (Phase 4)
- ❌ Merchant settlement (Phase 5)
