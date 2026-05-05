# Human Role · Planning Gate (Step 02)

Định nghĩa vai trò Human reviewer ở giai đoạn Planning Gate — chốt scope, architecture, effort trước khi unlock TDD.

## WHO

| Role | Responsibility |
|---|---|
| **Tech Lead** (Long N.) | Architecture · technical feasibility · dependency risk |
| **PM** (Quân T.) | Scope · timeline · resource allocation |
| **PO** (optional) | Business priority alignment |

## WHAT to validate

- [ ] Plan có đúng PRD scope không?
- [ ] Architecture decisions có sound không?
- [ ] Effort 12 weeks có realistic không?
- [ ] Risks identified đầy đủ + mitigated?
- [ ] Critical path đúng (KYC → Auth → Bank → Transfer)?
- [ ] Dependencies (Sepay, VNPT) đã contract ready?

## WHEN approve

- All 6 checklist sections ✓
- Zero blocking concern
- Reviewers sign-off

## WHEN reject

- Major scope mismatch → Planner Agent revise
- Architecture concern → Researcher re-investigate domain
- Effort estimate off > 30% → re-baseline với additional research

## SLA

- Review trong **48h** từ khi plan ready
- Revision cycle: max 2 vòng trước khi escalate to Steering Committee

## Output Artifacts

- `plans/reports/plan-review-checklist-260605.md` — checklist đã sign
- Approval comment trên plan.md (PR or commit message)
- Updated risk register nếu phát sinh concern mới
