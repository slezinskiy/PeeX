# DevOps - Security

[⬅ Назад до README](../README.md)

| # | Питання | Key | Інфраструктура |
|---|---------|-----|----------------|
| SEC-1 | [Automate security testing in pipelines and repositories](#sec-1-automate-security-testing-in-pipelines-and-repositories) | | 🤖 Згенеровано Claude |
| SEC-2 | [Configure and manage role-based access controls (RBAC)](#sec-2-configure-and-manage-role-based-access-controls-rbac) | ✅ | 🤖 Згенеровано Claude |
| SEC-3 | [Implement and manage application traffic security controls](#sec-3-implement-and-manage-application-traffic-security-controls) | | 🤖 Згенеровано Claude |
| SEC-4 | [Implement and manage multi-factor authentication for secure access control](#sec-4-implement-and-manage-multi-factor-authentication-for-secure-access-control) | | 🤖 Згенеровано Claude |

> **Легенда:** 📘 NEBo Task — офіційний таск із вихідного файлу; 🤖 Згенеровано Claude — таск створено Claude за зразком NEBo Tasks, це не офіційний матеріал.

---

## SEC-1. Automate security testing in pipelines and repositories

**Description**

Integrate automated security checks into CI/CD pipelines and source code repositories to detect vulnerabilities early in the development process. Implement practices such as SAST, DAST, dependency and container scanning, and configure repository security features like access controls, code scanning, and secret detection.

**Outcomes**

- Delivered pipelines and repository configurations with automated security tests and policies, ensuring continuous monitoring and mitigation of vulnerabilities throughout the development lifecycle.

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Build a "secure-by-default" repository and CI/CD pipeline for a sample containerized application. The pipeline must run SAST, dependency (SCA) scanning, secret detection, IaC scanning, container image scanning and DAST against a deployed test environment. The repository must enforce branch protection, required security checks, CODEOWNERS and secret push protection. Findings must be visible centrally and must block merges/deployments according to a defined severity policy.

**Prerequisites**

- Git repository on GitHub / GitLab / Azure DevOps with admin rights
- Sample web application with a Dockerfile and dependency manifest (e.g. `requirements.txt`, `package.json`)
- Terraform (or other IaC) code for the application infrastructure
- Container registry (AWS ECR or equivalent)
- A test environment URL for DAST (can be an ephemeral environment, see CICD-3)

**Guidance and Tips**

- Environment: GitHub Actions + AWS ECR is the simplest path; GitLab Ultimate has most scanners built in.
- Time Planning: 8–10 hours.
- Scope: one repository, one pipeline, all scanner types, one documented severity policy.
- Tips: Start in "report only" mode, then switch to "fail on HIGH/CRITICAL". Upload results in SARIF so they appear in the Security tab. Add a suppression/baseline file with justification instead of disabling rules.

**Acceptance Criteria**

Functional Requirements

- SAST runs on every pull request (e.g. CodeQL, Semgrep, SonarQube).
- Dependency/SCA scanning is enabled (Dependabot / Trivy fs / OWASP Dependency-Check) with automated update PRs.
- Secret detection runs in the pipeline (gitleaks / trufflehog) and push protection is enabled in the repository.
- IaC scanning runs on Terraform/Kubernetes manifests (Checkov / tfsec / kics).
- Built container images are scanned before push or before deploy (Trivy / Grype / ECR enhanced scanning).
- DAST (OWASP ZAP baseline or full scan) runs against the deployed test environment.
- Pipeline fails when findings of a defined severity (e.g. HIGH/CRITICAL) are detected.
- Branch protection: required reviews, required status checks (all scanners), no force-push, CODEOWNERS for security-sensitive paths.

Non-Functional Requirements

- Scan results are centralized (SARIF in GitHub Security tab, GitLab Security Dashboard, SonarQube or DefectDojo).
- Pipeline security stages add reasonable time (target < 10 min for PR checks).
- Suppressions are versioned and justified.
- No secrets in repository history or pipeline logs; pipeline uses OIDC to the cloud instead of static keys.

**Outcome Artifacts**

- Pipeline definition (e.g. `.github/workflows/security.yml`)
- Screenshot of a failed PR blocked by a security finding and the same PR passing after the fix
- Screenshot of branch protection / rulesets and secret push protection settings
- Security dashboard screenshot with findings
- ZAP report (HTML/JSON) from the test environment
- Short README: tools used, severity policy, how to handle false positives

**Vendor Annex**

- GitHub: CodeQL, Dependabot, Secret scanning + push protection, rulesets; `github/codeql-action`, `aquasecurity/trivy-action`, `zaproxy/action-baseline`
- GitLab: `include: template: Security/SAST.gitlab-ci.yml`, `Secret-Detection`, `Container-Scanning`, `DAST`
- AWS: ECR enhanced scanning (Amazon Inspector), CodeGuru Security, Security Hub aggregation
- Key Commands: `trivy image <img> --severity HIGH,CRITICAL --exit-code 1`, `gitleaks detect -v`, `checkov -d ./terraform`, `semgrep --config auto`

---

## SEC-2. Configure and manage role-based access controls (RBAC)

`Key`

**Description**

Configure and manage role-based access controls to ensure secure and least-privilege access to systems and services. Define and apply roles and permissions for users, groups, and service identities across infrastructure platforms, cloud environments, and critical tools. Regularly review and adjust access policies to maintain compliance with security best practices.

**Outcomes**

- Implemented RBAC policies in at least one system (e.g., cloud IAM, Kubernetes, database, CI/CD, SCM, or equivalent).
- Configuration artifacts or policy definitions showing applied roles and permissions.
- Evidence of access reviews or audits confirming enforcement of least-privilege principles.

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Design and implement an RBAC model across a cloud account and a Kubernetes cluster for at least three personas (e.g. Read-only/Auditor, Developer, Platform Admin) and at least one service identity (CI/CD pipeline or in-cluster workload). Roles must be defined as code, applied with least privilege, tested (allowed and denied actions) and reviewed with an access audit.

**Prerequisites**

- AWS account (or Organization) with IAM Identity Center, or Azure Entra ID / GCP IAM
- Kubernetes cluster (EKS / AKS / GKE / kind)
- Terraform
- Test users/groups

**Guidance and Tips**

- Time Planning: 8–12 hours.
- Scope: cloud IAM + Kubernetes RBAC + one service identity (e.g. GitHub Actions OIDC role or IRSA / EKS Pod Identity).
- Tips: Groups → roles, never users → permissions directly. Use namespace-scoped `Role` over `ClusterRole`. Use permission boundaries / SCPs as guardrails. Validate with `kubectl auth can-i` and IAM Policy Simulator.

**Acceptance Criteria**

Functional Requirements

- At least three roles/permission sets are defined and assigned to groups.
- Kubernetes `Role`/`ClusterRole` + `RoleBinding`s are mapped to cloud identities (EKS access entries / aws-auth, AKS Entra integration).
- At least one service identity uses short-lived credentials (OIDC federation, IRSA, Pod Identity, Managed Identity).
- Positive and negative tests prove each role can do exactly what it should (e.g. Developer cannot delete namespaces or read secrets in `prod`).
- An access review is performed (IAM Access Analyzer, last-accessed data, credential report, `kubectl-who-can`, rbac-tool) and at least one excessive permission is removed.

Non-Functional Requirements

- All roles and bindings are stored as code and applied through a pipeline.
- No wildcard `*:*` permissions except a break-glass role with MFA and alerting.
- Audit logging enabled (CloudTrail, Kubernetes audit logs).
- Review cadence and process documented.

**Outcome Artifacts**

- Terraform / YAML definitions of roles, policies and bindings
- RBAC matrix (role × resource × action)
- Terminal output of allow/deny tests (`kubectl auth can-i --as ...`, `aws sts get-caller-identity`, AccessDenied examples)
- Access review report (Access Analyzer findings, before/after diff)
- Short README: role model, onboarding/offboarding, review procedure

**Vendor Annex**

- AWS: IAM Identity Center permission sets, IAM roles, SCPs, permission boundaries, IAM Access Analyzer, EKS access entries
- Azure: Entra ID groups, Azure RBAC, PIM, AKS Entra integration
- GCP: IAM roles, Workload Identity, GKE RBAC
- Key Commands: `kubectl auth can-i --list --as=dev-user -n app`, `aws iam generate-service-last-accessed-details`, `aws accessanalyzer list-findings`

---

## SEC-3. Implement and manage application traffic security controls

**Description**

Configure and maintain security controls for application traffic by applying WAF rules, firewall policies, IDS/IPS systems, and network traffic monitoring. Ensure applications are protected from threats such as unauthorized access, DDoS attacks, and data exposure through proactive filtering and monitoring.

**Outcomes**

- Implemented firewall and WAF configurations along with traffic monitoring solutions, demonstrating effective protection of web applications and network communication.

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Protect a public web application behind a load balancer with layered traffic controls: WAF (managed + custom rules + rate limiting), network firewall policies (security groups / NACLs / Network Firewall or NSG), optional IDS/IPS, DDoS protection, and traffic monitoring (WAF logs, flow logs, threat detection). Demonstrate that malicious requests are blocked and legitimate traffic passes.

**Prerequisites**

- Public application behind ALB / Application Gateway / Cloud Load Balancer (see NET-1)
- Cloud account with WAF service
- Tools for testing: `curl`, `hey`/`ab`/`k6`, optionally `nikto`/`sqlmap` (only against your own environment)

**Guidance and Tips**

- Time Planning: 8–10 hours.
- Scope: WAF on the L7 load balancer, restrictive network rules for backends, flow logs, threat detection, one dashboard.
- Tips: Start WAF rules in Count mode, analyse logs, then switch to Block. Backends must accept traffic only from the load balancer security group.

**Acceptance Criteria**

Functional Requirements

- WAF is attached to the load balancer / CDN with managed rule groups (OWASP Core, Known Bad Inputs, SQLi) enabled.
- At least one custom rule (geo-blocking, IP set, header/path rule) and one rate-based rule are configured.
- Network firewall policies: backends accessible only from the LB; management ports not exposed to the internet; egress restricted.
- IDS/IPS or threat detection is enabled (AWS Network Firewall Suricata rules, GuardDuty, Azure Firewall Premium IDPS, or Suricata on-prem).
- DDoS protection is in place (Shield Standard/Advanced, Azure DDoS Protection, Cloud Armor).
- Traffic logging: WAF logs and VPC/NSG flow logs are collected and queryable.
- Tests prove blocking: SQLi/XSS payload → 403; burst traffic → rate limited; direct backend access → timeout/denied.

Non-Functional Requirements

- Rules are defined as code (Terraform) and versioned.
- False positives are analysed and handled with scoped exclusions.
- Alerts are configured for blocked-request spikes.
- TLS-only access (HTTP → HTTPS redirect, modern TLS policy).

**Outcome Artifacts**

- WAF Web ACL configuration (Terraform/JSON)
- Security group / firewall rules
- Test outputs (blocked/allowed requests)
- WAF logs and flow-log query results
- Dashboard / alarm configuration for traffic monitoring
- Short README: threat model, rule rationale, operational procedures

**Vendor Annex**

- AWS: AWS WAF (`AWSManagedRulesCommonRuleSet`, rate-based rules), Shield, Security Groups, NACL, Network Firewall, VPC Flow Logs, GuardDuty
- Azure: Application Gateway WAF v2 / Front Door WAF, NSG, Azure Firewall Premium, DDoS Protection
- GCP: Cloud Armor, VPC firewall rules, Cloud IDS
- On-prem: ModSecurity + OWASP CRS on Nginx, iptables/nftables, Suricata
- Key Commands: `aws wafv2 create-web-acl ...`, `curl "https://app/?id=1' OR '1'='1"`, `hey -n 5000 -c 100 https://app/`

---

## SEC-4. Implement and manage multi-factor authentication for secure access control

**Description**

Implement, configure, and maintain multi-factor authentication (MFA) as part of secure access control. Ensure MFA is enforced across relevant systems such as cloud services, identity providers, source control platforms, or critical infrastructure. Contribute to policy enforcement and monitor compliance to reduce risks from compromised credentials.

**Outcomes**

- Configured MFA settings or policies within an identity provider, cloud platform, SCM, or equivalent system.
- Documentation or configuration artifacts (e.g., access policies, enforcement rules) demonstrating MFA implementation.
- Evidence of compliance monitoring (e.g., reports, logs, or dashboards confirming MFA adoption).

### 🏗️ Інфраструктура — 🤖 Згенеровано Claude (не офіційний NEBo Task)

**Task Description**

Enforce MFA for human access to the cloud platform, identity provider and source control, add policy-level enforcement (deny sensitive actions without MFA), and set up continuous compliance monitoring that reports MFA adoption and flags non-compliant identities.

**Prerequisites**

- AWS account with IAM Identity Center (or Entra ID / Okta / Google Workspace)
- GitHub / GitLab organization with admin rights
- Test users and an authenticator app / FIDO2 key

**Guidance and Tips**

- Time Planning: 4–6 hours.
- Scope: IdP + cloud + SCM, one enforcement policy, one compliance report/dashboard.
- Tips: Prefer phishing-resistant MFA (FIDO2/WebAuthn). Keep a documented break-glass account with hardware MFA. Avoid long-lived IAM users entirely.

**Acceptance Criteria**

Functional Requirements

- MFA is required for all users in the IdP (IAM Identity Center "always-on" MFA / Entra Conditional Access).
- Root / global admin accounts have MFA enabled.
- Policy enforcement: IAM policy or SCP denies sensitive actions when `aws:MultiFactorAuthPresent` is false (for any remaining IAM users).
- SCM organization requires 2FA for all members.
- Compliance monitoring: AWS Config rules (`root-account-mfa-enabled`, `iam-user-mfa-enabled`, `mfa-enabled-for-iam-console-access`) or Security Hub CIS controls report status.
- A test demonstrates a user without MFA is blocked.

Non-Functional Requirements

- MFA policies are documented and stored as code where possible.
- Compliance status is reviewed periodically (report or dashboard).
- Recovery / lost-device procedure is documented.

**Outcome Artifacts**

- Screenshots of MFA enforcement settings (IdP, cloud, SCM)
- Policy JSON (MFA-condition deny policy / SCP)
- AWS Config / Security Hub compliance report or IAM credential report
- Evidence of blocked login/action without MFA
- Short README: MFA policy, supported factors, recovery and break-glass procedure

**Vendor Annex**

- AWS: IAM Identity Center MFA settings, IAM `aws:MultiFactorAuthPresent`, AWS Config managed rules, Security Hub CIS
- Azure: Entra ID Conditional Access, Security Defaults, MFA registration report
- GitHub: Organization → Authentication security → Require 2FA
- Key Commands: `aws iam generate-credential-report; aws iam get-credential-report`, `aws configservice describe-compliance-by-config-rule`
