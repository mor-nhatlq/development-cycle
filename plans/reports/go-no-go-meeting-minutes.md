# Go/No-Go Meeting Minutes · VietPay MVP Launch

**Date:** 2026-06-05 · **Time:** 15:00-16:00 ICT
**Location:** Hybrid (HCMC office + Google Meet)

## Attendees

- Long N. — Tech Lead
- Quân T. — PM
- Linh M. — Product Owner
- Tuấn P. — QA Lead
- Anh H. — Legal/Compliance
- Mai T. — SRE Lead

## Agenda

1. Review go-criteria checklist
2. Review residual risks
3. Launch decision
4. Post-launch monitoring plan

## Decisions

### Go-Criteria Status
- ✅ All 220 tests passing (218 + 2 skipped)
- ✅ Code review HIGH findings closed
- ✅ Security audit CRITICAL closed
- ✅ Performance SLA met
- ✅ SBV license received 2026-06-04
- ✅ Sepay + VNPT contracts production-tier active
- ✅ Rollback plan tested in staging

### Residual Risks Acceptance
- Peak traffic uncertainty — accepted with auto-scale mitigation
- eKYC manual review queue — accepted with surge plan

## Decision

**🟢 GO for launch** at 22:00 ICT 2026-06-05.

## Post-Launch Plan

- T+0 to T+1h: war-room (all hands on Slack #vietpay-launch)
- T+1h to T+24h: monitoring + on-call (SRE + dev primary)
- T+24h: launch retrospective scheduled
- T+7d: post-mortem if any P1 incident

## Action Items

- [ ] Mai T. — confirm PagerDuty rotation 6PM-6AM
- [ ] Linh M. — schedule App Store / Play Store release for 22:00
- [ ] Long N. — final smoke test 21:30
- [ ] Quân T. — comms post for marketing 22:15
