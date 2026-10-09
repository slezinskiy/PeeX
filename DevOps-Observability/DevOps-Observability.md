# DevOps - Observability

[⬅ Назад до README](../README.md)

| # | Питання | Key | Інфраструктура |
|---|---------|-----|----------------|
| OBS-1 | [Develop and automate monitoring runbooks for operational efficiency](#obs-1-develop-and-automate-monitoring-runbooks-for-operational-efficiency) | | 🤖 Згенеровано Claude |
| OBS-2 | [Develop and maintain monitoring dashboards](#obs-2-develop-and-maintain-monitoring-dashboards) | | 📘 NEBo Task |

> **Легенда:** 📘 NEBo Task — офіційний таск із вихідного файлу; 🤖 Згенеровано Claude — таск створено Claude за зразком NEBo Tasks, це не офіційний матеріал.

---

## OBS-1. Develop and automate monitoring runbooks for operational efficiency

**Description**

Design and implement monitoring runbooks that define sequential steps. Runbooks can contain routine reactions such as restarting services, executing scripts, or validating connectivity.

**Outcomes**

- Monitoring runbooks or sequential reactions setup for existing environment.

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Create at least three runbooks for typical alerts in an existing environment and automate them end-to-end: alert → event routing → automated sequential reaction (diagnostics, remediation, verification) → notification with results. At least one runbook must be fully automatic and at least one must include a manual approval step for a risky action.

**Prerequisites**

- Running environment with monitoring/alerting (CloudWatch alarms or Prometheus + Alertmanager)
- Automation engine: AWS Systems Manager Automation / Azure Automation / Rundeck / StackStorm / Ansible AWX / Kubernetes operator
- Notification channel (Slack / Teams / email / SNS)

**Guidance and Tips**

- Time Planning: 8–10 hours.
- Example runbooks: (1) service health check failed → collect diagnostics → restart service → verify endpoint; (2) disk usage > 85% → clean temp/log files → extend volume if still high (approval); (3) DB connectivity alert → check DNS, security groups, port, credentials → report.
- Tips: Each step must be idempotent and log its output. Link the runbook from the alert and the dashboard.

**Acceptance Criteria**

Functional Requirements

- At least three runbooks are documented with trigger, sequential steps, expected results and escalation path.
- Runbooks are automated (SSM Automation documents, Rundeck jobs, Ansible playbooks, Lambda) and triggered by alerts via EventBridge / Alertmanager webhook.
- Steps include: diagnostics collection, remediation (restart service / run script), and post-check (connectivity / health validation).
- At least one runbook includes a manual approval step.
- Execution results are sent to the notification channel and logged.
- Runbooks are tested by simulating the failure (stop service, fill disk, block port).

Non-Functional Requirements

- Runbooks are version-controlled and deployed as code.
- Automation runs with least-privilege role.
- Failed automation escalates to a human.
- MTTR before/after automation is measured or estimated.

**Outcome Artifacts**

- Runbook documents (Markdown) with links from alerts/dashboards
- Automation definitions (SSM documents YAML, Rundeck job definitions, playbooks)
- Event routing configuration (EventBridge rule / Alertmanager receiver)
- Execution logs and notification screenshots from simulated incidents
- Short README: runbook catalogue, triggers, ownership

**Vendor Annex**

- AWS: CloudWatch Alarms → EventBridge → SSM Automation (`AWS-RestartEC2Instance`, custom `aws:runCommand` steps, `aws:approve`), SNS
- Azure: Azure Monitor Action Groups → Automation Runbooks / Logic Apps
- Kubernetes: Alertmanager webhook → Robusta / custom controller
- On-prem: Rundeck / StackStorm / Ansible AWX
- Key Commands: `aws ssm create-document --document-type Automation ...`, `aws ssm start-automation-execution ...`

---

## OBS-2. Develop and maintain monitoring dashboards

**Description**

Create and maintain monitoring dashboards to organize metrics and logs into meaningful views and train team members to effectively use dashboards for operational awareness.

**Outcomes**

- Delivered monitoring dashboards that reflect the real-time state of infrastructure and applications.

### 🏗️ Інфраструктура — 📘 NEBo Task: DevOps - Develop and maintain monitoring dashboards

**Task Description**

Develop, standardize, and maintain monitoring dashboards that provide actionable visibility into service health, performance, reliability, and capacity. The dashboards must support day-to-day operations (ex.: triage, incident response, release verification) and ongoing improvement (ex.: SLO tracking, trend analysis, capacity planning). The work includes selecting relevant signals, building dashboards with consistent structure and naming, validating correctness of queries/metrics, and implementing a maintenance process.

**Prerequisites**

- Monitoring stack is already deployed and collecting relevant telemetry (metrics and optionally logs/traces).
- Metric naming and labeling standards exist or are defined (service name, environment, instance/pod, region/zone).
- Access to the monitoring platform (Grafana/CloudWatch/Azure Monitor/GCP Monitoring/etc.) with permissions to create/edit dashboards
- Service instrumentation exists for key application metrics (HTTP status codes, request duration, queue depth, custom KPIs)
- Alerting strategy/SLO definitions exist

**Guidance and Tips**

- Start from the user journey: what an on-call engineer needs in the first 5 minutes of an incident
- Prefer “overview first, details on demand”: top row shows health/SLO, then breakdowns and diagnostics
- Normalize units and panel naming. Use percentiles for latency (p50/p95/p99) and pair them with traffic and errors.
- Avoid expensive queries:
  - Use recording rules (Prometheus) or pre-aggregations where feasible.
  - Limit high-cardinality dimensions in default views; provide drilldowns for deep breakdowns.
- Add annotations/markers: deployments, scaling events, incidents, config changes.
- Use templates/variables: env, service, namespace, cluster, region; ensure sensible defaults.
- Document “how to read” for each dashboard: what normal looks like, what actions to take when abnormal.
- Establish a review loop: after each major incident or release, update dashboards based on what was missing.

**Acceptance Criteria**

Functional Requirements

- Provide at least 3 dashboards.
  - Service Overview (high-level health and SLO/SLA posture)
  - Service Performance (latency, throughput, errors, saturation)
  - Infrastructure/Dependencies (compute/container, datastore, queue, third-party, etc.)
- Each dashboard includes:
  - Time range controls and consistent refresh interval
  - Environment selector (at minimum: prod + non-prod)
  - Service selector (if multiple services exist)
  - Links or navigation to related dashboards/runbooks
- Dashboard panels must include, at minimum:
  - Golden Signals (or equivalent): latency, traffic, errors, saturation
  - Availability/health checks (synthetic or endpoint health)
  - Resource consumption for the service runtime (CPU, memory, restarts, disk/network if relevant)
  - Dependency health (database, cache, messaging, upstream/downstream services, third-party)
- Each panel query is validated by:
  - Verifying expected units and labels (ms vs s, %, bytes, req/s)
  - Cross-checking at least 3 representative time windows (e.g., last 15m, 24h, 7d).

Non-Functional Requirements

- Dashboards load within 5 sec for default time range under normal backend load
- Queries avoid high-cardinality explosions; panels use appropriate aggregation and downsampling
- Consistent naming, panel layout, units, and label usage across dashboards
- Dashboards and variables must not expose secrets, PII, or sensitive identifiers beyond approved scopes
- Access to dashboard editing is restricted; view access aligned to team policy
- Dashboards are reproducible in a new environment using the stored configuration and documented prerequisites.

**Outcome Artifacts**

- Screenshots (or exports) of each dashboard showing:
  - Overview dashboard with Golden Signals and SLO/SLA panel
  - Performance dashboard with latency percentiles and error breakdown
  - Dependency/infrastructure dashboard with saturation and resource views
- Exported dashboard definitions (JSON/YAML) committed to repository, with link/path.
- A short README including:
  - Dashboard inventory and purpose
  - Variable definitions and expected labels
  - Query validation notes (units, sources, assumptions)
- Demonstration notes or short recording showing:
  - Drilldown from a panel to logs/traces/dependency view
  - Use of variables (env/service)
  - Example of identifying a known event (deploy marker) on charts

**Vendor Annex**

Prometheus + Grafana
- Prometheus, Alertmanager (optional), Grafana, exporters
- Implementation notes:
  - Use recording rules for latency/error rate aggregations
  - Grafana variables: cluster, namespace, service, pod
  - Standard panels: rate(req), rate(err), histogram_quantile for latency, saturation via CPU throttling/memory working set

AWS CloudWatch
- CloudWatch Dashboards, CloudWatch Metrics/Logs, Container Insights, Metrics Insights
- Implementation notes:
  - Use widgets for ALB/ELB, ECS/EKS, RDS, SQS/SNS
  - Add deployment markers via EventBridge/annotations approach

Azure Monitor
- Azure Monitor Workbooks, Log Analytics, Application Insights

Google Cloud Operations Suite
- Cloud Monitoring Dashboards, Cloud Logging, Error Reporting, Trace
