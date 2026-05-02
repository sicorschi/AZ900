# On-Premises Evaluation and Cloud Improvement Mapping

## Scenario assessed

A mid-size company runs a customer order management application on-premises:

- 2 application servers behind a basic load balancer
- 1 primary SQL database server + nightly backup
- Single datacenter
- Manual patching and release process
- Limited centralized monitoring

This assessment maps current-state risks to cloud improvements.

## 1) Availability

### Current on-premises observations

- Single datacenter introduces site-level outage risk.
- Database failover is manual and recovery is slow.
- Maintenance windows often require partial downtime.

### Cloud improvements

- Deploy application tier across multiple Availability Zones.
- Use managed database with built-in high availability and automatic failover.
- Use health probes, autoscaling groups, and load balancers with zone redundancy.
- Use backup policies with point-in-time restore.

### Expected outcome

- Reduced unplanned downtime.
- Faster failover and service restoration.
- Higher uptime target (for example moving from about 99.5% toward 99.9%+ depending on architecture).

## 2) Scalability

### Current on-premises observations

- Capacity planning is hardware-bound and slow to change.
- Peak-season traffic causes latency spikes.
- Typical overprovisioning to cover peak demand increases idle cost.

### Cloud improvements

- Use horizontal autoscaling based on CPU, queue depth, or request rate.
- Use managed caching and CDN for read-heavy workloads.
- Separate compute and data tiers for independent scaling.
- Adopt platform services for burst workloads.

### Expected outcome

- Elastic scaling during peak periods.
- Better performance consistency.
- Less overprovisioned idle capacity.

## 3) Reliability

### Current on-premises observations

- Recovery procedures are partially documented and rarely tested.
- Backups exist but restore testing is infrequent.
- Limited fault isolation between components.

### Cloud improvements

- Implement infrastructure as code for repeatable, recoverable environments.
- Establish active-passive or active-active disaster recovery strategy across regions.
- Run automated backup verification and scheduled recovery drills.
- Use queue-based decoupling and retry/circuit-breaker patterns.

### Expected outcome

- Lower mean time to recovery (MTTR).
- Better resilience to component failures.
- Improved disaster recovery readiness and confidence.

## 4) Security

### Current on-premises observations

- Inconsistent patch cadence across servers.
- Broad network access rules and limited segmentation.
- Authentication is mostly local; weak centralized identity controls.
- Auditing is fragmented across systems.

### Cloud improvements

- Enforce centralized identity and least-privilege access controls (RBAC).
- Use network segmentation, private endpoints, and web application firewall protections.
- Enable managed key vault/secrets storage and encryption by default (at rest and in transit).
- Use continuous security posture management and threat detection.
- Centralize logs in a SIEM for alerting and compliance reporting.

### Expected outcome

- Reduced attack surface.
- Faster detection and response.
- Stronger compliance posture with auditable controls.

## 5) Manageability

### Current on-premises observations

- Manual server provisioning takes days or weeks.
- Monitoring and alerts are inconsistent.
- Configuration drift between environments causes deployment issues.

### Cloud improvements

- Use templates and infrastructure as code for standardized deployments.
- Use centralized observability (metrics, logs, traces, alerts).
- Implement CI/CD pipelines with policy checks and automated rollbacks.
- Apply tagging standards for ownership, cost allocation, and lifecycle.

### Expected outcome

- Faster provisioning and release cycles.
- Lower operational toil.
- Better governance and traceability.

## Summary mapping table

| Domain | On-premises pain point | Cloud improvement | Business effect |
|---|---|---|---|
| Availability | Single-site dependency | Multi-zone architecture + managed HA database | Higher uptime, fewer outages |
| Scalability | Slow hardware scaling | Autoscaling + managed platform services | Better peak handling, lower latency |
| Reliability | Untested recovery process | DR architecture + IaC + recovery drills | Faster, predictable recovery |
| Security | Inconsistent controls | Central IAM, segmentation, encryption, SIEM | Lower risk, improved compliance |
| Manageability | Manual operations | Automation, observability, CI/CD governance | Faster delivery, reduced ops overhead |

## 90-day adoption plan (practical)

1. Baseline current KPIs: uptime, MTTR, deployment frequency, incident count, patch SLA.
2. Migrate non-production to cloud landing zone with IAM, network, logging, and policy guardrails.
3. Implement IaC and CI/CD for the application stack.
4. Enable managed database HA, backups, and restore testing.
5. Add autoscaling and observability dashboards with alert thresholds.
6. Run failover and incident response tabletop exercises.

## KPI targets to validate improvement

- Availability: increase service uptime target and reduce outage minutes.
- Scalability: maintain p95 latency under peak load.
- Reliability: reduce MTTR and increase restore test success rate.
- Security: improve patch compliance and reduce critical findings age.
- Manageability: reduce lead time for changes and increase deployment frequency.
