# Production Risk Assessment · VietPay MVP

**Date:** 2026-06-05

## Identified Risks

### [MED-01] Peak load > forecast
- Day-1 launch traffic không predict được chính xác
- **Mitigation:** ECS auto-scale 2→20 + circuit breaker (Hystrix-style)
- **Owner:** SRE team
- **Trigger:** CPU > 70% → auto-scale; > 90% → page on-call

### [MED-02] Sepay degradation
- Sepay sandbox SLA 99% — prod có thể tệ hơn
- **Mitigation:** queue + retry với exponential backoff + user-facing degradation message
- **Owner:** payment-service team
- **Trigger:** error rate > 5% over 1 min → fall back to "delayed processing" UX

### [MED-03] eKYC review queue
- 5-8% applications need manual review — peak có thể ngập
- **Mitigation:** 24h SLA + auto-page on backlog > 100
- **Owner:** Compliance team
- **Trigger:** queue > 100 → escalate; > 500 → enable temp surge contractor

### [LOW-01] Push notification deliverability
- FCM/APNs có thể delay 2-30s normally
- **Mitigation:** in-app badge + pull-to-refresh fallback

### [LOW-02] Customer support overflow Day-1
- **Mitigation:** in-app FAQ + chatbot + extended on-call CS hours

## Monitoring

- Datadog dashboards: API latency, error rate, queue depth, eKYC backlog
- PagerDuty: P1 alerts → 24/7 rotation
- Status page: https://status.vietpay.vn

## Decision

**LOW residual risk** · production-ready.
