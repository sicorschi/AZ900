# KPI Dashboard Design for Cloud Adoption Value Tracking

## Purpose

Measure whether cloud adoption is producing expected outcomes across:

- Technical performance and resilience
- Security and operational excellence
- Financial efficiency
- Business delivery and customer impact

This dashboard is designed for monthly executive review and weekly engineering operations review.

## 1) Dashboard structure (tabs/sections)

1. Executive Summary (single-page scorecard)
2. Reliability and Performance
3. Security and Compliance
4. Cost and Financial Efficiency (FinOps)
5. Delivery Velocity and Engineering Productivity
6. Business Outcomes

## 2) KPI catalog (with formulas, targets, and ownership)

| KPI | Category | Definition / Formula | Target Example | Owner | Review Cadence |
|---|---|---|---|---|---|
| Service Availability (%) | Reliability | ((Total minutes - Downtime minutes) / Total minutes) x 100 | >= 99.9% | SRE Lead | Weekly/Monthly |
| MTTR (Mean Time to Recovery) | Reliability | Sum of incident recovery times / Number of incidents | <= 30 min | Incident Manager | Weekly |
| Change Failure Rate (%) | Reliability/DevOps | (Failed deployments / Total deployments) x 100 | <= 10% | Engineering Manager | Weekly |
| p95 API Latency (ms) | Performance | 95th percentile response time | <= 300 ms | App Team Lead | Daily/Weekly |
| Peak Autoscaling Success (%) | Scalability | Successful scale events / Total scale events | >= 98% | Platform Team | Weekly |
| Critical Vulnerabilities Open >30 days | Security | Count of critical findings older than 30 days | 0 | Security Lead | Weekly |
| Patch Compliance (%) | Security | Patched assets within SLA / Total assets | >= 95% | Infra Ops | Weekly |
| Policy Compliance Score (%) | Governance | Compliant resources / Total evaluated resources | >= 98% | Cloud Governance | Weekly |
| Cloud Spend vs Budget (%) | Financial | Actual monthly spend / Approved budget x 100 | 95-105% band | FinOps Lead | Weekly/Monthly |
| Unit Cost per Transaction ($) | Financial | Total cloud cost / Number of business transactions | -15% vs baseline | Product + FinOps | Monthly |
| Reserved/Committed Usage Coverage (%) | Financial | Covered steady-state usage / Total steady-state usage | >= 70% | FinOps Lead | Monthly |
| Deployment Frequency | Delivery | Production deployments per week | +50% vs baseline | Engineering Manager | Weekly |
| Lead Time for Change | Delivery | Commit-to-production median time | <= 1 day | DevOps Lead | Weekly |
| Customer Conversion Rate (%) | Business | Conversions / Total qualified sessions x 100 | +10% vs baseline | Product Owner | Monthly |
| Revenue Impact from Digital Channel ($) | Business | Period digital revenue - baseline period revenue | Positive trend | Business Owner | Monthly |
| Customer Ticket Rate per 1k Users | Business/Quality | Support tickets / 1,000 active users | -20% vs baseline | Support Manager | Monthly |

## 3) Baseline and target-setting method

Before measuring improvement, capture a pre-migration baseline for at least 8-12 weeks.

For each KPI:

- Baseline: median of pre-cloud period.
- Target: baseline improvement goal (for example, 20% better) or policy/SLA threshold.
- Guardrail: minimum acceptable floor to trigger escalation.

Example:

- Baseline p95 latency: 520 ms
- Target p95 latency: <= 300 ms
- Guardrail: alert if > 400 ms for 3 consecutive days

## 4) Executive scorecard design (Top 10 KPIs)

Use RAG status (Red/Amber/Green) with trend arrows and confidence notes.

Recommended top scorecard KPIs:

1. Availability (%)
2. MTTR
3. p95 latency
4. Change failure rate
5. Cloud spend vs budget
6. Unit cost per transaction
7. Policy compliance score
8. Critical vulnerabilities >30 days
9. Deployment frequency
10. Revenue/conversion uplift

Status logic example:

- Green: meets or exceeds target
- Amber: within 10% of target or deteriorating trend
- Red: misses target by >10% or breaches guardrail

## 5) Data sources and instrumentation map

| Domain | Typical Source Systems | Notes |
|---|---|---|
| Reliability/Performance | APM, application logs, load balancer metrics, synthetic tests | Standardize service naming and environment tags |
| Security/Compliance | CSPM tool, vulnerability scanner, SIEM, IAM audit logs | Track SLA aging of findings |
| Cost/FinOps | Cloud billing export, cost management tool, tagging catalog | Enforce tagging completeness for showback/chargeback |
| Delivery | CI/CD platform, source control analytics, incident tooling | Use DORA metric definitions consistently |
| Business | CRM, analytics platform, ERP/finance, support platform | Align financial calendar and attribution rules |

## 6) Governance model and meeting cadence

- Weekly ops review (engineering + security + FinOps): incidents, spend anomalies, policy violations.
- Monthly value review (IT + finance + business owners): KPI outcomes vs expected benefits.
- Quarterly optimization review: architecture changes, commitment strategies, roadmap adjustments.

Decision rules:

- If 2 or more critical technical KPIs are Red for 2 consecutive periods, trigger remediation plan.
- If spend is above 110% budget for 2 months, trigger cost optimization sprint.
- If business KPIs do not improve after 2 quarters, reassess workload architecture and product assumptions.

## 7) Example dashboard layout (single screen)

- Top row: 10 executive KPIs with RAG and 3-month trend sparkline.
- Middle left: Reliability/performance deep dive (availability, latency, MTTR, incidents).
- Middle right: Security + compliance posture (critical aging, policy compliance, patch SLA).
- Bottom left: Cost and unit economics (monthly spend, forecast, unit cost trend, commitment coverage).
- Bottom right: Delivery + business outcomes (deployment frequency, lead time, conversion, revenue trend).

## 8) Anti-patterns to avoid

- Tracking only infrastructure metrics and ignoring business outcomes.
- No baseline, making “improvement” impossible to prove.
- Too many KPIs without ownership.
- Cost tracking without unit economics (for example, cost per transaction/customer).
- Inconsistent tags causing unreliable cost allocation.

## 9) 30-60-90 day implementation plan

1. Days 1-30: define KPI dictionary, owners, formulas, and baseline data extraction.
2. Days 31-60: implement data pipelines, dashboard visuals, RAG thresholds, and alerting.
3. Days 61-90: run governance cadence, validate data quality, tune targets, and publish monthly scorecard.

## AZ-900 exam takeaway

A successful cloud KPI dashboard must prove both:

- Technical benefits (availability, scalability, reliability, security, manageability), and
- Business benefits (cost efficiency, speed of delivery, customer and revenue impact).
