# DevOps - Compute

[⬅ Назад до README](../README.md)

| # | Питання | Рівень | Key | Інфраструктура |
|---|---------|--------|-----|----------------|
| CMP-1 | [Monitor virtual machine resource usage](#cmp-1-monitor-virtual-machine-resource-usage-trainee) | Trainee | ✅ | 🤖 Згенеровано Claude |
| CMP-2 | [Perform capacity planning for virtual machines](#cmp-2-perform-capacity-planning-for-virtual-machines-lead) | Lead | | 🤖 Згенеровано Claude |

> **Легенда:** 📘 NEBo Task — офіційний таск із вихідного файлу; 🤖 Згенеровано Claude — таск створено Claude за зразком NEBo Tasks, це не офіційний матеріал.

---

## CMP-1. Monitor virtual machine resource usage (Trainee)

`Key`

**Description**

Monitor resource usage on a virtual machine (VM) using available system monitoring tools or command-line utilities. Collect and review key performance metrics such as CPU utilization, memory usage, and general system health to understand current operating conditions. Capture the collected data for a defined period and record any noticeable changes or peaks without performing root-cause analysis.

**Outcomes**

- Output or screenshots from monitoring tools or system commands.
- Recorded CPU and memory usage data for a defined time window.
- Short written summary (ticket, comment, or note) describing what metrics were collected, when and how they were collected, any visible changes or peaks observed.

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Launch a small Linux VM, collect CPU, memory, disk and load metrics for a defined time window (e.g. 60 minutes) using both command-line utilities and a cloud monitoring agent, generate an artificial load peak during the window, and write a short summary of what was observed.

**Prerequisites**

- One Linux VM (EC2 `t3.micro` / Azure B1s / GCP e2-micro or local VirtualBox)
- SSH or SSM Session Manager access
- Utilities: `top`/`htop`, `vmstat`, `free`, `sar` (sysstat), `iostat`, `stress-ng`

**Guidance and Tips**

- Time Planning: 2–3 hours.
- Tips: `sar` stores history (`/var/log/sa`), use it for the time window. Memory is not visible in default EC2 metrics — install CloudWatch Agent. Record timestamps of the load you generate.

**Acceptance Criteria**

Functional Requirements

- Metrics are collected for a defined window: CPU %, memory used/available, load average, disk usage, (optionally) network.
- At least two collection methods are used: CLI utilities and a monitoring tool (CloudWatch / Azure Monitor / Netdata / node_exporter + Grafana).
- A load peak is generated (`stress-ng --cpu 2 --vm 1 --timeout 300s`) and is visible in the data.
- Raw data is captured (e.g. `vmstat 5 720 > vmstat.log`, `sar -u -r` output, CSV export).

Non-Functional Requirements

- Summary states what, when, how, and the peaks observed — no root-cause analysis required.
- No credentials in screenshots.

**Outcome Artifacts**

- CLI outputs / log files for the time window
- Screenshots of monitoring graphs (CPU and memory) showing the peak
- Short note/ticket with the summary

**Vendor Annex**

- AWS: CloudWatch Agent (`mem_used_percent`, `disk_used_percent`), EC2 basic monitoring
- Azure: Azure Monitor Agent, VM Insights
- Linux: `sysstat`, `htop`, `vmstat`, `iostat`, `stress-ng`
- Key Commands: `sar -u -r 5 720`, `vmstat 5`, `free -m`, `df -h`, `uptime`

---

## CMP-2. Perform capacity planning for virtual machines (Lead)

**Description**

Develop and maintain resource planning for virtual machines by analyzing workload characteristics, monitoring utilization trends, and anticipating future demand. Use planning tools and telemetry to support sizing decisions and ensure implemented capacity aligns with business and technical expectations through regular audits.

**Outcomes**

- Documented capacity plan or sizing model for virtual machines.
- Utilization reports or assessment outputs justifying chosen specifications.
- Audit or review confirming alignment between planned and implemented capacity.

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

For a VM-based workload (e.g. web tier in an Auto Scaling Group + backend VMs), build a capacity plan: characterise the workload, collect utilization telemetry over a representative period, run load tests to find per-instance throughput limits, build a sizing model with growth forecast and headroom, recommend instance types/counts and purchasing options, implement the recommended sizing, and run an audit comparing planned vs actual capacity.

**Prerequisites**

- VM-based application with ≥2 instances and monitoring (CloudWatch / Azure Monitor / Prometheus)
- Utilization history (≥1–2 weeks preferred, or a synthetic load profile)
- Load testing tool (k6 / JMeter / Locust)
- Spreadsheet or script for the sizing model

**Guidance and Tips**

- Time Planning: 12–16 hours.
- Tips: Use p95 utilization, not average. Determine the bottleneck resource (CPU, memory, IOPS, network). Keep N+1 (or AZ-failure) headroom. Use Compute Optimizer / Azure Advisor as an input, not as the answer. Express growth scenarios (base / high).

**Acceptance Criteria**

Functional Requirements

- Workload characterization: traffic pattern, peak/off-peak, resource profile, SLOs.
- Utilization report for CPU, memory, disk, network (p50/p95/max) per instance group.
- Load test establishes max sustainable RPS per instance at SLO.
- Sizing model: forecast demand (e.g. 6–12 months), required instances = peak demand / per-instance capacity + headroom; instance type comparison including cost.
- Recommendations implemented: instance type/size, ASG min/max/desired, scaling policies, purchasing options (Savings Plans / Reserved / Spot).
- Audit compares planned vs implemented capacity (tags, inventory, utilization after change) and records deviations.

Non-Functional Requirements

- Assumptions and sensitivity are documented.
- Plan has review cadence (e.g. quarterly) and owner.
- Cost impact of the recommendation is quantified.

**Outcome Artifacts**

- Capacity plan document with sizing model (spreadsheet / notebook)
- Utilization reports and load test results
- Rightsizing recommendation outputs (Compute Optimizer / Advisor)
- IaC change implementing the sizing
- Audit report planned vs actual

**Vendor Annex**

- AWS: CloudWatch, Compute Optimizer, Cost Explorer, Auto Scaling predictive scaling, AWS Config inventory
- Azure: Azure Advisor, Azure Monitor, VMSS autoscale
- GCP: Recommender, Active Assist
- Key Commands: `aws compute-optimizer get-ec2-instance-recommendations`, `aws cloudwatch get-metric-statistics --extended-statistics p95 ...`, `k6 run load.js`
