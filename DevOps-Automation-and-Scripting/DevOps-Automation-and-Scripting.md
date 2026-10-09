# DevOps - Automation & Scripting

[⬅ Назад до README](../README.md)

| # | Питання | Key | Інфраструктура |
|---|---------|-----|----------------|
| AUT-1 | [Apply advanced query techniques to support infrastructure and operational tasks](#aut-1-apply-advanced-query-techniques-to-support-infrastructure-and-operational-tasks) | | 🤖 Згенеровано Claude |
| AUT-2 | [Implement structured logging in automation and infrastructure scripts](#aut-2-implement-structured-logging-in-automation-and-infrastructure-scripts) | ✅ | 📘 NEBo Task |
| AUT-3 | [Perform refactoring and re-engineering of DevOps scripts and tools](#aut-3-perform-refactoring-and-re-engineering-of-devops-scripts-and-tools-to-improve-quality-reliability-and-maintainability) | | 🤖 Згенеровано Claude |
| AUT-4 | [Review pull requests across the team to ensure code quality and standards compliance](#aut-4-review-pull-requests-across-the-team-to-ensure-code-quality-and-standards-compliance) | ✅ | 🤖 Згенеровано Claude |
| AUT-5 | [Use multiple programming and scripting languages in DevOps solutions](#aut-5-use-multiple-programming-and-scripting-languages-in-devops-solutions) | | 🤖 Згенеровано Claude |
| AUT-6 | [Write complex automation and scripts following best practices and vendor guidelines](#aut-6-write-complex-automation-and-scripts-following-best-practices-and-vendor-guidelines) | ✅ | 📘 NEBo Task |

> **Легенда:** 📘 NEBo Task — офіційний таск із вихідного файлу; 🤖 Згенеровано Claude — таск створено Claude за зразком NEBo Tasks, це не офіційний матеріал.

---

## AUT-1. Apply advanced query techniques to support infrastructure and operational tasks

**Description**

Write and optimize complex queries using advanced techniques (e.g., joins, conditionals, aggregations, pattern matching, filtering) across SQL/NoSQL databases, analytics platforms, and logging systems. Use these queries to support infrastructure operations, troubleshooting, and performance analysis at the project level.

**Outcomes**

- Delivered advanced query scripts that fulfilled project-level operational requirements (e.g., joins, aggregations, multi-source queries), integrated into infrastructure or operational workflows, and tracked through project tasks or tickets.

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Create a set of advanced operational queries across at least three different query engines (e.g. SQL on a relational DB, SQL over logs in a data lake, a log query language, a metrics query language) to answer real operational questions. Optimize at least one query and integrate the queries into an automated workflow (scheduled report, alert, or dashboard), tracking the work in tickets.

**Prerequisites**

- Access to at least three data sources, e.g.: RDS/PostgreSQL, ALB/CloudTrail/VPC Flow logs in S3, CloudWatch Logs, Prometheus
- Query engines: Athena / CloudWatch Logs Insights / PromQL / KQL / SQL client
- Scheduler (cron, Lambda + EventBridge, CI scheduled job)

**Guidance and Tips**

- Time Planning: 6–8 hours.
- Example questions: top 10 client IPs producing 5xx per hour (ALB logs, Athena, `GROUP BY` + window function); who changed security groups in the last 7 days (CloudTrail, join with IAM identity); slowest DB queries joined with table sizes (`pg_stat_statements` JOIN `pg_class`); error-rate per service (PromQL `sum by` / `rate`); rejected traffic by port (Flow Logs, regex parse).
- Tips: Partition and project data in Athena to reduce cost; use `EXPLAIN` to optimize SQL.

**Acceptance Criteria**

Functional Requirements

- At least 5 queries across ≥3 engines, using joins, aggregations, conditionals (`CASE`), pattern matching (regex/`LIKE`/`parse`), window functions or subqueries.
- At least one multi-source query (join across two datasets, e.g. flow logs + instance inventory).
- At least one query optimized with measurable improvement (scanned bytes / execution time).
- Queries are integrated into a workflow: scheduled report, alert, or dashboard panel.
- Work is tracked in tickets linked to commits.

Non-Functional Requirements

- Queries are versioned and parameterized (time range, environment).
- Query cost and runtime are documented.
- No PII exposed in outputs.

**Outcome Artifacts**

- Query files (`.sql`, `.logsinsights`, `.promql`) in a repository
- Outputs/screenshots of results
- Before/after optimization metrics
- Workflow definition (scheduler/alert/dashboard)
- Tickets with descriptions of the operational need

**Vendor Annex**

- AWS: Athena (partition projection), CloudWatch Logs Insights, CloudTrail Lake
- Azure: Log Analytics (KQL)
- GCP: BigQuery, Logs Explorer
- Generic: PostgreSQL, PromQL, Elasticsearch/OpenSearch DSL, `jq`
- Key Commands: `aws athena start-query-execution ...`, `aws logs start-query --query-string 'fields @timestamp | filter status>=500 | stats count() by bin(5m)'`

---

## AUT-2. Implement structured logging in automation and infrastructure scripts

`Key`

**Description**

Enhance automation and infrastructure scripts with structured logging. Apply multiple log levels, configurable settings, and clear documentation to provide meaningful insights for debugging, monitoring, and maintaining scripts in different environments.

**Outcomes**

- Automation scripts with structured logging committed to version control and used in operational workflows or pipelines, providing meaningful logs (levels, configuration) for debugging and monitoring.

### 🏗️ Інфраструктура — 📘 NEBo Task: DevOps - Implement structured logging in automation and infrastructure scripts

**Task Description**

This task covers enhancing an existing automation or infrastructure script, or creating a new one, by implementing structured logging, multiple log levels, and configuration-driven behavior. The script must emit both machine-readable and human-readable logs suitable for debugging, monitoring, and long-term maintenance across environments. Logging behavior must be adjustable without modifying code, ensuring maintainable and tunable observability for automation workflows

**Prerequisites**

- Experience with one scripting/programming language (Python, Bash, PowerShell, Go, or Node.js)
- Local or sandbox environment for development and testing
- Git client and repository for version control
- Basic understanding of logging concepts (log levels, message formats) and configuration files (YAML, JSON, INI, environment variables)
- Basic understanding of functions, modularization, reusable components, and language style guidelines

**Guidance and Tips**

- Environment: Use a local or sandbox environment where you can safely run and modify scripts. If using project code, ensure no sensitive data is logged and NDA rules are respected.
- Time Planning: Plan around 4–6 hours for refactoring the script, adding configuration and logging, testing different scenarios, and documenting the solution.
- Scope: Start with one meaningful script (e.g., deployment helper, backup script, health-check script, or infrastructure provisioning helper). Focus on end-to-end logging: initialization, main workflow, error paths, and external calls (APIs, databases, cloud CLIs).
- Tips: Aim for a clean separation between code, configuration, and logging. Make log levels and destinations configurable without changing code. Think about how other engineers would use your logs during incidents.

**Acceptance Criteria**

Functional Requirements

- Script emits logs in a structured format such as JSON or clearly structured key-value text
- At least three log levels are used consistently (INFO, WARNING, ERROR, DEBUG or equivalents)
- Logging behavior such as log level, output format, or destination is configurable by file or environment variables
- At least one additional script setting such as endpoints, paths, or feature flags is configurable via file or environment variables
- Errors, failures, and unexpected conditions include meaningful context such as timestamps, operation names, parameters, or correlation identifiers
- Running the script with at least two different configurations produces different but correct logging outputs

Non-Functional Requirements

- Logging failures are handled gracefully without causing script crashes
- Sensitive information such as passwords, secrets, tokens, or personal data is never logged or stored in configuration
- Documentation or README explains script purpose, log format, log levels, and configuration options
- Logging volume is appropriate for normal runs while DEBUG-level logging is available for troubleshooting without code changes

**Outcome Artifacts**

- Script source code in a Git repository demonstrating structured logging and configuration handling
- Configuration files (e.g., config.yaml, settings.json, .env) with non-sensitive example values
- Log output examples from at least two runs showing structured format, different log levels, and error handling
- Terminal output or screenshots confirming correct execution with multiple configuration profiles
- Short README documenting: script purpose, logging design, configuration options, and how to run the script

**Vendor Annex**

Python
- Typical Tools: Python 3, logging, logging.config, optional python-json-logger
- References: Python Logging HOWTO, Python logging.config, python-json-logger
- Key Commands:
  - `python3 script.py --config config.yaml`
  - `LOG_LEVEL=DEBUG python3 script.py`

Bash
- Typical Tools: Bash, logger, date, optional jq
- References: logger man page, GNU Bash Reference Manual, jq JSON processor
- Key Commands:
  - `./script.sh -c config.env`
  - `LOG_LEVEL=INFO ./script.sh`
  - `tail -f /var/log/your_script.log`

PowerShell
- Typical Tools: PowerShell 7+, Write-Verbose, Write-Information, Write-Warning, Write-Error, ConvertTo-Json
- References: PowerShell Documentation, About Logging and Transcripts, Advanced Functions
- Key Commands:
  - `.\script.ps1 -ConfigPath .\config.json -Verbose`
  - `$env:LOG_LEVEL = "Debug"; .\script.ps1`
  - `.\script.ps1 | ConvertTo-Json`

Non-Cloud / On-Prem
- Typical Tools: Linux shell, Python or Go helper scripts, syslog/journald, systemd or cron
- References: systemd-journald, rsyslog Documentation, crontab(5)
- Key Commands:
  - `APP_ENV=dev LOG_LEVEL=DEBUG ./infra_task.sh`
  - `APP_ENV=test LOG_LEVEL=ERROR ./infra_task.sh`
  - `journalctl -u infra-task.service`
  - `tail -f /var/log/infra_task.log`

---

## AUT-3. Perform refactoring and re-engineering of DevOps scripts and tools to improve quality, reliability, and maintainability

**Description**

Refactor and enhance DevOps scripts and tools to align with project coding standards and best practices. Apply improvements such as input validation, output verification, unit testing, and linting to ensure quality, reliability, and maintainability.

**Outcomes**

- _(У вихідному файлі Outcomes для цього питання відсутні.)_

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Take an existing "legacy" DevOps script or tool (monolithic Bash/Python script with hardcoded values, no error handling, no tests) and re-engineer it: split it into functions/modules, add input validation and output verification, externalize configuration, add unit tests and linting, and enforce quality gates in CI. Demonstrate that behaviour is preserved (characterization tests) and that quality metrics improved.

**Prerequisites**

- A legacy script (project script or a sample, e.g. backup/rotation/deploy script)
- Git repository and CI pipeline
- Linters/test frameworks for the language: ShellCheck + shfmt + bats, ruff/pylint + pytest, PSScriptAnalyzer + Pester

**Guidance and Tips**

- Time Planning: 8–10 hours.
- Tips: Write characterization tests before refactoring. Refactor in small commits. Use `set -euo pipefail` in Bash, type hints in Python. Mock external calls (moto for AWS, bats mocks). Add pre-commit hooks.

**Acceptance Criteria**

Functional Requirements

- Original script and its problems are documented (baseline: lint warnings, LOC, cyclomatic complexity, hardcoded values).
- Code is restructured into functions/modules with single responsibility.
- Input validation (arguments, env vars, file existence, formats) with clear error messages and exit codes.
- Output verification (e.g. checksum after copy, resource exists after creation, HTTP 200 after deploy).
- Unit tests cover main paths and failure paths (target ≥70% coverage for Python).
- Linting and formatting enforced locally (pre-commit) and in CI; pipeline fails on violations.
- Behaviour parity demonstrated by characterization tests.

Non-Functional Requirements

- No hardcoded secrets or environment-specific values.
- Backward-compatible CLI (or documented migration).
- Before/after quality metrics recorded.

**Outcome Artifacts**

- Git history showing incremental refactoring (diffs)
- Test suite and coverage report
- Linter output before/after
- CI pipeline with quality gates
- Short README / CHANGELOG describing improvements

**Vendor Annex**

- Bash: ShellCheck, shfmt, bats-core
- Python: ruff, black, mypy, pytest, pytest-cov, moto
- PowerShell: PSScriptAnalyzer, Pester
- Go: golangci-lint, `go test`
- Key Commands: `shellcheck *.sh`, `bats tests/`, `pytest --cov`, `pre-commit run --all-files`, `Invoke-ScriptAnalyzer -Path . -Recurse`

---

## AUT-4. Review pull requests across the team to ensure code quality and standards compliance

`Key`

**Description**

Participate in the review of pull/merge requests to validate code quality, adherence to standards, and alignment with project requirements. Provide constructive feedback through version control platforms or ticketing systems to support continuous improvement.

**Outcomes**

- Documented review comments in version control or task tracking systems.

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Set up a code review process for an infrastructure/automation repository and actively perform reviews: define a review checklist and PR template, configure CODEOWNERS and automated checks (lint, plan output, security scan) that support reviewers, and review a series of PRs (IaC, scripts, pipelines, Kubernetes manifests) leaving constructive, actionable comments.

**Prerequisites**

- Shared repository with teammates (or a training repository with prepared PRs)
- CI that runs checks on PRs
- Ticket tracker linked with VCS

**Guidance and Tips**

- Time Planning: ongoing; ≥10 reviewed PRs over 2–4 weeks.
- Tips: Review for correctness, security, readability, idempotency, cost, blast radius. Use "suggested changes". Distinguish blocking vs non-blocking comments (`nit:`). Post `terraform plan` output to the PR automatically (Atlantis / tfcmt).

**Acceptance Criteria**

Functional Requirements

- Review checklist and PR template exist in the repository.
- CODEOWNERS assigns reviewers automatically.
- Automated checks post results into the PR (lint, tests, `terraform plan`, security scan).
- At least 10 PRs reviewed covering different types (IaC, scripts, CI/CD, Kubernetes).
- Comments are specific, explain "why", and propose a fix; at least some lead to changes.
- Review findings are linked to tickets where follow-up is required.

Non-Functional Requirements

- Review turnaround time is tracked (target < 1 business day).
- Feedback is constructive and respectful.
- Recurring issues are turned into automated checks or standards updates.

**Outcome Artifacts**

- Links/screenshots of PRs with review comments and resulting changes
- Review checklist, PR template, CODEOWNERS
- PR automation configuration (Atlantis / GitHub Actions comments)
- Short summary: number of reviews, common findings, improvements introduced

**Vendor Annex**

- GitHub: PR reviews, suggested changes, CODEOWNERS, required reviews
- GitLab: MR approvals, Code Quality widget
- Tools: Atlantis, tfcmt, reviewdog, Danger, SonarQube PR decoration
- Key Commands: `gh pr review <n> --comment -b "..."`, `gh pr list --search "reviewed-by:@me"`

---

## AUT-5. Use multiple programming and scripting languages in DevOps solutions

**Description**

Apply more than one programming or scripting language to develop automation scripts and DevOps tools. Leverage the strengths of different languages (e.g., Python, Bash, PowerShell, Groovy, Golang) to address diverse project needs and workflows.

**Outcomes**

- Produced solutions implemented in multiple languages(Python, Bash, Powershell, Groovy, Java, Golang, etc.), demonstrating flexibility and adaptability in DevOps automation.

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Build a small DevOps toolkit where each component uses the language best suited for it, and the components work together in one workflow. Example: a Bash bootstrap/entrypoint, a Python tool using the cloud SDK (inventory/cleanup/reporting), a Go CLI or Kubernetes helper compiled to a single binary, a PowerShell script for Windows hosts, and a Groovy Jenkins shared library (or a GitHub Actions composite action) that orchestrates them in a pipeline.

**Prerequisites**

- Git repository and CI/CD (Jenkins for Groovy, or GitHub Actions/GitLab CI)
- Cloud account / Kubernetes cluster to operate on
- Toolchains for chosen languages

**Guidance and Tips**

- Time Planning: 12–16 hours.
- Tips: Justify the choice of language for each component (portability, ecosystem, performance, platform). Use a common interface between components (CLI args + JSON output). Apply the same quality bar (lint, tests, structured logging) in each language.

**Acceptance Criteria**

Functional Requirements

- At least three languages are used, each for a clearly different purpose.
- Components integrate in a single workflow/pipeline (e.g. Groovy/YAML pipeline → Bash → Python → Go).
- Each component has CLI/config parameters, error handling and meaningful exit codes.
- Each component has linting and at least basic tests in CI.
- The workflow is executed successfully against a real or sandbox environment.

Non-Functional Requirements

- Language choice rationale is documented.
- Consistent conventions across languages (logging format, config, naming).
- No hardcoded secrets.

**Outcome Artifacts**

- Repository with components per language
- Pipeline definition orchestrating the components
- Execution logs from the workflow
- README: architecture, language rationale, how to run each component

**Vendor Annex**

- Python: boto3 / azure-sdk / google-cloud, click/argparse, pytest
- Bash: ShellCheck, bats
- Go: cobra, client-go, aws-sdk-go-v2, `go build`
- PowerShell: AWS.Tools / Az modules, Pester
- Groovy: Jenkins Shared Libraries (`vars/`, `src/`), Jenkinsfile
- Key Commands: `go build -o bin/tool ./cmd/tool`, `python -m tool --env dev`, `pwsh ./Invoke-Task.ps1`

---

## AUT-6. Write complex automation and scripts following best practices and vendor guidelines

`Key`

**Description**

Develop or refactor scripts for complex automation tasks using functions, objects, or classes where applicable. Follow vendor guidelines and best practices to ensure readability, maintainability, and long-term supportability.

**Outcomes**

- Structured automation scripts (functions, classes, objects) committed to version control and used in operational workflows or pipelines, demonstrating adherence to vendor guidelines and best practices.

### 🏗️ Інфраструктура — 📘 NEBo Task: DevOps - Write complex automation and scripts following best practices and vendor guidelines

**Task Description**

This task covers developing or refactoring a structured automation script using functions, modules, or classes where appropriate. The script must follow vendor recommendations and language-specific best practices to ensure readability, maintainability, and long-term supportability. It must automate a multi-step or complex operational task and provide configuration options suitable for use in workflows or pipelines. The solution must demonstrate scalable, secure, and maintainable automation

**Prerequisites**

- Experience with one scripting/programming language (Python, Bash, PowerShell, Go, or Node.js)
- Local or sandbox environment for development and testing
- Git client and repository for version control
- Basic understanding of functions, modularization, reusable components, and language style guidelines
- Text editor or IDE and CLI shell access

**Guidance and Tips**

- Environment: Use a local or sandbox environment to design, implement, and test the script.
- Time Planning: Plan around 6–8 hours for development, refactoring, testing, and documenting.
- Scope: Choose one complex automation scenario such as deployment automation, pipeline helpers, multi-step workflows, infrastructure validation, or data processing.
- Tips: Focus on clear structure, proper error handling, parameterization, consistent formatting, and code reuse.

**Acceptance Criteria**

Functional Requirements

- The script includes structured components such as functions, modules, or classes to organize logic.
- The script automates a multi-step or complex task from start to finish (for example: multi-file processing, orchestrating multiple APIs, multi-stage data transformation).
- The script supports configuration through command-line arguments, configuration files, or environment variables.
- The script implements error handling for common failure scenarios (for example: missing inputs, failed external calls, permission errors).
- The script is stored in a Git repository and committed with meaningful commit messages.

Non-Functional Requirements

- The script follows vendor and language-specific style guidelines and coding conventions.
- Naming, formatting, and file structure are consistent and professional.
- The script is modular and reusable across different tasks or environments, with minimal duplication.
- Comments or documentation explain key logic, configuration options, and expected behavior.
- Hardcoded secrets are prohibited; secure configuration methods must be used for sensitive values.
- The script is designed so it can be updated or extended without breaking existing workflows or integrations.
- Performance is reasonable, and unnecessary resource consumption is avoided.

**Outcome Artifacts**

- Full script file(s) showing structured implementation
- Git diff displaying the development or refactoring history
- Terminal output demonstrating script execution with various parameters
- Terminal output showing error-handling behavior for invalid inputs
- Usage examples showing parameters, config files, or environment variables
- Short README documenting: script purpose, workflow, how to run it, configuration details, and best practices applied

**Vendor Annex**

Python
- Typical Tools: Python 3.x, virtual environments, argparse, logging, modules/packages
- References: PEP 8 Style Guide, Python Packaging Guide, Python Functions Tutorial
- Key Commands:
  - `python3 main.py --config config.yaml`
  - `pytest`
  - `pip install -r requirements.txt`

Bash
- Typical Tools: Bash functions, shell modules, set -euo pipefail, shellcheck
- References: Google Shell Style Guide, ShellCheck Documentation
- Key Commands:
  - `./script.sh --mode deploy`
  - `shellcheck script.sh`

PowerShell
- Typical Tools: Advanced functions, modules, structured error handling (try/catch), Pester
- References: PowerShell Coding Guidelines, About Functions Advanced, Pester Framework
- Key Commands:
  - `.\Run-Task.ps1 -ConfigPath .\config.json`
  - `Invoke-Pester`
