# V2 Risk Register

| ID | Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| R-V2-01 | Dual-write divergence between old/new wallet DB | MED | **HIGH** | Hourly consistency check job + automated reconciliation; abort cutover if drift > 0.01% |
| R-V2-02 | EVN bill lookup API rate-limited (1k req/min) | HIGH | MED | Cache bill lookups 60s; queue + backoff on 429 |
| R-V2-03 | Telco topup gateway downtime | MED | **HIGH** | Multi-provider failover (primary + 2 fallback); refund automation |
| R-V2-04 | Wallet split breaks 12 internal callers | MED | **HIGH** | Adapter facade in wallet-service; contract tests for all callers; phased rollout |
| R-V2-05 | Migration cutover during peak traffic | LOW | **HIGH** | Cutover window: 02:00-04:00 ICT Sunday (low traffic); pre-warmed read replicas |
| R-V2-06 | New bill-service security vulnerabilities | MED | **HIGH** | OWASP audit before launch; pen-test by external vendor; bug bounty $5k cap |
| R-V2-07 | Customer support load spike post-launch | HIGH | LOW | Pre-train CS team; in-app FAQ; chatbot for common bill questions |
| R-V2-08 | SBV regulatory delay on bill-payment license | LOW | **HIGH** | Pre-submitted application 2026-04; legal counsel on retainer |
| R-V2-09 | AWS RDS cross-region replica lag during migration | MED | MED | Switch to multi-AZ standby write; throttle writes if lag > 5s |
| R-V2-10 | App Store review reject (V2 build) | MED | MED | Submit beta 2 weeks early; pre-flight Apple guidelines audit |

## Owner

- Tech Lead: Long N. (technical risks R-V2-01, 04, 09)
- PM: Quân T. (process risks R-V2-05, 07)
- Legal: Anh H. (compliance R-V2-06, 08)

## Review Cadence

Weekly during V2 development. Daily 24h before cutover.
