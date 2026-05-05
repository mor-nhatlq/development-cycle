# Phase 01 — Infrastructure Setup

**Duration:** Week 1 (2026-05-05 → 2026-05-12) · **Priority:** P0 · **Status:** Not started
**Owner:** DevOps Lead · **Team:** 2 backend devs + 1 mobile dev

---

## Context Links

- [Master Plan](plan.md) · [System Architecture](../../docs/system-architecture.md)

## Overview

Setup foundation infrastructure cho VietPay MVP: AWS accounts, VPC, ECS, RDS, observability, CI/CD pipeline, mobile dev env. Mọi service phải deployable + observable trước khi feature work bắt đầu Phase 02.

## Key Insights

- AWS Organizations + multi-account: separate `prod` / `staging` / `dev` accounts để hard isolation
- Terraform Infrastructure-as-Code, không click console
- Cloudflare đặt trước AWS WAF — DDoS shielding tốt hơn + cheap
- DataDog Free trial 14 ngày → switch sang paid sau Phase 02 khi infra stable
- ECS Fargate over EKS — không cần k8s overhead với 6 services

## Requirements

### Functional
- 3 AWS accounts: dev, staging, production
- 1 VPC mỗi env, 3 AZ multi-AZ
- ECS Fargate cluster sẵn sàng nhận deploy
- RDS PostgreSQL 16 instance (dev: db.t4g.medium, prod: db.r6g.xlarge)
- ElastiCache Redis cluster
- MSK Kafka cluster (3 brokers)
- S3 buckets cho KYC docs (private, KMS encrypted, object lock)

### Non-functional
- Provisioning < 30 phút end-to-end (Terraform apply)
- Disaster recovery: snapshot daily, retention 30 ngày
- Cost: stay < $500/tháng cho dev + staging combined

## Architecture

VPC topology, ECS cluster, RDS Multi-AZ, ElastiCache cluster — chi tiết trong [system-architecture.md §9](../../docs/system-architecture.md).

## Related Code Files

### Create
- `infra/terraform/modules/vpc/`
- `infra/terraform/modules/ecs-cluster/`
- `infra/terraform/modules/rds/`
- `infra/terraform/modules/redis/`
- `infra/terraform/modules/kafka/`
- `infra/terraform/modules/observability/`
- `infra/terraform/envs/dev/main.tf`
- `infra/terraform/envs/staging/main.tf`
- `infra/terraform/envs/prod/main.tf`
- `.github/workflows/ci.yml` (lint, test, build container)
- `.github/workflows/cd-staging.yml`
- `.github/workflows/cd-production.yml`
- `monorepo/package.json` (Turborepo workspace)
- `services/_template/Dockerfile`
- `services/_template/package.json`
- `services/_template/src/main.ts` (NestJS skeleton)
- `mobile/package.json` (Expo workspace)
- `mobile/app.json`

## Implementation Steps

### Step 1 — AWS account setup (2 days)
1. Create AWS Organizations với 3 member accounts
2. Setup IAM roles cross-account (admin / dev / readonly)
3. Configure Cost Anomaly Detection + budget alerts ($500 dev/staging, $5000 prod)
4. Enable CloudTrail org-wide, log to dedicated audit account

### Step 2 — Terraform foundation (1 day)
1. Setup Terraform Cloud workspace per env
2. Create modules: `vpc`, `ecs-cluster`, `rds`, `redis`, `kafka`, `observability`
3. Encrypt state with KMS, lock via DynamoDB

### Step 3 — Network + compute (1 day)
1. VPC: 10.0.0.0/16, 3 AZ public + private + isolated subnets
2. NAT Gateway (1 per AZ for HA)
3. ECS Fargate cluster `vietpay-dev`, `vietpay-staging`, `vietpay-prod`
4. ECR repositories cho 6 services

### Step 4 — Data layer (1 day)
1. RDS PostgreSQL 16 Multi-AZ, encryption at rest, automated backup 35 days
2. ElastiCache Redis 7 cluster, 3 master + 3 replica
3. MSK Kafka 3.7, 3 brokers
4. S3 buckets với object lock + KMS encryption

### Step 5 — Observability (1 day)
1. DataDog agent on ECS
2. Sentry projects cho mobile + backend
3. CloudWatch log groups cho ECS task logs
4. Distributed tracing OpenTelemetry config

### Step 6 — CI/CD pipeline (1 day)
1. GitHub Actions workflow: lint → test → build → push ECR → deploy ECS
2. Blue/green deploy via CodeDeploy
3. Mobile EAS build setup

## Todo List

- [ ] AWS Organizations setup với 3 accounts
- [ ] IAM cross-account roles + SSO
- [ ] Cost budgets + anomaly detection
- [ ] CloudTrail org-wide + audit account
- [ ] Terraform Cloud workspaces (dev/staging/prod)
- [ ] Terraform module: vpc
- [ ] Terraform module: ecs-cluster
- [ ] Terraform module: rds
- [ ] Terraform module: redis
- [ ] Terraform module: kafka
- [ ] Terraform module: observability
- [ ] VPC dev provisioned
- [ ] VPC staging provisioned
- [ ] VPC prod provisioned
- [ ] ECS Fargate cluster × 3 envs
- [ ] ECR repositories × 6 services
- [ ] RDS PostgreSQL × 3 envs
- [ ] ElastiCache Redis × 3 envs
- [ ] MSK Kafka × 3 envs
- [ ] S3 buckets cho KYC + receipts + assets
- [ ] DataDog agent deployed
- [ ] Sentry projects (mobile + backend)
- [ ] CloudWatch log groups
- [ ] GitHub Actions CI workflow
- [ ] GitHub Actions CD staging
- [ ] GitHub Actions CD prod (with approval gate)
- [ ] Service template skeleton (NestJS)
- [ ] Mobile Expo workspace bootstrap
- [ ] EAS Build config (iOS + Android)
- [ ] Documentation: deployment runbook
- [ ] Smoke deploy: dummy "hello world" service to all envs

## Success Criteria

- ✅ Hello-world service deployable tới dev / staging / prod via CI/CD
- ✅ Logs visible trong DataDog within 30 giây
- ✅ Metrics dashboard có baseline RED metrics
- ✅ Database connection từ ECS service tới RDS works
- ✅ Redis ping works từ service
- ✅ Kafka produce + consume works
- ✅ Mobile build có thể tạo iOS .ipa + Android .apk via EAS
- ✅ Cost run-rate < $500/tháng cho dev + staging

## Risk Assessment

| Risk | Probability | Impact | Mitigation |
|------|:-----------:|:------:|------------|
| AWS account creation delay (legal entity) | Low | High | Start admin process Week 0 |
| Terraform state corruption | Low | High | TF Cloud + DynamoDB locking + backup state |
| Network misconfig blocking service comm | Medium | Medium | Smoke test end-to-end, isolated test |
| Cost spike (someone provision wrong) | Low | Medium | Budget alerts + auto-stop dev EOD |

## Security Considerations

- IAM least-privilege, no `*:*` policies
- KMS keys per env, key rotation enabled
- VPC flow logs enabled, log to S3
- Security Groups deny-by-default, explicit allow
- Secrets Manager cho DB passwords, không hardcode
- AWS Config rules enabled (encryption, public S3 deny, MFA on root)

## Next Steps

- Phase 02 (Auth + eKYC) bắt đầu khi all checkboxes done
- Doc impact: update [deployment-guide.md](../../docs/deployment-guide.md) với runbook đã tạo
