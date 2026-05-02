# Azure Storage Redundancy Comparison: LRS vs. ZRS vs. GRS vs. GZRS

## Context

A business-critical workload requires high availability, low recovery point objectives, and must balance protection against realistic failure scenarios without unnecessary cost.

## Redundancy Options Explained

### LRS — Locally Redundant Storage

Replicates data three times within a single datacenter in one region.

- Protects against: drive and rack-level failures inside one datacenter.
- Does not protect against: datacenter outage, zone outage, regional disaster.
- Availability SLA: designed for high durability within a single facility.

### ZRS — Zone-Redundant Storage

Replicates data synchronously across three Availability Zones in one region.

- Protects against: single datacenter or zone failure within the region.
- Does not protect against: full regional outage or disaster.
- Availability SLA: higher than LRS due to zone-level separation.

### GRS — Geo-Redundant Storage

Replicates data three times locally (like LRS) in a primary region, then replicates asynchronously to a secondary region.

- Protects against: full regional outage.
- Secondary region data is readable only after Microsoft initiates a failover (unless RA-GRS is enabled).
- RPO: up to 15 minutes of data loss is possible due to asynchronous replication.
- Does not protect against: simultaneous zone failures within the primary region before geo-failover.

### GZRS — Geo-Zone-Redundant Storage

Replicates data synchronously across three Availability Zones in the primary region (like ZRS), then replicates asynchronously to a secondary region (like GRS).

- Protects against: zone failure in the primary region and full regional outage.
- Combines the in-region resilience of ZRS with the cross-region recovery of GRS.
- RPO: similar to GRS for cross-region events (asynchronous replication), but near-zero RPO for zone-level failures (synchronous).
- RA-GZRS adds read access to the secondary region without waiting for failover.

## Comparison Table

| Option | Replication scope | Zone fault tolerance | Region fault tolerance | RPO (zone failure) | RPO (regional failure) | Relative cost |
|---|---|---|---|---|---|---|
| LRS | Single datacenter | No | No | High risk | High risk | Lowest |
| ZRS | 3 zones, same region | Yes | No | Near zero | High risk | Low to medium |
| GRS | Single datacenter + async secondary region | No | Yes (after failover) | High risk | Up to ~15 minutes | Medium |
| GZRS | 3 zones + async secondary region | Yes | Yes (after failover) | Near zero | Up to ~15 minutes | Highest |

## Business-Critical Workload Requirements

For a business-critical workload the key constraints are typically:

- **RPO**: must be as low as possible; data loss of more than a few minutes is not acceptable.
- **Availability**: the workload must survive zone-level failures without manual intervention or failover.
- **Budget**: cost is a constraint but protection cannot be sacrificed for savings that create unacceptable risk.

## Evaluation per Option

### LRS

Does not meet requirements. A single datacenter failure would cause data loss and outage. Not appropriate for business-critical workloads.

### ZRS

Meets zone-fault-tolerance requirements for in-region scenarios. Near-zero RPO for zone failures. Does not protect against a full regional disaster, which may be acceptable for some workloads but is a gap for truly critical ones.

### GRS

Adds cross-region protection but leaves zone-level failures unprotected in the primary region. Asynchronous replication means up to 15 minutes of data loss in a regional event. Failover to the secondary region is not automatic by default.

### GZRS

Meets all three requirements:

- Zone-level failures are handled with synchronous replication and near-zero RPO.
- Regional failures are covered by geo-replication to a secondary region.
- RA-GZRS adds read access to the secondary without waiting for failover, improving recovery flexibility.

## Recommendation

**Use GZRS (or RA-GZRS).**

| Reason | Detail |
|---|---|
| RPO | Near-zero for zone failures (synchronous). Minimized for regional events (asynchronous, typically under 15 minutes). |
| Availability | Survives zone-level failures automatically. Covers regional disasters with geo-redundancy. |
| Budget | Most expensive option, but cost increase over GRS is justified when zone-level resilience is required for business-critical data. |

If read access to the secondary region is needed during a regional outage without waiting for Microsoft-initiated failover, enable **RA-GZRS**.

## When to Accept a Lower Tier

| Scenario | Acceptable option |
|---|---|
| Workload tolerates zone outage but not regional disaster | GRS |
| Workload tolerates full regional outage and has no geo-recovery need | ZRS |
| Dev, test, or non-critical workloads only | LRS |

## Final Takeaway

For business-critical workloads where both zone-level resilience and regional disaster recovery matter, GZRS is the right choice. The additional cost compared to GRS or ZRS is justified by the combination of synchronous zone redundancy and asynchronous geo-replication, giving the lowest realistic RPO across failure scenarios.
