# Azure Storage Service Selection: Four Workload Patterns

## Services Covered

- Azure Blob Storage
- Azure Files
- Azure Disk Storage
- Azure Queue Storage
- Azure Table Storage (included as relevant alternative)

## Workload 1: Media Asset Archive (Images and Videos)

### Pattern

A media company stores large volumes of images, video files, and rendered assets. Files are uploaded frequently and must be retained long-term. Access is mostly read-heavy and can tolerate some latency.

### Recommended Service: Azure Blob Storage

| Criterion | Justification |
|---|---|
| Performance | Blob Storage supports large object sizes and high-throughput reads. Tiering (Hot, Cool, Archive) allows balancing access speed vs. cost. |
| Durability | Locally redundant, zone-redundant, and geo-redundant options available. Data is highly durable with multiple replicas. |
| Access requirements | HTTP/HTTPS access, integration with CDN for global delivery, lifecycle policies for automatic tiering to cheaper storage over time. |

**Why not the others:** Azure Files is designed for file shares, not object-scale media. Disk Storage is for VM-attached block storage. Queue and Table are not suitable for large binary objects.

## Workload 2: Shared Network Drive for Office Applications

### Pattern

An organization needs a shared file system that multiple users and applications can mount as a network drive. Teams use it to read and write documents collaboratively across Windows and Linux machines.

### Recommended Service: Azure Files

| Criterion | Justification |
|---|---|
| Performance | Supports SMB and NFS protocols. Standard and Premium tiers match different throughput needs. |
| Durability | Data is replicated with LRS, ZRS, or GRS options. Snapshots are available for recovery. |
| Access requirements | Can be mounted directly as a drive letter on Windows or Linux. Integrates with on-premises via Azure File Sync. No application code changes needed. |

**Why not the others:** Blob Storage requires API access and does not support native drive mounting. Disk Storage is attached per VM and cannot be shared across multiple machines simultaneously.

## Workload 3: High-Performance Database for a Virtual Machine

### Pattern

A company runs a SQL Server instance on a VM. The database requires low-latency disk I/O, consistent throughput, and dedicated storage that is attached directly to the VM.

### Recommended Service: Azure Disk Storage (Managed Disks)

| Criterion | Justification |
|---|---|
| Performance | Premium SSD and Ultra Disk options deliver low latency and high IOPS. Well-suited for transaction-intensive database workloads. |
| Durability | Managed disks provide built-in replication within the region. Snapshots and Azure Backup are available for recovery. |
| Access requirements | Block storage attached to a specific VM. The OS and application treat it as a local disk with no protocol translation required. |

**Why not the others:** Blob and Files are object/file storage, not block storage. Queue and Table have no role in VM disk I/O.

## Workload 4: Decoupled Message Queue for Microservices

### Pattern

An e-commerce platform has an order processing system where the web front end places orders into a queue, and a backend worker independently processes them. The two components need to be decoupled so that bursts in orders do not overwhelm the backend.

### Recommended Service: Azure Queue Storage

| Criterion | Justification |
|---|---|
| Performance | Designed for large numbers of small messages. Supports high-throughput enqueue and dequeue operations at scale. |
| Durability | Messages are stored durably and replicated. Messages can be configured with visibility timeouts and retry handling. |
| Access requirements | Simple HTTP-based API. Easy to integrate with any language. Native integration with Azure Functions and Logic Apps for event-driven processing. |

**Why not the others:** Blob and Disk are not designed for message delivery or queue semantics. Azure Service Bus is a stronger alternative for advanced messaging scenarios, but Queue Storage is sufficient for simple decoupling at scale.

## Summary Table

| Workload | Service | Performance Fit | Durability | Access Model |
|---|---|---|---|---|
| Media asset archive | Azure Blob Storage | High throughput for large objects, tiered for cost | LRS / ZRS / GRS, lifecycle policies | HTTP/HTTPS, CDN |
| Shared network drive | Azure Files | SMB/NFS with standard and premium tiers | LRS / ZRS / GRS, snapshots | Mount as drive, File Sync |
| VM-attached database | Azure Disk Storage | Premium SSD / Ultra Disk for low latency IOPS | Regional replication, snapshots | Block device on VM |
| Microservice message queue | Azure Queue Storage | High-throughput small messages | Durable, replicated, retry support | HTTP API, Functions, Logic Apps |

## Selection Principles

- Choose the storage type that matches the **data shape**: objects, files, blocks, or messages.
- Consider **access protocol** first: does the workload need a file path, a drive letter, a block device, or an API?
- Evaluate **performance tier** based on latency and IOPS requirements.
- Match **redundancy options** to availability and compliance requirements.
- Factor in **cost**: Archive blob tiers and Queue Storage are cheap; Ultra Disk is expensive but necessary for extreme IOPS workloads.
