# Financial Comparison: CapEx vs OpEx for Cloud Migration

## Scenario

A company runs a line-of-business application currently hosted on-premises and is evaluating a move to a consumption-based cloud model.

Assumptions for a 3-year period:

- Current on-premises environment requires server refresh now.
- Cloud target uses pay-as-you-go services sized to current demand.
- Values are illustrative for decision-making practice (AZ-900 style), not vendor quotes.

## 1) Cost structure comparison

| Cost Category | On-Premises (CapEx-heavy) | Consumption Cloud (OpEx-heavy) |
|---|---|---|
| Hardware purchase | Large upfront capital outlay in Year 0 | None upfront |
| Software licenses | Often upfront + annual support | Usually included in service pricing or subscription |
| Datacenter facilities | Power, cooling, rack space | Included in provider pricing |
| IT operations labor | Higher for patching, hardware lifecycle, break/fix | Lower for hardware tasks; focus shifts to governance/optimization |
| Scalability cost | Buy for peak, often overprovisioned | Scale up/down with usage |
| Cash flow profile | Spiky, front-loaded | Smoother, periodic operating expense |
| Financial treatment | Capitalized asset + depreciation | Expense as incurred |

## 2) Example 3-year financial view

### A) On-premises (CapEx-heavy)

- Initial hardware/network/storage purchase: $240,000 (Year 0)
- Initial software licenses: $60,000 (Year 0)
- Implementation/migration project: $30,000 (Year 0)
- Annual maintenance/support contracts: $45,000 per year
- Power/cooling/facility allocation: $18,000 per year
- Additional admin/ops labor allocation: $40,000 per year

3-year total cost:

- Upfront total: $330,000
- Recurring total: 3 x ($45,000 + $18,000 + $40,000) = $309,000
- 3-year TCO: $639,000

### B) Cloud consumption model (OpEx-heavy)

- Upfront migration and redesign effort: $70,000 (one-time)
- Cloud services (compute, storage, database, networking): $12,500 per month average
- Cloud support plan and monitoring tools: $2,000 per month
- Cloud operations/FinOps labor allocation: $18,000 per year

3-year total cost:

- Upfront total: $70,000
- Recurring cloud services: 36 x ($12,500 + $2,000) = $522,000
- Cloud labor: 3 x $18,000 = $54,000
- 3-year TCO: $646,000

## 3) Interpretation

In this example:

- 3-year TCO is similar (On-prem: $639,000 vs Cloud: $646,000).
- The key difference is cash-flow shape and risk profile:
  - On-prem requires high upfront investment and capacity planning risk.
  - Cloud reduces upfront commitment and improves elasticity, but can become expensive without cost governance.

## 4) Why organizations still choose cloud when TCO is close

- Faster provisioning and time-to-market.
- Better elasticity for variable demand.
- Improved business continuity options.
- Access to managed services that reduce delivery time for new features.

## 5) Decision checklist (finance + technology)

Use cloud OpEx if these are true:

- Demand is variable or hard to forecast.
- Preserving cash and avoiding large upfront CapEx is important.
- The business values speed and experimentation.
- You can enforce cost controls (budgets, rightsizing, reservations/savings plans, lifecycle policies).

Keep or use CapEx/private hosting if these are true:

- Workload is highly stable at high utilization for years.
- You already own depreciated infrastructure with spare capacity.
- Regulatory or technical constraints strongly limit public cloud use.

## 6) AZ-900 exam takeaway

CapEx vs OpEx is not only about total dollars. It is also about:

- Timing of spend (upfront vs periodic),
- Flexibility (fixed capacity vs elastic consumption),
- Operational responsibility and business agility.
