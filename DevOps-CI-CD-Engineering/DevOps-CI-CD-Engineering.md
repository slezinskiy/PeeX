# DevOps - CI/CD Engineering

[⬅ Назад до README](../README.md)

| # | Питання | Key | Інфраструктура |
|---|---------|-----|----------------|
| CICD-1 | [Adjust a branching strategy](#cicd-1-adjust-a-branching-strategy) | | 🤖 Згенеровано Claude |
| CICD-2 | [Integrate automated security scans into continuous integration and deployment (delivery) flows](#cicd-2-integrate-automated-security-scans-into-continuous-integration-and-deployment-delivery-flows) | | 🤖 Згенеровано Claude |
| CICD-3 | [Automate provisioning and teardown of testing environments *(назва відновлена)*](#cicd-3-automate-provisioning-and-teardown-of-testing-environments) | | 🤖 Згенеровано Claude |

> **Легенда:** 📘 NEBo Task — офіційний таск із вихідного файлу; 🤖 Згенеровано Claude — таск створено Claude за зразком NEBo Tasks, це не офіційний матеріал.

---

## CICD-1. Adjust a branching strategy

**Description**

Define and adjust a branching strategy that aligns with project requirements, team workflows, and release processes. Configure supporting practices such as code review rules and automated branch management within version control systems.

**Outcomes**

- Implemented and documented a branching strategy (e.g., GitFlow, GitHub Flow, GitLab Flow, or trunk-based development) with supporting CI/CD and VCS configurations.

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Analyse the release requirements of a project, choose and justify a branching strategy, and implement it in the VCS and CI/CD: branch protection / rulesets, review rules, naming conventions, automated branch management, mapping of branches/tags to pipeline stages and environments, versioning and release tagging, and a hotfix flow.

**Prerequisites**

- Repository on GitHub / GitLab / Azure Repos / Bitbucket with admin rights
- CI/CD pipeline with at least two environments (e.g. stage, prod)
- Description of team size, release cadence and support requirements

**Guidance and Tips**

- Time Planning: 6–8 hours.
- Tips: Trunk-based / GitHub Flow fits continuous delivery; GitFlow fits scheduled releases with multiple supported versions. Prefer short-lived branches. Automate version bumping (semantic-release / release-please). Document the decision as an ADR.

**Acceptance Criteria**

Functional Requirements

- A branching strategy is selected and justified (ADR comparing at least two options).
- Protected branches/rulesets: required PR, ≥1 approval, CODEOWNERS review, required status checks, linear history or merge policy, no force-push/deletion.
- Branch naming convention is enforced (ruleset or CI check), commit message convention (Conventional Commits) is validated.
- Automated branch management: auto-delete merged branches, stale branch cleanup, auto-merge for dependency updates (optional).
- Pipeline behaviour per branch type: feature → build/test; main → deploy to stage; tag/release → deploy to prod.
- Release tagging and changelog are automated.
- Hotfix flow is demonstrated end-to-end.

Non-Functional Requirements

- Strategy is documented in `CONTRIBUTING.md` and understandable for new team members.
- VCS settings are stored as code where possible (Terraform GitHub/GitLab provider).
- Lead time and merge frequency are measurable.

**Outcome Artifacts**

- ADR / document describing the branching strategy and diagram of the flow
- Screenshots or Terraform code of branch protection / rulesets
- `CODEOWNERS`, PR template, `CONTRIBUTING.md`
- Pipeline definitions showing branch/tag-based behaviour
- Example PRs, release tags and changelog
- Hotfix flow evidence

**Vendor Annex**

- GitHub: Rulesets, Branch protection, CODEOWNERS, auto-merge, release-please / semantic-release
- GitLab: Protected branches, Merge request approvals, Push rules, `rules:` in `.gitlab-ci.yml`
- Azure DevOps: Branch policies, build validation
- Key Commands: `git switch -c feature/JIRA-123-short-desc`, `gh api repos/{owner}/{repo}/rulesets`, `terraform apply` (github provider)

---

## CICD-2. Integrate automated security scans into continuous integration and deployment (delivery) flows

**Description**

Incorporate automated security scanning tools, such as SAST, DAST, or dependency scanners, into CI/CD pipelines to detect vulnerabilities early in the development lifecycle. Ensure scans cover both code and artifacts to enforce security and compliance standards.

**Outcomes**

- _(У вихідному файлі Outcomes для цього питання відсутні.)_

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

> Перетинається з [SEC-1](../DevOps-Security/DevOps-Security.md#sec-1-automate-security-testing-in-pipelines-and-repositories). Тут фокус — на **стадіях пайплайна та артефактах** (образи, SBOM, підпис, admission policy), а не на налаштуваннях репозиторію.

**Task Description**

Extend a CI/CD pipeline with security stages covering source code and produced artifacts across the whole flow: commit (SAST, secrets, SCA, IaC scan) → build (image scan, SBOM generation, artifact signing) → deploy (signature verification / admission policy) → post-deploy (DAST). Define quality gates per stage and demonstrate that a vulnerable change or unsigned artifact cannot reach production.

**Prerequisites**

- Working CI/CD pipeline that builds a container image and deploys it (Kubernetes preferred)
- Container registry
- Test/staging environment reachable by the pipeline

**Guidance and Tips**

- Time Planning: 10–12 hours.
- Tips: Scan the image that you actually deploy (by digest). Generate SBOM once and scan it repeatedly. Use cosign keyless signing with OIDC. Enforce at admission with Kyverno / OPA Gatekeeper / ECR + signature verification.

**Acceptance Criteria**

Functional Requirements

- CI stage: SAST, SCA, secret scanning and IaC scanning run on every PR.
- Build stage: image is scanned; SBOM (CycloneDX/SPDX) is generated and stored as an artifact.
- Artifacts are signed (cosign / Notation) and attested.
- Deploy stage: the cluster/platform rejects unsigned or non-compliant images (admission policy).
- Delivery stage: DAST runs against staging after deployment; prod promotion depends on the result.
- Quality gates fail the pipeline on defined severity thresholds; exceptions are tracked with expiry.
- Scheduled re-scan of already deployed images detects newly published CVEs.

Non-Functional Requirements

- Results are aggregated in one place (DefectDojo / Security Hub / GitHub Security).
- Pipeline credentials are short-lived (OIDC).
- Scanning time is acceptable and cached where possible.

**Outcome Artifacts**

- Pipeline definition with all security stages
- SBOM file and signature verification output
- Admission policy YAML and rejected deployment output
- DAST report from staging
- Screenshots of failing and passing quality gates
- Short README: stages, tools, thresholds, exception process

**Vendor Annex**

- Tools: Semgrep / CodeQL / SonarQube, Trivy / Grype, Syft, cosign, Kyverno / Gatekeeper, OWASP ZAP, Checkov
- AWS: ECR enhanced scanning (Inspector), CodePipeline + CodeBuild, Signer, Security Hub
- Key Commands: `syft <img> -o cyclonedx-json > sbom.json`, `grype sbom:sbom.json --fail-on high`, `cosign sign --yes <img@digest>`, `cosign verify <img> --certificate-identity ...`

---

## CICD-3. Automate provisioning and teardown of testing environments

> ⚠️ У вихідному файлі `peex.txt` у цього питання **відсутня назва** (є лише Description/Outcomes). Назву відновлено Claude за змістом.

**Description**

Configure automated provisioning and teardown of testing environments with consistency and repeatability.

**Outcomes**

- Implemented automated environment setup and cleanup within CI/CD pipelines.

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Implement ephemeral (per-pull-request) testing environments: when a PR is opened the pipeline provisions an isolated environment (infrastructure + application + test data), runs tests against it and posts the URL to the PR; when the PR is merged/closed, or after a TTL, the environment is destroyed automatically. Environments must be consistent (same IaC/Helm as stage/prod) and cost-controlled.

**Prerequisites**

- CI/CD platform with PR/MR events
- Kubernetes cluster (namespace-per-PR) or cloud account (Terraform workspace-per-PR)
- IaC (Terraform) and/or Helm charts for the application
- Wildcard DNS / ingress (optional, for preview URLs)

**Guidance and Tips**

- Time Planning: 8–10 hours.
- Tips: Name everything by PR number (`pr-123`). Tag all resources (`env=ephemeral`, `pr=123`, `ttl=...`). Add a scheduled cleanup job as a safety net for orphaned environments. Seed test data automatically.

**Acceptance Criteria**

Functional Requirements

- Opening/updating a PR triggers provisioning of an isolated environment.
- Environment is built from the same IaC/Helm code used for permanent environments (parameterized).
- Automated tests (smoke/integration/DAST) run against the environment; results and URL are posted to the PR.
- Closing/merging the PR triggers full teardown (resources, DNS records, secrets, state).
- A scheduled job removes environments older than the TTL.
- Concurrent environments for multiple PRs work without conflicts.

Non-Functional Requirements

- Provisioning time is measured (target < 15 min).
- Teardown leaves no orphaned resources (verified by tag-based inventory).
- Ephemeral environments have resource quotas / cost limits.
- Pipeline uses least-privilege credentials scoped to ephemeral resources.

**Outcome Artifacts**

- Pipeline definitions for create / destroy / scheduled cleanup
- Terraform / Helm parameterization for ephemeral environments
- PR screenshot with environment URL and test results
- Logs proving teardown and resource inventory after cleanup
- Short README: lifecycle, naming, quotas, troubleshooting

**Vendor Annex**

- GitHub Actions: `on: pull_request: types: [opened, synchronize, closed]`, environments
- GitLab: Review Apps with `environment: on_stop:` and `auto_stop_in:`
- Kubernetes: namespace-per-PR + Helm, ArgoCD ApplicationSet Pull Request generator
- Terraform: `terraform workspace new pr-123`, `terraform destroy -auto-approve`
- AWS: Resource Groups Tagging API for orphan detection: `aws resourcegroupstaggingapi get-resources --tag-filters Key=env,Values=ephemeral`
