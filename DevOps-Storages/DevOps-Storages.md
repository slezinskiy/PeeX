# DevOps - Storages

[⬅ Назад до README](../README.md)

| # | Питання | Рівень | Інфраструктура |
|---|---------|--------|----------------|
| STG-1 | [Implement and manage data archiving and backup strategies](#stg-1-implement-and-manage-data-archiving-and-backup-strategies-middle) | Middle | 📘 NEBo Task |
| STG-2 | [Configure and control permissions for storage resources](#stg-2-configure-and-control-permissions-for-storage-resources) | | 🤖 Згенеровано Claude |
| STG-3 | [Implement and automate data migration](#stg-3-implement-and-automate-data-migration) | | 🤖 Згенеровано Claude |
| STG-4 | [Select and apply diverse storage services for workload needs](#stg-4-select-and-apply-diverse-storage-services-for-workload-needs) | | 🤖 Згенеровано Claude |

> **Легенда:** 📘 NEBo Task — офіційний таск із вихідного файлу; 🤖 Згенеровано Claude — таск створено Claude за зразком NEBo Tasks, це не офіційний матеріал.

---

## STG-1. Implement and manage data archiving and backup strategies (Middle)

**Description**

Configure and maintain data archiving, backups, and versioning in storage services, ensuring alignment with retention policies and recovery requirements. Use automation tools, pipelines, or scheduling to streamline backup and archive operations.

**Outcomes**

- Implemented archiving, versioning, and backup solutions for storage services, supported by automation scripts, pipelines, or built-in service features (e.g., lifecycle policies, cloud backup services).

### 🏗️ Інфраструктура — 📘 NEBo Task: DevOps - Implement and manage data archiving and backup strategies

**Task Description**

This task covers configuring and maintaining data archiving, backup, and versioning solutions aligned with retention policies and recovery requirements. It includes implementing lifecycle policies, configuring automated backups, setting up snapshot schedules, and validating recovery procedures. The solution must demonstrate comprehensive data protection practices including automated retention, archive transitions, restore validation, and operational readiness

**Prerequisites**

- Access to cloud provider account (AWS, Azure, GCP) or on-premise infrastructure
- Existing storage resources (S3 buckets, EBS volumes, Azure Storage, GCP buckets)
- Understanding of backup concepts (RPO, RTO, retention policies)
- CLI tools for chosen platform
- Test data for backup/restore validation
- Understanding of storage tiers and lifecycle management

**Guidance and Tips**

- Environment: Use cloud provider free tier with lifecycle policies and backup services
- Time Planning: Plan around 10-12 hours for backup configuration, lifecycle setup, and recovery testing
- Scope: Implement automated backups for at least 2 resource types (e.g., S3 and EBS, or Blob and VM)
- Tips: Test restore procedures thoroughly. Document RPO/RTO requirements. Automate backup scheduling. Use lifecycle policies for cost optimization. Verify backup integrity. Document recovery procedures clearly

**Acceptance Criteria**

Functional Requirements

- An automated backup solution is configured for at least two different resource types (for example, storage, databases, file shares).
- Backup schedules follow a defined retention policy specifying frequency and duration.
- Lifecycle policies automatically transition data into archival or lower-cost tiers based on retention rules.
- Successful backup creation is verified for each protected resource type.
- A successful restore from backup is demonstrated, validating data integrity and completeness.
- Backup monitoring or alerting is configured to detect failures or anomalies.
- Recovery procedures are fully documented and tested end-to-end

Non-Functional Requirements

- Backup configurations align with RPO (Recovery Point Objective) and RTO (Recovery Time Objective) requirements.
- Cost optimization is applied through appropriate selection of storage tiers and retention periods.
- Backup integrity verification is enabled, ensuring backups are usable before retention expiration.
- Retention policies adhere to compliance or organizational requirements.
- Recovery procedures are documented in a clear and reproducible manner.
- Automation minimizes manual intervention in both backup and retention workflows.

**Outcome Artifacts**

- Backup plan/policy configuration showing schedule and retention
- Lifecycle policy configuration for archive transitions
- Screenshots of successful backup creation
- Screenshots showing data in archive storage tier
- Terminal output or screenshots of successful restore operation
- Backup monitoring dashboard or alerts configuration
- Test results showing recovered data integrity
- Documentation of RPO/RTO targets and how they're met
- Short README documenting: backup strategy, retention policies, recovery procedures, and testing results

**Vendor Annex**

AWS Backup & S3 Lifecycle
- Typical Tools: AWS Backup, S3 Lifecycle, EBS Snapshots, AWS CLI
- References: AWS Backup Documentation, S3 Lifecycle Policies, EBS Snapshots
- Key Commands:
  - `aws s3api put-bucket-lifecycle-configuration ...`
  - `aws s3api get-bucket-lifecycle-configuration ...`
  - `aws ec2 create-snapshot --volume-id <id>`
  - `aws backup create-backup-plan --backup-plan ...`
  - `aws ec2 copy-snapshot ...`

Azure Backup
- Typical Tools: Azure Backup, Azure Storage Lifecycle, Azure CLI
- References: Azure Backup Documentation, Blob Lifecycle Management
- Key Commands:
  - `az backup vault create ...`
  - `az backup protection enable-for-vm ...`
  - `az backup protection backup-now ...`

GCP Backup & Archive
- Typical Tools: GCP Cloud Storage, Persistent Disk Snapshots, gcloud CLI
- References: GCP Storage Lifecycle, Compute Engine Snapshots
- Key Commands:
  - `gsutil lifecycle set lifecycle.json gs://<bucket>`
  - `gcloud compute disks snapshot <disk> --zone <zone>`
  - `gcloud compute resource-policies create snapshot-schedule <name>`

On-Prem / Generic Backup
- Typical Tools: BorgBackup, Restic, Veeam, rsync, cron
- References: Restic Documentation, BorgBackup Documentation
- Key Commands:
  - `restic backup <path>`
  - `restic snapshots`
  - `rsync -avh <src> <dest>`
  - `crontab -e`

---

## STG-2. Configure and control permissions for storage resources

**Description**

Set up and manage access controls for storage services by defining object- or bucket-level permissions based on usage requirements. Configure ACLs or equivalent policies to ensure secure and controlled access to storage resources.

**Outcomes**

- _(У вихідному файлі Outcomes для цього питання відсутні.)_

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Design and implement an access model for object storage used by several consumers with different needs: an application (read/write to its prefix), an analytics role (read-only), an external partner (time-limited access to specific objects), a public static asset path (read via CDN only), and administrators. Implement bucket- and object/prefix-level permissions with policies (and ACLs only where justified), encryption key policies, and verify allowed/denied access.

**Prerequisites**

- Cloud account with object storage (S3 / Azure Blob / GCS)
- Terraform
- Test identities (IAM roles/users, service accounts)

**Guidance and Tips**

- Time Planning: 6–8 hours.
- Tips: On AWS prefer "Bucket owner enforced" (ACLs disabled) and use bucket policies + IAM; explain when ACLs are still needed. Keep Block Public Access on and serve public content via CloudFront OAC. Use presigned URLs / SAS tokens for temporary access. Restrict by `aws:SourceVpce` / `aws:PrincipalOrgID` where applicable.

**Acceptance Criteria**

Functional Requirements

- At least 4 access patterns are implemented: application RW on prefix, read-only role, temporary external access (presigned URL / SAS), admin.
- Bucket policy enforces TLS (`aws:SecureTransport`) and encryption (SSE-KMS) on upload.
- Block Public Access is enabled; public content served only through CDN with origin access control.
- KMS key policy restricts decrypt to the intended principals.
- Access from a specific VPC endpoint or network only (for internal bucket).
- Allowed and denied operations are tested and documented for each persona.
- Access analysis performed (IAM Access Analyzer for S3 / storage account access review).

Non-Functional Requirements

- All permissions are defined as code.
- Server access logs or CloudTrail data events are enabled for audit.
- Least privilege — no `s3:*` on `*` for non-admin roles.

**Outcome Artifacts**

- Terraform code: buckets, bucket policies, IAM policies, KMS key policy, access points (optional)
- Permission matrix (persona × prefix × action)
- Test outputs (AccessDenied / success), presigned URL expiry demonstration
- Access Analyzer findings
- Short README: access model and change procedure

**Vendor Annex**

- AWS: S3 bucket policies, IAM, S3 Access Points, Object Ownership, Block Public Access, presigned URLs, KMS, CloudFront OAC, IAM Access Analyzer
- Azure: Azure RBAC data roles, SAS tokens, stored access policies, private endpoints
- GCP: Uniform bucket-level access, IAM conditions, signed URLs
- Key Commands: `aws s3 presign s3://bucket/key --expires-in 300`, `aws s3api put-bucket-policy ...`, `aws s3api put-public-access-block ...`

---

## STG-3. Implement and automate data migration

**Description**

Configure automated processes to migrate data from on-premises or other storage systems to the cloud.

**Outcomes**

- Delivered scripts or automated pipelines for data migration, with documented tasks covering transfers from on-premises to cloud storage environments.

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Migrate a dataset from an on-premises (or simulated on-prem) file storage (NFS/SMB share or local disk on a VM) to cloud object/file storage with an automated, repeatable process: initial bulk transfer, scheduled incremental syncs, integrity verification, logging/reporting, and a final cutover. Implement it both with a managed transfer service and/or a script-based pipeline.

**Prerequisites**

- Source: VM with NFS/SMB share or local directory with test data (≥ several GB, many files, nested folders) — can be simulated with a VM in another VPC / VirtualBox
- Target: S3 / EFS / FSx (or Azure Blob / Files, GCS)
- Transfer tool: AWS DataSync agent / Storage Gateway / `aws s3 sync` / rclone / AzCopy
- Scheduler or pipeline (cron, systemd timer, CI scheduled job, EventBridge)

**Guidance and Tips**

- Time Planning: 8–10 hours.
- Tips: Separate initial load and incremental sync. Verify with checksums (DataSync verification or manifest with SHA256). Preserve metadata where needed. Limit bandwidth to avoid impacting production. Log transferred/failed files in structured format.

**Acceptance Criteria**

Functional Requirements

- Migration plan: data inventory, size, change rate, transfer window, cutover approach.
- Automated initial transfer from on-prem source to cloud target.
- Scheduled incremental syncs transfer only changed files.
- Integrity verification after each run (checksums / object counts / sizes).
- Failures are retried and reported; run summary is produced (files, bytes, duration, errors).
- Final cutover executed and source set to read-only.

Non-Functional Requirements

- Data encrypted in transit and at rest.
- Transfer credentials are least-privilege and not hardcoded.
- Bandwidth throttling configured.
- Process is idempotent and safe to re-run.

**Outcome Artifacts**

- Migration scripts / DataSync task configuration (Terraform)
- Scheduler / pipeline definition
- Run logs and summary reports for initial and incremental runs
- Verification report (checksums, counts)
- Runbook: migration steps, cutover, rollback

**Vendor Annex**

- AWS: DataSync (agent, location, task, verification), Storage Gateway, Snow Family (large volumes), Transfer Family
- Azure: AzCopy, Azure Data Box, Storage Mover
- GCP: Storage Transfer Service (on-prem agents)
- Generic: rclone, rsync, restic
- Key Commands: `aws datasync start-task-execution --task-arn ...`, `aws s3 sync /data s3://bucket/data --exact-timestamps`, `rclone sync /data s3:bucket/data --checksum --bwlimit 50M`

---

## STG-4. Select and apply diverse storage services for workload needs

**Description**

Select different storage types based on workload purpose, such as archives, backups, images, or static website content.

**Outcomes**

- Implemented storage solutions with different storage types (e.g., regular, archive, internal, or internet-facing) tailored to workload and project needs.

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

For a sample application, analyse storage requirements and implement at least four different storage types, each chosen for its workload: block storage for VM/database, shared file storage for containers, object storage for application assets, internet-facing static website, internal-only storage, and archive storage for backups/logs. Document the selection with a comparison of performance, durability, access pattern and cost.

**Prerequisites**

- Cloud account
- Terraform
- Sample application (web + containers/VMs) and sample data (images, logs, backups)

**Guidance and Tips**

- Time Planning: 8–10 hours.
- Tips: Build a decision matrix (access pattern, latency, throughput/IOPS, sharing, durability, retention, cost/GB, retrieval cost). Use lifecycle to move data automatically between tiers. Internet-facing ≠ public bucket: use CDN with OAC.

**Acceptance Criteria**

Functional Requirements

- At least 4 storage types implemented, for example:
  - Block: EBS gp3 for VM / database (with sizing of IOPS/throughput)
  - File: EFS for shared container volumes (Kubernetes PVC via CSI)
  - Object (internal): private S3 bucket accessed via VPC endpoint
  - Object (internet-facing): static website on S3 + CloudFront with TLS
  - Archive: S3 Glacier Flexible / Deep Archive for backups/logs
- Each storage is used by the workload (mounted, uploaded, served).
- Lifecycle policies transition data between tiers.
- Selection rationale and cost estimation documented.

Non-Functional Requirements

- Encryption at rest everywhere; TLS for internet-facing.
- Internal storage is not reachable from the internet.
- Infrastructure is defined as code.

**Outcome Artifacts**

- Decision matrix and architecture diagram
- Terraform code for all storage resources
- Evidence of usage (mounted volumes, website URL, archive objects storage class)
- Cost estimate (AWS Pricing Calculator / Infracost)
- Short README

**Vendor Annex**

- AWS: EBS, EFS, FSx, S3 storage classes (Standard, IA, Intelligent-Tiering, Glacier), S3 static website + CloudFront, instance store
- Azure: Managed Disks, Azure Files, Blob tiers (Hot/Cool/Cold/Archive), Static website on Storage
- GCP: Persistent Disk, Filestore, Cloud Storage classes
- Key Commands: `aws s3 website s3://bucket --index-document index.html`, `aws s3 cp backup.tar s3://archive/ --storage-class DEEP_ARCHIVE`, `infracost breakdown --path .`
