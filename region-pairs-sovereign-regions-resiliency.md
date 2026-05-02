# Region Pairs, Sovereign Regions, and Resiliency for Strict Data Residency

## 1) What Are Region Pairs?

In Azure, many public cloud regions are paired with another region in the same geography (for example, a primary and a secondary region). This pairing is designed to support platform-level resiliency and recovery planning.

Key characteristics:

- Regions in a pair are physically separated to reduce the chance of both being impacted by the same event.
- Platform updates are often sequenced across paired regions to reduce simultaneous risk.
- Several Azure services provide built-in disaster recovery patterns that can use the paired region.

Important note: Region pairs improve resilience, but they may still create data-residency concerns if replication crosses a boundary your regulation does not allow.

## 2) What Are Sovereign Regions?

Sovereign regions (or sovereign clouds) are Azure environments created for governments or highly regulated sectors, with stronger control boundaries for data location, operations, and compliance.

Typical characteristics:

- Data residency and processing controls are stricter than in the global public cloud.
- Operational boundaries, identity integration, and service availability can differ from global Azure.
- Compliance frameworks and accreditation targets are tailored to national/regulatory requirements.

Examples include national cloud environments where legal or regulatory rules require data to stay under a specific jurisdiction and operational model.

## 3) Region Pairs vs. Sovereign Regions

- Region pairs are a resiliency concept in Azure geographies.
- Sovereign regions are a compliance/jurisdiction concept.
- For strict residency, compliance boundaries take priority over convenience of cross-region replication.

## 4) Recommended Resiliency Approach for Strict Data Residency

Assumption: The workload must keep customer data inside one legal jurisdiction, and cross-border replication is not permitted.

### A. Primary Pattern

Use a two-layer resiliency model:

1. High availability inside one allowed region:
- Deploy across Availability Zones (zone-redundant architecture).
- Use zone-redundant services where available (compute, storage, database options).

2. Disaster recovery only to an allowed secondary region in the same residency boundary:
- Use an approved in-country/in-boundary secondary region (or sovereign counterpart) only if regulation permits.
- If no allowed secondary region exists, design for in-region DR plus immutable backups and rapid rebuild.

### B. Data Strategy

- Keep primary data stores in-region with customer-managed keys if required.
- Use geo-replication only when the target region is explicitly allowed by policy.
- Prefer zone-redundant storage/database options before enabling cross-region replication.
- Encrypt data at rest and in transit, and control key residency.

### C. Network and Access Controls

- Private endpoints and private networking paths.
- Restrict egress to approved destinations.
- Centralized policy controls to block non-approved regions.

### D. Governance and Guardrails

- Azure Policy at management-group/subscription scope:
  - Allowed locations policy.
  - Deny creation of resources outside approved regions.
  - Enforce tagging (`DataResidency`, `Classification`, `Owner`).
- Separate subscriptions for regulated workloads.
- Continuous compliance monitoring and evidence retention.

### E. Backup and Recovery

- Use immutable backups retained in the same residency boundary.
- Define clear RTO/RPO targets per tier:
  - Tier 1: very low RTO/RPO with zone redundancy.
  - Tier 2: moderate RTO/RPO with restore-based DR.
- Run regular failover/failback and restore drills.

## 5) Practical Decision Flow

1. Confirm legal residency boundary (country/jurisdiction).
2. Identify Azure regions (or sovereign cloud) that are compliant.
3. Determine whether cross-region replication within that boundary is allowed.
4. If yes, use zone-redundant primary + compliant secondary DR region.
5. If no, use strong intra-region HA + immutable in-boundary backups + tested rebuild automation.

## 6) Final Recommendation

For strict data residency workloads, prioritize:

1. Sovereign or jurisdiction-compliant region selection first.
2. Availability Zones for day-to-day resilience.
3. Cross-region DR only when the second region is explicitly compliant.
4. Policy-based enforcement to prevent accidental out-of-boundary deployments.

This approach balances resiliency and regulatory compliance without violating residency constraints.
