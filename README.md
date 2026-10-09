# PeEx — внутрішня атестація DevOps Engineer

Джерело: `Peex.txt`. 10 доменів, 30 питань: 10 з офіційним 📘 NEBo Task, 20 з 🤖 інфраструктурою, згенерованою Claude (у файлах доменів вони позначені явно).

## Домени

| Домен | Файл | Питань | 📘 NEBo | 🤖 Claude |
|-------|------|:------:|:-------:|:---------:|
| DevOps - Security | [DevOps-Security.md](DevOps-Security/DevOps-Security.md) | 4 | 0 | 4 |
| DevOps - Databases | [DevOps-Databases.md](DevOps-Databases/DevOps-Databases.md) | 3 | 1 | 2 |
| DevOps - Network | [DevOps-Network.md](DevOps-Network/DevOps-Network.md) | 1 | 1 | 0 |
| DevOps - Observability | [DevOps-Observability.md](DevOps-Observability/DevOps-Observability.md) | 2 | 1 | 1 |
| DevOps - Infrastructure as Code | [DevOps-Infrastructure-as-Code.md](DevOps-Infrastructure-as-Code/DevOps-Infrastructure-as-Code.md) | 2 | 1 | 1 |
| DevOps - CI/CD Engineering | [DevOps-CI-CD-Engineering.md](DevOps-CI-CD-Engineering/DevOps-CI-CD-Engineering.md) | 3 | 0 | 3 |
| DevOps - Compute | [DevOps-Compute.md](DevOps-Compute/DevOps-Compute.md) | 2 | 0 | 2 |
| DevOps - Automation & Scripting | [DevOps-Automation-and-Scripting.md](DevOps-Automation-and-Scripting/DevOps-Automation-and-Scripting.md) | 6 | 2 | 4 |
| DevOps - Containers | [DevOps-Containers.md](DevOps-Containers/DevOps-Containers.md) | 3 | 3 | 0 |
| DevOps - Storages | [DevOps-Storages.md](DevOps-Storages/DevOps-Storages.md) | 4 | 1 | 3 |
| **Разом** | | **30** | **10** | **20** |

---

## Приклади інфраструктури, що покривають максимум питань

> 🤖 Розділ згенеровано Claude на основі аналізу всіх 30 питань. Ідея: замість 30 окремих лабораторних побудувати 3 пов'язані проєкти (на AWS, бо це основний профіль), кожен з яких одночасно закриває багато питань. Разом вони покривають **30 / 30**.

### Інфраструктура A — «Cloud-native мікросервісна платформа на AWS EKS»

**Склад**

- **IaC:** Terraform (модулі VPC, EKS, RDS, ElastiCache, S3, WAF) + remote state; GitHub Actions з OIDC → AWS.
- **Репозиторій та CI/CD:** trunk-based / GitHub Flow, rulesets, CODEOWNERS, Conventional Commits, release-please; стадії: SAST (Semgrep/CodeQL), SCA, gitleaks, Checkov, Trivy image scan, SBOM (Syft), cosign-підпис, DAST (OWASP ZAP) на preview-середовищі.
- **Preview-середовища:** namespace `pr-<N>` + Helm на кожен PR, автоматичне видалення при закритті та TTL-cleanup.
- **EKS:** Karpenter / Cluster Autoscaler + HPA (CPU + custom metrics через Prometheus Adapter), requests/limits, Kyverno (перевірка підписів образів).
- **Мережа:** AWS Load Balancer Controller → ALB (ACM TLS, path/host routing) + AWS WAF (managed rules, rate-based, geo) + Shield; Istio (mTLS STRICT, retries/timeouts/outlier detection, rate limit) + NetworkPolicies (Calico/Cilium), GuardDuty, VPC Flow Logs.
- **Секрети:** AWS Secrets Manager + Secrets Store CSI Driver (ASCP) + IRSA/Pod Identity, автоматична ротація.
- **Доступ:** IAM Identity Center (обов'язкове MFA, permission sets), EKS access entries + Kubernetes RBAC по namespace, Access Analyzer, AWS Config MFA rules.
- **Дані:** RDS PostgreSQL Multi-AZ (Flyway-міграції в пайплайні, Performance Insights), ElastiCache Redis (TLS + AUTH, доступ лише з підмереж EKS), S3 для app assets (bucket policies, KMS).
- **Observability:** kube-prometheus-stack + Grafana (3 дашборди: Overview/Performance/Dependencies, deploy-анотації), Loki/CloudWatch Logs, Jaeger/Kiali; Alertmanager → вебхук → автоматизовані runbook-и.
- **Бекап та DR:** AWS Backup (RDS, EBS PV), Velero для Kubernetes-ресурсів, S3 lifecycle + CRR; Terraform-модулі, параметризовані за регіоном для DR.

**Покриває (20):**
SEC-1, SEC-2, SEC-3, SEC-4 · DB-1, DB-2, DB-3 · NET-1 · OBS-1, OBS-2 · IAC-2 · CICD-1, CICD-2, CICD-3 · CNT-1, CNT-2, CNT-3 · STG-1, STG-2, STG-4

---

### Інфраструктура B — «Гібридна VM-платформа: міграція з on-prem в AWS + HA/DR»

**Склад**

- **Симульований on-prem:** окремий VPC або VirtualBox/Vagrant: VM з PostgreSQL, NFS-шара з даними, legacy-скрипти.
- **Цільова платформа в AWS (Terraform):** EC2 Auto Scaling Group у 3 AZ за ALB/NLB (TLS, health checks, failover), Launch Template + SSM Agent + CloudWatch Agent.
- **Configuration management:** Ansible (ролі, inventory за тегами, Molecule-тести, запуск з пайплайна/SSM State Manager), архітектурний документ + ADR щодо SCM (IAC-1).
- **Міграції:** AWS DMS (full load + CDC) з on-prem PostgreSQL → RDS; AWS DataSync (агент) NFS → S3/EFS з верифікацією та інкрементальними синками.
- **Сховища:** EBS gp3, EFS, приватний S3 через VPC endpoint, статичний сайт S3 + CloudFront (OAC), архів у Glacier Deep Archive; bucket policies, presigned URLs, KMS.
- **HA/DR:** RDS Multi-AZ + cross-region read replica, S3 CRR, AWS Backup cross-region copy, Route 53 failover; DR-навчання через пайплайн із вимірюванням RTO/RPO.
- **Compute-операції:** збір метрик CPU/RAM (sar/vmstat + CloudWatch Agent) з навантаженням `stress-ng`; capacity planning (k6 load test, Compute Optimizer, sizing model, аудит planned vs actual).
- **Observability:** CloudWatch Dashboards, Alarms → EventBridge → SSM Automation runbooks (restart service, cleanup disk з approval, перевірка з'єднання з БД).
- **Безпека:** WAF на ALB, Security Groups/NACL, IAM Identity Center + MFA, RBAC-ролі для операторів.
- **Запити:** Athena по ALB/VPC Flow/CloudTrail логах, CloudWatch Logs Insights, SQL по `pg_stat_statements`.

**Покриває (20):**
SEC-2, SEC-3, SEC-4 · DB-2, DB-3 · NET-1 · OBS-1, OBS-2 · IAC-1, IAC-2 · CMP-1, CMP-2 · STG-1, STG-2, STG-3, STG-4 · AUT-1 · CICD-1, CICD-2, CICD-3 (Terraform workspace на PR)

---

### Інфраструктура C — «DevOps Toolkit: багатомовний репозиторій інструментів для A та B»

**Склад**

- **Python `opsctl`** (класи, модулі, argparse/click, config YAML + env, structured JSON logging, pytest + moto): інвентаризація ресурсів, звіт про бекапи, cleanup preview-середовищ за тегами, DataSync/S3-sync helper.
- **Bash:** bootstrap/entrypoint-скрипти та legacy-скрипт ротації бекапів, відрефакторений (ShellCheck, bats, `set -euo pipefail`, валідація вхідних даних, перевірка контрольних сум).
- **Go:** невеликий CLI/Kubernetes-хелпер (client-go), наприклад перевірка готовності сервісів або аудит RBAC.
- **PowerShell:** скрипт для Windows-хостів (Pester, PSScriptAnalyzer).
- **Groovy:** Jenkins Shared Library (або GitHub Actions composite action), що оркеструє всі компоненти.
- **Queries:** `queries/` з Athena SQL, Logs Insights, PromQL, PostgreSQL-запитами, які запускає `opsctl report` за розкладом.
- **Процес:** branching strategy, PR template, review checklist, CODEOWNERS, pre-commit, CI з lint/tests/security scans; ≥10 PR-рев'ю.
- **Runbook-скрипти:** кроки діагностики та ремедіації, які викликають SSM Automation / Alertmanager-вебхуки з A і B.

**Покриває (12):**
AUT-1, AUT-2, AUT-3, AUT-4, AUT-5, AUT-6 · CICD-1, CICD-2 · SEC-1 · OBS-1 · STG-3 · DB-2

---

### Матриця покриття

| ID | Питання | 📘/🤖 | A (EKS) | B (VM/Hybrid) | C (Toolkit) |
|----|---------|:-----:|:-------:|:-------------:|:-----------:|
| SEC-1 | Automate security testing in pipelines and repositories | 🤖 | ✅ | | ✅ |
| SEC-2 | Configure and manage RBAC | 🤖 | ✅ | ✅ | |
| SEC-3 | Application traffic security controls | 🤖 | ✅ | ✅ | |
| SEC-4 | Multi-factor authentication | 🤖 | ✅ | ✅ | |
| DB-1 | In-memory data solutions | 📘 | ✅ | | |
| DB-2 | Automated database migration processes | 🤖 | ✅ | ✅ | ✅ |
| DB-3 | Maintain and troubleshoot database services | 🤖 | ✅ | ✅ | |
| NET-1 | Load balancing solutions | 📘 | ✅ | ✅ | |
| OBS-1 | Monitoring runbooks automation | 🤖 | ✅ | ✅ | ✅ |
| OBS-2 | Monitoring dashboards | 📘 | ✅ | ✅ | |
| IAC-1 | Design/architecture of configuration management | 📘 | | ✅ | |
| IAC-2 | HA & DR using IaC | 🤖 | ✅ | ✅ | |
| CICD-1 | Branching strategy | 🤖 | ✅ | ✅ | ✅ |
| CICD-2 | Security scans in CI/CD | 🤖 | ✅ | ✅ | ✅ |
| CICD-3 | Provisioning/teardown of testing environments | 🤖 | ✅ | ✅ | |
| CMP-1 | Monitor VM resource usage | 🤖 | | ✅ | |
| CMP-2 | Capacity planning for VMs | 🤖 | | ✅ | |
| AUT-1 | Advanced query techniques | 🤖 | | ✅ | ✅ |
| AUT-2 | Structured logging in scripts | 📘 | | | ✅ |
| AUT-3 | Refactoring of DevOps scripts | 🤖 | | | ✅ |
| AUT-4 | Pull request reviews | 🤖 | | | ✅ |
| AUT-5 | Multiple programming languages | 🤖 | | | ✅ |
| AUT-6 | Complex automation following best practices | 📘 | | | ✅ |
| CNT-1 | Autoscaling for containerized workloads | 📘 | ✅ | | |
| CNT-2 | Secret management for containers | 📘 | ✅ | | |
| CNT-3 | Service-to-service traffic security | 📘 | ✅ | | |
| STG-1 | Archiving and backup strategies | 📘 | ✅ | ✅ | |
| STG-2 | Permissions for storage resources | 🤖 | ✅ | ✅ | |
| STG-3 | Automated data migration | 🤖 | | ✅ | ✅ |
| STG-4 | Diverse storage services | 🤖 | ✅ | ✅ | |
| | **Разом** | | **20** | **20** | **12** |

### Рекомендований порядок

1. **A + C** закривають **27/30**. Не покритими лишаються лише IAC-1, CMP-1 і CMP-2.
2. **B** додає IAC-1, CMP-1, CMP-2 і дає другий, VM-орієнтований доказ для мережі, сховищ, DR і міграцій.
3. **C** варто вести паралельно з A/B: його скрипти працюють з реальними ресурсами цих платформ, і це закриває вимогу «used in operational workflows or pipelines».

### Примітки щодо вихідних даних

- У третього питання домену CI/CD у `Peex.txt` **немає назви**. Її відновлено за змістом як *«Automate provisioning and teardown of testing environments»*.
- Для IAC-2, CICD-2, AUT-3 і STG-2 у вихідному файлі **немає Outcomes**.
- У NEBo Task для CNT-2 немає Prerequisites, Outcome Artifacts і Vendor Annex. Їх доповнено та позначено як 🤖.
- Службові поля з порталу (кількість менторів, «Request NEBo Mentor») прибрано. Позначки `Key` і рівні (Trainee/Middle/Lead) збережено.
