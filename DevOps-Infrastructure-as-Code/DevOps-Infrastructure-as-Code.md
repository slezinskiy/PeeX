# DevOps - Infrastructure as Code

[⬅ Назад до README](../README.md)

| # | Питання | Key | Інфраструктура |
|---|---------|-----|----------------|
| IAC-1 | [Contribute to design and architecture of configuration management systems](#iac-1-contribute-to-design-and-architecture-of-configuration-management-systems) | ✅ | 📘 NEBo Task |
| IAC-2 | [Implement automated availability and disaster recovery using infrastructure as code](#iac-2-implement-automated-availability-and-disaster-recovery-using-infrastructure-as-code) | | 🤖 Згенеровано Claude |

> **Легенда:** 📘 NEBo Task — офіційний таск із вихідного файлу; 🤖 Згенеровано Claude — таск створено Claude за зразком NEBo Tasks, це не офіційний матеріал.

---

## IAC-1. Contribute to design and architecture of configuration management systems

`Key`

**Description**

Participate in the design and architecture of configuration management systems by creating infrastructure diagrams, schemas, and CI/CD workflows for applying configuration changes. Collaborate with teams to align design decisions with current state, project needs, and automation best practices.

**Outcomes**

- Delivered architecture diagrams, CI/CD schemas, and documented tasks reflecting contributions to configuration management design and implementation.

### 🏗️ Інфраструктура — 📘 NEBo Task: DevOps - Contribute to design and architecture of configuration management systems

**Task Description**

Create an architecture document for introducing a Software Configuration Management (SCM) system such as Ansible, Puppet, Salt, or Chef into the current environment used by globally distributed development teams. The goal is to define the target architecture, operational model, rollout plan, and governance required to manage configuration at scale. The document must capture architectural decisions, estimates, constraints, quality attributes, and practical guidance for implementation and operations.

**Prerequisites**

- An existing infrastructure or application environment that requires configuration management.
- Basic understanding of current configuration practices used in the project.
- Access to high-level system architecture and operational requirements.
- Availability of stakeholders or reference materials describing current pain points or gaps.

**Guidance and Tips**

- Focus on design and architectural contribution rather than hands-on implementation.
- Identify configuration scope, ownership boundaries, and lifecycle stages.
- Propose how configurations are structured, versioned, tested, and promoted across environments.
- Address idempotency, consistency, drift detection, and rollback concepts.
- Consider security aspects such as secrets handling and access controls.
- Document assumptions, trade-offs, and constraints clearly.
- Align the proposed design with existing tooling and team capabilities.

**Acceptance Criteria**

Functional Requirements

- Define an architecture vision describing the target state, system context, key components, integration points, and the rationale for selected SCM tooling.
- Provide high-level and component-level design diagrams that reflect environments, inventory model, control planes, execution nodes/agents, secrets handling, artifact storage, and CI/CD integrations.
- Document architectural decisions with decision records that include considered options, trade-offs, and final choices.
- Produce size, effort, schedule, and team estimates based on the proposed architecture, including skills required and RACI-style ownership.
- Include proposals covering technical scope relevant to the architect’s responsibilities such as module structure, code organization, branching/versioning strategy, and promotion across environments.
- Provide quantitative calculations and forecasts: estimated number of managed servers, capacity and performance expectations, infrastructure and licensing costs, run-time windows, rollout speed, and expected configuration drift reduction.
- Specify non-functional requirements and constraints: security, compliance, reliability, scalability, performance, operability, maintainability, portability, and tooling constraints already present in the organization.
- Define standards and guidelines for contributors: coding style for playbooks/manifests/states, testing approach, linting, secrets management, idempotency expectations, change review, naming conventions, and documentation rules.
- Create implementation and adoption roadmaps: phases, milestones, pilot scope, success metrics, rollout waves, deprecation plan for legacy scripts, and handover to operations.
- Outline verification and validation: unit tests, integration tests, canary or blue/green config rollout, pre-flight checks, drift detection, and rollback procedures.
- Provide a measurable set of success metrics and KPIs: mean time to configure, change failure rate, configuration compliance rate, drift detection mean time, and rollout duration.

Non-Functional Requirements

- The architecture document is complete, consistent, and free of ambiguities; diagrams accurately reflect described components and data flows.
- All recommendations align with security best practices including least privilege, encrypted secrets at rest and in transit, auditable changes, and policy-as-code where applicable.
- Design emphasizes idempotency, repeatability, modularity, and reusability of configuration artifacts across environments and teams.
- Estimates and cost models are transparent, with assumptions stated and sensitivity to scale documented.
- Operational considerations are included: monitoring, logging, alerting, SLOs for configuration runs, maintenance windows, backup and recovery for state and inventory.
- Compliance and governance are addressed: change control workflow, peer review, artifact signing, and traceability from request to applied change.
- Documentation is version-controlled and organized for easy consumption by engineering and operations; all templates and examples are runnable.
- The roadmap minimizes risk through staged adoption, pilots, rollback strategies, and clear entry/exit criteria for each phase.

**Outcome Artifacts**

- Architecture or design document describing the configuration management approach.
- Diagram illustrating configuration flow, components, and responsibilities.
- Documented comparison or rationale for selected configuration management patterns or tools.
- Version-controlled design artifacts or review comments showing contribution.

---

## IAC-2. Implement automated availability and disaster recovery using infrastructure as code

**Description**

Implement high availability (HA) and disaster recovery (DR) strategies through infrastructure-as-code and configuration management tools. Automate provisioning, validation, and recovery processes while ensuring changes are tested and aligned with defined RTO and RPO objectives.

**Outcomes**

- _(У вихідному файлі Outcomes для цього питання відсутні.)_

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Using Terraform (or equivalent) and a configuration management tool, deploy a web application with high availability inside one region (multi-AZ) and a disaster recovery setup in a second region (pilot light or warm standby). Define RTO/RPO targets, automate data replication and failover, and run an automated DR drill that proves the targets are met.

**Prerequisites**

- Cloud account with access to two regions
- Terraform + remote state (S3 + DynamoDB lock / Terraform Cloud)
- Configuration management: Ansible / cloud-init / SSM State Manager
- Sample stateful application (web tier + relational database + object storage)
- DNS zone (Route 53 / Azure DNS / Cloud DNS)

**Guidance and Tips**

- Time Planning: 14–18 hours.
- Scope: multi-AZ HA in primary region + DR region + automated failover + one DR drill.
- Tips: Choose the DR strategy from RTO/RPO (backup&restore → pilot light → warm standby → active/active). Parametrize Terraform modules by region so DR is the same code. Keep DR region scaled down to save cost. Automate the drill via pipeline.

**Acceptance Criteria**

Functional Requirements

- RTO and RPO targets are defined and justified.
- Primary region: compute in an Auto Scaling Group / scale set across ≥2 AZs behind a load balancer; database Multi-AZ.
- DR region: infrastructure defined by the same IaC modules; database cross-region replica (Aurora Global DB / RDS cross-region read replica) and object storage replication (S3 CRR).
- Backups are copied cross-region (AWS Backup copy job or equivalent).
- DNS failover with health checks routes traffic to the DR region automatically or via one-command runbook.
- Configuration management brings new instances to the desired state automatically.
- DR drill is executed via pipeline/script: simulate primary failure → promote replica → scale DR → switch DNS → validate → fail back.

Non-Functional Requirements

- Measured RTO/RPO during the drill meet the targets.
- IaC is modular, versioned, plan/apply runs in CI with review.
- Drift detection is in place (`terraform plan` scheduled).
- DR cost is estimated and optimized.

**Outcome Artifacts**

- Terraform modules and environment configurations (primary / DR)
- Architecture diagram (HA + DR)
- RTO/RPO definition and drill report with timings
- Pipeline / script used for failover and failback
- Screenshots: Route 53 health check failover, replica promotion, app served from DR region
- Short README: DR strategy, procedures, responsibilities

**Vendor Annex**

- AWS: Auto Scaling Groups, ALB, RDS Multi-AZ / Aurora Global Database, S3 Cross-Region Replication, AWS Backup cross-region copy, Route 53 failover routing, AWS Elastic Disaster Recovery
- Azure: Availability Zones, Azure Site Recovery, geo-replication, Traffic Manager / Front Door
- GCP: Regional MIGs, Cloud SQL cross-region replica, Cloud DNS routing policies
- Key Commands: `terraform workspace select dr && terraform apply`, `aws rds promote-read-replica ...`, `aws rds failover-global-cluster ...`, `aws route53 get-health-check-status ...`
