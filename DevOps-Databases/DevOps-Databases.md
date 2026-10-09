# DevOps - Databases

[⬅ Назад до README](../README.md)

| # | Питання | Key | Інфраструктура |
|---|---------|-----|----------------|
| DB-1 | [Configure and maintain in-memory data solutions](#db-1-configure-and-maintain-in-memory-data-solutions) | | 📘 NEBo Task |
| DB-2 | [Implement and support automated database migration processes](#db-2-implement-and-support-automated-database-migration-processes) | | 🤖 Згенеровано Claude |
| DB-3 | [Maintain and troubleshoot database services](#db-3-maintain-and-troubleshoot-database-services) | | 🤖 Згенеровано Claude |

> **Легенда:** 📘 NEBo Task — офіційний таск із вихідного файлу; 🤖 Згенеровано Claude — таск створено Claude за зразком NEBo Tasks, це не офіційний матеріал.

---

## DB-1. Configure and maintain in-memory data solutions

**Description**

Provision and configure in-memory data services such as Redis, Cassandra, or cloud-native caching solutions with the required network and service settings. Manage user access, permissions, and integrations to support high-performance data processing and caching needs.

**Outcomes**

- Provisioned and maintained in-memory services with proper configuration, user management, and connectivity for application workloads. (e.g. Redis, Cassandra, Azure Cache, Memorystore).

### 🏗️ Інфраструктура — 📘 NEBo Task: DevOps - Configure and maintain in-memory data solutions

**Task Description**

This task covers provisioning and configuring in-memory data services such as Redis, Memcached, or cloud-native caching solutions. It includes configuring network access restrictions, authentication, persistence options (where applicable), security controls, and application integration for high-performance caching and data processing needs. The solution must demonstrate correct deployment, secure access, and successful operation of an in-memory data service

**Prerequisites**

- Access to cloud provider account or local infrastructure
- Understanding of caching concepts and in-memory databases
- Redis CLI or equivalent client tools
- Basic understanding of key-value operations
- Sample application for cache integration (optional)
- Network configuration knowledge

**Guidance and Tips**

- Environment: Use cloud provider free-tier or trial (Azure Cache for Redis, AWS ElastiCache, GCP Memorystore) or local Redis installation
- Time Planning: Plan around 6-8 hours for provisioning, configuration, testing, and connection validation
- Scope: Deploy Redis or Memcached, configure authentication, test basic operations, configure network access
- Tips: Understand difference between Redis and Memcached. Enable authentication/TLS for Redis. Test connection from application. Monitor cache hit rates. Document use cases and configuration

**Acceptance Criteria**

Functional Requirements

- In-memory database instance provisioned (Redis, Memcached, or equivalent)
- Database accessible only from authorized sources (VPC/VNet restriction)
- Authentication configured and tested
- Successful connection from authorized client
- Basic operations verified (SET, GET, DEL, INCR)
- Data persistence configured (if using Redis)
- Connection tested from application or CLI client

Non-Functional Requirements

- Network access properly restricted
- Authentication required for connections
- TLS/SSL enabled for data in transit
- Appropriate instance sizing for workload
- Monitoring enabled for performance metrics

**Outcome Artifacts**

- Screenshots of provisioned in-memory database showing configuration
- Network configuration showing restricted access
- Authentication/security configuration
- Terminal output showing successful connection
- Commands demonstrating basic operations (SET, GET, etc.)
- Connection string documentation (credentials redacted)
- Performance metrics or monitoring configuration
- Short README documenting: database type, use cases, connection details, and operational procedures

**Vendor Annex**

AWS ElastiCache (Redis)
- Typical Tools: AWS ElastiCache, Redis CLI, AWS CLI
- References: ElastiCache for Redis
- Key Commands:
  - `aws elasticache create-cache-cluster ...`
  - `redis-cli -h <endpoint> --tls -a <auth-token>`
  - `redis-cli INFO memory`

Azure Cache for Redis
- Typical Tools: Azure Cache for Redis, Redis CLI, Azure CLI
- References: Azure Cache for Redis documentation, Quickstart guide
- Key Commands:
  - `az redis create ...`
  - `az redis list-keys ...`
  - `redis-cli -h <host> -p 6380 --tls -a <key>`

GCP Memorystore for Redis
- Typical Tools: GCP Memorystore, Redis CLI, gcloud
- References: Memorystore for Redis, Quickstart
- Key Commands:
  - `gcloud redis instances create <name> ...`
  - `gcloud redis instances describe <name>`
  - `redis-cli -h <instance-ip> -p 6379`

---

## DB-2. Implement and support automated database migration processes

**Description**

Implement, improve, and support automated processes for migrating databases across environments or to cloud platforms. Ensure migration workflows include both data and configurations, minimize downtime, and preserve data integrity.

**Outcomes**

- Automated migration scripts or pipelines that handle schema/configuration and data.
- Documentation of migration steps, rollback or recovery procedures, and validation checks.
- Evidence of successful execution of migrations between environments or to a cloud platform.

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Implement two automated migration flows: (1) versioned schema migrations promoted through environments (dev → stage → prod) by a CI/CD pipeline, and (2) a data migration of a self-managed database (on-prem VM or EC2) to a managed cloud database with minimal downtime using full load + change data capture (CDC). Both flows must include validation and a tested rollback/recovery procedure.

**Prerequisites**

- Source database: PostgreSQL/MySQL on a VM/EC2/Docker with sample data (e.g. pgbench, Sakila, Pagila)
- Target: Amazon RDS / Aurora (or Azure Database / Cloud SQL)
- Migration tool for schema: Flyway / Liquibase / Alembic
- Data migration tool: AWS DMS / Azure DMS / Database Migration Service (GCP) / `pg_dump` + logical replication
- CI/CD platform (GitHub Actions / GitLab CI / Jenkins)

**Guidance and Tips**

- Time Planning: 10–14 hours.
- Scope: one database, ≥3 versioned schema migrations, one full-load + CDC migration, one cutover.
- Tips: Never edit applied migrations — add new ones. Take a snapshot before every prod migration. Use backward-compatible (expand/contract) schema changes for zero-downtime deploys. Enable DMS validation.

**Acceptance Criteria**

Functional Requirements

- Schema migrations are versioned in Git and applied automatically by a pipeline per environment, with manual approval for prod.
- Migration includes configuration (parameter group, users/roles, extensions) defined as code.
- Data migration performs full load + ongoing replication (CDC) to the target database.
- Validation checks run automatically: row counts, checksums / DMS data validation, application smoke test.
- Cutover procedure is executed with measured downtime.
- Rollback is demonstrated: schema rollback (undo migration / snapshot restore) and data rollback plan (reverse replication or restore point).

Non-Functional Requirements

- Downtime during cutover is minimized and documented (target: minutes).
- Credentials are stored in a secrets manager, not in pipeline code.
- Migrations are idempotent and safe to re-run.
- Migration logs are retained and auditable.

**Outcome Artifacts**

- Migration scripts (`V1__init.sql`, `V2__...`) and pipeline definition
- DMS task / replication configuration (Terraform/JSON)
- Pipeline run logs for dev → stage → prod promotion
- Validation report (row counts, checksums, DMS validation status)
- Rollback test evidence
- Runbook: migration steps, cutover checklist, rollback procedure

**Vendor Annex**

- AWS: DMS (replication instance, endpoints, task with `full-load-and-cdc`), RDS snapshots, Secrets Manager
- Azure: Azure Database Migration Service
- GCP: Database Migration Service
- Generic: Flyway, Liquibase, `pg_dump`/`pg_restore`, PostgreSQL logical replication
- Key Commands: `flyway -url=... migrate`, `flyway info`, `aws dms create-replication-task --migration-type full-load-and-cdc ...`, `aws rds create-db-snapshot ...`

---

## DB-3. Maintain and troubleshoot database services

**Description**

Perform regular maintenance and troubleshooting of database infrastructure, including monitoring, configuration adjustments, and resolving performance or availability issues. Participate in tasks such as migrations, upgrades, and incident resolution.

**Outcomes**

- Documented maintenance and troubleshooting activities (a ticket), covering configuration changes, issue resolution, and database migrations.

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Operate a managed relational database through a realistic maintenance cycle: enable monitoring, tune configuration, perform an engine version upgrade with minimal downtime, and resolve at least two simulated incidents (performance and availability). Every activity must be documented as a ticket with root cause, actions and verification.

**Prerequisites**

- Amazon RDS / Aurora PostgreSQL or MySQL (or self-managed DB on a VM)
- Load generator (pgbench, sysbench, k6 against an app)
- Monitoring: CloudWatch, Performance Insights, or Prometheus `postgres_exporter` + Grafana
- Ticket tracker (Jira / GitHub Issues)

**Guidance and Tips**

- Time Planning: 8–10 hours.
- Scope: monitoring + 1 configuration change + 1 upgrade + 2 incidents.
- Incident ideas: missing index causing slow queries; connection exhaustion (`max_connections`); storage running out; long-running lock; failover of Multi-AZ instance.
- Tips: Enable `pg_stat_statements`. Use RDS Blue/Green Deployments for upgrades. Always capture "before" and "after" metrics.

**Acceptance Criteria**

Functional Requirements

- Monitoring and alerting cover CPU, memory, connections, storage, replication lag, slow queries.
- At least one configuration adjustment via parameter group (as code) with justification and measured effect.
- Engine minor/major version upgrade performed with a documented plan, backup and rollback option.
- At least two incidents reproduced and resolved; diagnosis uses DB tooling (`EXPLAIN ANALYZE`, `pg_stat_activity`, Performance Insights).
- Multi-AZ failover (or replica promotion) tested and time-to-recover measured.
- Each activity is recorded in a ticket.

Non-Functional Requirements

- Maintenance windows and backup retention are defined.
- Changes are applied through IaC / change management, not ad hoc console clicks.
- Recovery time and impact are documented for each incident.

**Outcome Artifacts**

- Tickets with problem statement, root cause, fix, verification
- Parameter group diff and metrics before/after
- Upgrade plan and execution log
- Screenshots of monitoring dashboards and alarms during incidents
- Short README: maintenance procedures, troubleshooting playbook

**Vendor Annex**

- AWS: RDS Performance Insights, Enhanced Monitoring, CloudWatch alarms, RDS Blue/Green Deployments, Multi-AZ
- Azure: Azure Database for PostgreSQL — Query Store, Intelligent Performance
- GCP: Cloud SQL Query Insights
- Key Commands: `aws rds reboot-db-instance --force-failover`, `aws rds create-blue-green-deployment ...`, `SELECT * FROM pg_stat_statements ORDER BY total_exec_time DESC LIMIT 10;`
