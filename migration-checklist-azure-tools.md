# Azure Migration Checklist: Choosing the Right Tool for Each Phase

## Tools Covered

| Tool | Primary purpose |
|---|---|
| Azure Migrate | Discover, assess, and migrate on-premises VMs, servers, databases, and apps |
| Azure Data Box | Offline bulk data transfer when network bandwidth is insufficient |
| AzCopy | Command-line bulk copy to and from Azure Storage over the network |
| Azure Storage Explorer | GUI-based manual file and blob management |
| Azure File Sync | Ongoing hybrid sync between on-premises file servers and Azure Files |

---

## Phase 1: Discovery and Assessment

**Goal:** Understand what exists on-premises before migrating anything.

- [ ] Install the **Azure Migrate** appliance in the on-premises environment.
- [ ] Run discovery to inventory VMs, physical servers, and dependencies.
- [ ] Complete the assessment to receive sizing recommendations and estimated Azure costs.
- [ ] Review compatibility reports and flag workloads that need remediation.
- [ ] Identify databases for migration using the Azure Migrate database assessment tool.
- [ ] Group workloads by migration wave based on priority and complexity.

**Tool used:** Azure Migrate

**Why:** Azure Migrate is the central hub for discovery, dependency analysis, and sizing recommendations before any move begins.

---

## Phase 2: Large-Scale Offline Data Transfer

**Goal:** Move large volumes of data (typically above 10 TB) when network bandwidth is limited or transfer time is impractical.

- [ ] Calculate total data volume and estimated network transfer time.
- [ ] If transfer time exceeds acceptable limits, order an **Azure Data Box** device.
- [ ] Copy data from on-premises NAS or file servers to the Data Box.
- [ ] Ship the device back to Microsoft.
- [ ] Confirm data ingestion into the target Azure Storage account.
- [ ] Validate file counts and integrity after ingestion.

**Tool used:** Azure Data Box

**Why:** Data Box eliminates dependency on network speed for massive datasets. It is the right choice when online transfer would take weeks or longer.

---

## Phase 3: Online Bulk File and Blob Transfer

**Goal:** Copy files, blobs, or objects to Azure Storage over the network for datasets that fit within available bandwidth.

- [ ] Install or verify **AzCopy** is available on the source machine.
- [ ] Authenticate using a SAS token or Azure AD credentials.
- [ ] Run a dry-run or benchmark copy to estimate transfer time.
- [ ] Execute the copy command for the source directory or storage container.
- [ ] Use the `--recursive` and `--overwrite` flags as needed.
- [ ] Review the job log for failed or skipped files.
- [ ] Re-run for any failed transfers.

**Tool used:** AzCopy

**Why:** AzCopy is optimized for high-throughput scripted transfers. It handles large numbers of files and supports resume, making it reliable for automated migration pipelines.

---

## Phase 4: Manual Verification, Small Transfers, and Ad Hoc Management

**Goal:** Inspect, upload, or move specific files or containers manually during or after migration.

- [ ] Open **Azure Storage Explorer** and connect to the target storage account.
- [ ] Verify that expected files and containers were created correctly.
- [ ] Upload any remaining small files or configuration assets manually.
- [ ] Manage access tiers, metadata, or SAS tokens as needed.
- [ ] Use drag-and-drop to handle edge cases or exceptions.
- [ ] Confirm file properties and permissions before cutover.

**Tool used:** Azure Storage Explorer

**Why:** Storage Explorer provides a visual interface for spot-checking, exception handling, and small-scale operations that do not justify scripting.

---

## Phase 5: VM and Workload Migration

**Goal:** Lift and shift virtual machines and workloads from on-premises to Azure.

- [ ] In **Azure Migrate**, select the assessed VMs for replication.
- [ ] Configure the replication settings (region, disk type, network).
- [ ] Start initial replication and monitor progress.
- [ ] Run a test migration in an isolated network to validate functionality.
- [ ] Fix any configuration or connectivity issues found during the test.
- [ ] Schedule the cutover during an approved maintenance window.
- [ ] Execute the final cutover and verify the VM is running in Azure.
- [ ] Decommission the on-premises VM after validation.

**Tool used:** Azure Migrate

**Why:** Azure Migrate handles the full replication, test migration, and cutover workflow for VMs. It minimizes downtime by keeping on-premises and cloud VMs in sync until cutover.

---

## Phase 6: Hybrid File Server Sync (Ongoing or Transition)

**Goal:** Keep on-premises file servers and Azure Files in sync during a transition period, or permanently for a hybrid model.

- [ ] Create an Azure Files share in the target storage account.
- [ ] Install the **Azure File Sync** agent on the on-premises file server.
- [ ] Register the server with the Storage Sync Service.
- [ ] Create a sync group and add the cloud endpoint (Azure Files share).
- [ ] Add the on-premises server endpoint (folder path).
- [ ] Monitor initial sync completion and verify file consistency.
- [ ] Enable cloud tiering if local disk space needs to be managed.
- [ ] Validate that users can access files from both on-premises and cloud paths.
- [ ] Plan cutover to cloud-only access if full migration is the end goal.

**Tool used:** Azure File Sync

**Why:** File Sync enables a gradual transition for file server workloads. It keeps both sides synchronized, supports cloud tiering, and avoids forcing an immediate hard cutover for users.

---

## Tool Selection Summary

| Migration need | Best tool |
|---|---|
| Discover and assess on-premises environment | Azure Migrate |
| Migrate VMs to Azure | Azure Migrate |
| Transfer large data volumes offline (>10 TB or limited bandwidth) | Azure Data Box |
| Bulk online file/blob transfer via script or pipeline | AzCopy |
| Manual inspection, small uploads, and ad hoc management | Azure Storage Explorer |
| Hybrid or gradual file server migration with ongoing sync | Azure File Sync |

---

## General Migration Checklist (All Phases)

- [ ] Define migration scope and include/exclude list.
- [ ] Set RTO and RPO targets per workload.
- [ ] Establish rollback plan for each wave.
- [ ] Assign ownership per workload and migration phase.
- [ ] Confirm networking, DNS, and firewall readiness in Azure.
- [ ] Validate identity and access configuration before cutover.
- [ ] Run post-migration smoke tests for each workload.
- [ ] Update monitoring and backup policies for migrated resources.
- [ ] Decommission on-premises resources only after validation is complete.
