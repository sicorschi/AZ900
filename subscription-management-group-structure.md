# Subscription and Management-Group Structure for Multi-Department and Compliance-Boundary Organizations

## Design Goals

1. Isolate workloads by department, environment, and compliance scope.
2. Apply governance at scale through Azure Policy, RBAC, and budgets.
3. Reduce blast radius (security, cost, and operations).
4. Keep onboarding consistent using repeatable landing-zone patterns.

## Recommended Management-Group Hierarchy

```text
Tenant Root Group
├── Platform
│   ├── Identity
│   ├── Connectivity
│   └── Management
├── Sandbox
├── Corp (Non-Regulated)
│   ├── Finance
│   ├── HR
│   ├── Sales
│   └── Engineering
└── Regulated
    ├── PCI
    ├── PHI
    └── Sovereign
```

## How to Place Subscriptions

Use a consistent pattern per department and per environment:

- `sub-<dept>-dev`
- `sub-<dept>-test`
- `sub-<dept>-prod`

For regulated workloads, keep separate production subscriptions under dedicated compliance management groups:

- `sub-finance-prod-pci` -> under `Regulated/PCI`
- `sub-health-prod-phi` -> under `Regulated/PHI`
- `sub-gov-prod-sov` -> under `Regulated/Sovereign`

Platform subscriptions are separated from app/workload subscriptions:

- `sub-platform-identity`
- `sub-platform-connectivity`
- `sub-platform-management`

## Governance Model by Layer

### Tenant Root Group

- Global guardrails only (minimum baseline).
- Core policies: approved locations, required tags, diagnostic settings baseline.

### Platform

- Shared services governance.
- Strict RBAC and change control.
- Policies for networking, identity hardening, and logging standards.

### Corp (Non-Regulated)

- Department autonomy with centralized guardrails.
- Standard policy set: backup, monitoring, allowed SKUs, naming/tagging.

### Regulated

- Compliance-specific controls and evidence collection.
- Stronger policies: customer-managed keys, private endpoints, restricted regions.
- Dedicated security operations and audit retention settings.

### Sandbox

- Innovation zone with time-boxed, lower-risk controls.
- Budget caps and auto-cleanup policies.
- No production data allowed.

## Policy and RBAC Recommendations

1. Assign compliance policies at management-group level, not individually per subscription.
2. Use least privilege RBAC with department-scoped groups.
3. Separate duties:
   - Platform team: Owner on Platform subscriptions.
   - Department ops: Contributor on their own non-prod/prod subscriptions.
   - Security/compliance: Reader + Policy Insights + Security roles across regulated groups.
4. Enforce mandatory tags:
   - `CostCenter`
   - `DataClassification`
   - `Environment`
   - `Owner`
5. Use management locks and Azure Policy exemptions with approval workflow.

## Budget and Cost Segmentation

- Budget per subscription (department + environment).
- Cost alerts at 50/75/90/100%.
- Showback/chargeback by tag and subscription.
- Keep shared platform costs isolated in Platform subscriptions.

## Networking and Connectivity Pattern

- Central hub network in `sub-platform-connectivity`.
- Spokes per department subscription.
- Regulated spokes separated and tightly controlled (NSGs, firewall, private DNS, egress rules).

## Example Mapping (Condensed)

| Department | Compliance Needs | Management Group | Subscriptions |
|---|---|---|---|
| Finance | PCI | Regulated/PCI | `sub-finance-dev`, `sub-finance-test`, `sub-finance-prod-pci` |
| HR | PHI | Regulated/PHI | `sub-hr-dev`, `sub-hr-test`, `sub-hr-prod-phi` |
| Sales | Non-regulated | Corp/Sales | `sub-sales-dev`, `sub-sales-test`, `sub-sales-prod` |
| Engineering | Mixed | Corp/Engineering + Regulated (as needed) | `sub-eng-dev`, `sub-eng-test`, `sub-eng-prod`, regulated prod subscriptions if required |

## Why This Structure Works

- Clear governance inheritance from management groups.
- Strong compliance isolation without creating excessive complexity.
- Better operational ownership by department.
- Predictable policy, security, and cost management at scale.
